# Deployment Guide: [PROJECT NAME]

**Last updated:** [YYYY-MM-DD]
**Owner:** [NAME / TEAM]
**Production URL:** [https://your-app.com]
**Hosting platform:** [Railway / Vercel / Fly.io / AWS / etc.]

---

## Prerequisites

Before deploying, ensure you have the following installed and configured:

| Tool | Version | Install |
|------|---------|---------|
| [Node.js] | [v20+] | [https://nodejs.org] |
| [pnpm / npm / yarn] | [v9+] | [https://pnpm.io] |
| [Docker] | [optional] | [https://docker.com] |
| [Platform CLI] | [latest] | [e.g., `npm i -g railway`] |

**Required access:**

- [ ] Access to the [Railway / Vercel / AWS] project
- [ ] Access to the secrets manager or `.env` file for your environment
- [ ] Merge rights to the `main` branch (for production deploys)

---

## Environment Variables

See `templates/extras/environment-variable-manifest.md` for the full registry. Key variables required before deployment:

| Variable | Description | Where to set |
|----------|-------------|--------------|
| `DATABASE_URL` | Full connection string for the database | [Railway / Vercel / .env] |
| `SECRET_KEY` | Application secret for signing sessions/tokens | [Railway / Vercel / .env] |
| `[VARIABLE_NAME]` | [Description] | [Where to set] |
| `[VARIABLE_NAME]` | [Description] | [Where to set] |

> Never commit secrets to version control. Use `.env.example` to document required keys without values.

---

## Local Development

```bash
# 1. Clone the repo
git clone [REPO_URL]
cd [PROJECT_DIR]

# 2. Install dependencies
[npm install | pnpm install | yarn]

# 3. Copy environment file and fill in values
cp .env.example .env

# 4. Run database migrations (if applicable)
[npx prisma migrate dev | npm run db:migrate | etc.]

# 5. Seed the database (if applicable)
[npm run db:seed]

# 6. Start the development server
[npm run dev | pnpm dev]
```

**Dev server runs at:** `http://localhost:[PORT]`

**Hot reload:** [Yes / No — describe behavior]

---

## Build

```bash
# Install dependencies (CI/production)
[npm ci | pnpm install --frozen-lockfile]

# Run linting and type checks
[npm run lint]
[npm run typecheck]

# Run tests
[npm run test]

# Build production artifacts
[npm run build]
```

**Build output:** `[./dist | ./.next | ./build]`

**Estimated build time:** [~60 seconds]

---

## Deploy

### [Platform Name] (Primary)

```bash
# Option A: Deploy via CLI
[railway up | vercel --prod | fly deploy]

# Option B: Deploy via Git push (if CD is configured)
git push origin main
# → Triggers automatic deploy on [Railway / Vercel / etc.]
```

**Automatic deploys:** [Yes — any push to `main` triggers a deploy. | No — deploys are manual.]

**Deploy pipeline:** [Link to CI/CD config, e.g., `.github/workflows/deploy.yml`]

### Database migrations on deploy

```bash
# Run migrations against the production database
[npx prisma migrate deploy | npm run db:migrate:prod]
```

> Run migrations **before** deploying the new application version if the migration is additive. Run **after** only if the old code is compatible with the new schema.

---

## Verification Steps

After every deploy, confirm the following:

- [ ] **Health check:** `GET [https://your-app.com/health]` returns `200 OK`
- [ ] **Homepage loads** without console errors
- [ ] **Authentication works:** Can log in with a test account
- [ ] **Database connected:** [Key page or API route that requires DB] loads correctly
- [ ] **[Critical feature]:** [Manually verify the most important user action works]
- [ ] **Error tracking:** Check [Sentry / Datadog / etc.] for any spike in new errors

---

## Rollback Procedure

### Option A: Redeploy a previous version (preferred)

```bash
# Railway
railway rollback

# Vercel
vercel rollback [DEPLOYMENT_URL]

# Fly.io
fly releases
fly deploy --image [PREVIOUS_IMAGE_TAG]
```

### Option B: Revert the commit and redeploy

```bash
git revert HEAD
git push origin main
# Wait for automatic deploy, or trigger manually
```

### Rolling back database migrations

> Only roll back migrations if the migration is the confirmed cause of the incident.

```bash
# Prisma
npx prisma migrate resolve --rolled-back [MIGRATION_NAME]

# Manual rollback — connect to DB and run:
[SQL to undo the migration change]
```

**After rollback:** Notify the team, open a postmortem, and do not redeploy without reviewing the root cause.

---

---

## Prompt: How to generate this document

```
I need to write a deployment guide for [PROJECT NAME].

Here is the project context:
- Tech stack: [e.g., Next.js 14, PostgreSQL, Prisma]
- Hosting platform: [e.g., Railway for backend, Vercel for frontend]
- CI/CD: [e.g., GitHub Actions, or none — manual deploys]
- Database: [e.g., PostgreSQL with Prisma migrations]
- Key environment variables: [list the main ones]

Please generate a complete deployment guide with these sections:
1. Prerequisites — tools, versions, and access needed before deploying
2. Environment variables — table of all required vars with descriptions and where they're set
3. Local development — step-by-step commands to get the project running locally
4. Build — commands to lint, test, and build production artifacts
5. Deploy — commands for the specific hosting platform, including how migrations are handled
6. Verification steps — a checklist to confirm the deploy succeeded
7. Rollback procedure — how to revert a bad deploy, including database rollback considerations

Use real commands and accurate flags for the tools listed. Format commands as code blocks. Keep explanations brief.
```

---

## Prompt: How to update this document

```
I have an existing deployment guide for [PROJECT NAME] that needs to be reviewed and updated.

Here is the current guide:
[PASTE GUIDE HERE]

Here is what has changed:
[Describe changes — e.g., migrated from Heroku to Railway, added a new required env var, changed the build command, added a test step to CI]

Please:
1. Update all sections affected by the changes above
2. Verify that all commands match the current tech stack and platform
3. Add or remove environment variable rows to match the current set
4. Update the "Last updated" date to today
5. Flag any steps that seem outdated or that you're uncertain about, so I can verify them

Do not rewrite sections that haven't changed. Keep the same structure and tone.
```
