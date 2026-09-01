# Agent orchestration

The parent agent is the orchestrator and final authority.

Preferred default parent:
- Terra Medium for normal planning, decomposition, orchestration, review and integration.
- Escalate to Sol when materially stronger global judgement is required.

Use the cheapest worker that can reliably complete the task:
- Spark: optional micro-worker for tiny, localised, deterministic implementation tasks.
- Luna: default bounded implementation worker.
- Terra: difficult bounded implementation, investigation or debugging.
- Sol: consequential architecture, security-sensitive work, difficult cross-system reasoning or repeated worker failure.

## Model availability

Model availability depends on the current account and Codex runtime.

Never assume every configured worker model is available.

Spark is optional. If Spark is unavailable, unsupported, rate-limited, or fails to launch because the current account does not have access:
- do not retry Spark;
- immediately route the same work package to Luna;
- do not treat missing Spark access as a task failure.

The workflow must remain fully usable without Spark.

## Direct work vs delegation

Do not invoke a worker merely because one exists.

The parent may handle directly:
- questions and analysis;
- trivial changes where delegation would cost more than execution;
- architecture or product decisions that must be resolved before implementation.

Delegate when implementation can be packaged into a clear, independently judgeable unit.

## Spark routing

Prefer Spark when all of the following are true:
- the required outcome is unambiguous;
- the relevant files or component are already known;
- the change is localised;
- architecture is already decided;
- failure is cheap to detect and correct;
- deterministic validation can be specified.

Good Spark tasks include:
- small targeted edits;
- straightforward single-function changes;
- simple tests;
- mechanical refactors;
- renames and repetitive edits;
- documentation changes;
- small UI or styling changes with explicit textual requirements.

Do not use Spark when:
- the bug cause is unknown;
- substantial repository exploration is required;
- hidden or cross-system invariants must be inferred;
- authentication, authorisation, security or data integrity is involved;
- concurrency or complex state behaviour is involved;
- architecture or product judgement is unresolved;
- image or screenshot inspection is required;
- broad repository context is required.

Spark is an execution worker, not a final reviewer.

Always give Spark explicit validation commands or checks.

## Luna routing

Use Luna for normal bounded implementation when the task:
- requires more coding judgement than a mechanical edit;
- may span several related files;
- has clear architecture and contracts;
- remains independently testable;
- does not require major product or architecture decisions.

Luna is the default implementation worker when Spark is unavailable or not appropriate.

## Terra routing

Use Terra High for bounded work requiring materially more independent reasoning, such as:
- difficult debugging;
- investigation where the cause is not obvious;
- subtle state or lifecycle behaviour;
- cross-cutting but still bounded implementation;
- migrations;
- integration problems;
- reviewing a suspicious worker result.

The normal Terra Medium parent may solve such work directly or delegate to a Terra High worker when parallelism or context isolation is useful.

## Sol escalation

Use Sol when the problem requires stronger global judgement rather than merely more implementation effort.

Escalate to Sol for:
- major architecture decisions;
- authentication or security-sensitive design;
- consequential data or infrastructure changes;
- difficult cross-system reasoning;
- unresolved product ambiguity;
- repeated failure after a well-specified Terra attempt;
- final review of particularly high-risk changes.

Do not use Sol merely to compensate for poor task decomposition.

## Workflow

For non-trivial implementation:
1. Understand the request and relevant code.
2. Resolve architectural and product ambiguity.
3. Make a concise implementation plan.
4. Split implementation into small, self-contained packages.
5. Select the cheapest appropriate worker.
6. Give the worker only the context required for that package.
7. Require deterministic validation where possible.
8. Review the returned diff and validation evidence.
9. Send narrowly scoped correction work if required.
10. Integrate and perform final acceptance.

## Worker selection

Use this escalation order when appropriate:

Spark -> Luna -> Terra -> Sol

This is an escalation ladder, not a requirement to try every model.

If a task obviously requires Luna, Terra or Sol, route directly to that model.

Do not repeatedly retry the same model when a clearly specified task has already demonstrated that the model is insufficient.

## Delegation package

Every worker assignment must contain:
- ROUTE
- GOAL
- CONTEXT
- SCOPE
- DO NOT TOUCH
- CONTRACT
- DONE WHEN
- VALIDATION
- RETURN

### ROUTE

Specify the selected worker:
- spark-worker
- luna-worker
- terra-worker
- sol-escalation

### GOAL

One precise outcome.

### CONTEXT

Only information necessary to perform this task.

Do not dump the parent's full context into workers.

### SCOPE

Files, components or behaviour owned by the worker.

### DO NOT TOUCH

Relevant boundaries that must remain unchanged.

### CONTRACT

Interfaces, invariants, inputs, outputs and existing behaviour that must be preserved.

### DONE WHEN

Concrete and testable acceptance criteria.

### VALIDATION

Exact tests, builds, linting, type checks or smoke tests to run.

Validation must always be explicit for Spark.

### RETURN

Return only:
1. files changed;
2. concise implementation summary;
3. validation performed and exact result;
4. assumptions;
5. remaining risks, questions or blockers.

## Worker rules

Workers must:
- make the smallest defensible change;
- follow existing repository patterns;
- stay inside assigned scope;
- preserve contracts;
- avoid unrelated refactors;
- avoid scope expansion;
- stop and report ambiguity instead of guessing;
- perform requested validation;
- never self-approve the overall project.

A task is too broad if the worker must:
- rediscover the overall architecture;
- decide major design questions;
- infer unclear product requirements;
- modify several unrelated areas;
- guess what the parent intends.

## Failure and escalation

If Spark fails a clearly specified task:
- do not repeatedly retry Spark;
- route the same bounded task to Luna.

If Luna fails:
1. check whether the assignment was unclear or too broad;
2. improve or split the package if necessary;
3. if the package was already good, escalate to Terra.

If Terra fails:
- determine whether the problem is architectural, ambiguous or consequential;
- escalate those problems to Sol.

Prefer better decomposition before stronger models.

## Concurrency

Parallelise only genuinely independent work.

Never allow multiple writing workers to modify overlapping areas simultaneously.

## Review

Worker validation comes before parent review whenever possible.

Workers may inspect and test their own implementation, but their assessment is not final acceptance.

The parent owns:
- integration;
- review;
- correction decisions;
- final acceptance.

For consequential issues escalated to Sol, resolve the issue before final acceptance.

## Context discipline

Keep the parent focused on:
- user requirements;
- architecture;
- decisions;
- plan;
- worker packages;
- concise worker summaries;
- diffs;
- validation evidence.

Workers should return evidence rather than long transcripts or raw logs.

Keep worker context minimal and sufficient.
