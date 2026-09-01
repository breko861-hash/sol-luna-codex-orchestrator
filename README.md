# Sol + Luna Codex Orchestrator

A lightweight Codex multi-agent setup for getting strong results without using the strongest model or highest reasoning effort for every step.

The current recommended routing is:

- **Sol Medium** - default parent for planning, architecture, decomposition, orchestration, review, integration and final acceptance
- **Spark** - optional ultra-fast micro-worker for tiny, deterministic edits when available
- **Luna High** - default implementation worker
- **Terra High** - difficult bounded implementation, investigation and debugging
- **Sol High / xHigh** - very difficult architecture, security-sensitive work and genuinely hard global reasoning

> **Sol thinks globally. Spark and Luna execute bounded work. Terra handles harder bounded problems. Stronger Sol reasoning is reserved for genuinely difficult cases.**

The original Sol + Luna setup is preserved on the [`v1-sol-luna`](https://github.com/breko861-hash/sol-luna-codex-orchestrator/tree/v1-sol-luna) branch.

## Why this routing?

The parent keeps the broad repository and product context, resolves ambiguity, then hands workers small, self-contained packages with explicit scope and validation.

Sol Medium remains the default orchestrator because decomposition, architectural judgement, integration and final review benefit from the stronger parent model, while most implementation work can be delegated more cheaply.

```text
Sol Medium
    |
    |-- tiny + deterministic + Spark available --> Spark
    |
    |-- normal bounded implementation -----------> Luna High
    |
    |-- difficult bounded reasoning -------------> Terra High
    |
    `-- very hard global / architecture / security -> Sol High / xHigh
```

Spark is optional. The workflow remains fully usable without it.

## Spark availability

GPT-5.3-Codex-Spark is an optional model and may not be available on every ChatGPT plan, account or Codex runtime.

The included instructions therefore treat Spark as an optimisation rather than a dependency.

If Spark is unavailable, unsupported, rate-limited, or fails to launch because the current account lacks access, the same work package should immediately fall back to Luna.

This is useful if the same Codex configuration is shared between accounts with different model access.

The included Spark profile intentionally does not pin a reasoning-effort value so it is less likely to break as support evolves. Set a supported effort locally if you want to tune it.

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
| Default parent / orchestrator | Sol Medium |
| Optional tiny deterministic worker | Spark, when available |
| Default implementation worker | Luna High |
| Difficult bounded worker | Terra High |
| Hard architecture / security / global reasoning | Sol High |
| Exceptional unresolved cases | Sol xHigh |

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

Use it when implementation requires real coding judgement but Sol has already resolved architecture, scope and contracts.

### Terra

Use Terra High when a bounded task needs more independent reasoning, such as difficult debugging, subtle state behaviour, migrations, investigation or cross-cutting implementation.

Terra is not the default parent. It is the stronger bounded-worker tier between Luna and a higher-effort Sol escalation.

### Sol

Sol Medium remains the orchestrator and final authority for normal work.

Raise Sol to High for difficult architecture, security-sensitive design, consequential cross-system reasoning, unresolved ambiguity or failures that remain after a well-specified Terra attempt.

Use Sol xHigh only for exceptional cases where High is still insufficient or the consequences justify the additional reasoning cost.

## Workflow

```text
Sol Medium
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
select appropriate worker
  |
  +--> Spark when tiny + deterministic + available
  |
  +--> Luna High for normal implementation
  |
  +--> Terra High for difficult bounded work
  |
  `--> Sol High/xHigh for genuinely hard global problems
  |
  v
implementation + deterministic validation
  |
  v
Sol reviews diff + evidence
  |
  v
correction if needed
  |
  v
Sol integrates and accepts
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

1. Keep Sol Medium as the default orchestrator and final authority.
2. Resolve ambiguity before delegation.
3. Give workers one clear outcome at a time.
4. Keep worker context minimal and sufficient.
5. Use Spark opportunistically for tiny deterministic work when available.
6. Use Luna for normal implementation and Terra High for harder bounded work.
7. Raise Sol reasoning only when stronger global judgement is genuinely needed.
8. Require deterministic validation where possible.
9. Let workers validate their own implementation, but not self-accept it.
10. Fix poor decomposition before escalating model strength.
11. Do not repeatedly retry a worker that has already failed a well-specified package.
12. Parallelise only genuinely independent work.
