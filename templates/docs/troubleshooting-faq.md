# Troubleshooting FAQ: [PROJECT NAME]

**Last updated:** [YYYY-MM-DD]
**Owner:** [NAME / TEAM]

Use this document to diagnose and resolve common problems. If your issue isn't listed, check the runbook or open an incident.

---

## General Issues

---

### Problem: [The app won't start locally]

**Symptoms:** Running `[npm run dev]` fails immediately or crashes on startup.

**Cause:** Missing environment variables, incompatible Node version, or missing dependencies.

**Fix:**

1. Confirm you have copied `.env.example` to `.env` and filled in all required values.
2. Confirm your Node version matches the project requirement: `node --version` (required: `[v20+]`).
3. Delete `node_modules` and reinstall: `rm -rf node_modules && [npm install | pnpm install]`.
4. If using Docker, rebuild the image: `docker compose down && docker compose up --build`.
5. Check the error message for the specific variable or module that is missing and address it directly.

---

### Problem: [Login / authentication is broken]

**Symptoms:** Users cannot log in. Login form submits but redirects back to login, or returns a 401/403.

**Cause:** Session secret mismatch, expired cookies, or misconfigured auth provider.

**Fix:**

1. Confirm `[SESSION_SECRET | AUTH_SECRET | NEXTAUTH_SECRET]` is set and consistent across all instances.
2. Clear browser cookies for the app's domain and try again.
3. Confirm the auth callback URL in your provider (e.g., Google, GitHub OAuth) matches the current environment URL exactly, including protocol and trailing slash.
4. Check logs for `[invalid signature | jwt malformed | csrf]` errors.
5. If using a third-party auth provider, check their status page for outages.

---

### Problem: [Database connection fails]

**Symptoms:** App returns 500 errors. Logs show `Connection refused`, `ECONNREFUSED`, or `Can't reach database server`.

**Cause:** Wrong `DATABASE_URL`, database service is stopped, or too many connections.

**Fix:**

1. Confirm `DATABASE_URL` is set and points to the correct host and database name.
2. Test connectivity directly: `psql $DATABASE_URL -c "SELECT 1;"`.
3. Check the database service is running in [Railway / Supabase / your platform] dashboard.
4. Check active connection count (see runbook — Database section). If at the max, restart the app or increase the connection pool limit.
5. If using Prisma, confirm the datasource `url` in `schema.prisma` uses the env var: `url = env("DATABASE_URL")`.

---

### Problem: [API requests return CORS errors]

**Symptoms:** Browser console shows `CORS policy: No 'Access-Control-Allow-Origin' header`. Requests work in Postman but not in the browser.

**Cause:** The API is not configured to allow requests from the frontend's origin.

**Fix:**

1. Confirm the `[CORS_ORIGIN | ALLOWED_ORIGINS]` env var is set to the frontend's URL (no trailing slash).
2. If the backend is Next.js, check that the API route includes the correct CORS headers or uses a CORS middleware.
3. Confirm the frontend is sending requests to the correct API URL (`[NEXT_PUBLIC_API_URL]` or equivalent).
4. For local development, confirm both frontend (`localhost:[PORT]`) and backend origins are added to the allowed list.

---

### Problem: [File uploads fail or return errors]

**Symptoms:** Upload UI shows an error. Logs show a storage error or 413 (payload too large).

**Cause:** Missing storage credentials, bucket not configured, or file size exceeds limit.

**Fix:**

1. Confirm storage credentials (`[S3_ACCESS_KEY | R2_ACCESS_KEY | STORAGE_KEY]`) are set correctly.
2. Confirm the bucket name and region match the env vars.
3. Check if the file exceeds the configured size limit. Adjust `maxFileSize` in the upload config if needed.
4. For 413 errors on a proxied server (nginx, Vercel), increase the `client_max_body_size` or configure `body-parser` limits.

---

### Problem: [Emails are not being delivered]

**Symptoms:** Transactional emails (welcome, password reset, notifications) are not arriving. No errors logged.

**Cause:** API key missing or invalid, domain not verified, or emails landing in spam.

**Fix:**

1. Confirm `[EMAIL_API_KEY | RESEND_API_KEY | SENDGRID_API_KEY]` is set.
2. Log into the email provider dashboard and check the delivery log for the specific email.
3. Confirm the sending domain is verified (check DNS records: SPF, DKIM, DMARC).
4. Check the recipient's spam folder.
5. Test delivery by sending a test email via the provider's dashboard.

---

### Problem: [Payments are not processing]

**Symptoms:** Checkout fails. Stripe/payment provider returns an error. Webhooks are not being received.

**Cause:** Test/live key mismatch, webhook secret wrong, or webhook URL not configured.

**Fix:**

1. Confirm `[STRIPE_SECRET_KEY]` matches the environment (test keys start with `sk_test_`, live with `sk_live_`).
2. Confirm `[STRIPE_WEBHOOK_SECRET]` matches the webhook endpoint's signing secret in the Stripe dashboard.
3. Confirm the webhook URL registered in Stripe points to the current environment's URL.
4. For local development, use the Stripe CLI to forward webhooks: `stripe listen --forward-to localhost:[PORT]/api/webhooks/stripe`.

---

### Problem: [Background jobs / cron tasks are not running]

**Symptoms:** Scheduled tasks haven't run. Queue depth is growing. Job status shows "pending" indefinitely.

**Cause:** Worker process is stopped, job queue connection is misconfigured, or the cron schedule expression is wrong.

**Fix:**

1. Confirm the worker service is running in [Railway / Fly.io] dashboard (it's a separate service from the web process).
2. Confirm the queue connection string (`[REDIS_URL | DATABASE_URL]`) is set for the worker service.
3. Check the cron schedule expression using [crontab.guru](https://crontab.guru) to confirm it triggers when expected.
4. Check worker logs for errors or exceptions.
5. Manually trigger the job to confirm it works: `[npm run jobs:run -- --job=JOB_NAME | equivalent command]`.

---

## Environment-Specific Issues

---

### Local development

**Problem: Hot reload is not working.**
Restart the dev server. On macOS, if file watchers hit the OS limit, run: `sudo sysctl -w kern.maxfiles=65536 kern.maxfilesperproc=65536`.

**Problem: `localhost` and container services can't see each other (Docker).**
Use the service name as the hostname inside Docker Compose (e.g., `postgres://db:5432/mydb` not `localhost:5432`).

**Problem: Environment variables are undefined in the browser.**
Confirm browser-exposed variables are prefixed with `NEXT_PUBLIC_` (Next.js) or `VITE_` (Vite). Restart the dev server after adding new env vars.

---

### Staging

**Problem: Staging database has stale or missing data.**
Run the seed script: `[npm run db:seed:staging]`. If staging data needs to mirror production, use a sanitized export (remove PII before copying).

**Problem: Staging behaves differently from local but matches production.**
Check whether the staging environment has a different value for a feature flag or configuration variable. Compare env vars between environments.

---

### Production

**Problem: A deploy succeeded but the bug is still present.**
Confirm the deploy finished and the new version is serving: check `[/api/version | deployment timestamp]` or review the deploy log for the expected commit SHA.

**Problem: A user reports a bug that cannot be reproduced locally or in staging.**
Check production logs filtered by that user's ID or session. Look for environment-specific data (edge case in their data, a feature flag state, or a race condition under load).

**Problem: Production is slow after a deploy.**
Check for a new query introduced in the deploy that lacks an index. Pull slow query logs from the DB (see runbook). Consider rolling back if degradation is severe.

---

---

## Prompt: How to generate this document

```
I need to write a troubleshooting FAQ for [PROJECT NAME].

Here is the project context:
- Tech stack: [e.g., Next.js, PostgreSQL, Prisma, Railway, Stripe, Resend]
- Auth method: [e.g., NextAuth, Clerk, custom JWT]
- Key integrations: [list external services used]
- Common failure points I've already encountered: [describe any known issues]

Please generate a troubleshooting FAQ with these sections:

1. General issues — for each of these common problem categories: [app won't start, auth broken, database connection fails, CORS errors, file uploads, email delivery, payments, background jobs] — write an entry with:
   - Problem: [clear name]
   - Symptoms: [what the user or developer sees]
   - Cause: [root cause explanation]
   - Fix: [numbered step-by-step resolution, most likely fix first]

2. Environment-specific issues — separate subsections for local development, staging, and production. List common gotchas specific to each environment.

Use the actual tool names, env var names, and commands from the stack listed above. Be specific — avoid generic advice like "check your configuration." Make every step actionable.
```

---

## Prompt: How to update this document

```
I have an existing troubleshooting FAQ for [PROJECT NAME] that needs to be reviewed and updated.

Here is the current FAQ:
[PASTE FAQ HERE]

Here is what I want to add or change:
[Describe new issues discovered in production, changed tools or integrations, renamed env vars, new environments added, etc.]

Please:
1. Add new FAQ entries for any new issues described above
2. Update existing entries where the fix steps have changed (e.g., tool or command changed)
3. Remove or mark as outdated any entries that no longer apply
4. Add any new environment-specific issues to the appropriate subsection
5. Update the "Last updated" date to today

Keep entries in the same format: Problem / Symptoms / Cause / Fix. Do not rewrite entries that haven't changed.
```
