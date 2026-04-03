# Session Manifest

**Date:** [YYYY-MM-DD]
**Objective:** [One sentence — what this session set out to accomplish]
**Mode:** [Explore | Ship]
**Constraints:** [Time box, token budget, "no new dependencies", etc.]

## Tools Used

- [Tool name — e.g. Cursor, Claude, GitHub Copilot, terminal]
- [Tool name]

## Repos / Files in Scope

- [repo-name / path/to/file.ext]
- [repo-name / path/to/file.ext]

## Plan Summary

[2–5 sentences describing the approach taken at the start of the session.]

---

## What Changed

[Narrative summary of what was built, fixed, or discovered. Keep it scannable — bullet points are fine.]

- [Change 1]
- [Change 2]

## Files Touched

| File | Action |
|------|--------|
| [path/to/file.ext] | [Created / Modified / Deleted] |
| [path/to/file.ext] | [Created / Modified / Deleted] |

## Prompts Handed Off

[List any prompts saved to the prompt handoff log, or "None".]

- [Brief description of prompt and target tool]

## Decisions Captured

[Decisions logged to the decision log during this session, or "None".]

- [Decision title — see decision log for full record]

## Docs Updated

[Spec, README, project summary, or other docs updated, or "None".]

- [Doc name and what changed]

## Validation Performed

[Tests run, manual checks completed, or "None — not applicable this session".]

- [Validation step and result]

## Open Issues

[Things left unresolved that a future session must address.]

- [ ] [Issue description]

## Recommended Next Session

**Suggested objective:** [One sentence]
**Mode:** [Explore | Ship]
**Start with:** [First action the next session should take]

---

## Prompt: How to generate this document

```
I'm starting a build session and need to create a session manifest.

Project: [project name]
Session date: [YYYY-MM-DD]
Objective: [what I'm trying to accomplish today]
Mode: [Explore or Ship]
Constraints: [time, budget, or other limits]
Tools I plan to use: [list]
Files/repos in scope: [list]

Write a session manifest using the standard template. Fill in everything you can from the information above. Leave placeholders for fields I'll complete at the end of the session (What Changed, Files Touched, Decisions Captured, Validation Performed, Open Issues, Recommended Next Session).
```

---

## Prompt: How to update this document

```
I've just finished a build session. Here is my session manifest with some fields incomplete:

[paste manifest]

Here is what actually happened during the session:
- What changed: [description]
- Files touched: [list]
- Prompts handed off: [list or "none"]
- Decisions made: [list or "none"]
- Docs updated: [list or "none"]
- Validation performed: [list or "none"]
- Open issues remaining: [list or "none"]
- My recommendation for the next session: [description]

Update the manifest by filling in all incomplete fields. Keep the tone factual and concise. Do not add new sections.
```
