# Agent orchestration

The parent agent is the orchestrator and final authority.

Use this routing by default:
- Sol: planning, architecture, decomposition, integration, review and final acceptance.
- Luna: default implementation worker.
- Terra: fallback for bounded implementation tasks that require materially more independent reasoning, exploration or debugging.

## Workflow

For non-trivial implementation:
1. Understand the request and relevant code.
2. Resolve architectural or product ambiguity before delegation.
3. Make a concise implementation plan.
4. Split work into small, self-contained packages.
5. Delegate implementation to Luna.
6. Review returned changes and validation evidence.
7. Send narrowly scoped correction work back if needed.
8. Sol performs final acceptance.

Do not delegate unresolved architecture or product decisions.

## Delegation rules

Each worker assignment must have one clear outcome and include:
- GOAL
- CONTEXT
- SCOPE
- DO NOT TOUCH
- CONTRACT
- DONE WHEN
- VALIDATION
- RETURN

Keep assignments bite-sized.

A task is too broad if the worker must:
- rediscover the overall architecture;
- decide major design questions;
- infer unclear product requirements;
- modify several unrelated areas;
- guess what the parent intends.

Workers must:
- make the smallest defensible change;
- follow existing repository patterns;
- avoid unrelated refactors;
- avoid scope expansion;
- stop and report ambiguity instead of guessing.

Do not let multiple writing workers modify overlapping areas at the same time.

## Review

Workers may test their own implementation, but their own assessment is not final acceptance.

Sol owns:
- review;
- integration;
- acceptance;
- deciding whether correction work is required.

If a worker fails:
1. first check whether the assignment was unclear or too broad;
2. tighten or split the assignment;
3. retry with Luna when appropriate;
4. use Terra if the bounded task genuinely requires stronger independent reasoning;
5. use stronger Sol reasoning when the problem is architectural or fundamentally ambiguous.

Do not escalate models merely to compensate for poor decomposition.

## Context discipline

Keep Sol focused on:
- user requirements;
- architecture;
- decisions;
- plan;
- worker summaries;
- diffs;
- validation evidence.

Workers should return concise evidence, not long transcripts or raw logs.
