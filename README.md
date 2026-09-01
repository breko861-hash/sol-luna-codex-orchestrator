# Sol + Luna Codex Orchestrator

A lightweight Codex multi-agent setup for getting strong results without using the most expensive model for every step.

The current recommended routing is:

- **Terra** - default parent for planning, decomposition, orchestration, review and integration
- **Spark** - optional ultra-fast micro-worker for tiny, deterministic edits when available
- **Luna** - default implementation worker
- **Terra High** - difficult bounded implementation, investigation and debugging
- **Sol** - escalation for architecture, security-sensitive work and genuinely difficult global reasoning

> **Use the cheapest capable worker. Keep global judgement with the parent. Escalate only when the task requires it.**

The original Sol + Luna setup is preserved on the [`v1-sol-luna`](https://github.com/breko861-hash/sol-luna-codex-orchestrator/tree/v1-sol-luna) branch.

## Why this routing?

Most implementation work does not need the strongest model available.

The parent should keep the broad repository and product context, resolve ambiguity, then hand workers small, self-contained packages with explicit scope and validation.

That gives each model a specific job instead of simply using one model for everything.

```text
Terra Medium
    |
    |-- tiny + deterministic + Spark available --> Spark
    |
    |-- normal bounded implementation -----------> Luna High
    |
    |-- difficult bounded reasoning -------------> Terra High
    |
    `-- architecture / security / hard escalation -> Sol High
```

Spark is optional. The workflow remains fully usable without it.

## Spark availability

GPT-5.3-Codex-Spark is currently an optional research-preview model and may not be available on every ChatGPT plan, account or Codex runtime.

The included instructions therefore treat Spark as an optimisation rather than a dependency.

If Spark is unavailable, unsupported, rate-limited, or fails to launch because the current account lacks access, the same work package should immediately fall back to Luna.

This is useful if the same Codex configuration is shared between accounts with different model access.

The included Spark profile intentionally does not pin a reasoning-effort value so it is less likely to break as preview support evolves. Set a supported effort locally if you want to tune it.

## Quick start

Copy the contents of `templates/` into the root of your Codex project.

```text
your-repo/
├── AGENTS.md
├── .agents/
│   └── skills/
│       └── delegate-work/
│           └── SKILL.md
└── .codex/
    └── agents/
        ├── spark-worker.toml
        ├── luna-worker.toml
        ├── terra-worker.toml
        └── sol-escalation.toml
```

If your project already has an `AGENTS.md`, merge the orchestration section rather than replacing project-specific instructions.

## Recommended routing

| Role | Model / effort |
|---|---|
| Default parent / orchestrator | Terra Medium |
| Optional tiny deterministic worker | Spark, when available |
| Default implementation worker | Luna High |
| Difficult bounded worker | Terra High |
| Architecture / security / hard escalation | Sol High |
| Exceptional cases | Stronger Sol reasoning only when justified |

Do not automatically use the strongest reasoning level.

## Worker selection

### Spark

Use Spark for tasks that are tiny, localised, unambiguous and cheap to verify, for example:

- targeted edits
- straightforward single-function changes
- simple tests
- mechanical refactors
- renames and repetitive edits
- documentation changes
- small UI or styling changes with explicit textual requirements

Do not use Spark for broad exploration, unknown-cause debugging, architecture, auth/security work, complex state, concurrency or tasks that depend on image understanding.

Always give Spark explicit validation.

### Luna

Luna remains the normal implementation worker.

Use it when implementation requires real coding judgement but the parent has already resolved architecture, scope and contracts.

### Terra

Use Terra High when a bounded task needs more independent reasoning, such as difficult debugging, subtle state behaviour, migrations, investigation or cross-cutting implementation.

### Sol

Use Sol as an escalation model when the problem requires stronger global judgement, especially architecture, security-sensitive design, consequential cross-system changes or repeated failure after a well-specified Terra attempt.

## Workflow

```text
parent
  |
  v
understand request + relevant code
  |
  v
resolve architecture / product ambiguity
  |
  v
split implementation into small work packages
  |
  v
select cheapest appropriate available worker
  |
  v
implementation + deterministic validation
  |
  v
parent reviews diff + evidence
  |
  v
correction if needed
  |
  v
parent integrates and accepts
```

Workers validate their own implementation, but they do not self-accept the overall result.

## Delegation package

Every worker package uses the same contract:

- `ROUTE`
- `GOAL`
- `CONTEXT`
- `SCOPE`
- `DO NOT TOUCH`
- `CONTRACT`
- `DONE WHEN`
- `VALIDATION`
- `RETURN`

The detailed format lives in:

`templates/.agents/skills/delegate-work/SKILL.md`

## Normal prompts after setup

Once installed, prompts can stay focused on the actual requirement:

```text
Add passwordless login using magic links.

Keep the existing auth architecture and email provider.
Do not change the existing password login flow.
Done when the flow works end-to-end and the relevant tests pass.
```

You should not need to repeatedly append orchestration instructions.

## Included files

- `templates/AGENTS.md`
- `templates/.agents/skills/delegate-work/SKILL.md`
- `templates/.codex/agents/spark-worker.toml`
- `templates/.codex/agents/luna-worker.toml`
- `templates/.codex/agents/terra-worker.toml`
- `templates/.codex/agents/sol-escalation.toml`
- `examples/example-prompt.md`

## Philosophy

1. Resolve ambiguity before delegation.
2. Give workers one clear outcome at a time.
3. Keep worker context minimal and sufficient.
4. Use the cheapest model that can reliably perform the package.
5. Treat optional models as optimisations, not dependencies.
6. Require deterministic validation where possible.
7. Let workers validate their own implementation, but not self-accept it.
8. Fix poor decomposition before escalating model strength.
9. Do not repeatedly retry a worker that has already failed a well-specified package.
10. Parallelise only genuinely independent work.
