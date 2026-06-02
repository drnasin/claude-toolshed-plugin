# claude-toolshed-plugin

A [Claude Code](https://claude.ai/code) plugin that wires two high-leverage development workflows into your IDE: an adversarial code-review partner powered by Codex CLI, and an end-to-end GitHub issue implementor. Ships with Windows toast notification hooks so you know when the agents finish without watching the terminal.

---

## What's inside

| Component | Type | Purpose |
|---|---|---|
| `codex-debate-partner` | Agent | Claude + Codex adversarial planning & review |
| `github-issue-implementor` | Agent | Full issue → branch → code → tests → CI → PR flow |
| `/debate` | Slash command | Launch the debate agent from the prompt |
| `/issue` | Slash command | Launch the issue implementor from the prompt |
| `hooks.json` | Hooks | BurntToast desktop notifications on agent events |

---

## Agents

### codex-debate-partner

**The idea:** every non-trivial plan has blind spots. This agent forces two independent perspectives — Claude forms its own position from repository evidence, then runs Codex CLI as an adversarial critic (not a collaborator). Codex is explicitly told to *attack* assumptions, not improve the plan. The agent then reconciles the disagreements against primary-source evidence.

**Best used for:**
- Architecture decisions and design trade-offs
- Migration plans (database, framework, API)
- Refactor strategy on complex modules
- Security and authorization design
- Pre-implementation plan critique

**How it works:**
1. Claude reads the repository and forms an independent position
2. Codex CLI runs with an adversarial prompt — it must find unsafe, incomplete, over-scoped, or incorrect assumptions
3. Up to 3 debate rounds; each disagreement is resolved against code/docs/tests
4. Output: a concrete recommendation with stated trade-offs and rejected alternatives

Runs on **Opus** at **xhigh** effort.

---

### github-issue-implementor

**The idea:** automate the repetitive scaffolding of issue work — reading the issue, picking a branch, writing tests, running CI, opening a PR — so you focus on the actual problem, not the ceremony.

**The flow:**
```
issue analysis → risk assessment → plan
  → feature branch → implementation → tests
  → Codex review (proportional to risk) → commit
  → PR into develop → Playwright check → green CI → merge
```

**Risk-based behaviour:**
- **Low risk** — implement, commit, PR. Skip Codex unless requested.
- **Medium risk** — implement, Codex diff review before commit.
- **High risk** — plan critique by Codex before coding, Codex diff review before commit, invariants defined up front.

**High-risk invariants enforced by default:**
- Users never access another user's private data
- Authorization is always server-side
- Existing public API response shape is never silently changed
- Destructive operations have an explicit rollback path

Playwright verification runs for browser-testable changes. GitHub Actions CI is checked after the PR opens; when Playwright and CI are green, the agent merges the PR into `develop`.

Adapts model automatically: Sonnet for low/medium risk, escalates to Opus for high risk, migrations, and architecture work.

---

## Slash commands

```
/debate <topic or plan>
/issue <issue-number>
```

Both commands simply launch the matching agent with whatever arguments you pass. You can use them with a GitHub issue number, a free-text description, or a URL.

**Examples:**
```
/debate should this service use the strategy pattern?
/debate review my migration plan before I code it
/issue 42
/issue 23 — focus on the auth part only
```

---

## Notification hooks

Three BurntToast events fire automatically:

| Event | Notification |
|---|---|
| Claude Code waiting for input | "Waiting for your input" |
| `codex-debate-partner` finishes | "Codex has finished the analysis" |
| `github-issue-implementor` finishes | "GitHub agent has finished the implementation" |

---

## Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed
- [Codex CLI](https://github.com/openai/codex) installed and in PATH
- [BurntToast](https://github.com/Windos/BurntToast) PowerShell module (for notifications)

```powershell
Install-Module -Name BurntToast -Scope CurrentUser -Force
```

---

## Installation

```bash
claude plugin install https://github.com/drnasin/claude-toolshed-plugin
```

---

## Repository layout

```
claude-toolshed-plugin/          # plugin package root
├── .claude-plugin/
│   └── plugin.json              # plugin manifest
├── agents/
│   ├── codex-debate-partner.md  # debate agent definition
│   └── github-issue-implementor.md
├── commands/
│   ├── debate.md                # /debate command
│   └── issue.md                 # /issue command
└── hooks/
    └── hooks.json               # notification hooks
```

---

## License

MIT — see [LICENSE](LICENSE) if present, otherwise use freely.

Author: [Ante Drnasin](https://github.com/drnasin)
