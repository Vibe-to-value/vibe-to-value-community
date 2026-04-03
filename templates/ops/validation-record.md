# Validation Record

Evidence of testing and verification for a session. One record per session. Save this alongside the session manifest.

---

**Date:** [YYYY-MM-DD]
**Session objective:** [What this session set out to accomplish — copy from session manifest]
**Overall status:** [PASS | PARTIAL | FAIL | NOT APPLICABLE]

---

## Tests Run

| Test | Description | Expected result | Actual result | Status |
|------|-------------|-----------------|---------------|--------|
| [Test name] | [What was tested] | [What should happen] | [What actually happened] | [PASS / FAIL / SKIP] |
| [Test name] | [Description] | [Expected] | [Actual] | [PASS / FAIL / SKIP] |

## Commands Executed

```
[Paste the exact commands run, with their output. Include terminal sessions, test runners, linters, build commands, etc.]

$ [command]
[output]

$ [command]
[output]
```

## Manual Checks

| Check | Steps taken | Result |
|-------|-------------|--------|
| [Check description] | [What you did to verify it] | [What you observed] |
| [Check description] | [Steps] | [Result] |

## Observed Results

[Narrative summary of what you saw. Useful for captures that don't fit neatly into the tables above — e.g., screenshots taken, UI behavior verified, data spot-checked.]

- [Observation]
- [Observation]

## Open Failures and Blockers

[Tests that failed and have not yet been resolved. Each item should map to an open issue in the session manifest and/or backlog.]

- [ ] **[Failure title]:** [What failed, what error or behavior was observed, and what the impact is]
- [ ] **[Blocker title]:** [What is blocking progress and what would unblock it]

---

## Prompt: How to generate this document

```
I need to create a validation record for a build session.

Project: [project name]
Date: [YYYY-MM-DD]
Session objective: [what the session was trying to accomplish]

Here is the validation evidence I collected:

Tests run:
- [Test name]: tested [what], expected [result], got [result] — [PASS/FAIL]

Commands executed:
[paste terminal output or describe commands run]

Manual checks:
- [What I checked and what I observed]

Failures or blockers:
- [Describe any failures that remain open]

Generate a complete validation record using the standard template. Fill in all sections with the evidence above. Mark the overall status as PASS, PARTIAL, FAIL, or NOT APPLICABLE based on the evidence.
```

---

## Prompt: How to update this document

```
Here is my current validation record:

[paste record]

I need to update it with the following new evidence:

New tests run:
- [Test name]: [description], expected [result], got [result] — [PASS/FAIL]

New commands executed:
[output]

New manual checks:
- [Check and result]

Resolved failures (mark these closed):
- [Failure title] — resolved by [what fixed it]

New open failures:
- [Failure title]: [description]

Update the record. Change the overall status if the new evidence changes it. Mark resolved failures as closed (check the box or note resolution). Do not remove any existing entries — add new rows or append to sections.
```
