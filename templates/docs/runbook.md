# Operations Runbook: [PROJECT NAME]

**Last updated:** [YYYY-MM-DD]
**Owner:** [NAME / TEAM]
**Production URL:** [https://your-app.com]
**Status page:** [https://status.your-app.com | N/A]
**On-call rotation:** [Link or names]

---

## Service Overview

**What it does:** [One or two sentences — what the service does and why it exists.]

**Where it runs:**

| Component | Platform | Region | URL |
|-----------|----------|--------|-----|
| [Web app] | [Vercel / Railway] | [us-east-1] | [https://...] |
| [API / backend] | [Railway / Fly.io] | [us-east-1] | [https://...] |
| [Database] | [Railway PostgreSQL / Supabase] | [us-east-1] | [internal] |
| [Background jobs] | [Railway worker / Inngest] | [us-east-1] | [N/A] |

**Dependencies:**

| Service | Purpose | Owned by | Status page |
|---------|---------|----------|-------------|
| [Stripe] | [Payment processing] | [Third party] | [https://status.stripe.com] |
| [Resend] | [Transactional email] | [Third party] | [https://status.resend.com] |
| [SERVICE] | [PURPOSE] | [Internal / Third party] | [URL] |

---

## Common Incidents

---

### Incident: [App is down / returning 5xx errors]

**Symptoms:**
- Users report the app is unavailable or showing an error page
- Health check endpoint returns non-200
- Error spike in [Sentry / Datadog / logs]

**Initial checks:**

1. Check [Railway / Vercel] dashboard — is the deployment healthy?
2. Check for a recent deploy: `[railway status | vercel list]`
3. Check error logs (see Logs and Monitoring section)
4. Check database connectivity (see Database section)
5. Check third-party service status pages (see Dependencies above)

**Resolution steps:**

- **If caused by a bad deploy:** Roll back using the procedure in the Deployment section.
- **If database is unreachable:** Confirm the DATABASE_URL env var is set and the DB service is running. Restart the database if needed.
- **If a third-party service is down:** Confirm the outage on their status page. Enable any fallback behavior in the app. Post a status update.
- **If cause is unknown:** Restart the application service: `[railway restart | fly restart]`

**When to escalate:** If the app is not restored within 30 minutes, or if data loss is suspected.

---

### Incident: [Slow response times / high latency]

**Symptoms:**
- Pages take more than [5 seconds] to load
- API responses exceed [2 seconds]
- Users report timeouts

**Initial checks:**

1. Check current CPU and memory usage on [Railway / Fly.io] dashboard.
2. Look for slow queries in the database (see Database section — `pg_stat_statements` or equivalent).
3. Check for a background job or cron that may be consuming resources.
4. Review recent code changes for N+1 queries or missing indexes.

**Resolution steps:**

- **If DB queries are slow:** Identify the slow query, add an index if missing, or kill long-running queries.
- **If memory is exhausted:** Restart the service. Consider increasing the memory limit.
- **If a background job is the cause:** Pause or cancel the job via [the job dashboard / CLI].

**When to escalate:** If latency remains elevated after 15 minutes with no clear cause.

---

### Incident: [Emails not sending]

**Symptoms:**
- Users report not receiving [welcome / reset / notification] emails
- Email error in logs: `[error message pattern]`

**Initial checks:**

1. Check [Resend / SendGrid / etc.] dashboard for delivery failures.
2. Confirm the `[EMAIL_API_KEY]` env var is set correctly.
3. Check that the sending domain is verified.
4. Look for exceptions in the email job logs.

**Resolution steps:**

- **If API key is wrong:** Update the env var and restart the service.
- **If domain is unverified:** Complete domain verification in the email provider dashboard.
- **If provider is down:** Queue emails locally and retry when the provider recovers.

**When to escalate:** If emails are confirmed delivered but not received, the issue may be spam filtering — escalate to the email provider.

---

### Incident: [DATABASE INCIDENT — e.g., migration failed / data inconsistency]

**Symptoms:**
- [Describe specific symptoms]

**Initial checks:**

1. [Check.]
2. [Check.]

**Resolution steps:**

- [Step.]

**When to escalate:** [Condition.]

---

## Deployment

### Deploy

```bash
# Trigger a production deploy
[railway up | vercel --prod | git push origin main]
```

**Automatic deploys:** [Enabled on push to `main` | Manual only]

### Verify after deploy

- [ ] `GET [https://your-app.com/health]` returns `200`
- [ ] No new errors in [Sentry / logs] within 5 minutes of deploy
- [ ] [Key user flow] works end-to-end

### Rollback

```bash
# Roll back to the previous deployment
[railway rollback | vercel rollback [DEPLOY_URL]]
```

For full rollback steps including database, see the Deployment Guide.

---

## Logs and Monitoring

### Where to find logs

| Log type | Location | How to access |
|----------|----------|---------------|
| Application logs | [Railway dashboard] | `railway logs` or dashboard → Deployments → Logs |
| Error tracking | [Sentry] | [https://sentry.io/organizations/YOUR_ORG] |
| Database logs | [Railway PostgreSQL] | Dashboard → Database → Logs |
| Email logs | [Resend dashboard] | [https://resend.com/logs] |

### Filtering logs

```bash
# Tail live logs
railway logs --tail

# Search for errors in the last hour
railway logs | grep -i "error\|exception\|fatal"

# Filter by request path
railway logs | grep "POST /api/[endpoint]"
```

### Key metrics to monitor

| Metric | Normal range | Alert threshold | Where to check |
|--------|-------------|-----------------|----------------|
| HTTP 5xx rate | < 0.1% | > 1% | [Sentry / Railway metrics] |
| P95 response time | < [500ms] | > [2000ms] | [Railway metrics] |
| Database connections | < [80% of max] | > 90% | [DB dashboard] |
| Background job queue depth | < [100] | > [500] | [Job dashboard] |
| Memory usage | < [70%] | > 85% | [Railway metrics] |

---

## Database

### Connection

```bash
# Connect via CLI (local tunnel or direct)
[psql $DATABASE_URL]
[railway connect PostgreSQL]
```

> Never run destructive queries (`DROP`, `DELETE` without `WHERE`, `TRUNCATE`) in production without a backup confirmed.

### Diagnostic queries

```sql
-- Check active connections
SELECT count(*) FROM pg_stat_activity WHERE state = 'active';

-- Find long-running queries (>30 seconds)
SELECT pid, now() - query_start AS duration, query
FROM pg_stat_activity
WHERE state = 'active' AND now() - query_start > interval '30 seconds'
ORDER BY duration DESC;

-- Check table sizes
SELECT relname AS table, pg_size_pretty(pg_total_relation_size(relid)) AS size
FROM pg_catalog.pg_statio_user_tables
ORDER BY pg_total_relation_size(relid) DESC;

-- [Add project-specific diagnostic queries here]
-- e.g., count records in key tables, check for stuck jobs, etc.
```

### Data repair

> Only run repair scripts after confirming the scope of the issue and, if possible, taking a snapshot.

```sql
-- [Example: Fix stuck jobs older than 1 hour]
-- UPDATE jobs SET status = 'failed' WHERE status = 'running' AND started_at < NOW() - INTERVAL '1 hour';

-- [Add project-specific repair queries here]
```

---

## Contacts and Escalation

| Role | Name | Contact | When to reach |
|------|------|---------|---------------|
| Primary on-call | [NAME] | [Slack @handle / phone] | Severity 1–2 incidents |
| Secondary on-call | [NAME] | [Slack @handle / phone] | Primary unavailable |
| Database admin | [NAME / Team] | [Slack #channel] | DB issues, data repair |
| Hosting platform support | [Railway / Vercel] | [support URL] | Platform outages |
| Security incidents | [NAME / Team] | [Email / Slack] | Any suspected data breach |

**Escalation path:**
1. First responder investigates using this runbook.
2. If unresolved in [30 minutes], page the secondary on-call.
3. If data loss or security breach is suspected, immediately notify [NAME] and stop the service.

---

---

## Prompt: How to generate this document

```
I need to write an operations runbook for [PROJECT NAME].

Here is the project context:
- What it does: [brief description]
- Tech stack: [e.g., Next.js, PostgreSQL, Railway, Vercel]
- Key external dependencies: [e.g., Stripe, Resend, Auth0]
- Team size: [number of people who will use this runbook]
- Monitoring tools: [e.g., Sentry, Datadog, or none]

Please generate a complete operations runbook with these sections:
1. Service overview — what it does, where it runs (table of components), and dependencies (table with status pages)
2. Common incidents — for each of these scenarios: [app is down, slow performance, email failures, migration failures] — include symptoms, initial checks, resolution steps, and when to escalate
3. Deployment — commands to deploy, post-deploy verification checklist, and rollback
4. Logs and monitoring — where logs live, how to filter them, and a key metrics table with normal ranges and alert thresholds
5. Database — connection commands, diagnostic SQL queries, and data repair notes
6. Contacts and escalation — table of roles and a numbered escalation path

Write as if the reader is the on-call engineer responding to an alert at 2am. Be specific and procedural. Assume they have repo access and CLI tools installed.
```

---

## Prompt: How to update this document

```
I have an existing operations runbook for [PROJECT NAME] that needs to be reviewed and updated.

Here is the current runbook:
[PASTE RUNBOOK HERE]

Here is what has changed since the last update:
[Describe changes — e.g., migrated from Heroku to Railway, added Sentry for error tracking, new third-party dependency added, team contacts changed, new common incident pattern discovered]

Please:
1. Update all affected sections based on the changes above
2. Review every command and confirm it matches the current platform and tools
3. Add any new incident patterns that have been discovered in production
4. Update the contacts table if personnel have changed
5. Flag any sections that reference removed tools or outdated procedures
6. Update the "Last updated" date to today

Keep the same structure. Do not rewrite sections that haven't changed.
```
