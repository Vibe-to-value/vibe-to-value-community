# Best-Practice Warnings Reference

A catalog of anti-patterns the watchdog should flag. Use this as a reference during audits and watchdog sessions. Each warning includes the signal to look for, why it matters, and a suggested correction.

This is a reference list, not a template to fill in.

---

## Session Discipline

### W-S1: Scope Drift

**Signal:** A session manifest shows an objective that grew mid-session, or the "What Changed" section contains work unrelated to the stated objective. Alternatively, a commit message or diff touches files that fall outside the session's declared scope.

**Why it matters:** Scope drift is the primary driver of unexpected token costs and rework. Work done outside the intended focus is rarely tested, rarely documented, and often breaks things that were working.

**Suggested correction:** Stop the session, log the scope drift in the manifest's Open Issues section, and open a backlog item for the out-of-scope work. Start a new session with that item as its objective.

---

### W-S2: Multiple Objectives in One Session

**Signal:** A session manifest lists more than one objective in the header, or the "What Changed" section describes work on two or more independent features, bugs, or infrastructure items.

**Why it matters:** Multiple objectives make it impossible to cleanly revert, impossible to attribute cost, and difficult to document accurately. Sessions with compound objectives are the leading source of hard-to-debug regressions.

**Suggested correction:** Split future compound sessions into separate Ship sessions, each with a single objective. If the session is already complete, write separate manifest summaries for each objective worked and note the violation.

---

### W-S3: No Mode Declared

**Signal:** A session manifest exists but the Mode field is blank, says "N/A", or is missing entirely.

**Why it matters:** Mode (Explore vs. Ship) governs which tools, model tiers, and cost controls are appropriate. An undeclared mode is almost always a session that used Ship-level resources for Explore-level work, or vice versa.

**Suggested correction:** Retroactively identify the actual mode based on what was done and update the manifest. Establish a habit of declaring mode before the first prompt of every session.

---

### W-S4: Session Ended Without Summary

**Signal:** A session manifest exists with a populated header and plan but has blank or placeholder fields in What Changed, Files Touched, Validation Performed, and Recommended Next Session.

**Why it matters:** An incomplete session manifest is nearly as costly as no manifest. The next developer (or the same developer next week) cannot orient themselves, decisions are lost, and validation evidence vanishes.

**Suggested correction:** Complete the manifest immediately at session end, even if briefly. A two-sentence summary is better than blank fields. Use the session-end prompt to close any open manifests.

---

## Cost Discipline

### W-C1: Premium Model Used for Routine Task

**Signal:** A session manifest or prompt log shows a premium model (e.g., GPT-4o, Claude Opus, or equivalent) used for tasks such as reformatting text, generating boilerplate, writing simple scripts, or answering factual questions.

**Why it matters:** Premium models cost 10–20× more per token than standard or fast models for tasks where the quality difference is negligible. Habitual use of premium models for routine work compounds into significant unnecessary cost.

**Suggested correction:** Establish a model tier policy and log it in the project summary. Default to the fastest adequate model. Reserve premium models for multi-step reasoning, architecture decisions, and complex debugging.

---

### W-C2: Full Rewrite Instead of Targeted Repair

**Signal:** A session diff shows a large file replaced entirely when only a small portion had defects, or a prompt log contains a prompt asking an AI to "rewrite" or "redo" a component that had a single identified issue.

**Why it matters:** Full rewrites consume large token budgets, discard working code, introduce new bugs, and reset the documentation trail. Targeted repair is almost always cheaper and safer.

**Suggested correction:** When a bug is found, identify the minimal change needed to fix it. Prompt for the specific repair, not a full replacement. Reserve rewrites for cases where the existing code is structurally unsalvageable.

---

### W-C3: UI Polishing Before Data Validation

**Signal:** Commit history or session manifests show styling, layout, or visual polish work completed before the data layer (API responses, database reads, form submissions) has been validated end-to-end.

**Why it matters:** UI built on unvalidated data frequently requires rework when the data shape turns out to be different from what was assumed. Polishing the wrong structure doubles the cost.

**Suggested correction:** Adopt a data-first sequence: validate the data flow completely before investing in visual presentation. Log this as a project rule in the project summary.

---

### W-C4: Unnecessary Artifact Generation

**Signal:** The repository contains generated files (reports, exports, test data sets, AI-generated documentation stubs) that are not referenced in any manifest, backlog item, or active doc and show no evidence of use.

**Why it matters:** Every artifact generated by an AI tool has a cost. Artifacts that go unused represent pure waste. They also clutter the repository and make it harder to find genuinely useful files.

**Suggested correction:** Before generating any artifact, confirm it has a specific destination and user. Audit the repository quarterly and delete or archive orphaned generated files.

---

## Context Discipline

### W-X1: Repeated Context Restatement

**Signal:** Multiple session prompts (visible in the prompt handoff log or reconstructed from manifests) begin by re-explaining the project's purpose, stack, conventions, or constraints — information that should be captured in a project summary.

**Why it matters:** Restating context at the start of every session is a direct, avoidable cost. At scale, repeated context injection can consume a significant portion of a session's token budget before any real work begins.

**Suggested correction:** Create or update the project summary so it can be loaded at session start with a single prompt. Eliminate context restatement from session-opening prompts.

---

### W-X2: No Project Summary Loaded

**Signal:** Session manifests show no reference to a project summary being loaded at session start, or the project summary file does not exist, is empty, or was last updated more than 30 days ago.

**Why it matters:** Without a loaded project summary, each session starts with incomplete context. The AI tool makes assumptions, misses conventions, and produces work that conflicts with previous decisions.

**Suggested correction:** Create a project summary if one does not exist. Update it after any session that changes architecture, conventions, or priorities. Load it explicitly at the start of every session.

---

### W-X3: Decisions Left Only in Chat

**Signal:** The decision log is sparse or missing, but session manifests or commit messages reference choices (e.g., "switched to Supabase," "decided to use server actions," "removed the caching layer") that are not recorded anywhere durable.

**Why it matters:** Decisions left only in chat history are effectively lost once the session ends. Future sessions, new team members, and post-incident reviews have no record of why things are the way they are.

**Suggested correction:** After any session where a meaningful decision was made, add an entry to the decision log before closing. The entry does not need to be long — one sentence on the decision and one on the rationale is sufficient.

---

## Documentation Discipline

### W-D1: Major Change Without Doc Update

**Signal:** A commit diff shows significant changes to architecture, data model, API routes, environment variables, or authentication — but no corresponding update to the spec, deployment guide, runbook, or limitations doc.

**Why it matters:** Documentation that lags behind the code becomes actively harmful. It misleads future developers, extends debugging time, and creates false confidence during incident response.

**Suggested correction:** Add a documentation check to the session-closing checklist. Before a session is marked complete, confirm whether any changed component requires a doc update.

---

### W-D2: Deployment Without Guide Update

**Signal:** A session manifest shows a deployment to production or staging, but the deployment guide's last-modified date predates the deployment, or the deployment guide makes no mention of the infrastructure or process that was used.

**Why it matters:** An outdated deployment guide is a risk multiplier. The next deployment, performed under time pressure, will follow the outdated guide and may fail or cause an incident.

**Suggested correction:** Treat deployment guide maintenance as a mandatory post-deploy step, not an optional cleanup task. Update the guide before marking a deployment session complete.

---

### W-D3: Recurring Issue Without Runbook Entry

**Signal:** The same class of problem appears in more than one session manifest's Open Issues or What Changed sections, but there is no runbook entry, troubleshooting FAQ item, or documented resolution procedure.

**Why it matters:** Recurring issues that are solved repeatedly from scratch without documentation represent compounding waste. Each recurrence costs diagnosis time that a runbook entry would eliminate.

**Suggested correction:** After the second occurrence of any issue, add a runbook or FAQ entry. It does not need to be comprehensive — the reproduction steps and the fix are sufficient.

---

## Backlog Discipline

### W-B1: New Feature Added Mid-Session Without Backlog Entry

**Signal:** A session manifest's What Changed section or a commit message describes new functionality that does not correspond to any backlog item, and no new backlog item was created during the session.

**Why it matters:** Features added outside the backlog are invisible to anyone reviewing project progress. They inflate scope, consume unbudgeted resources, and are frequently underdocumented.

**Suggested correction:** If new work is added mid-session, pause and add a backlog item before continuing. Even a one-line entry is sufficient to maintain traceability.

---

### W-B2: Backlog Stale for More Than a Week

**Signal:** The backlog file's last-modified timestamp is more than seven days old, but session manifests show active development during that period. Alternatively, the backlog contains only items in the "Now" column with no movement into "Done."

**Why it matters:** A stale backlog no longer reflects reality. Sessions begin to drift toward whatever feels urgent rather than what has been deliberately prioritized. This is the first symptom of planning-execution divergence.

**Suggested correction:** Update the backlog at the end of every session as a closing step. Move completed items to Done, add any new items surfaced during the session, and review whether the current Now items still reflect the right priorities.

---

## Prompt Discipline

### W-P1: Same Prompt Rebuilt from Scratch

**Signal:** The prompt handoff log is empty or has few entries despite multiple sessions using similar tool configurations, or session notes describe spending time "setting up the prompt" for tasks that have been done before.

**Why it matters:** Rebuilding prompts from memory is slow, imprecise, and inconsistent. A prompt that took 20 minutes to tune should never need to be reconstructed from scratch. Prompt reuse is one of the highest-leverage cost reduction habits.

**Suggested correction:** After any session that produces a prompt worth keeping, save it to the prompt handoff log with a brief label. Before a session that involves a recurring task type, check the log first.

---

### W-P2: No Prompt Handoff Log for Cross-Tool Work

**Signal:** A session manifest shows work handed off between tools (e.g., from Perplexity Computer to Cursor, or from Cursor to Lovable), but the prompt handoff log has no entry for the handoff, and there is no evidence of what instructions were passed.

**Why it matters:** Cross-tool handoffs without documented prompts are high-risk. The receiving tool starts with incomplete context, makes assumptions, and produces work that conflicts with the originating session's decisions. Debugging cross-tool errors without a handoff log is expensive.

**Suggested correction:** Before handing off to another tool, write the handoff prompt to the prompt handoff log. The entry should include: the receiving tool, the task description, and any constraints or context the receiving tool needs.

---

---

## Prompt: How to generate this document

```
I want to create a customized version of the best-practice warnings reference for my specific project.

Here is my project context:
- Project name: [PROJECT NAME]
- Tools I use: [LIST — e.g., Perplexity Computer, Cursor, Lovable, Supabase, GitHub]
- Team size: [Solo / 2-3 people / larger team]
- Current maturity: [Early / Developing / Strong / Highly Disciplined]
- Most common process failures I've noticed: [LIST — be honest]

Please produce a customized warnings reference that:
1. Keeps all standard warning categories and items from the base reference
2. Adds a "Project-Specific Warnings" section at the end with 3-5 additional warnings tailored to my tools, team size, and known failure patterns
3. For any base warning that is especially relevant to my context, adds a note explaining how it applies specifically to my setup
4. Marks any warnings that I have already addressed with a "✓ Addressed" note and a one-line description of how

Format it as a markdown reference document I can save to my project's ops folder.
```

---

## Prompt: How to update this document

```
I want to add new warnings to my best-practice warnings reference based on recent watchdog audits and process reviews.

Here is my current warnings reference:
[PASTE REFERENCE HERE]

Here are the issues or anti-patterns my watchdog audits found in the past [time period]:
[LIST OBSERVATIONS — e.g., "Three sessions this month had empty validation sections", "Found two prompts rebuilt from scratch that existed in an older session", "Noticed we keep re-explaining the auth model at session start"]

For each observation, please:
1. Check whether it maps to an existing warning in the reference — if so, note that it's a recurrence and add a "Recurrence note" to the existing entry
2. If it represents a new anti-pattern not covered by an existing warning, draft a new warning entry with: a warning ID (use the next available number in the relevant category), a signal, a why-it-matters, and a suggested correction
3. Identify whether any warnings in the current reference should be elevated in priority based on the frequency of recurrence

Output the complete updated reference with all additions and recurrence notes incorporated.
```
