# Astra/Sol + Spark/Luna Codex Orchestrator

A lightweight Codex multi-agent setup that keeps the parent focused on judgement while delegating bounded implementation when it actually helps.

The current setup is parent-aware rather than Astra-only:

- **GPT-6 Astra Low** - preferred parent for larger or genuinely agentic work that benefits from planning, decomposition, delegation and review
- **Sol** - also a valid parent for smaller or straightforward coding work where delegation overhead may not be worth it
- **Spark** - optional ultra-fast micro-worker for tiny deterministic edits when available
- **Luna High** - default bounded implementation worker
- **Sol Medium** - harder bounded worker when Astra is the parent and Luna is not enough
- **Astra Medium/High** - exceptional parent-level reasoning when Astra Low is not enough

> **The active parent owns the global problem. Delegate bounded work only when it saves context, time or cost.**

## Preserved older setups

- [`v1-sol-luna`](https://github.com/breko861-hash/sol-luna-codex-orchestrator/tree/v1-sol-luna) - original Sol + Luna setup
- [`v2-sol-spark-luna-terra`](https://github.com/breko861-hash/sol-luna-codex-orchestrator/tree/v2-sol-spark-luna-terra) - Sol Medium parent, Spark/Luna workers, Terra High escalation
- [`v3-astra-low-parent`](https://github.com/breko861-hash/sol-luna-codex-orchestrator/tree/v3-astra-low-parent) - Astra Low parent, Spark/Luna workers and Sol Medium escalation

## Why the instructions are leaner now

Newer models need less scaffolding than earlier coding agents. The current template deliberately keeps `AGENTS.md` focused on durable repository-wide behaviour instead of turning it into a long execution recipe.

The delegation skill also uses progressive disclosure: its root file is a small router, while the detailed work-package format lives in a supporting reference that is only needed when work is actually delegated.

Verification is proportional to the change rather than ritualistic, and persistence is explicit so the active parent continues through implementation, relevant checks and fixes when the requested outcome clearly requires end-to-end completion.

## Parent-aware routing

### When Astra Low is the parent

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

### When Sol is the parent

```text
Sol
    |
    |-- small / straightforward work ------------> handle directly
    |
    |-- tiny delegated work ----------------------> Spark if available
    |
    |-- normal delegated implementation ----------> Luna High
    |
    `-- harder work ------------------------------> handle directly in Sol
```

If Sol itself is the active parent, do not spawn a Sol worker merely to recreate the parent. Increase Sol reasoning only when the task genuinely justifies it.

Spark is optional. If the current account or Codex runtime cannot use it, the same delegated task falls back to Luna.

## Quick start

Copy the contents of `templates/` into the root of your project.

Use **Astra Low** as the parent for larger/orchestrated work, or **Sol** as the parent for smaller/straightforward work.

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

| Situation | Recommended model / effort |
|---|---|
| Larger / agentic parent | Astra Low |
| Smaller / straightforward parent | Sol |
| Optional tiny deterministic worker | Spark, when available |
| Default bounded implementation worker | Luna High |
| Hard bounded worker under Astra | Sol Medium |
| Exceptional Astra parent reasoning | Astra Medium/High |

## What each model does

### Astra Low

Use as the parent when the task benefits from planning, decomposition, multi-step coordination, review and integration. For non-trivial implementation, it should generally orchestrate rather than write every bounded change itself.

### Sol as parent

Use for smaller or straightforward coding work where spawning workers would add unnecessary overhead. Sol may still delegate to Spark or Luna when doing so clearly saves context, time or cost.

For harder work, the active Sol parent handles the problem directly. Raise Sol reasoning only when justified rather than routing through a separate Sol worker.

### Spark

Use for very small, localised, deterministic edits that are cheap to verify. It is an optimisation, not a dependency.

### Luna High

Use for normal bounded implementation once the active parent has resolved the important product and architecture decisions.

### Sol Medium worker

When Astra is the parent, use Sol Medium if Luna is not enough for a bounded problem such as harder debugging, investigation, subtle state behaviour or implementation that needs materially more independent reasoning.

If the problem becomes a genuinely global architecture, security or product decision, return it to the active parent instead of silently taking ownership of it.

### Astra Medium/High

Reserve higher Astra reasoning for exceptional parent-level problems where Astra Low is not enough, especially consequential architecture, security-sensitive design, difficult cross-system reasoning or unresolved failures that change the overall approach.

## Completion and verification

The active parent should continue through implementation, relevant verification and fixes caused by the requested change. It should not stop at the first plausible implementation when the requested outcome clearly means making the feature or fix work end-to-end.

Verification should be proportional to risk. Run meaningful required checks, but do not add or repeatedly rerun tests for tiny reversible changes just because a generic instruction says to test everything.

## Delegation skill

The skill description is intentionally short:

`Use when delegating bounded coding work to a subagent.`

The root skill handles routing. The more detailed handoff template lives in:

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
2. Let the active parent use judgement instead of turning every task into a rigid recipe.
3. Use Astra Low for larger orchestrated work and Sol for smaller straightforward work.
4. Delegate only when it provides a real benefit.
5. Keep worker context minimal and sufficient.
6. Use Spark opportunistically and fall back cleanly to Luna.
7. Use Luna High for normal bounded implementation.
8. When Astra is the parent, use Sol Medium only when the bounded task really needs it.
9. Verify proportionately to risk.
10. Define completion clearly enough that the parent keeps going until the requested outcome actually works.
11. Preserve older setups in version branches instead of forcing one configuration onto every model generation.
