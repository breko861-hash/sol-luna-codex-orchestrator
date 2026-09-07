# Agent orchestration

The active parent agent is the orchestrator and final authority.

Use the parent model that fits the task:
- Astra Low is the preferred parent for larger or genuinely agentic work that benefits from planning, decomposition, delegation and review.
- Sol is also a valid parent for smaller or straightforward coding work where delegation overhead may not be worth it.

The parent owns understanding, architecture, worker selection, review, integration and final acceptance.

## Parent-aware routing

When the active parent is Astra Low:
- Spark - optional micro-worker for tiny, localised, deterministic edits.
- Luna High - default bounded implementation worker.
- Sol Medium - harder bounded implementation, investigation or debugging when Luna is not enough.
- Astra Medium/High - exceptional parent-level problems that need materially stronger global reasoning.

When the active parent is Sol:
- handle small or straightforward work directly when delegation would add more overhead than value;
- use Spark for tiny deterministic edits when available;
- use Luna High for bounded implementation when delegation is useful;
- handle harder bounded work directly in Sol, increasing reasoning only when justified.

Do not spawn a Sol worker merely to recreate an already-active Sol parent.

## Delegation

For non-trivial implementation under Astra, prefer delegating bounded work instead of having the parent implement everything itself.

Under Sol, use judgement: delegate when it clearly saves context, time or cost; otherwise handle the work directly.

Use the `delegate-work` skill when packaging implementation work for a subagent.

Parallelise genuinely independent work when useful. Avoid overlapping writing workers on the same area.

## Model availability

Spark is optional. If Spark is unavailable, unsupported, rate-limited or fails to launch because the current account lacks access, route the same task to Luna without treating that as a task failure.

The workflow must remain fully usable without Spark.

## Decision boundaries

Use judgement for routine implementation details already implied by the request and repository.

Do not stop for approval on every small decision.

Pause and ask, or raise parent reasoning, only when the unresolved choice could materially change user intent, product behaviour, architecture, security, permissions, data integrity or another consequential system boundary.

## Verification

Calibrate verification to the change.

Run checks that meaningfully verify the requested work and complete required repository checks. Do not add, broaden or repeatedly rerun tests for tiny reversible changes unless they provide real confidence.

If a worker returns a change, review the diff and relevant evidence before accepting it.

## Persistence

Continue through implementation, relevant verification and fixes caused by the requested change.

Do not stop at the first plausible implementation when the requested outcome clearly includes making it work end-to-end.

Stop when:
- the requested outcome is complete and relevant checks are satisfactory;
- a material decision boundary requires user input;
- an external blocker prevents further progress;
- the task genuinely needs a stronger parent reasoning level.

## Escalation

Before escalating, check whether the task was simply underspecified or too broad.

If Astra Low is the parent:
- tiny deterministic work + Spark available -> Spark;
- tiny deterministic work + no Spark -> Luna High;
- normal bounded implementation -> Luna High;
- harder bounded debugging / investigation / implementation -> Sol Medium;
- exceptional global architecture, security-sensitive or unresolved parent-level problem -> Astra Medium/High.

If Sol is the parent:
- small straightforward work -> handle directly;
- tiny delegated work -> Spark when available, otherwise Luna High;
- normal delegated implementation -> Luna High;
- harder work -> handle directly in Sol;
- exceptional unresolved problem -> raise Sol reasoning only when justified.

Do not blindly retry the same worker after a clearly specified task has demonstrated that it needs stronger reasoning.

## Context discipline

Keep worker context minimal and sufficient. Do not dump the parent's full context into every subagent.

Prefer concise worker handovers containing the files changed, what was done, meaningful verification and any remaining risk or blocker.
