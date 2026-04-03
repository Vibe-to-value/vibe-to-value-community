# Environment Variable Manifest: [PROJECT NAME]

**Last updated:** [YYYY-MM-DD]
**Owner:** [NAME / TEAM]

This document is the single source of truth for all environment variables, secrets, and config used by this project. Update it whenever variables are added, removed, or changed.

> **Security note:** This file documents variable names and descriptions only. Never store actual secret values here. Secrets live in [Railway / Vercel / AWS Secrets Manager / 1Password / etc.].

---

## Development

Variables used in local development. Set these in your `.env` file (copied from `.env.example`).

| Variable | Description | Required | Default | Where set | Sensitive |
|----------|-------------|----------|---------|-----------|-----------|
| `DATABASE_URL` | Full PostgreSQL connection string | Yes | — | `.env` | Yes |
| `[SESSION_SECRET]` | Secret used to sign session tokens | Yes | — | `.env` | Yes |
| `[NEXT_PUBLIC_APP_URL]` | Public base URL of the app | Yes | `http://localhost:3000` | `.env` | No |
| `[EMAIL_API_KEY]` | API key for transactional email provider | No | — | `.env` | Yes |
| `[STRIPE_SECRET_KEY]` | Stripe secret key (use test key locally) | No | — | `.env` | Yes |
| `[STRIPE_PUBLISHABLE_KEY]` | Stripe publishable key (use test key locally) | No | — | `.env` | No |
| `[STRIPE_WEBHOOK_SECRET]` | Stripe webhook signing secret (use CLI secret locally) | No | — | `.env` | Yes |
| `[STORAGE_ACCESS_KEY]` | Access key for file storage (R2 / S3) | No | — | `.env` | Yes |
| `[STORAGE_SECRET_KEY]` | Secret key for file storage | No | — | `.env` | Yes |
| `[STORAGE_BUCKET]` | Storage bucket name | No | `dev-[PROJECT]-uploads` | `.env` | No |
| `[FEATURE_FLAG_NAME]` | Enables [feature] in development | No | `false` | `.env` | No |
| `[VARIABLE_NAME]` | [Description] | [Yes/No] | [Default or —] | `.env` | [Yes/No] |

**Notes:**
- Copy `.env.example` to `.env` before running the project locally.
- Never commit `.env` to version control.
- For Stripe webhooks locally, use `stripe listen --forward-to localhost:[PORT]/api/webhooks/stripe` and copy the printed webhook signing secret.

---

## Staging

Variables used in the staging environment. Set in [Railway / Vercel / platform dashboard].

| Variable | Description | Required | Default | Where set | Sensitive |
|----------|-------------|----------|---------|-----------|-----------|
| `DATABASE_URL` | Staging database connection string | Yes | — | [Railway environment] | Yes |
| `[SESSION_SECRET]` | Secret for signing sessions (different from prod) | Yes | — | [Railway environment] | Yes |
| `[NEXT_PUBLIC_APP_URL]` | Staging URL | Yes | `https://staging.your-app.com` | [Railway environment] | No |
| `[EMAIL_API_KEY]` | Email API key (use test/sandbox key for staging) | Yes | — | [Railway environment] | Yes |
| `[STRIPE_SECRET_KEY]` | Stripe test secret key | Yes | — | [Railway environment] | Yes |
| `[STRIPE_PUBLISHABLE_KEY]` | Stripe test publishable key | Yes | — | [Railway environment] | No |
| `[STRIPE_WEBHOOK_SECRET]` | Stripe test webhook secret | Yes | — | [Railway environment] | Yes |
| `[STORAGE_ACCESS_KEY]` | Storage access key for staging bucket | No | — | [Railway environment] | Yes |
| `[STORAGE_SECRET_KEY]` | Storage secret key | No | — | [Railway environment] | Yes |
| `[STORAGE_BUCKET]` | Staging bucket name | No | `staging-[PROJECT]-uploads` | [Railway environment] | No |
| `[VARIABLE_NAME]` | [Description] | [Yes/No] | [Default or —] | [Where set] | [Yes/No] |

**Notes:**
- Staging should use test/sandbox credentials for all third-party services (Stripe test mode, email sandbox, etc.).
- Staging database is separate from production and can be reset freely.

---

## Production

Variables used in the production environment. Set in [Railway / Vercel / platform dashboard]. Access is restricted to [authorized team members].

| Variable | Description | Required | Default | Where set | Sensitive |
|----------|-------------|----------|---------|-----------|-----------|
| `DATABASE_URL` | Production database connection string | Yes | — | [Railway environment] | Yes |
| `[SESSION_SECRET]` | Secret for signing sessions | Yes | — | [Railway environment] | Yes |
| `[NEXT_PUBLIC_APP_URL]` | Production URL | Yes | `https://your-app.com` | [Railway environment] | No |
| `[EMAIL_API_KEY]` | Email API key (live) | Yes | — | [Railway environment] | Yes |
| `[STRIPE_SECRET_KEY]` | Stripe live secret key | Yes | — | [Railway environment] | Yes |
| `[STRIPE_PUBLISHABLE_KEY]` | Stripe live publishable key | Yes | — | [Railway environment] | No |
| `[STRIPE_WEBHOOK_SECRET]` | Stripe live webhook secret | Yes | — | [Railway environment] | Yes |
| `[STORAGE_ACCESS_KEY]` | Storage access key for production bucket | Yes | — | [Railway environment] | Yes |
| `[STORAGE_SECRET_KEY]` | Storage secret key | Yes | — | [Railway environment] | Yes |
| `[STORAGE_BUCKET]` | Production bucket name | Yes | `prod-[PROJECT]-uploads` | [Railway environment] | No |
| `[SENTRY_DSN]` | Sentry data source name for error tracking | No | — | [Railway environment] | No |
| `[VARIABLE_NAME]` | [Description] | [Yes/No] | [Default or —] | [Where set] | [Yes/No] |

**Notes:**
- Production secrets should be rotated on a schedule and whenever a team member with access leaves.
- Production `DATABASE_URL` and payment keys must never be used in development or staging.
- Changes to production env vars should be logged (who changed what and when).

---

## Variable Rotation Log

Track when sensitive variables were last rotated.

| Variable | Last rotated | Rotated by | Next rotation due |
|----------|-------------|------------|------------------|
| `[SESSION_SECRET]` | [YYYY-MM-DD] | [NAME] | [YYYY-MM-DD] |
| `[STRIPE_SECRET_KEY]` | [YYYY-MM-DD] | [NAME] | [Rotate on leak only] |
| `[DATABASE_URL]` | [YYYY-MM-DD] | [NAME] | [Rotate on team change] |
| `[VARIABLE_NAME]` | [YYYY-MM-DD] | [NAME] | [YYYY-MM-DD or policy] |

---

---

## Prompt: How to generate this document

```
I need to catalog all environment variables used in my [PROJECT NAME] codebase.

Here is my project context:
- Tech stack: [e.g., Next.js 14, Prisma, PostgreSQL]
- Hosting: [e.g., Railway for backend, Vercel for frontend]
- Key integrations: [e.g., Stripe, Resend, Cloudflare R2, Sentry]
- Environments: [development, staging, production / just development and production]

Please help me create a complete environment variable manifest by:

1. Analyzing common patterns for the tech stack listed — what env vars does Next.js, Prisma, Stripe, etc. typically require?
2. Building three environment tables (development, staging, production) each with columns: Variable, Description, Required (Yes/No), Default, Where set, Sensitive (Yes/No)
3. Noting where the same variable has a different value per environment (e.g., test vs. live Stripe keys)
4. Including a variable rotation log table for sensitive secrets
5. Adding practical notes under each environment about what to watch out for

After generating the manifest, I will paste in my actual `.env.example` or a list of variables from my codebase so you can fill in or adjust the table.

[PASTE YOUR .env.example OR VARIABLE LIST HERE]
```

---

## Prompt: How to update this document

```
I have an existing environment variable manifest for [PROJECT NAME] and I need to verify it matches the current codebase.

Here is the current manifest:
[PASTE MANIFEST HERE]

Here is the current state of the codebase — paste one or more of these:
- `.env.example` contents: [PASTE]
- Output of `grep -r "process.env\." --include="*.ts" --include="*.js" -h | sort -u`: [PASTE]
- List of variables currently set in [Railway / Vercel] dashboard: [PASTE]

Please:
1. Identify any env vars present in the code but missing from the manifest — add them
2. Identify any env vars in the manifest that no longer appear in the code — flag for removal
3. Identify any variables where the description or "where set" column looks wrong based on context
4. Flag any variables marked Required=Yes that have no default and may be unset in some environments
5. Update the "Last updated" date

Do not change actual secret values — this manifest should only contain names, descriptions, and metadata.
```
