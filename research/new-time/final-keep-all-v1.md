# Final keep-all v1 — new-time (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Fuses 7+6 twin pairs (3+4 merged 2-to-1 by design). Full pair mechanics + attacks saved.

## Round 1 — pair fusion
- resume pair → resume-restores-logical-maxtick-cow-only-not-wall: not-wall (rollback forgery) + logical/maxtick (Lamport max) + cow-only (no in-place snapshot mutation). Split rejected (atomic resume handler).
- tick pair → tick-on-send-only-no-shared-clock-now: only + no-clock-object + no-now-value; freeze-is-intentional; epoch = N sends OR M deadlines.
- tsc+dissent triple → lfenced-same-slice-tsc-delta-only-skew-as-dissent-not-fault: LFENCE+RDTSC fenced pairs, same-slice validity (APIC/RDPID/preemption/SMI_COUNT), skew→coalesced dissent (threshold + backoff), never fault/slash. Merge kept (split would let implementer panic by default).
- wall pair → outward-signed-wall-range-only: wall never inward; outward signed [earliest,latest] via epoch key (TPM anchors key, not per-message); diagnostic display labeled fiction.
- dual-budget pair → dual-budget-apic-deadline-timer-plus-rapl-joule-lease: APIC per-core re-armed + RAPL coarse lease with margin; either expiry suspends to dissent; local enforcement only; causality triangle (maxtick orders, TSC measures, budget bounds).
- epoch pair → sparse-tpm-epoch-hash-trail (identical strings): sparse anchor every K epochs (1024 sends / N deadlines tunable); digest = {maxtick head, wall range, dissent count, lease consumption, prev hash}; proves order/continuity/anti-rollback, never duration; resume verifies head before maxtick restore.

## Round 2 — attacks + fixes
- max(TSC) poisoning → Lamport only, TSC/wall feed dissent/range only.
- In-place snapshot increment → COW copy + seal + TLB/EPT flush.
- Stale wall after suspend → widen range, never shrink.
- Resume-vs-send incoherence → resume is join event (max+1), not send.
- Maxtick-for-timeout → forbidden; APIC bounds liveness.
- Sneaky `now()` global → compiler lint denies static NOW/raw rdtsc; capabilities only.
- Frozen time bug-claim → intentional, documented.
- Anchor livelock (no sends) → hybrid epoch (sends OR deadlines).
- Cross-core TSC compare → same-slice only, SMT off, no migration.
- LFENCE insufficiency → narrows error to theft; theft → dissent.
- Per-delta MSR cost → fast path + slow triage tiers.
- Dissent flood → threshold + coalesce + backoff.
- Double handling (dissent + lease) → dissent signals, lease enforces.
- TPM-per-message → epoch key signs, TPM anchors key.
- Wall for logs → allowed display, labeled fiction.
- TPM NV wear → sparse anchor + NVMe chain between.
- Resume-before-verify → order: TPM head → chain head → maxtick.
- Overall: no wall inward, no maxtick timeouts, no theft faults, no precise joules, no per-tick TPM. 6 minimal complete (resume, tick, measure, outward, bound, anchor).

## Final (6)
- resume-restores-logical-maxtick-cow-only-not-wall: resume restores causality via COW copy, never wall.
- tick-on-send-only-no-shared-clock-now: causality advances only on send; no shared clock object or now value.
- lfenced-same-slice-tsc-delta-only-skew-as-dissent-not-fault: fenced TSC pairs valid same-slice only; skew dissents, never faults.
- outward-signed-wall-range-only: wall emitted only as signed range outward; never inward, never a point.
- dual-budget-apic-deadline-timer-plus-rapl-joule-lease: per-core deadline stops hang, coarse joule lease stops burn; either suspends.
- sparse-tpm-epoch-hash-trail: sparse TPM-anchored hash-chained epoch digests; proves order/continuity, never duration.
