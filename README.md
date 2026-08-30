# Sol + Luna Codex Orchestrator

A lightweight, cost-efficient Codex multi-agent setup:

- **Sol** - planning, architecture, orchestration, review and final acceptance
- **Luna** - default implementation worker
- **Terra** - optional fallback for harder bounded implementation/debugging tasks

> **Sol thinks globally. Luna executes locally. Sol accepts the final result.**

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
        ├── luna-worker.toml
        └── terra-worker.toml
```

If your project already has an `AGENTS.md`, merge the orchestration section rather than replacing project-specific instructions.

## Recommended routing

| Role | Model / effort |
|---|---|
| Default orchestrator | Sol Medium |
| Default implementation worker | Luna High |
| Difficult bounded worker task | Terra High |
| Difficult architecture / review | Sol High |
| Exceptional cases | Sol xhigh |

Do not automatically use the strongest reasoning level.

## Workflow

```text
Sol
 ↓
understand + plan + resolve architecture
 ↓
small self-contained work packages
 ↓
Luna
 ↓
implementation + validation
 ↓
Sol
 ↓
review / correction / final acceptance
```

Use Terra only when a **bounded** implementation task genuinely needs materially more independent reasoning, exploration or debugging.

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
- `templates/.codex/agents/luna-worker.toml`
- `templates/.codex/agents/terra-worker.toml`
- `examples/example-prompt.md`

## Philosophy

1. Resolve ambiguity before delegation.
2. Give workers one clear outcome at a time.
3. Keep worker context minimal and sufficient.
4. Let workers validate their own implementation, but not self-accept it.
5. Keep final review and acceptance with Sol.
6. Fix poor decomposition before escalating model strength.
7. Parallelise only genuinely independent work.
