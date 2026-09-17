# Final keep-all v1 — new-observer (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (7) with brutal FINAL (6). Thompson: observer in TCB, must use hardware truth, never guest printf. Don't fight CPU.

## Round 1 — one by one (all 7 v5 survive, all 6 brutal fuse, zero drops)
- ept-ad-poll-sentry → fused ept-ad-poll-bind-sentry: HW-set A/D without exits; batched+bind, coarse granularity documented (large pages), race-tolerated epochs, adapt cadence.
- ptwrite-filtered-trace-witness → fused bounded-ptwrite-filtered-witness: RTIT-gated, IP/CR3 filters, ToPA caps, overflow-as-fault, per-CPU lock-free; witness (markers) not reconstruction; per-lane TSC correlation only.
- joule-time-pmu-lease-judge kept: package-RAPL estimator + PMU/TSC deltas as conservative fail-closed lease judge, never billing meter; split measurement (judge) from decision (ballot).
- ipi-vector-dissent-ballot-mesh kept: stewarded 8-bit vectors, dissent-only, timeout fail-closed, rate-limited; ballot epoch shared with lease judge.
- iommu-fault-ir-firewall-tripwire: fault-only VT-d/IR (no snoop — guard retained to prevent drift); fault-log drain, quarantine on overflow/unknown.
- endurance-capped-cow-append-trail: observer-owned EPT-protected pages, capped/rotated/GC-budgeted, checksum-chained, fail-closed when full.
- same-core-avx-zero-scrubber: conditional (XSTATE_BV-gated) VZEROUPPER/ALL/XRSTOR-zero on observer-domain entry/exit; same-core only; SMM opacity assumed, residency minimized.

## Round 2 — attacks + fixes
- A/D sees all accesses? No — large-page coarse; batch+epoch tolerance documented.
- PTWRITE always/lossless? No — CPUID-gated, bounded, overflow flag, fail-closed gap.
- Per-task joule metering? No — package estimator + lease semantics.
- Unlimited IPI vectors/reliable delivery? No — reservation table, dissent-only, offline mask, timeout fail-closed.
- IOMMU content snoop? No — faults only; retained guard phrase.
- Infinite trail? No — capped/rotated/GC + tamper-evident chain + protected pages.
- Cross-core AVX scrub helps? No — same-core only retained; SMM opacity noted.
- No drops from attacks; all fixes are scoping/bounding, never removal.

## Final (7)
- ept-ad-poll-bind-sentry: polls HW-set EPT A/D in batches with no traps to watch bound pages without VMEXIT storms.
- bounded-ptwrite-filtered-witness: filtered PTWRITE markers into per-core bounded buffers as loss-aware witness.
- joule-time-pmu-lease-judge: estimates package-RAPL plus PMU/TSC deltas as conservative fail-closed lease judge.
- ipi-vector-dissent-ballot-mesh: one stewarded 8-bit IPI vector set for silent-agree dissent-only ballots, timeout fail-closed.
- iommu-fault-ir-firewall-tripwire: fault-only VT-d/IR faults as fail-closed firewall tripwire with no snoop claim.
- endurance-capped-cow-append-trail: capped CoW append log with rotation; respects endurance and crash consistency.
- same-core-avx-zero-scrubber: conditionally zeroes AVX/YMM/ZMM where residue lives on same core before handoff.
