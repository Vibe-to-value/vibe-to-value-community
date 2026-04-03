# Session Start Checklist

Complete every item before writing a line of code or sending the first prompt.

---

## Checklist

- [ ] **Single objective defined.** I can state in one sentence what this session will accomplish.
- [ ] **Mode declared.** This session is: `Explore` (learn/prototype, output is findings) or `Ship` (deliver working output, output is committed code/docs).
- [ ] **Budget / time constraint set.** Hard stop at: [time or token limit].
- [ ] **Relevant context loaded.** I have reviewed:
  - [ ] Project spec or project summary
  - [ ] Decision log (recent entries)
  - [ ] Last session manifest (or "no prior session")
- [ ] **Backlog reviewed.** The objective I'm working on is a **Now** item. If it isn't, I've confirmed why I'm pulling it forward.
- [ ] **Evidence repo path confirmed.** I know where validation results, logs, and screenshots will be saved: `[path]`

---

## Session Header (fill in before starting)

**Date:** [YYYY-MM-DD]
**Objective:** [One sentence]
**Mode:** [Explore | Ship]
**Hard stop:** [Time or limit]
**Context loaded:** [List what you reviewed]
**Backlog item reference:** [Now item ID or title]
**Evidence path:** [path/to/evidence/]

---

## Prompt: How to generate this document

```
I'm about to start a build session and need a session start checklist.

Project: [project name]
Today's date: [YYYY-MM-DD]
Objective: [what I want to accomplish]
Mode: [Explore or Ship]
Hard stop: [time or token limit]
Context I plan to review: [spec, decision log, last session manifest, etc.]
Backlog item I'm working on: [item title or ID]
Evidence path: [where I'll save outputs]

Generate a filled-in session start checklist using the standard template. Pre-fill the Session Header with the information above. Mark all checklist boxes as unchecked — I'll check them off as I go.
```

---

## Prompt: How to update this document

```
Here is my session start checklist template:

[paste checklist]

I want to update the standard checklist items themselves (not the filled-in header). Here are the changes I need:

[describe what to add, remove, or reword]

Revise only the checklist items. Keep the Session Header section and the formatting unchanged.
```
