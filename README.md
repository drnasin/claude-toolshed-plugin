# claude-toolshed-plugin

A Claude Code workflow plugin for adversarial Codex review and evidence-based GitHub issue implementation.

## Components

| Component | Type | Purpose |
| --- | --- | --- |
| `codex-debate-partner` | Agent | Uses Codex CLI as an independent adversarial critic for non-trivial plans and diffs |
| `github-issue-implementor` | Agent | Runs a risk-based issue → implementation → verification → PR workflow |
| `/debate` | Command | Starts the adversarial review agent |
| `/ghissue` | Command | Starts the GitHub issue implementor |

## GitHub Issue Workflow

`github-issue-implementor` derives the shortest safe workflow from the issue, repository evidence, and risk:

- **Low risk:** focused implementation and verification; Codex review is optional unless requested.
- **Medium risk:** Codex review when business rules are ambiguous or the change crosses systems.
- **High risk:** issue-specific invariants and Codex plan review before code, then Codex diff review before PR.

When a repository contains `.ai/`, the agent uses confirmed project-local guidance for orientation, conventions, invariants, known pitfalls, regressions, smoke tests, browser flows, review packets, and release checks. Placeholders and example commands are not treated as repository facts.

The workflow may create a branch, commit, push, and open a PR when implementation is requested. It does not merge merely because `/ghissue` was invoked. Merge requires an explicit request or later user approval, green required CI, and successful required verification.

## Codex Debate Workflow

`codex-debate-partner` keeps Claude as the primary engineer and uses Codex CLI as a read-only critic. It defaults to one focused critique round and permits additional rounds only for material unresolved disagreements.

The Claude model is inherited from the current session. Codex reasoning effort is selected by risk:

- `medium` for scoped medium-risk reviews;
- `high` for non-trivial reviews without security or data impact;
- `xhigh` for auth, payments, migrations, destructive operations, public APIs, and architecture.

Repository evidence, tests, runtime behavior, and official documentation outrank both models.

## Commands

```text
/debate <topic, plan, or diff review request>
/ghissue <issue number, URL, or instruction>
```

Examples:

```text
/debate review this migration plan before implementation
/ghissue 42
/ghissue 23 — investigate only
/ghissue 57 — implement and merge after required checks pass
```

## Prerequisites

- Claude Code CLI
- Codex CLI in `PATH` for adversarial review
- GitHub CLI (`gh`) for issue, PR, and CI operations
- Repository-specific test, analysis, build, and browser tools when required by the change

## Installation

Add the marketplace once:

```bash
claude plugin marketplace add https://github.com/drnasin/claude-toolshed-plugin
```

Install the plugin:

```bash
claude plugin install claude-toolshed-plugin@claude-toolshed-plugin
```

After a published update:

```bash
claude plugin update claude-toolshed-plugin@claude-toolshed-plugin
```

Restart Claude Code after installing or updating so the new agent and command definitions are loaded.

## Repository Layout

```text
.
├── .claude-plugin/
│   └── marketplace.json
├── README.md
└── claude-toolshed-plugin/
    ├── .claude-plugin/
    │   └── plugin.json
    ├── agents/
    │   ├── codex-debate-partner.md
    │   └── github-issue-implementor.md
    └── commands/
        ├── debate.md
        └── ghissue.md
```

## Local Validation

From the repository root:

```bash
claude plugin validate ./claude-toolshed-plugin
claude plugin details claude-toolshed-plugin@claude-toolshed-plugin
```

The first command validates the local source. The second inspects the currently available marketplace version, which may differ until local changes are committed, pushed, and published.

## License

MIT, as declared in the plugin manifest.
