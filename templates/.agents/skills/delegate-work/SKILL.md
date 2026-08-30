---
name: delegate-work
description: Use whenever the parent agent is preparing implementation work for a subagent. Convert the task into a small, self-contained worker package before delegation.
---

# Delegate Work

Before spawning an implementation worker, create a package containing:

## GOAL
One precise outcome.

## CONTEXT
Only the information needed for this task.

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
