# Vibe to Value — Community Templates and Skills

Open-source templates, skills, and prompts for cost-effective AI-assisted software development.

Companion assets for the https://www.reddit.com/r/vibetovalue/ community and [*Vibe to Value: Build AI Products Faster Without Wasting Money*]

## What's here

### Skills

Reusable instruction sets you load into your AI tools:

- **[Cost-Effective Developer](skills/cost-effective-developer.md)** — Load into your build session. Keeps each session focused, cost-conscious, and well-documented. Pushes evidence (session manifests, prompts, docs, validation) to your project repository.
- **[Repository Watchdog Auditor](skills/repository-watchdog-auditor.md)** — Load into a separate review session. Audits your repository for evidence that cost-effective practices are actually being followed.

### Templates

Every template includes three parts:
1. **The template itself** — copy it into your project and fill in the placeholders.
2. **A generation prompt** — paste it into your AI tool to create the document from scratch.
3. **A maintenance prompt** — paste it into your AI tool to review and update an existing version.

#### Core ops templates (`templates/ops/`)

| Template | Purpose |
|----------|---------|
| [session-manifest.md](templates/ops/session-manifest.md) | Record of a single build session |
| [session-start.md](templates/ops/session-start.md) | Pre-session checklist |
| [session-end.md](templates/ops/session-end.md) | Closing checklist + summary |
| [prompt-handoff-log.md](templates/ops/prompt-handoff-log.md) | Cross-tool prompt capture |
| [project-summary.md](templates/ops/project-summary.md) | Living project context |
| [decision-log.md](templates/ops/decision-log.md) | Choices and reasoning |
| [validation-record.md](templates/ops/validation-record.md) | Test and check evidence |
| [backlog.md](templates/ops/backlog.md) | Now / Next / Later / Done |

#### Documentation templates (`templates/docs/`)

| Template | Purpose |
|----------|---------|
| [spec.md](templates/docs/spec.md) | Living product specification |
| [deployment-guide.md](templates/docs/deployment-guide.md) | Build and deploy instructions |
| [runbook.md](templates/docs/runbook.md) | Operations and incident response |
| [troubleshooting-faq.md](templates/docs/troubleshooting-faq.md) | Common problems and fixes |
| [limitations-and-assumptions.md](templates/docs/limitations-and-assumptions.md) | Known constraints |

#### Extended templates (`templates/extras/`)

| Template | Purpose |
|----------|---------|
| [incident-postmortem.md](templates/extras/incident-postmortem.md) | Post-incident review |
| [environment-variable-manifest.md](templates/extras/environment-variable-manifest.md) | Secrets and config registry |
| [design-system.md](templates/extras/design-system.md) | Visual language documentation |
| [architecture-review.md](templates/extras/architecture-review.md) | Periodic architecture assessment |
| [reader-worksheet.md](templates/extras/reader-worksheet.md) | Self-assessment for vibe coders |
| [best-practice-warnings.md](templates/extras/best-practice-warnings.md) | Anti-patterns the watchdog should flag |

### Samples

Filled-in examples showing what good output looks like:

- [Session manifest example](samples/session-manifest-example.md)
- [Watchdog audit example](samples/watchdog-audit-example.md)

## Quick start

1. **Copy templates into your project.** Copy the `ops/` and `docs/` folders from `templates/` into the root of your project repo. Pick extras as needed.
2. **Use the generation prompts.** Open any template, scroll to "Prompt: How to generate this document," and paste that prompt into your AI tool along with context about your project. The tool will fill in the template for you.
3. **Load the development skill.** Add `skills/cost-effective-developer.md` as instructions in your build tool. It will guide your sessions and push evidence to the repository.
4. **Run the watchdog periodically.** In a separate session, load `skills/repository-watchdog-auditor.md` and point it at your repo. It will audit your process and suggest improvements.
5. **Keep templates current.** Use the maintenance prompts in each template to review and update documents as your project evolves.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose improvements, add templates, or fix issues.

## License

These templates and skills are provided under the [MIT License](LICENSE). You are free to use, modify, and distribute them. If you find them useful, consider sharing your improvements back with the community.
