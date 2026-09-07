# Work package reference

Use this only when a worker needs a structured handoff.

Keep the package short. Include only what materially helps the worker complete the bounded task.

## ROUTE

Choose one:
- `spark-worker` - tiny deterministic edit when Spark is available;
- `luna-worker` - normal bounded implementation;
- `sol-escalation` - harder bounded debugging, investigation or implementation when Astra is the active parent.

If Sol is already the active parent, do not use `sol-escalation` merely to recreate the parent. Let Sol handle the harder bounded work directly.

## GOAL

One concrete outcome.

## CONTEXT

Only the repository and product context needed for this task.

## SCOPE

Files, components or behaviour the worker owns.

## CONSTRAINTS

Important interfaces, invariants, boundaries or behaviour that must remain intact.

Avoid long generic rule lists.

## DONE WHEN

State the observable completion condition.

## VALIDATION

Specify only checks that materially verify the change.

For tiny reversible edits, a focused inspection or narrow check may be enough. For risky changes, require the relevant tests, build, lint, type checks or smoke tests.

## RETURN

Ask for a concise handover containing:
1. files changed;
2. what changed;
3. meaningful verification and result;
4. any remaining risk, assumption or blocker.

## Escalation

If a worker fails, first decide whether the task was unclear or too broad.

If Astra is the active parent and the package was already clear:
- Spark -> Luna High;
- Luna High -> Sol Medium;
- Sol Medium -> return to the Astra parent and consider Astra Medium/High for a genuinely global or consequential problem.

If Sol is the active parent and the package was already clear:
- Spark -> Luna High or direct Sol handling;
- Luna High -> direct Sol handling;
- raise Sol reasoning only if the problem genuinely warrants it.

Do not force every task through every tier.
