# Contributing to Vibe to Value Community

Thank you for helping improve these templates and skills. Contributions from real-world vibe coding projects make the whole system better.

## What you can contribute

- **Improve existing templates.** Clarify language, add missing fields, improve generation or maintenance prompts.
- **Add new templates.** If your project needs a document type that isn't covered, propose it.
- **Share samples.** Filled-in examples help others understand what good output looks like.
- **Fix errors.** Typos, broken formatting, unclear instructions — all welcome.
- **Improve skills.** Suggest refinements to the Cost-Effective Developer or Watchdog Auditor skills based on your experience using them.

## How to contribute

1. **Fork this repository.**
2. **Create a branch** for your change (`git checkout -b improve-runbook-template`).
3. **Make your changes.** Follow the existing file structure and formatting conventions.
4. **Test your changes.** If you modified a template, try using it with an AI tool to confirm the generation and maintenance prompts still work well.
5. **Submit a pull request.** Describe what you changed and why.

## Template structure

Every template file should contain three sections separated by horizontal rules (`---`):

1. **The template** — with `[PLACEHOLDER]` brackets for fields the user fills in.
2. **Generation prompt** — under `## Prompt: How to generate this document`. A complete, copy-pasteable prompt.
3. **Maintenance prompt** — under `## Prompt: How to update this document`. A complete, copy-pasteable prompt.

## Naming conventions

- Use lowercase with hyphens: `incident-postmortem.md`, not `Incident_Postmortem.md`.
- Place ops templates in `templates/ops/`.
- Place documentation templates in `templates/docs/`.
- Place extended templates in `templates/extras/`.
- Place filled-in examples in `samples/`.

## Code of conduct

Be constructive, specific, and respectful. These tools are designed for practitioners — contributions should reflect real-world use, not theoretical ideals.

## Questions

Open an issue if you are unsure whether a contribution fits. We would rather discuss it first than have you do unnecessary work.
