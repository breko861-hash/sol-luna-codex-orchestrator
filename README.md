# Astra + Spark/Luna Codex Orchestrator

A lightweight Codex multi-agent setup for using a strong parent model without spending it on every implementation step.

Current recommended routing:

- **GPT-6 Astra Low** - default parent for planning, architecture, decomposition, orchestration, review, integration and final acceptance
- **Spark** - optional ultra-fast micro-worker for tiny deterministic edits when available
- **Luna High** - default bounded implementation worker
- **Sol Medium** - harder bounded implementation, debugging and investigation
- **Astra Medium/High** - exceptional global architecture, security-sensitive or unresolved parent-level problems

> **Astra keeps the global context. Spark and Luna do most bounded implementation. Sol Medium handles harder bounded work. Raise Astra only when the parent problem itself is genuinely difficult.**

## Preserved older setups

- [`v1-sol-luna`](https://github.com/breko861-hash/sol-luna-codex-orchestrator/tree/v1-sol-luna) - original Sol + Luna setup
- [`v2-sol-spark-luna-terra`](https://github.com/breko861-hash/sol-luna-codex-orchestrator/tree/v2-sol-spark-luna-terra) - Sol Medium parent, Spark/Luna workers, Terra High escalation

## Why the instructions are leaner now

GPT-6 Astra needs less scaffolding than earlier coding models. The current template deliberately keeps `AGENTS.md` focused on durable repository-wide behaviour instead of turning it into a long execution recipe.

The delegation skill is also smaller. Its root file acts as a router, and the detailed work-package format lives in a supporting reference that is only needed when work is actually being delegated.

The setup also avoids ritualistic testing. Verification should be proportional to the change, while persistence is explicit so the parent keeps going through implementation, relevant checks and fixes instead of stopping after a first plausible pass.

## Routing

```text
Astra Low
    |
    |-- tiny + deterministic + Spark available --> Spark
    |
    |-- normal bounded implementation -----------> Luna High
    |
    |-- harder bounded work ----------------------> Sol Medium
    |
    `-- exceptional parent-level problem --------> Astra Medium/High
```

Spark is optional. If the current account or Codex runtime cannot use it, the same task falls back to Luna.

## Quick start

Select **GPT-6 Astra Low** as the parent in Codex, then copy the contents of `templates/` into the root of your project.

```text
your-repo/
├── AGENTS.md
├── .agents/
│   └── skills/
│       └── delegate-work/
│           ├── SKILL.md
│           └── references/
│               └── work-package.md
└── .codex/
    └── agents/
        ├── spark-worker.toml
        ├── luna-worker.toml
        └── sol-escalation.toml
```

If your project already has an `AGENTS.md`, merge the orchestration guidance rather than replacing project-specific instructions.

## Recommended roles

| Role | Model / effort |
|---|---|
| Default parent / orchestrator | Astra Low |
| Optional tiny deterministic worker | Spark, when available |
| Default implementation worker | Luna High |
| Hard bounded worker | Sol Medium |
| Exceptional global parent reasoning | Astra Medium/High |

## What each model does

### Astra Low

Owns the global problem: understanding the request, resolving architecture and product ambiguity, deciding what to delegate, reviewing worker output, integrating changes and deciding when the task is actually complete.

For non-trivial implementation, it should generally orchestrate rather than write every bounded change itself.

### Spark

Use for very small, localised, deterministic edits that are cheap to verify. It is an optimisation, not a dependency.

### Luna High

Use for normal bounded implementation once the parent has made the important product and architecture decisions.

### Sol Medium

Use when Luna is not enough for a bounded problem - for example harder debugging, investigation, subtle state behaviour or implementation that needs materially more independent reasoning.

Sol Medium is not the parent in this setup. If the problem becomes a genuinely global architecture, security or product decision, return it to Astra.

### Astra Medium/High

Reserve higher Astra reasoning for exceptional parent-level problems where Low is not enough, especially consequential architecture, security-sensitive design, difficult cross-system reasoning or unresolved failures that change the overall approach.

## Completion and verification

The parent should continue through implementation, relevant verification and fixes caused by the requested change. It should not stop at the first plausible implementation when the requested outcome clearly means making the feature or fix work end-to-end.

Verification should be proportional to risk. Run meaningful required checks, but do not add or repeatedly rerun tests for tiny reversible changes just because a generic instruction says to test everything.

## Delegation skill

The skill description is intentionally short so it is easy for Codex to route correctly:

`Use when delegating bounded coding work to a subagent.`

The root skill only handles routing. The more detailed handoff template is in:

`templates/.agents/skills/delegate-work/references/work-package.md`

That keeps detailed instructions out of context until they are actually needed.

## Normal prompts after setup

Once installed, prompts can stay focused on the actual requirement:

```text
Add passwordless login using magic links.

Keep the existing auth architecture and email provider.
Do not change the existing password login flow.
Done when the flow works end-to-end and the relevant checks pass.
```

You should not need to append orchestration instructions to every prompt.

## Included files

- `templates/AGENTS.md`
- `templates/.agents/skills/delegate-work/SKILL.md`
- `templates/.agents/skills/delegate-work/references/work-package.md`
- `templates/.codex/agents/spark-worker.toml`
- `templates/.codex/agents/luna-worker.toml`
- `templates/.codex/agents/sol-escalation.toml`
- `examples/example-prompt.md`

## Philosophy

1. Keep repository-wide instructions short and durable.
2. Let Astra use judgement instead of turning every task into a rigid recipe.
3. Prefer delegation for non-trivial bounded implementation.
4. Keep worker context minimal and sufficient.
5. Use Spark opportunistically and fall back cleanly to Luna.
6. Use Luna High for normal work and Sol Medium only when the bounded task really needs it.
7. Raise Astra reasoning only when the parent-level problem is genuinely difficult.
8. Verify proportionately to risk.
9. Define completion clearly enough that the parent keeps going until the requested outcome actually works.
10. Preserve older setups in version branches instead of making one configuration fit every account and model generation.
