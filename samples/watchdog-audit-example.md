# Repository Watchdog Audit

- **Repositories reviewed:** field-notes-app (main branch)
- **Period covered:** 2026-03-24 to 2026-03-31
- **Evidence sources inspected:** ops/sessions/, ops/prompts/, ops/context/project-summary.md, ops/context/decision-log.md, ops/backlog.md, docs/deployment-guide.md, docs/spec.md, git log (past 14 days)

## Strengths

- Five of seven sessions in the review period have complete session manifests. Each of the five documented sessions has a single declared objective and a Mode field set to either Explore or Ship — the core session discipline habit is forming.
- The project summary in `ops/context/project-summary.md` is current (last updated 2026-03-29) and accurately reflects the current tech stack, key conventions, and the active phase of development.
- The deployment guide was updated after the infrastructure migration on March 27 (Railway → Render). The migration checklist in the guide is complete and includes the environment variable changes made during that session.
- The backlog shows active use: six items have moved from Now to Done in the review period, and three new items were added mid-period with clear descriptions and priority labels.

## Weaknesses

- Two sessions (March 25 and March 28) have no session manifests. Based on the commit history for those dates, meaningful work was done — the March 25 commits show a new authentication flow added, and the March 28 commits show database schema changes. There is no record of objectives, decisions, or validation for either session.
- The decision log has only four entries despite the commit history revealing at least seven architecture-level choices during the review period. Notable undocumented decisions include: the choice to use Drizzle ORM over Prisma (visible in the March 24 commits), the decision to move from cookie-based auth to JWTs (March 25 commits), and the removal of the file upload feature from scope (referenced in a March 29 commit message but absent from the decision log and backlog).
- Validation records are present for three of seven sessions. The four sessions without validation records include the two undocumented sessions (expected) and two documented Ship sessions where validation sections were left as placeholders.

## Missing Evidence

- `ops/prompts/` is empty. The session manifest from March 26 explicitly notes a prompt was handed to Lovable for UI work, and the March 30 manifest notes a Cursor handoff for a refactor. Neither prompt was saved. There is no evidence of prompt reuse anywhere in the repository.
- `docs/troubleshooting-faq.md` does not exist. Three sessions in the period reference debugging the same Drizzle query issue in different forms — once in an Explore session, once in a Ship session, and once in the March 28 undocumented session's commit message ("fix drizzle relation query again"). This is a textbook case for a FAQ entry.
- `docs/limitations-and-assumptions.md` does not exist. The spec references several external dependencies (a third-party mapping API, a PDF generation service) without documenting any assumptions about their reliability or behavior.

## Risk Signals

- The March 28 session appears to have combined at least three objectives based on commit messages: "add user profile schema," "fix auth token refresh," and "update dashboard query." No manifest exists to confirm whether this was intentional or represents uncontrolled scope drift. This is the most significant process risk identified in the review.
- The project summary references a "v2 public launch" milestone with a target of April 15, but the backlog contains no items tagged to this milestone and no v2-specific work is visible in the session manifests. There is a planning-execution gap that will likely surface as missed-deadline risk.
- The `docs/deployment-guide.md` documents the Render deployment correctly, but the section on rollback procedure was not updated after the infrastructure migration. It still references the Railway CLI for rollback — a process that no longer applies to the current hosting environment.

## Recommended Corrections

1. Write retroactive session manifests for March 25 and March 28 based on commit history and any available notes. Focus especially on the March 28 session — document whether the compound work was intentional and what validation (if any) was performed on the auth token fix.
2. Add decision log entries for the Drizzle ORM choice, the JWT auth decision, and the removal of file upload from scope. Each entry needs only a one-sentence decision and a one-sentence rationale. Three entries; estimated time: 15 minutes.
3. Create `docs/troubleshooting-faq.md` and add an entry for the recurring Drizzle relation query issue. Include reproduction steps and the fix. This alone will save time in future sessions.
4. Save the Lovable UI prompt and the Cursor refactor prompt to `ops/prompts/` retroactively. Going forward, treat prompt saving as a mandatory step before any cross-tool handoff.
5. Update the rollback procedure in `docs/deployment-guide.md` to reflect the Render CLI process, and remove or archive the Railway CLI instructions.
6. Review the v2 launch milestone: either add v2 items to the backlog with realistic sizing, or update the project summary to reflect a revised timeline. The current gap between the stated target and the visible backlog is a planning risk.

## Process Maturity Rating

**Developing** — The foundation is in place: session manifests are being written for most sessions, the backlog is actively used, and the project summary is maintained. The gaps — two undocumented sessions, an empty prompts folder, a sparse decision log, and missing troubleshooting documentation — indicate that the habits are not yet consistent across all session types. The multi-objective March 28 session and the outdated rollback procedure are the most urgent issues to address. With three to four weeks of consistent practice on the corrections above, this project would qualify as Strong.
