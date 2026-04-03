# Limitations and Assumptions: [PROJECT NAME]

**Last updated:** [YYYY-MM-DD]
**Owner:** [NAME / TEAM]
**Version:** [1.0]

This document records what the project assumes to be true, what it knowingly cannot do well, and what it deliberately excludes. Keeping this current prevents surprises when assumptions break or constraints become blockers.

---

## Assumptions

These are things the project assumes are true about the environment, users, and data. If an assumption turns out to be wrong, it may require significant rework.

### Environment assumptions

- **[Hosting stability]:** [The hosting platform (Railway / Vercel / etc.) maintains at least 99.9% uptime. The project does not implement self-healing beyond automatic restarts provided by the platform.]
- **[Single region]:** [All services (web, database, workers) run in the same region. Cross-region latency has not been accounted for.]
- **[Outbound internet access]:** [The backend can make outbound HTTP requests to third-party APIs (Stripe, Resend, etc.) without firewall restrictions.]
- **[Environment variable availability]:** [All required environment variables are set before the application starts. There is no runtime fallback for missing secrets.]
- **[Add assumption]:** [Description.]

### User assumptions

- **[Authentication]:** [Users authenticate via [email/password | OAuth | magic link]. Multi-factor authentication is not implemented.]
- **[Single account per email]:** [Each user has one account. Merging accounts or changing the email address on an account is not supported.]
- **[Browser support]:** [Users access the app via a modern browser (Chrome, Firefox, Safari, Edge — last two major versions). Internet Explorer and legacy mobile browsers are not supported.]
- **[Connectivity]:** [Users have a stable internet connection. Offline usage, progressive web app behavior, and background sync are not implemented.]
- **[Technical literacy]:** [Users are expected to [describe the minimum technical level — e.g., "be comfortable with basic SaaS tools" / "be developers comfortable with CLI tools"].]
- **[Add assumption]:** [Description.]

### Data assumptions

- **[Data volume]:** [The system is designed for up to [X] records in [key table]. Query performance has not been tested beyond this scale. Indexes and pagination are in place up to this threshold.]
- **[Data integrity]:** [Input data is validated at the API layer. No assumptions are made about the validity of data imported from external systems without transformation/validation.]
- **[Retention]:** [Data is retained indefinitely unless a user explicitly deletes their account. There is no automated purge, archival, or data lifecycle policy.]
- **[Locale]:** [The application stores and displays dates in UTC. Timezone conversion is handled in the browser. Content is English-only.]
- **[Add assumption]:** [Description.]

---

## Known Limitations

These are areas where the system is incomplete, fragile, or underperforms. They are known and accepted for now, but will require attention before the project can scale or harden.

### Incomplete implementations

- **[[Feature name] is partially built]:** [Describe what works and what doesn't. E.g., "The CSV export feature exports all rows but does not apply the current filter state. Users must filter results manually after export."]
- **[Error handling is inconsistent]:** [Some API routes return generic 500 errors without descriptive messages. Client-side error handling does not distinguish between network failures and API errors in all cases.]
- **[Add limitation]:** [Description.]

### Fragile areas

- **[[Component or integration] has no retry logic]:** [E.g., "Outbound email sending is fire-and-forget. If the email provider returns a transient error, the email is lost. There is no queue or retry mechanism."]
- **[Background jobs are not idempotent]:** [If a job fails and is retried, it may produce duplicate records or side effects. Each job must be manually inspected after a failure.]
- **[Session management]:** [Sessions are stored in [cookies / database]. If the [session table / cookie secret] changes, all active sessions are invalidated and users are logged out.]
- **[Race conditions in [area]]:** [Concurrent requests to [endpoint] can result in [describe the race condition — e.g., "duplicate records if two users submit the same form simultaneously"]. A database-level unique constraint prevents data corruption but does not provide a clean user-facing error.]
- **[Add limitation]:** [Description.]

### Deferred work

- **[Search is not implemented]:** [The app has no full-text search. Users must browse or filter to find records. A proper search integration (e.g., Algolia, Postgres full-text search) is deferred.]
- **[No automated tests]:** [The project has [no tests / only unit tests / only integration tests]. Regressions are caught manually. A test suite is planned but not yet in place.]
- **[Monitoring is minimal]:** [Error tracking is [not configured / handled only by Sentry]. There are no alerting rules, dashboards, or SLO tracking in place.]
- **[Accessibility (a11y)]:** [The UI has not been audited for accessibility. Known gaps include [describe — e.g., missing ARIA labels on icon buttons, no keyboard navigation for custom dropdowns].]
- **[Add deferred item]:** [Description.]

---

## Intentional Constraints

These are deliberate design choices that limit scope. They are not bugs or oversights — they are tradeoffs made to keep the project simple and focused.

- **[Single-tenant only]:** [The application is built for a single organization or user base. Multi-tenancy (data isolation per organization, per-tenant billing, custom domains) is out of scope. Implementing it would require significant schema and auth changes.]
- **[No admin UI]:** [Administrative actions (resetting passwords, manually adjusting records, managing feature flags) are done via direct database access or CLI scripts. A dedicated admin interface is out of scope.]
- **[No API versioning]:** [The public API has no versioning scheme. Breaking changes will be communicated to consumers directly. This is acceptable while the consumer base is small.]
- **[No internationalization (i18n)]:** [The application is English-only. Dates, currencies, and text are not localized. Adding i18n would require a full translation infrastructure that is not warranted at current scale.]
- **[No mobile app]:** [The product is a web application. There is no native iOS or Android app. The web app is responsive but is not optimized for mobile as a primary use case.]
- **[Add constraint]:** [Description and rationale.]

---

---

## Prompt: How to generate this document

```
I need to write a limitations and assumptions document for [PROJECT NAME].

Here is the project context:
- What it does: [brief description]
- Tech stack: [languages, frameworks, infrastructure]
- Current stage: [prototype / MVP / early production / scaling]
- Team size: [number of engineers]
- Known shortcuts or quick decisions that may cause problems later: [list any]

Please generate a limitations and assumptions document with three sections:

1. Assumptions — what the project assumes to be true about the environment (hosting, third-party services, outbound access), users (auth method, browser support, technical ability, connectivity), and data (volume limits, integrity, retention, locale). For each assumption, explain what would break if the assumption turned out to be wrong.

2. Known limitations — areas that are incomplete (partially built features), fragile (missing retry logic, race conditions, no idempotency), or deferred (search, testing, monitoring, accessibility). Be honest and specific — this is for the team's benefit.

3. Intentional constraints — deliberate design choices that limit scope, with a one-line rationale for each. Examples: single-tenant only, no admin UI, no API versioning, English only, no mobile app.

Write this as a team-facing document, not a public-facing one. Be direct about what doesn't work well. Avoid defensive language.
```

---

## Prompt: How to update this document

```
I have an existing limitations and assumptions document for [PROJECT NAME] that needs to be reviewed and updated.

Here is the current document:
[PASTE DOCUMENT HERE]

Here is what has changed since the last update:
[Describe what was built, what was fixed, what new shortcuts were taken, what new constraints were introduced, or what assumptions have been validated or invalidated.]

Please:
1. Remove any limitations that have been addressed and resolved
2. Add new limitations or deferred items that have emerged
3. Update any assumptions that have been validated (mark them confirmed) or invalidated (explain the impact)
4. Add any new intentional constraints introduced by recent design decisions
5. Update the "Last updated" date and version number

Keep the same structure and direct tone. Flag anything that looks like it should be escalated or addressed before the next release.
```
