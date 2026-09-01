---
name: delegate-work
description: Use whenever the parent agent is preparing implementation work for a subagent. Select the cheapest appropriate available worker and convert the task into a small, self-contained work package before delegation.
---

# Delegate Work

Before spawning an implementation worker:

1. Resolve architecture and product ambiguity.
2. Decide whether delegation is actually worthwhile.
3. Select the cheapest available worker capable of reliably completing the task.
4. Create a self-contained work package.

## ROUTE

Choose one:

### spark-worker

Use for tiny, localised and deterministic changes.

The worker should not need substantial exploration or independent design judgement.

Spark is optional. If it is unavailable, unsupported, rate-limited, or fails to launch because the current account lacks access, immediately route the same package to `luna-worker`. Do not retry Spark and do not treat missing Spark access as a task failure.

### luna-worker

Default for normal bounded implementation.

Use when the task requires meaningful coding judgement but architecture and behaviour are already defined.

### terra-worker

Use for difficult bounded debugging, investigation, exploration or cross-cutting implementation.

### sol-escalation

Use when materially stronger global judgement is required, including architecture, security-sensitive decisions, consequential cross-system reasoning or repeated worker failure.

Do not escalate merely because decomposition is poor.

## GOAL

One precise outcome.

## CONTEXT

Only information needed for this task.

Do not pass the parent's full context unless it is genuinely required.

## SCOPE

Files, components or behaviour the worker owns.

## DO NOT TOUCH

Relevant boundaries that must remain unchanged.

## CONTRACT

Interfaces, invariants, inputs, outputs or behaviour that must be preserved.

## DONE WHEN

Concrete, testable acceptance criteria.

## VALIDATION

Exact tests, builds, linting, type checks or smoke tests to run.

Validation must always be explicit for Spark.

## RETURN

Return only:
1. files changed;
2. concise implementation summary;
3. validation run and exact result;
4. assumptions;
5. remaining risks, questions or blockers.

Before delegation, reject and split the package if:
- it contains multiple loosely related outcomes;
- architecture is unresolved;
- acceptance criteria are unclear;
- it overlaps another active writing worker;
- the worker would need to guess parent intent.

Pass the smallest sufficient context.

## Escalation

Do not repeatedly retry a model on the same clearly specified failure.

Default escalation:

`spark-worker -> luna-worker -> terra-worker -> sol-escalation`

This is not a mandatory sequence. Skip levels when the nature of the task clearly requires a stronger worker.
