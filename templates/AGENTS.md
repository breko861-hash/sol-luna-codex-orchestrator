# Agent orchestration

Use GPT-6 Astra Low as the default parent for normal work.

The parent owns understanding, architecture, decomposition, worker selection, review, integration and final acceptance.

Preferred routing:
- Spark - optional micro-worker for tiny, localised, deterministic edits.
- Luna High - default bounded implementation worker.
- Sol Medium - harder bounded implementation, investigation and debugging when Luna is not enough.
- Astra Medium/High - exceptional global problems that need materially stronger parent reasoning.

## Delegation

For non-trivial implementation, prefer delegating bounded work instead of having the Astra parent implement everything itself.

Delegate when a task can be given one clear outcome, sufficient local context and a concrete definition of done.

Parallelise genuinely independent work when it saves time or improves quality. Do not run overlapping writing workers on the same area.

Use the `delegate-work` skill when packaging implementation work for a subagent.

## Model availability

Spark is optional. If Spark is unavailable, unsupported, rate-limited or fails to launch because the current account lacks access, route the same task to Luna without treating that as a task failure.

The workflow must remain fully usable without Spark.

## Decision boundaries

Use judgement for routine implementation details that are already implied by the request and repository.

Do not stop for approval on every small decision.

Pause and ask, or escalate parent reasoning, only when the unresolved choice could materially change:
- user intent;
- product behaviour;
- architecture;
- security or permissions;
- data integrity;
- another consequential system boundary.

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
- the task should be escalated to a stronger model.

## Escalation

Use the cheapest model that is likely to complete the bounded task reliably.

Default routing:
- tiny deterministic work + Spark available -> Spark;
- tiny deterministic work + no Spark -> Luna High;
- normal bounded implementation -> Luna High;
- harder bounded debugging / investigation / implementation -> Sol Medium;
- exceptional global architecture, security-sensitive or unresolved parent-level problem -> Astra Medium/High.

Do not blindly retry the same worker after a clearly specified task has demonstrated that it needs stronger reasoning.

Before escalating, check whether the task was simply underspecified or too broad.

## Context discipline

Keep worker context minimal and sufficient. Do not dump the parent's full context into every subagent.

Prefer concise worker handovers containing the files changed, what was done, meaningful verification and any remaining risk or blocker.
