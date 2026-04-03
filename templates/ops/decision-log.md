# Decision Log

A chronological record of significant decisions. Log a decision when it meaningfully affects direction, architecture, or constraints — not for every small implementation choice.

**Rule of thumb:** If you'd have to explain *why* you did it this way to a new collaborator, log it here.

---

## [001] [Decision Title]

**Date:** [YYYY-MM-DD]

**Context:**
[What situation, constraint, or question prompted this decision? 2–4 sentences.]

**Options Considered:**

| Option | Summary |
|--------|---------|
| [Option A] | [Brief description] |
| [Option B] | [Brief description] |
| [Option C] | [Brief description — add or remove rows as needed] |

**Decision Made:**
[Option X — state the choice clearly in one sentence.]

**Reasoning:**
[Why this option over the others? What evidence or logic drove the choice?]

**Trade-offs Accepted:**
[What are you giving up or taking on by making this choice? What becomes harder or riskier?]

---

## [002] [Decision Title]

**Date:** [YYYY-MM-DD]

**Context:**
[Context]

**Options Considered:**

| Option | Summary |
|--------|---------|
| [Option A] | [Description] |
| [Option B] | [Description] |

**Decision Made:**
[Choice]

**Reasoning:**
[Reasoning]

**Trade-offs Accepted:**
[Trade-offs]

---

<!-- Add new entries above this line in reverse chronological order (newest first). Copy the entry block and increment the number. -->

---

## Prompt: How to generate this document

```
I need to start a decision log for my project.

Project: [project name]

Here are the first decisions I want to record:

Decision 1:
- Title: [title]
- Date: [YYYY-MM-DD]
- Context: [what prompted this decision]
- Options considered: [list the options and a brief description of each]
- Decision made: [what was chosen]
- Reasoning: [why]
- Trade-offs accepted: [what you're giving up]

[Repeat for additional decisions]

Generate a complete decision log with all entries formatted using the standard template. Number entries starting from 001. Order them newest first.
```

---

## Prompt: How to update this document

```
Here is my current decision log:

[paste log]

I need to add the following new decision:

- Title: [title]
- Date: [YYYY-MM-DD]
- Context: [what prompted this decision]
- Options considered: [list]
- Decision made: [choice]
- Reasoning: [why]
- Trade-offs accepted: [what you're giving up]

Add this as a new entry at the top of the log (newest first). Assign it the next sequential number. Do not modify existing entries.
```
