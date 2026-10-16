# Evidence and measurement

Apply only requirements relevant to the operator and requested claim. Reuse existing validated evaluators and document their coverage; do not infer coverage from a green exit code alone.

## Objective acceptance

Before candidate development, define the primary objectives, hard constraints, key workloads and metric scopes, meaningful-gain criteria, and final objectives relative to the original starting baseline. For each objective's improvement, specify the maximum allowed deterioration of every other key metric. Record separate direct-parent and fixed-baseline limits so individually small regressions cannot accumulate beyond the total budget. Use existing user authorization; resolve material missing tradeoff permissions at setup. Missing limits do not permit regression. Freeze the treatment of measurement uncertainty separately from permitted tradeoffs; uncertainty cannot make an unverified budget pass.

Qualification is conjunctive: correctness, determinism, and batch-invariance requirements pass; every key metric remains within both sets of regression budgets; and a declared objective shows a meaningful benefit. Assess all required cases, including regressions outside the case selected to demonstrate a gain. An aggregate may be used only with the frozen weights and required per-case limits. Improvement in one metric never cancels an over-budget regression in another. A tradeoff is eligible only within its pre-agreed limits.

Numerical acceptance establishes required accuracy. In a performance-focused task, extra accuracy after those gates pass is supplementary evidence, not a qualifying benefit by itself. An independent accuracy objective must have explicit latency and memory cost limits. Keep the declared memory metric and phase fixed: lower forward-retained storage cannot stand in for a required F+B peak reduction. Report auxiliary measurements alongside the actual objective rather than switching which metric determines success.

Record correctness repairs separately, including their error, latency, and memory effects against the original baseline. A repair may supply a correct working parent but cannot reset the fixed comparison or automatically count toward a performance-release target. It qualifies only if the complete release predicate, including the task's meaningful-benefit rule, holds. If correctness and the agreed cost budgets cannot both be satisfied, retain the repair evidence and report the optimization goal as incomplete; do not weaken correctness or silently accept the costs.

For every release, retain source-bound comparisons with both the direct parent and the original baseline. The parent comparison tests incremental usefulness; the baseline comparison enforces cumulative limits. Comparable unchanged-path evidence may be reused only with justified source and workload identity. At completion, directly measure the final implementation against the original baseline and evaluate all endpoint targets and budgets. Neither improvement over a degraded parent nor merely having endpoint measurements establishes task success.

## Correctness

Separate the independent mathematical reference, performance comparator, and direct parent. Matching the parent does not prove mathematical correctness. Include all requested outputs, auxiliary outputs, and gradients. Specify loss/upstream gradients, masked and empty behavior, aliasing, layout, boundary cases, and dispatch boundaries where relevant.

Use error measures appropriate to the numerical domain. Relative error is unstable near zero; preserve absolute error and rounding-floor evidence when relevant. Match nonfinite patterns according to the contract rather than dropping them. Distinguish approximate correctness, determinism on repeated identical inputs, and bitwise equivalence to the parent. No universal bitwise requirement applies to every optimization.

Define repeated-input determinism and batch-invariance checks under the operator's actual semantics. Compare sample-local observables under reordered, duplicated, and mixed batches with matching per-sample inputs and upstream gradients. Batch-aggregated outputs and parameter gradients require their own semantic and repeated-identical-batch checks; they are not expected to match across different batches. These invariant gates must pass before any performance or optional accuracy benefit can qualify a release.

Preserve existing tests and tolerances. Additional tests must assert observable behavior or a meaningful invariant. Validate native public dtypes; report the scope of any higher-precision intermediate calculation. Use a bounded reference for large inputs only after validating its equivalence on tractable inputs. Code sanitizers are additional evidence, not a complete proof of memory safety or mathematical correctness.

### Numerical verification gates

Use [Apodex verification](https://github.com/ApodexAI/apodex-core/tree/427959f2a4bd7cb072e369b74b8dc22160dbe6c6/apodex_core/verification) as the default numerical-validation model: separate error-magnitude and directional-drift gates. Prefer its evaluator when compatible; a validated project equivalent is acceptable. Pin the evaluator version and configuration, but do not treat upstream thresholds, dtypes, or sampling budgets as universal requirements.

Before confirmation, record applicability and coverage. Changes that may alter floating-point behavior, including fusion, accumulation/reduction order, casts, precision, or approximations, must pass both gates on affected outputs and gradients before release, subject only to the scoped WARN policy below. Exact/integer, data-movement-only, or demonstrably numerically unchanged paths may instead use justified exact/invariant checks; document why the statistical gates do not apply and retain the alternative evidence. Missing tools or budget alone cannot justify inapplicability; unmet required checks remain unverified.

Evaluate the candidate, a trusted working-precision reference, and an independently validated higher-precision mathematical reference (golden) on identical inputs and weights. Check golden's actual forward/backward computation; widening input dtypes alone does not establish its precision or independence.

Assess error magnitude against golden separately from signed candidate-reference drift. Decide in the task contract whether reference alignment or improved mathematical accuracy governs acceptance: a more accurate candidate can have systematic differences from the reference. Resolve directional alarms using that objective and the existing hard gates.

Use independent input replicates as statistical samples and task-relevant buckets such as channels or heads; correlated tensor elements are not independent samples. Include legal probe distributions that can expose drift hidden by symmetric inputs or global averaging. Combine statistical significance with effect size and rounding floors calibrated to the actual comparison precision. Freeze task-specific thresholds, sampling budgets, and stopping rules before confirmation. Use known faulty and known acceptable implementations to check a new or materially changed gate's detection and false alarms.

For tail estimates, state the model assumptions and distinguish fitted bounds from empirical maxima. If fitting fails or the budget ends before required confidence is established, retain the observations and mark that claim unverified. These gates supplement the task's direct correctness, coverage, and runtime checks; passing them does not prove algebraic equivalence or end-to-end training behavior.

FAIL, unresolved WARN, missing required evidence, or unestablished required confidence blocks release. A WARN exception must identify its cause, gate, operator/input scope, and evidence; satisfy the predefined alignment/accuracy objective and all hard constraints; and show the other gate passes. Revalidate the exception for the candidate instead of inheriting a blanket allowlist. When using Apodex, its default `testcases` run exercises method fixtures, not production acceptance: use `--source operations` or a task-specific adapter bound to the real candidate, and inspect per-case verdicts and coverage rather than trusting the exit code alone.

## Timing

Use matching inputs and runtime settings, synchronize appropriately, exclude compilation and warmup from steady-state latency, and preserve raw samples. Prefer paired baseline/candidate measurements on an uncontended device with alternated order. Record hardware, software, timing scope, execution mode, warmup, sample count, and variation.

Separate forward, backward, forward+backward, eager/graph, and kernel/end-to-end claims. Freeze the confirmation matrix, weights, thresholds, and comparison before measuring formal gains. Treat tiny/noisy changes as inconclusive when evidence does not separate them from variation. Bound extra confirmation attempts in advance; keep all attempts.

An aggregate must not conceal a forbidden per-case regression. Apply the frozen latency budgets against both parent and original baseline, and distinguish a statistically resolved change from the task's minimum useful gain. A speedup is baseline latency divided by candidate latency. A 1.05x speedup is not a 5% latency reduction. Compare endpoints directly rather than multiplying ratios from different runs or case sets.

## Memory, precision, and costs

Define memory phase, allocator metric, live-before-call subtraction, included outputs/gradients, and excluded cache. Repeat measurements separately from timing; do not mix reserved memory, incremental allocated peak, and total process memory.

Keep precision, latency, memory, compilation, and implementation complexity as separate dimensions. Apply task-specific hard constraints before accepting a benefit. Disclose added scratch space or restricted domains. Unknown costs remain unknown; subprocess wall time is not pure GPU execution time.

## Identity and coverage

Bind evidence to source/configuration, reference, test/evaluator, workload/seed, and environment identities. Source includes build options and non-Python files when relevant. Modified candidates require fresh evidence for the modified snapshot. Record each retry separately and retain its failure.

Compare expected and observed case identities, not only counts. Reject missing required cases, duplicates, unapproved skips, nonfinite metrics, truncated results, and mismatched identities. Derive summaries from raw data. If a protocol changes, retain the old evidence and explicitly identify which comparisons were rerun.

Repeatedly observed seeds are regression seeds. Keep a genuinely untouched confirmation set when feasible; otherwise disclose that no unseen holdout was used.

## Release audit

Before claiming completion, verify each task requirement against actual files and measurements. Confirm release source, runtime import/entry path, complete relevant test inventory, fixed baseline/reference integrity, raw results, numerical-gate applicability and outcomes (or justified alternatives), scoped WARN exceptions, both regression-budget checks, meaningful benefits, and final overall objectives. Missing or indirect evidence means that requirement remains unverified.

### Effective implementation change

For a counted release, compare the candidate with its accepted parent across the effective implementation: kernels, dispatch, memory/lifetime handling, and relevant build configuration, including non-Python sources. Require a meaningful change exercised through the public entry on supported inputs. Record the changed mechanism and its evidence. A different hash alone is insufficient: comments, formatting, renaming, reports, tests, metadata, and unreachable code cannot supply the required implementation change. Exact repackaging or reverting to an already delivered implementation does not produce another distinct release.

For an optimization target, require a demonstrated meaningful benefit in an objective designated by the contract, with every other key metric within its agreed parent and fixed-baseline budgets. Extra precision or a correctness repair is not automatically an eligible performance benefit. Every metric need not improve in every release, but none may exceed its permitted regression. Do not impose a universal minimum patch size or gain threshold; use the task's prospective usefulness criteria. Splitting a change into unqualified fragments cannot create releases.

### Promotion and completion checks

For a release-count campaign, use a small executable checker integrated with promotion and final acceptance. Reuse project formats with the required `trial_001` / `version_001` identity convention; a universal schema is not required. A command-line checker must exit nonzero for missing, malformed, stale, or inconsistent required evidence, exceeded regression budgets, missing meaningful benefits, or unmet endpoint objectives. Derive progress from inspected artifacts rather than a supplied count. Separate pre-activation, post-entry, and completion checks; only completion requires the full target count and all final endpoint objectives. Check:

- Baseline exclusion, unique release identities, expected parent/lineage, and source/build differences against parents and previously delivered implementations. Validate the documented effective-change and benefit evidence; hashes cannot establish semantic usefulness by themselves.
- Required trial/version naming, the fixed original baseline, and the frozen objective/budget definitions. Reject undeclared tradeoffs, replacement of the original baseline with a repair, and a qualifying benefit selected from an objective that the contract did not authorize.
- Every key metric's limits against both parent and original baseline, across the required workload inventory. Recompute outcomes from the measurements; reject out-of-budget cases even if another case or metric improves. Keep correctness-repair costs and optional accuracy gains separately attributed.
- Source, reference, protocol, evaluator, environment, case inventory, and raw-result identities for every required qualification claim. Check case identities and hard-gate outcomes, not just a successful process exit or a summary marked passed.
- Sealed source/evidence integrity and a successful actual-entry activation receipt for every counted release. Before activation the candidate remains pending; only the post-entry check can add it to completed progress.
- Resolved public-entry identity and required behavior in a fresh process. Testing a candidate by direct path does not prove the default import works. Final acceptance also checks that the current default matches the requested final selection.
- Eligible distinct release count against the requested target, with incomplete, failed, duplicate, and invalidated deliverables reported explicitly. Keep historical activations separate where a target requires currently usable versions.
- At completion, all final objectives and cost limits against the fixed starting baseline, using the direct endpoint measurements. Meeting the version count and collecting a complete comparison table are necessary but insufficient; unmet overall objectives must make completion fail.

Exercise a new or changed checker with meaningful passing and failing fixtures: more trials than releases, missing qualification evidence, unchanged or cosmetic-only candidates, a stale default entry, a changed sealed source, failed/pending entry verification, and repeated activation of one release. Confirm failed activation does not increase progress and recovery retains a verified entry. Mechanism usefulness still needs evidence-based review; do not claim a hash checker proves it.

Include objective-level fixtures: a precision gain that exceeds the latency budget; lower forward-live memory with an over-budget F+B peak; a parent-relative improvement that breaches the original-baseline limit; a correctness-only repair offered as a performance version; and enough otherwise qualified versions with a failing final objective. Each must fail the relevant promotion or completion check. Also include a useful change within all agreed budgets that passes, and reject alternate trial/version names for new records.

If the checker fails, keep the campaign incomplete and diagnose or continue within the authorized budget. A final report, directory named after the target, or metadata validator cannot override failed release acceptance.
