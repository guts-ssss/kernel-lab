---
name: kernel-lab
description: Develop and optimize numerical compute kernels through evidence-driven experiments, independent trials, progressive validation, and reproducible releases. Use for kernel optimization or a kernel development campaign; adapt the workflow to analysis-only requests and the project's existing tools.
---

# Kernel Lab

Use the user's target, constraints, and existing implementation to guide an exploratory development loop. Preserve the ability to change mechanisms, diagnose failures, branch, and compose improvements. A release is a tested implementation with evidence, not a completed checklist or an incremented number.

## Start or resume

Read the project's contract, current continuation record, active implementation, and relevant experiment evidence. Check authoritative process/job handles before treating outstanding work as stopped. Record unknowns as unknown; a status file alone does not prove a process is alive.

For a new campaign, identify:

- mathematical/API behavior, input domain, dtypes, layouts, gradient and determinism requirements;
- frozen starting implementation, independent reference, existing tests and tolerances;
- primary objectives, meaningful-gain criteria, maximum tolerated regressions for every other key metric, hardware, editable scope, resource budget and endpoint;
- commands and evidence needed to validate the actual implementation users will call.

Use existing project formats and runners where suitable, with the trial/version naming below. Establish a small task-specific contract where they are absent. Do not impose DSA-specific shapes, a language, version count, hardware, tolerances, or performance thresholds on another kernel. Read [evaluation.md](references/evaluation.md) when selecting or assessing measurements, and [records.md](references/records.md) when establishing experiment records or resuming a campaign.

For implementation work, assess the [Apodex-based numerical verification gates](references/evaluation.md#numerical-verification-gates) when defining acceptance. Applicable gates are required for release; record evidence-backed inapplicability or equivalent validation, not a silent skip.

## Objectives and tradeoff limits

At task setup, agree on the objectives, metric scopes, meaningful benefits, endpoint targets, and maximum tolerated degradation in other metrics when one objective improves. Use limits already supplied by the user; resolve missing tradeoff permissions before work that depends on them. An unspecified allowance does not authorize regression. Freeze separate limits against the direct parent and the original starting baseline, including key-case limits and how measurement uncertainty is handled. Do not choose budgets after seeing candidate results. See [objective acceptance](references/evaluation.md#objective-acceptance) for the checks.

Correctness, determinism, and batch invariance are prerequisites under the declared semantics and supported domain. Separate required numerical accuracy from optional further accuracy gains. In a performance-focused task, reducing error after the numerical requirements pass does not alone earn a version. If accuracy is an independent objective, define its allowed latency and memory costs at setup. No gain offsets another metric exceeding its agreed regression budget or a failed hard gate.

Keep correctness repairs separately accounted for. A repaired implementation may become a working parent, but it never replaces the fixed starting baseline or erases repair costs. A repair alone does not qualify as a performance improvement. Compare every release candidate with both its direct parent and the fixed starting baseline; recovery from a slower parent cannot establish that the overall speed objective was achieved.

```text
Qualified release = correctness and invariants pass
                    AND all key metrics satisfy their regression budgets
                    AND a declared objective has a meaningful benefit

Task complete = required qualified-release count is met, when requested
                AND the final implementation meets the overall objectives
                    relative to the fixed starting baseline
```

## Release targets and counting

When the user requests a number of versions or a version endpoint, express it in the task contract as a required number of additional qualified releases relative to the stated starting point. The starting baseline does not count; resuming does not reset the starting point or target. Keep trial, execution, and release identities separate; failed trials never consume release progress. Only an explicit request for a number of experiments makes trial count the endpoint.

A counted release must change the effective implementation relative to its accepted parent, satisfy the qualified-release predicate above, preserve a sealed source/evidence snapshot, and pass verification through the actual activated default entry. Count it only after that verification succeeds. Screening success, superseded candidates, incomplete activation, closeout reports, cosmetic edits, and repeated packaging or activation of an existing implementation do not add releases. Previously released versions remain recoverable when a successor becomes active. At completion count only releases still eligible under the contract; report any invalidated historical releases separately. A sufficient count cannot compensate for unmet endpoint objectives. See [records.md](references/records.md) for release records and recovery, and [evaluation.md](references/evaluation.md) for change and count audits.

Name trials `trial_001`, `trial_002`, and so on, and releases `version_001`, `version_002`, and so on. Use these forms consistently in new directory basenames, metadata IDs, reports, and continuation records; do not abbreviate them or create alternate labels. Number the two sequences independently, using at least three digits. Execution identities remain distinct from trial/version identities. Preserve immutable historical evidence paths when resuming older campaigns; see [records.md](references/records.md#identities-and-naming).

Unless the user specifies a trial budget, do not impose a fixed trial limit or a fixed number of trials per release. Continue exploring until both the requested release count and the overall endpoint objectives are met, subject to the user's resource limits and stop conditions. This does not remove the bounds on confirmation reruns or permit selecting favorable timing samples. If a stop condition is reached before completion, retain the last verified entry and report the achieved/required release count and unmet objectives as incomplete. Do not reinterpret the target, weaken acceptance, or invent releases to claim completion.

## Development loop

1. **Establish the starting point.** Snapshot the original baseline, reference, and acceptance rules, including both sets of regression budgets and endpoint targets. Run the relevant baseline checks and preserve their actual results. If the baseline fails, diagnose it and record repairs separately from optimization benefits; preserve their costs against the original baseline and do not silently relax the contract.
2. **Study bottlenecks and history.** Inspect code, measure stages, read prior experiments, or run small diagnostic probes. Choose the order based on available evidence. Profiling explains behavior; it is not a substitute for final latency measurement.
3. **Propose a mechanism and useful outcome.** State what may improve, on which inputs, and at what cost. A short hypothesis suffices for an exploratory trial. New observations may change the hypothesis; distinguish an exploratory discovery from a prospectively specified confirmation.
4. **Create an independent trial.** Start from a declared parent. Normally use an accepted release; diagnostic, repair, and follow-on optimization trials may use a retained trial snapshot without promoting it. Record that working parent, the last accepted release, and the fixed original baseline; an unreleased repair does not advance version progress. Isolate source edits and associate every result with the exact source and evaluation configuration measured.
5. **Implement and add meaningful regressions.** Change the allowed implementation. Add tests for the new mechanism or diagnosed bug. Preserve independent reference behavior and established tolerances. Algorithm changes, fusion, tiling, precision techniques, dispatch, and lifetime changes are options, not a prescribed search order.
6. **Screen cheaply.** Compile, check correctness, and measure the relevant local outcome. Vary the screening cost to suit the hypothesis. A passing screen makes a trial promising; it does not make it a release.
7. **Learn from failure.** Keep compiler errors, numerical differences, unsupported assumptions, regressions, and inconclusive timing. Diagnose or change direction using the evidence. Do not repeatedly benchmark until a favorable sample appears. Corrections and reruns get distinct evidence identities.
8. **Compose when needed.** If the accepted parent changed, or several mechanisms are combined, form a new candidate on that parent and validate the composition. Preserve donor trial identities and inherited fixes. If nothing changed and the trial already has the right parent, avoid a redundant rebase.
9. **Qualify each release candidate.** Freeze the candidate before qualification; do not edit sources used by running jobs. Check correctness, determinism, batch invariance, applicable numerical gates, every key metric's parent and fixed-baseline regression budgets, and the declared meaningful benefit. Report speed, numerical error, and memory separately. Missing required evidence, unresolved numerical-gate warnings, incompatible identities, or any failed acceptance condition prevent release. Qualify the combination itself; donor results cannot prove combined gains. Any subsequent implementation edit requires evidence for the changed snapshot.
10. **Seal, activate, and verify.** Preserve final source, reference identity, tests, commands, environment, raw measurements, decisions, and lineage. After checking the expected parent, point the task-designated default entry to the sealed implementation and verify its source identity and required behavior in a fresh process using the normal public entry. Only then commit release status and progress. If verification fails, restore the previous verified entry without overwriting an unexpected newer activation; retain the failure. Keep previous releases recoverable. Activation refers to the agreed task entry, not implicit permission to deploy elsewhere.
11. **Continue or finish.** Choose the next experiment from evidence and remaining resources. At the requested endpoint, directly compare the selected final implementation to the original starting baseline and require the overall objectives to pass. Resolve coverage gaps, audit the artifacts, and report gains, costs, failures, and limitations. For a release target, also derive the completed count from validated release artifacts and activation evidence, and verify the requested final default. Existing endpoint measurements, a sufficient version count, directory names, or self-reported counters cannot establish completion without the endpoint benefit and budget checks. Do not multiply intermediate speedups or create no-op releases to meet a version target.

## Exploration and protocol changes

Keep search decisions with the developer/agent. Do not force a fixed number of proposals, a specific optimization sequence, or a synthetic score across unlike units. Multiple successful trials can remain alternatives; select or compose them according to the user's objective. Success on one domain may justify guarded dispatch only after the complete dispatch and its boundaries are tested.

When layout or boundary handling slows optimization of the compute core, consider first developing on a small, representative subset of the supported inputs, then extending the candidate to the remaining layouts and cases. Choose the subset to preserve the bottleneck being studied; include complex layouts when they are central to it. This staging narrows only the current development scope: keep the full input contract and acceptance criteria unchanged. Before qualifying any release, validate the complete candidate against all task requirements, including dispatch and fallback paths where used.

If a reference or evaluator bug is demonstrated, record the issue, fix the affected protocol explicitly, and reevaluate the baseline and affected candidates. Preserve old failures and artifacts; report the changed comparison. User-defined semantics and constraints cannot be loosened merely to make a candidate pass.

Historical successes are hypotheses to test under current conditions. Disclose previously seen implementations and test data. Preserve unsuccessful directions; do not claim unseen discovery or clean holdout evaluation when the data were visible.

## Tools, handoff, and truthful claims

Use project scripts for repetitive or fragile operations, especially measurements and identity checks. For a release-count campaign, reuse or add a small release checker and make its passing result a condition of promotion and completion. It must reject missing or inconsistent evidence; see [evaluation.md](references/evaluation.md) for checks. A new scheduler, database, or API runtime is not a prerequisite.

Keep a compact continuation record with the requested endpoint, verified release progress, actual current parent/default, pending publication stage, exact job handles, evidence paths, unresolved questions, and the next useful action. Reconcile progress and default-entry state with artifacts on resume. Persist enough context to resume from files rather than memory.

Distinguish these claims in the final report: workflow followed; kernel gains measured; skill transferred to another task; skill causally increased gains. A single successful campaign supports the first two, not automatically the latter two. Cross-kernel use tests transfer; a controlled comparison is needed for a causal gain claim.

This skill coordinates the developer's available tools; it does not independently schedule work or guarantee that an agent follows instructions. Respect the task's authorization and resource limits. A read-only analysis request does not authorize running an optimization campaign.
