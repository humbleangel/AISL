# Swarm review v3 — new-observer (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3.

## Collisions resolved
- page-fault-witness vs oracle/pairing/inversion: single-fault verdict to ORACLE; semantics to PAIRING/INVERSION; OBSERVER keeps sequence witness.
- pmu-power-judge vs oracle/pairing: energy proof to ORACLE; power model to PAIRING; OBSERVER keeps PMU speculation-anomaly sampling.
- tpm-intent-quote vs oracle/substrate/pairing/abundance: DROPPED from observer; PCR comparison to ORACLE, chain to SUBSTRATE/ABUNDANCE.
- ring-minus-one-sentry vs inversion/substrate/scale: topology to INVERSION, EPT mechanism to SUBSTRATE; OBSERVER keeps tenant role only.
- nvme-append-trail vs substrate/oracle/time/scale: storage to SUBSTRATE, verdict to ORACLE, history to TIME/SCALE; OBSERVER owns append-only audit sink.
- sibling-core-dissenter vs organization/scarcity: policy/structure to ORGANIZATION; OBSERVER owns SMT-sibling lockstep-compare mechanism.
- intent-emit-comparator vs interface/failure/organization: translation to INTERFACE, merge gate to ORGANIZATION; OBSERVER owns post-signoff emit-vs-execute drift detection.

## New insights (holes no item owned)
- DMA watcher missing: NVMe/NIC are bus masters, EPT alone does not stop them — IOMMU/VT-d witness required.
- Control-register/MSR watcher missing: CR3/EFER/MSR/microcode writes silently repoint the trusted world.
- Resume/time-anchor watcher missing: S3/snapshot-resume tampering and TSC-vs-causal-tick drift (host time lie) unwatched.
- Lane-level watcher missing: AVX lane divergence needs its own comparator; core-level dissent is insufficient.

## Completed sublist v3
- intent-emit-comparator: detects drift between signed intent and actually emitted calls/pages after signoff.
- ring-minus-one-sentry: live host tenant watching guest kernel from VT-x host, consumes kernel-is-guest without owning EPT.
- pmu-speculation-judge: PMU/perf-counter anomaly detector for speculation/exfil behavior, energy budget ceded to oracle.
- page-fault-sequence-witness: logs fault stream as tamper-evident history, single-fault verdict ceded to oracle.
- nvme-append-trail: append-only operator/intent trail sink on NVMe, storage and powercut verdict ceded outward.
- sibling-core-dissenter: SMT-sibling lockstep re-execution comparator, structure/policy ceded outward.
- iommu-dma-witness: traps DMA writes outside allowed pages, the missing bus-master watcher.
