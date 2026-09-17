# Swarm review v5 — new-observer (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 reality check.

## Round-2 kills
- Full-PT always-on cheap decode killed (PT floods cache; SMM/ME invisible; LBR only 32 entries).
- IOMMU content-snoop killed (fault-queue + remap faults only; successful DMA payload invisible without bounce-copy).
- Cross-core/SMT direct AVX-residue read killed (same-logical-core only; cross-core is timing/power side-channel).
- Per-module joule attribution killed (package-granular RAPL; cap/abort only).
- VMEXIT-storm sentry killed (poll EPT A/D bits instead).

## Final v5
- ept-ad-poll-sentry: polls EPT A/D bits per-core + VMFUNC EPTP switch; traps only on violation, no VMEXIT storm.
- ptwrite-filtered-trace-witness: per-core ToPA with IP-filter + PTWRITE intent markers + LBR-32 crosscheck; overflow is fault.
- joule-time-pmu-judge: PMU retired-vs-spec + package RAPL cap + TSC-delta skew detector; aborts on stolen-time, never attributes per-module joules.
- ipi-vector-dissent-mesh: dissent by x2APIC IPI vector number only, no shared memory; quorum is live-core vectors.
- iommu-fault-ir-witness: VT-d fault-queue + interrupt-remap faults only; payload snoop explicitly forbidden as fiction.
- endurance-capped-append-trail: NVMe append-only FUA pages with monotonic hash; write-cap aborts on endurance/flush-lie.
- same-core-avx-zero-sweeper: VZEROALL on every domain switch; witnesses only same-core residue, cross-core read killed.
