# Product Spec: [PROJECT NAME]

**Version:** [1.0]
**Last updated:** [YYYY-MM-DD]
**Owner:** [NAME / TEAM]
**Status:** [Draft | Review | Approved | Superseded]

---

## Overview

### What it does

[One to three sentences describing the product or feature. Focus on what it does, not how it's built.]

### Who it's for

**Primary users:** [Describe the primary user persona — their role, context, and goal.]

**Secondary users:** [Describe any secondary personas, or write "None".]

**Out-of-scope users:** [Who is explicitly not the target audience.]

---

## Features

> Repeat this block for each feature or capability.

### Feature: [FEATURE NAME]

**Description:** [What this feature does and why it exists.]

**Behavior:**

- [Describe behavior in plain language — what happens when the user does X.]
- [Include edge cases, error states, and empty states.]
- [Note any conditional logic — e.g., "If user is not authenticated, redirect to login."]

**Acceptance criteria:**

- [ ] [Specific, testable criterion — e.g., "User can submit the form with valid input and receives a confirmation message."]
- [ ] [Another criterion.]
- [ ] [Another criterion.]

---

### Feature: [FEATURE NAME]

**Description:** [What this feature does and why it exists.]

**Behavior:**

- [Describe behavior.]

**Acceptance criteria:**

- [ ] [Criterion.]

---

## Data Model

### Entities

#### [ENTITY NAME] (e.g., User)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| [id] | [UUID] | Yes | [Primary key] |
| [created_at] | [timestamp] | Yes | [Creation timestamp] |
| [field_name] | [type] | [Yes/No] | [Description] |

#### [ENTITY NAME] (e.g., Order)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| [id] | [UUID] | Yes | [Primary key] |
| [user_id] | [UUID FK] | Yes | [References User.id] |
| [field_name] | [type] | [Yes/No] | [Description] |

### Relationships

- [User] has many [Orders]
- [Order] belongs to [User]
- [Add additional relationships as needed]

### Storage

**Database:** [e.g., PostgreSQL on Railway / Supabase / PlanetScale]

**File storage:** [e.g., Cloudflare R2 / S3 / None]

**Caching:** [e.g., Redis / none]

---

## User Flows

### Flow 1: [PRIMARY FLOW NAME — e.g., User signs up and completes onboarding]

1. User arrives at [starting point].
2. User [takes action].
3. System [responds with what].
4. User [next step].
5. Flow ends at [end state].

**Error path:** [What happens if step N fails — e.g., "If email is already in use, user sees inline error and is offered a login link."]

---

### Flow 2: [FLOW NAME]

1. [Step.]
2. [Step.]
3. [Step.]

---

## Out of Scope

The following are explicitly **not** part of this spec. They may be addressed in a future version.

- [Feature or capability that is excluded, and briefly why.]
- [Another exclusion.]
- [Another exclusion.]

---

---

## Prompt: How to generate this document

```
I need to write a product spec for [PROJECT NAME].

Here is what I know about the project:
- What it does: [brief description]
- Who it's for: [target users]
- Key features: [list the main features]
- Tech stack: [languages, frameworks, database]

Please generate a complete product spec using this structure:
1. Overview — what it does, who it's for, who it's NOT for
2. Features — for each feature: description, behavior (including edge cases and error states), and acceptance criteria as a checklist
3. Data model — entities with field tables, relationships, and storage choices
4. User flows — numbered step-by-step walkthroughs of primary journeys, including error paths
5. Out of scope — what is explicitly excluded from this version

Use practical, concise language. Write acceptance criteria as testable statements. Be specific rather than generic.
```

---

## Prompt: How to update this document

```
I have an existing product spec for [PROJECT NAME] that needs to be reviewed and updated.

Here is the current spec:
[PASTE SPEC HERE]

Here is what has changed since the last update:
[Describe what changed — new features added, features removed, behavior changes, data model changes, etc.]

Please:
1. Update any sections affected by the changes described above
2. Flag any acceptance criteria that are now incomplete or contradicted by the changes
3. Move anything no longer being built to the Out of Scope section
4. Note the version number and update the "Last updated" date to today
5. Summarize the changes you made at the top as a brief changelog entry

Keep the same document structure and tone. Do not rewrite sections that haven't changed.
```
