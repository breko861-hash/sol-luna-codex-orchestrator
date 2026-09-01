# Agent orchestration

The parent agent is the orchestrator and final authority.

Preferred default parent:
- Sol Medium for normal planning, architecture, decomposition, orchestration, review, integration and final acceptance.
- Keep Sol at Medium by default. Raise Sol to High or xHigh only when the problem genuinely requires materially stronger global reasoning.

Use workers for implementation according to task difficulty:
- Spark: optional micro-worker for tiny, localised, deterministic implementation tasks.
- Luna: default bounded implementation worker.
- Terra High: difficult bounded implementation, investigation or debugging.
- Sol High/xHigh: very difficult architecture, security-sensitive work, consequential cross-system reasoning, or problems that remain unresolved after a well-specified Terra attempt.

Do not use Terra as the default orchestrator. Sol Medium owns the overall plan, decomposition, review and acceptance.

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

Sol may handle directly:
- questions and analysis;
- trivial changes where delegation would cost more than execution;
- architecture or product decisions that must be resolved before implementation;
- final integration and acceptance.

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

Terra remains a worker/escalation tier. Project-level architecture, product decisions, review and final acceptance remain with Sol.

## Sol escalation

Sol Medium remains the parent throughout normal work.

Raise Sol to High when the problem requires materially stronger global judgement, including:
- major architecture decisions;
- authentication or security-sensitive design;
- consequential data or infrastructure changes;
- difficult cross-system reasoning;
- unresolved product ambiguity;
- repeated failure after a well-specified Terra attempt;
- particularly high-risk final review.

Use Sol xHigh only for exceptional cases where High is still insufficient or the consequences justify the additional reasoning cost.

Do not raise Sol reasoning merely to compensate for poor task decomposition.

## Workflow

For non-trivial implementation:
1. Sol Medium understands the request and relevant code.
2. Sol resolves architectural and product ambiguity.
3. Sol makes a concise implementation plan.
4. Sol splits implementation into small, self-contained packages.
5. Sol selects the cheapest appropriate available worker.
6. Give the worker only the context required for that package.
7. Require deterministic validation where possible.
8. Sol reviews the returned diff and validation evidence.
9. Send narrowly scoped correction work if required.
10. Sol integrates and performs final acceptance.

## Worker selection

Use this routing by default:

- Tiny deterministic task + Spark available -> Spark.
- Tiny deterministic task + Spark unavailable -> Luna.
- Normal bounded implementation -> Luna.
- Difficult bounded implementation/debugging/investigation -> Terra High.
- Very difficult global/architectural/security-sensitive problem -> Sol High.
- Exceptional unresolved problem -> Sol xHigh.

This is a routing guide, not a requirement to try every model in sequence.

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

Do not dump Sol's full context into workers.

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
- guess what Sol intends.

## Failure and escalation

If Spark fails a clearly specified task:
- do not repeatedly retry Spark;
- route the same bounded task to Luna.

If Luna fails:
1. check whether the assignment was unclear or too broad;
2. improve or split the package if necessary;
3. if the package was already good, escalate the bounded task to Terra High.

If Terra fails:
- determine whether the problem is architectural, ambiguous, security-sensitive or otherwise consequential;
- raise Sol from Medium to High for those problems;
- use Sol xHigh only when the problem remains exceptional and unresolved.

Prefer better decomposition before stronger models.

## Concurrency

Parallelise only genuinely independent work.

Never allow multiple writing workers to modify overlapping areas simultaneously.

## Review

Worker validation comes before Sol review whenever possible.

Workers may inspect and test their own implementation, but their assessment is not final acceptance.

Sol owns:
- integration;
- review;
- correction decisions;
- final acceptance.

For consequential issues, raise Sol reasoning effort before final acceptance when justified.

## Context discipline

Keep Sol focused on:
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
