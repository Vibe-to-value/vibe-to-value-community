# Architecture Review: [PROJECT NAME]

**Review date:** [YYYY-MM-DD]
**Next review:** [YYYY-MM-DD — recommend quarterly or after major changes]
**Reviewer(s):** [NAME(S)]
**Owner:** [NAME / TEAM]

---

## System Overview

**What it does:** [Two to three sentences describing what the system does, who uses it, and its current scale. E.g., "A multi-tenant SaaS application that allows [users] to [do X]. Currently serving [N] users and processing [N] requests/day."]

**Architecture style:** [e.g., Monolith / Monorepo with separate services / Microservices / Serverless / Edge-first]

**Current growth stage:** [e.g., MVP / Early production (~100 users) / Scaling (1k–10k users) / Mature]

**Diagram:** [Link to architecture diagram in Figma / Miro / Excalidraw, or note "None — described in Data Flow section below"]

---

## Component Inventory

| Component | Purpose | Technology | Owner | Status |
|-----------|---------|------------|-------|--------|
| [Web frontend] | [User-facing UI] | [Next.js 14, React, Tailwind] | [NAME / Team] | [Active] |
| [API / backend] | [Business logic, data access] | [Next.js API routes / Express / Fastify] | [NAME / Team] | [Active] |
| [Database] | [Primary data store] | [PostgreSQL on Railway] | [NAME / Team] | [Active] |
| [Background worker] | [Async jobs, scheduled tasks] | [Node.js / Inngest / BullMQ] | [NAME / Team] | [Active] |
| [File storage] | [User uploads, attachments] | [Cloudflare R2 / AWS S3] | [NAME / Team] | [Active] |
| [Email service] | [Transactional email] | [Resend / SendGrid] | [Third party] | [Active] |
| [Auth] | [User authentication] | [NextAuth / Clerk / Custom JWT] | [NAME / Third party] | [Active] |
| [Payments] | [Billing and subscriptions] | [Stripe] | [Third party] | [Active] |
| [Error tracking] | [Runtime error capture] | [Sentry] | [Third party] | [Active] |
| [COMPONENT] | [PURPOSE] | [TECHNOLOGY] | [OWNER] | [Active / Deprecated / Planned] |

---

## Data Flow Description

### Primary request path

1. **User → Frontend:** User action triggers a request from the browser.
2. **Frontend → API:** [Next.js server action / fetch to /api/] carries the request with [session cookie / JWT / API key].
3. **API → Auth:** Authentication is verified [via NextAuth session / JWT validation / middleware].
4. **API → Database:** Authorized request queries [PostgreSQL via Prisma / Drizzle / raw SQL].
5. **API → Frontend:** Response is returned as [JSON / streamed HTML / server component].

### Async / background paths

- **[Job trigger]:** When [event X happens], a job is enqueued in [BullMQ / Inngest / etc.].
- **Worker:** The worker process picks up the job and performs [action — e.g., send email, resize image, sync data].
- **Webhooks:** [Stripe / third party] sends webhook events to `[/api/webhooks/stripe]`. The endpoint validates the signature and enqueues downstream processing.

### External integrations

| Integration | Direction | Protocol | Auth method |
|-------------|-----------|----------|-------------|
| [Stripe] | Outbound + inbound webhook | HTTPS | [API key + webhook signature] |
| [Resend] | Outbound | HTTPS | [API key] |
| [Cloudflare R2] | Outbound | S3-compatible HTTPS | [Access key + secret] |
| [INTEGRATION] | [In/Out/Both] | [Protocol] | [Auth method] |

---

## Security Considerations

### Authentication and authorization

- **Auth method:** [NextAuth with [provider] / Clerk / Custom JWT / Session cookies]
- **Session storage:** [HTTP-only cookie / JWT in localStorage / Database-backed session]
- **Authorization model:** [Role-based (admin/user/guest) / Ownership-based (users can only access their own records) / No authorization — single user system]
- **Known gaps:** [List any auth/authz gaps — e.g., "No rate limiting on login endpoint", "API keys are not scoped to specific operations"]

### Data protection

- **Data in transit:** [All traffic uses HTTPS. Certificates managed by [platform / Cloudflare].]
- **Data at rest:** [Database encryption at rest is [enabled by default on Railway / not configured].]
- **Sensitive fields:** [Passwords are hashed with [bcrypt / argon2]. Payment data is never stored — tokenized via Stripe.]
- **PII handling:** [Describe where PII is stored and any masking or anonymization in logs.]
- **Known gaps:** [e.g., "Logs may contain email addresses", "No data retention/purge policy"]

### Secrets management

- **Where secrets live:** [Railway environment variables / Vercel / AWS Secrets Manager]
- **Secret rotation policy:** [See environment variable manifest — rotation log section]
- **Known gaps:** [e.g., "No automated secret rotation", "Dev team shares a single database user"]

---

## Scalability Assessment

| Dimension | Current state | Bottleneck risk | Notes |
|-----------|--------------|-----------------|-------|
| Database connections | [Max ~20 connections, using [Prisma connection pool]] | [Medium] | [Increase pool or add PgBouncer if concurrency grows] |
| API throughput | [Single Railway instance, ~N req/s] | [Low at current scale] | [Railway auto-scales on paid plan] |
| Background jobs | [Single worker, sequential processing] | [Medium — jobs queue if volume spikes] | [Add concurrency or second worker instance if queue depth grows] |
| File uploads | [Streamed to R2 via signed URL] | [Low — offloaded to Cloudflare] | [Max file size: [N]MB, enforced at API layer] |
| Search | [Database LIKE queries] | [High at scale] | [Will need full-text search (Postgres FTS or Algolia) above ~[N] records] |
| Email volume | [Transactional only, low volume] | [Low] | [Current plan supports [N] emails/month] |

**Scale ceiling estimate:** [At current architecture, the system can comfortably support approximately [N] concurrent users / [N] requests per day before [specific component] becomes the bottleneck.]

---

## Technical Debt Inventory

| Item | Severity | Description | Effort to fix | Priority |
|------|----------|-------------|--------------|----------|
| [Missing indexes] | [High] | [Table X has no index on [column], causing full table scans on the most common query] | [Low — 1 migration] | [High] |
| [No retry on email] | [Medium] | [Email sending is fire-and-forget. Transient failures silently drop emails] | [Medium — add queue + retry logic] | [Medium] |
| [N+1 queries in [route]] | [Medium] | [Route [/api/X] issues N+1 queries for [relationship]. Will degrade at scale] | [Low — add include/join] | [High] |
| [No automated tests] | [High] | [Zero test coverage. Regressions caught manually] | [High — ongoing investment] | [Medium] |
| [ITEM] | [Low/Med/High] | [Description] | [Low/Med/High effort] | [Low/Med/High] |

---

## Integration Points

Describe each external dependency and its failure behavior.

| Integration | What breaks if it's down | Fallback | SLA |
|-------------|-------------------------|----------|-----|
| [PostgreSQL] | [Entire application — all reads and writes fail] | [None — critical dependency] | [[Railway SLA]] |
| [Stripe] | [Checkout and billing fail. Existing users unaffected] | [Show "payment unavailable" message] | [99.99%] |
| [Resend] | [Transactional emails not sent. No user-visible error] | [Retry next request cycle / silently queue] | [[Resend SLA]] |
| [R2 / S3] | [File uploads and downloads fail] | [Return error, allow retry] | [[Cloudflare SLA]] |
| [INTEGRATION] | [IMPACT] | [FALLBACK] | [SLA] |

---

## Recommendations

List specific, actionable improvements identified during this review. Prioritize by impact and urgency.

### High priority

- **[Add a deploy health check]:** The deploy pipeline does not verify that the app is healthy after deploy. A failed deploy may serve errors for minutes before detection. Add a health check endpoint and fail the deploy if it doesn't return 200 within [30 seconds]. Estimated effort: [low].
- **[Add indexes to [table/column]]:** [Specific query pattern is causing slow reads at current scale. Add the index before user growth continues.] Estimated effort: [low].

### Medium priority

- **[Add email retry queue]:** Email sends are fire-and-forget. Implement a retry mechanism (using [BullMQ / Inngest / database queue]) so transient provider failures do not silently drop emails. Estimated effort: [medium].
- **[RECOMMENDATION]:** [Description. Estimated effort: low/medium/high.]

### Low priority / future consideration

- **[Introduce API versioning]:** As the API is exposed to more consumers, versioning will allow breaking changes without coordinating simultaneous client updates. Estimated effort: [medium — future].
- **[RECOMMENDATION]:** [Description.]

---

## Review Summary

| Area | Rating | Notes |
|------|--------|-------|
| Architecture clarity | [Green / Yellow / Red] | [e.g., Clean monolith, well understood] |
| Security | [Green / Yellow / Red] | [e.g., Auth is solid; secrets management needs work] |
| Scalability | [Green / Yellow / Red] | [e.g., Fine at current scale; search will be a bottleneck at 10x] |
| Technical debt | [Green / Yellow / Red] | [e.g., Manageable; missing tests is the biggest risk] |
| Observability | [Green / Yellow / Red] | [e.g., Sentry is set up; no structured logging or dashboards] |
| Documentation | [Green / Yellow / Red] | [e.g., Runbook exists; data model not documented] |

**Overall rating:** [Green / Yellow / Red]

**Most important action before next review:** [Single most important thing to address.]

---

---

## Prompt: How to generate this document

```
I need to create an architecture review document for [PROJECT NAME] based on the existing codebase.

Here is the project context:
- What it does: [brief description and current scale]
- Tech stack: [list all technologies — frontend, backend, database, hosting, third-party services]
- Architecture style: [monolith / serverless / microservices / etc.]
- Team size: [number of engineers]
- Current growth stage: [MVP / early production / scaling]

To help you understand the system, I'll provide one or more of the following:
[PASTE ANY OF THESE]
- Directory structure: [paste output of `tree -L 3` or similar]
- Package.json or key dependencies
- Database schema (Prisma schema, SQL, or description)
- List of API routes
- Description of how the system works

Please generate a complete architecture review with these sections:
1. System overview — what it does, architecture style, current scale, link/note on diagram
2. Component inventory — table of every major component with purpose, technology, owner, and status
3. Data flow description — numbered primary request path, async/background paths, external integrations table
4. Security considerations — auth/authz model, data protection, secrets management, and known gaps
5. Scalability assessment — table rating each dimension (DB, API, jobs, storage, search, email) with bottleneck risk
6. Technical debt inventory — table of specific debt items with severity, description, effort, priority
7. Integration points — table of each external dependency with failure impact and fallback
8. Recommendations — high/medium/low priority improvements with effort estimates
9. Review summary — stoplight table across key areas with an overall rating

Be specific and honest. Flag gaps even when they're uncomfortable. Every recommendation should be actionable, not generic.
```

---

## Prompt: How to update this document

```
I have an existing architecture review for [PROJECT NAME] and it's time to conduct a periodic review.

Here is the previous architecture review:
[PASTE PREVIOUS REVIEW HERE]

Here is what has changed since the last review:
[Describe changes — e.g., migrated database, added new service, changed hosting platform, grew from 100 to 1000 users, added new third-party integrations, addressed technical debt items]

Here is additional context for this review:
[PASTE ANY OF THESE]
- Current directory structure or major new files/folders
- New dependencies added to package.json
- New database tables or schema changes
- Performance observations or incidents since last review
- Status updates on previous action items

Please:
1. Update the Component Inventory to reflect added, removed, or changed components
2. Update the Data Flow if new integrations or paths have been added
3. Reassess Security, Scalability, and Technical Debt based on changes and growth
4. Mark previous recommendations as "Complete", "In Progress", or "Deferred" and add new recommendations
5. Update the Integration Points table for any new or removed dependencies
6. Update the Review Summary stoplight table
7. Update the review date and set the next review date
8. Note the most important action before the next review

Keep what hasn't changed. Focus on what's new or different.
```
