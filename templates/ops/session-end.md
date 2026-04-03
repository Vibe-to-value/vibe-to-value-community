# Session End Checklist + Closing Summary

Complete the checklist before closing any tool. Then fill in the closing summary.

---

## End-of-Session Checklist

- [ ] **Session manifest written.** A complete manifest for this session has been saved to `ops/session-manifest.md` (or dated copy).
- [ ] **Cross-tool prompts saved.** Any prompts intended for a different tool have been recorded in the prompt handoff log.
- [ ] **Docs updated.** Spec, README, project summary, or any other living doc affected by this session has been updated.
- [ ] **Backlog updated.** Completed items moved to Done (with date). New items discovered during the session added to Later (or Now/Next if justified).
- [ ] **Decision log updated.** Any significant decision made this session has been logged with context and reasoning.
- [ ] **Validation recorded.** Test results, commands run, and manual checks have been saved to the validation record.

---

## Closing Summary

**Date:** [YYYY-MM-DD]
**Objective:** [What this session set out to accomplish]
**Mode:** [Explore | Ship]

### What Changed

[2–5 bullet points. What exists now that didn't before, or what is different/fixed.]

- [Change]
- [Change]

### Files Touched

| File | Action |
|------|--------|
| [path/to/file.ext] | [Created / Modified / Deleted] |

### Prompts Handed Off

[Prompts saved for use in another tool, or "None".]

- [Brief description + target tool]

### Decisions Captured

[Decisions logged this session, or "None".]

- [Decision title]

### Docs Updated

[Living docs updated this session, or "None".]

- [Doc name — what changed]

### Validation Performed

[What was tested or verified, or "None — not applicable".]

- [Step and result]

### Open Issues

[Unresolved items a future session must address.]

- [ ] [Issue]

### Recommended Next Session

**Suggested objective:** [One sentence]
**Mode:** [Explore | Ship]
**Start with:** [First action]

---

## Prompt: How to generate this document

```
I've just finished a build session and need to write my closing summary and check off my end-of-session checklist.

Project: [project name]
Date: [YYYY-MM-DD]
Objective: [what I was trying to accomplish]
Mode: [Explore or Ship]

Here is what happened:
- What changed: [description]
- Files touched: [list]
- Prompts handed off: [list or "none"]
- Decisions made: [list or "none"]
- Docs updated: [list or "none"]
- Validation performed: [list or "none"]
- Open issues: [list or "none"]
- Recommended next session: [description]

Generate a complete session end document. Mark all checklist items as checked (I've confirmed each one). Fill in the closing summary with the information above.
```

---

## Prompt: How to update this document

```
Here is my completed session end document:

[paste document]

I need to make the following corrections or additions:

[describe what changed — e.g., "I found one more file I touched", "add an open issue", "the decision title is wrong"]

Update only the affected fields. Keep all checked boxes checked. Do not reformat the document.
```
