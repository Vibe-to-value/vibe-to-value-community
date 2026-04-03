# Incident Postmortem: [INCIDENT TITLE]

**Postmortem author:** [NAME]
**Postmortem date:** [YYYY-MM-DD]
**Reviewed by:** [NAME(S)]

---

## Incident Details

| Field | Value |
|-------|-------|
| **Incident date** | [YYYY-MM-DD] |
| **Severity** | [SEV1 — Total outage / SEV2 — Major degradation / SEV3 — Minor impact] |
| **Duration** | [X hours Y minutes] |
| **Detected at** | [HH:MM TZ] |
| **Resolved at** | [HH:MM TZ] |
| **Services affected** | [e.g., Web app, API, Payment processing] |
| **Incident commander** | [NAME] |

---

## Summary

[Two to four sentences describing what happened, the impact on users, and how it was resolved. Written for a non-technical audience. E.g., "On [date], users were unable to log in to [Project Name] for approximately [X] hours due to a misconfigured environment variable deployed in version [X]. Approximately [N] users were affected. The issue was resolved by reverting the deployment and correcting the configuration."]

---

## Timeline of Events

All times in [UTC / ET / your timezone].

| Time | Event |
|------|-------|
| [HH:MM] | [What happened — e.g., Deploy of v1.4.2 completed] |
| [HH:MM] | [First alert or user report received] |
| [HH:MM] | [On-call engineer paged / began investigating] |
| [HH:MM] | [Initial hypothesis formed: [what was suspected]] |
| [HH:MM] | [Action taken: [e.g., rolled back deploy, restarted service]] |
| [HH:MM] | [Incident resolved — service restored to normal] |
| [HH:MM] | [Post-resolution monitoring confirmed no recurrence] |

---

## Root Cause

**Root cause:** [One or two sentences identifying the specific technical cause. Be precise. E.g., "The `DATABASE_URL` environment variable was overwritten with an invalid value during a manual configuration update, causing all database queries to fail at connection time."]

**Contributing factors:**

- [Factor 1 — e.g., No staging environment to catch the configuration change before production]
- [Factor 2 — e.g., The deploy pipeline did not include a health check that would have failed immediately]
- [Factor 3 — if applicable]

**Why was detection delayed?**
[Explain if the incident was not detected immediately — e.g., "Monitoring only checked HTTP status codes, not database connectivity. Users reported the issue 12 minutes before the automated alert fired."]

---

## Impact

| Metric | Value |
|--------|-------|
| Users affected | [Estimated number or percentage] |
| Requests failed | [Estimated number] |
| Revenue impact | [Estimated $ or "Unknown"] |
| Data lost or corrupted | [Yes — describe / No] |
| SLA breached | [Yes / No] |

**User impact description:** [Plain language description of what users experienced — e.g., "Users who attempted to log in received a generic error page. Users who were already logged in could not load any data. No data was lost."]

---

## What Went Well

- [Something the team did well — e.g., "The on-call engineer identified and resolved the issue within 20 minutes of the alert."]
- [Another positive — e.g., "Communication to affected users was sent within 30 minutes via the status page."]
- [Another positive — if applicable]

---

## What Went Wrong

- [Something that didn't work — e.g., "The deploy pipeline had no health check, so the bad deploy reached production silently."]
- [Another issue — e.g., "The runbook for database connectivity issues was missing from the operations docs."]
- [Another issue — if applicable]

---

## Action Items

| # | Action | Owner | Due date | Status |
|---|--------|-------|----------|--------|
| 1 | [Add database connectivity health check to deploy pipeline] | [NAME] | [YYYY-MM-DD] | [Open] |
| 2 | [Add runbook entry for this incident type] | [NAME] | [YYYY-MM-DD] | [Open] |
| 3 | [Set up alerting rule for database connection errors] | [NAME] | [YYYY-MM-DD] | [Open] |
| 4 | [Action item] | [NAME] | [YYYY-MM-DD] | [Open] |

---

## Lessons Learned

- [Key takeaway — e.g., "Manual configuration changes to production should require a second pair of eyes."]
- [Another lesson — e.g., "Our monitoring was too coarse — we need application-level health checks, not just HTTP status codes."]
- [Another lesson — if applicable]

---

---

## Prompt: How to generate this document

```
I need to write an incident postmortem immediately after an incident. Here is what I know:

Incident details:
- What happened: [describe the incident]
- When it started and ended: [times]
- What was affected: [services, users]
- Root cause (if known): [describe]
- How it was resolved: [steps taken]
- Timeline of events: [paste any notes, Slack messages, or log entries in chronological order]

Please write a complete incident postmortem with these sections:
1. Incident details table — date, severity, duration, detected/resolved times, affected services, incident commander
2. Summary — 2-4 sentences for a non-technical audience
3. Timeline — chronological table of key events with times
4. Root cause — precise technical cause plus contributing factors and why detection was delayed
5. Impact — table with users affected, requests failed, revenue impact, data loss, SLA breach + plain language description
6. What went well — honest positives from the response
7. What went wrong — honest gaps in process, tooling, or communication
8. Action items — table with action, owner, due date, status (all "Open" initially)
9. Lessons learned — 2-4 durable insights for the team

Be specific and honest. The purpose is to improve, not to assign blame. Avoid vague statements like "better monitoring" — each action item should be a concrete, assignable task.
```

---

## Prompt: How to update this document

```
I have an existing incident postmortem that I need to review for action item completion.

Here is the postmortem:
[PASTE POSTMORTEM HERE]

Here is what has been completed or changed since the postmortem was written:
[Describe which action items were completed, which were deprioritized, and any new information about the root cause or impact that has emerged.]

Please:
1. Update the Status column in the action items table — mark each as "Complete", "In Progress", "Deprioritized", or "Open"
2. Add a "Follow-up notes" field for any action items that were closed differently than planned
3. If any action items were never completed and the risk still exists, flag them clearly
4. If new information about the root cause emerged after the postmortem, add a "Post-postmortem update" note at the top of the Root Cause section
5. Do not change the original timeline, summary, or lessons learned — this is a historical record

The goal is to close the loop on this incident, not to rewrite history.
```
