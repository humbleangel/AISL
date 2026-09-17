# Swarm review v2 — new-oracle (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- reproducible-boot-proof: confuses determinism with correctness — identically reproducible boot can be identically backdoored.
- diverse-double-compile: not new (Thompson 1984 / Wheeler DDC), and vacuous on x86 unless microcode + seed assembler trusted.
- contracts-as-spec: circular — spec written in same language as impl, no judge for the judge.
- fault-injection-behavior: a test method, not an oracle — no pass/fail predicate stated.
- budget-proof-memory-time: unverifiable static fiction on shared caches/SMT/SMM/thermal throttling.
- trace-equivalence: assumes linear retired-instruction trace is truth, ignores interrupts/paging/speculation/DMA; overlaps reproducible-boot.

## Completed sublist v2
- hand-seed-double-compile: only hand-written machine-code seed is trusted, DDC loop makes Thompson-trojan absence mechanically checkable.
- tpm-pcr-verdict: TPM-quoted PCRs decide correctness, metal attests boot/runtime instead of log comparison.
- mmu-fault-verdict: paging+rings decide correctness — illegal intent must trap as #PF/#GP, hardware is the assert engine.
- vtx-replay-identity: VT-x/EPT record-replay decides correctness as bit-identical guest state across replay, no heisen-excuse.
- cache-silence-verdict: perf-counter/L1/TLB footprint decides correctness — functionally right but secret-dependent caching is wrong.
- rapl-energy-budget-proof: RAPL/TSC/page-count attestation decides correctness on cycles/joules/pages, not static WCET proof.
- nvme-powercut-recovery: abrupt-powercut + NVMe remount decides correctness as recoverable state, not clean-shutdown tests.
