# Minimal campaign records

Prefer the project's existing record formats, with the required trial/version identities below. Plain Markdown, JSON, diffs, and logs are sufficient; no database or prescribed enclosing directory layout is required.

## Task contract

Record the requested endpoint, fixed starting baseline and reference identities, public semantics, supported domain, implementation constraints, primary objectives, hard gates, benchmark/confirmation method, available resources, and stop conditions. At setup, establish release tradeoffs using existing user instructions. Distinguish initial assumptions and unresolved limits from agreed permissions and verified facts; unresolved release tradeoffs do not block exploratory trials within existing authorization and resources. Record any subsequent protocol change with its reason and impact; do not grant a new release tradeoff budget after observing a candidate's failure.

For each key metric in release acceptance, record its unit, direction of improvement, measurement phase, required cases, meaningful-gain threshold, maximum tolerated regression against the release comparison parent, maximum cumulative regression against the fixed starting baseline, and final endpoint target. Define the handling of uncertainty and any aggregate weights/per-case limits. Every other metric's allowed cost must be specified when a gain in one objective can justify a release tradeoff; an unspecified cost allowance is not unlimited. Separate required accuracy from an independent accuracy objective and its explicit latency/memory budgets. Keep actual resource limits and any user-specified trial limits distinct from release regression budgets.

Record correctness, determinism, and batch-invariance checks with their observable scope. Keep correctness repairs in a separate ledger identifying the original defect, repair trial, validation, costs versus the fixed starting baseline, and whether the full release predicate permits counting it. A repair may establish a source parent; preserve the original baseline identity and its measurements throughout the campaign.

Before confirmation, record the chosen numerical validation and affected output/gradient coverage, including applicability of project-specific profiles. Any inapplicability or equivalent validation needs its rationale and evidence; an omitted decision is not an exemption.

For a version target, record the following information using the project's existing format:

| Field | Meaning |
|---|---|
| Starting point and `target_release_count` | Required additional qualified releases; exclude the starting baseline. Map an endpoint label to this count explicitly. |
| Comparison identities | Preserve source ancestry separately from the release comparison chain and fixed starting baseline. The release comparison parent is the previous qualified delivery, or the first release's task-agreed comparator. Count separate release branches only if requested; retain donor trials. |
| Acceptance rules | Declared meaningful benefits, required evidence, invariant/numerical gates, both regression budgets for every key metric, and separate repair accounting. Correctness alone does not earn a performance version. |
| Overall objectives | Explicit final targets against the fixed starting baseline. Meeting the version count does not establish that these targets passed. |
| Default entry and final selection | The authorized import/call path to activate and the user's requested final version; normally the final accepted successor unless specified otherwise. |
| Resource limits and stop conditions | Limits independent of trial count; do not invent a trial cap from the release target. |

## Identities and naming

Use `trial_001`, `trial_002`, and so on for trials, and `version_001`, `version_002`, and so on for releases. These are the only forms for new trial/release directory basenames, metadata IDs, report labels, and continuation records. Use independently increasing positive integers, zero-padded to three digits below 1000 and written in full thereafter. Do not use abbreviations, suffix variants, or alternative names. The starting baseline is not a counted version.

Keep execution identities distinct: a rerun of unchanged source gets a new execution identity associated with its existing trial, never another trial/version count. Changes to a frozen implementation require a new trial ID. Never reuse an identity for different sealed content. A reserved version ID in pending or failed activation state does not advance qualified-release progress; derive that count from verified releases, not the largest numeric suffix. Display release progress separately from trials attempted and executions performed.

When resuming historical campaigns, retain existing immutable source/evidence paths and their recorded identities. Apply this convention to all newly allocated trial/version identities; do not rename sealed historical artifacts to satisfy the new naming convention.

## Trial record

Keep the `trial_001`-style identifier, source parent, current release comparison parent, fixed starting baseline, short research question, and changed files/patch or snapshot. Record donor mechanisms when used. Do not require each trial to predict benefits for every objective, improve an unmet metric, or complete the release budget/evidence record. Unknown effects remain unknown. Each execution records source identity, exact command, environment, actual input/seed scope, start/end or live handle, log/result paths, and observations. Identify comparison roles wherever measurements are available; source-parent diagnostics are separate from release and endpoint verdicts.

Use whatever state labels are clear, but distinguish untested, screen-passed, qualified, sealed, activation-pending, and released. Infrastructure failures, implementation failures, and inconclusive measurements should remain distinguishable. Record a failure's scope without treating release rejection as abandonment of its source or mechanisms. A repair or rerun adds evidence instead of replacing the earlier failure; changes to a frozen failed prototype get a new trial identity with that prototype as source parent. Research findings and retained alternatives do not add releases until they complete the requested release process.

## Release record and activation

Bind each `version_001`-style release to its source parent, release comparison parent, fixed starting baseline, contributing trial IDs, effective implementation diff and mechanism, declared meaningful benefit, every release-comparison-parent and fixed-baseline budget verdict, source/build manifest, protocol/reference/evaluator identities, required raw evidence, and actual-entry verification receipt. The source parent may be a failed or unreleased trial; preserve its ancestry without substituting it for the release comparison parent. Attribute repairs and extra accuracy separately from performance gains. Keep the qualification snapshot and its evidence immutable; append activation or invalidation records referencing that snapshot rather than editing sealed implementation or replacing prior results. Field names and enclosing storage layout may vary.

Record full numerical qualification evidence for the chosen method here, linking any earlier diagnostic executions rather than requiring every trial to run it. For the [Apodex profile](evaluation.md#apodex-profile), include the alignment/accuracy objective; candidate, working-precision reference, and golden identities and golden-validation evidence; evaluator version/configuration; input distribution and seed scope, comparison dtype, and bucket definition; effect-size/rounding floors, thresholds, sampling budget, and stopping rule; actual replicate count, stop reason, separate magnitude and drift results, and fit/fallback status. Whenever these gates are run, preserve raw per-replicate measurements in immutable execution evidence, even if the evaluator overwrites a latest-results summary. Link validation controls and any WARN exception to its diagnosed cause, acceptance rationale, applicable scope, and the other gate's passing evidence; an exception cannot waive an existing hard constraint or carry over without candidate-specific revalidation.

Use the following transitions, or equivalent project states:

1. **Qualified to sealed:** verify the conjunctive release predicate and required evidence against the frozen candidate, then preserve the source and evidence manifest under a unique pending `version_001`-style identity. A pending attempt does not advance the qualified-release count.
2. **Sealed to activation-pending:** record the previous verified entry and expected release comparison parent, check they are still current, and switch the task's default entry atomically or with the project's equivalent recoverable operation. Serialize promotions or check the accepted release and current entry identities at the switch to avoid replacing a newer release.
3. **Activation-pending to released:** in a fresh process, load the normal public entry, record its resolved implementation/build identity, and run the contract's entry checks. Verify that the entry still selects this snapshot before recording the successful activation receipt and committing release progress.
4. **Failed or interrupted activation:** retain logs and pending state. On failure restore the prior verified entry only if the default still points to this pending candidate. On resume inspect the actual pointer, receipts, and job state before finishing or rolling back; never infer release success from a switched pointer or a completed candidate test alone.

Once released, a version retains its historical activation receipt when a successor becomes current; it need not remain the active default to count. Rollback or reactivation of an existing release is an activation event, not another new release. Keep historical releases and currently eligible deliverables distinguishable. If a later finding invalidates a release's hard gates, append the finding, exclude it from a target requiring usable versions, and report the shortfall. Do not erase history or reuse its identity to hide the invalidation.

## Trial alternatives and composition

Keep alternatives and useful mechanisms from failed trials with their known tradeoffs and limitations. If composing, record the source parent, donors, application order, conflict decisions, and final tested snapshot. When the accepted release chain changes, refresh the release comparison parent and affected qualification comparisons before activation; a new comparison identity alone does not require a source rebase. Verify that activation does not overwrite an unexpected newer release.

## Continuation

Record the requested endpoint, achieved/required eligible release count, historical release count if different, all three comparison identities, actual default entry, canonical trial/version IDs, and pending qualification/sealing/activation stage. Include available release-comparison-parent and fixed-baseline metric results, remaining release regression budgets, separately accounted repair costs, and unmet final objectives. Include launched task handles and their source snapshots, resource ownership, unresolved errors, and the next useful research action. Unmet objectives inform prioritization without requiring every next trial to improve them directly. Recompute progress from release artifacts on resume and reconcile it with the actual entry; counters are summaries, not authority. A stale lock or an old log line is not a live-process check. Never restart an uncertain job solely because a monitoring call timed out.

When an actual resource, permission, external-dependency, user, or platform boundary prevents further work, record the observed condition and evidence, its impact, and what would allow resumption. This is a factual continuation record, not proof that all algorithms or directions were exhausted. Candidate rejection or difficult research alone does not establish such a boundary. Record the achieved/required count and unmet overall objectives without changing the endpoint; even a sufficient count cannot compensate for failing final targets. Preserve the last verified entry and enough evidence to continue the same campaign.

## Skill effectiveness report

Link real actions and artifacts to the workflow's phases. Preserve whether the skill was read before the work and any changes made to the skill during the campaign. Identify departures and why they were appropriate. Reading a skill or passing its metadata validator does not by itself demonstrate behavioral adoption.

Report kernel gains with actual endpoint measurements; report transfer with a separate kernel/task that used the same general instructions. Keep task-specific constants and DSA history outside the general skill. Do not call a historically informed replay blind discovery. Do not infer causal improvement from an uncontrolled before/after run.
