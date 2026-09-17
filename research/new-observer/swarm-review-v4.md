# Swarm review v4 — new-observer (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (EPT sentry, PT/LBR witness, energy judge, IPI dissent, DMA witness, capped trail, vector sweeper) attacked in round 2 for over-trust (EPT/PT/PMU lie under firmware betrayal + thermal PT drops), non-independent dissent (same package shares power/cache/speculation), unbounded trail, missing AVX observer, missing degraded-core observer, witness overload vs review bandwidth.

## Round-1 → Round-2 changes
- Generic sentry replaced with EPT-module trap (no VMEXIT storm).
- PMU+RAPL+TSC merged into one energy-time judge.
- Dissent forced over IPI mesh, never shared memory.
- Trail capped by endurance; vector-residue sweeper made first-class.

## Final v4
- ept-trap-sentry: observes only via ept-is-module violations, no VMEXIT storm, respects privilege-crossing-budget.
- pt-lbr-sequence-witness: replaces software comparator with CPU's own PT/LBR trace as fault+branch sequence identity.
- energy-time-judge: merges PMU+RAPL+TSC/APIC into one bounded-energy-time verdict: speculation, joules, deadline together.
- ipi-dissent-mesh: sibling dissent only via interrupt-is-send IPI mailbox, never coherent shared memory.
- iommu-dma-witness: all DMA/NVMe traffic observed at iommu-dma-firewall; CPU trace alone is blind.
- crash-trail-cap: nvme-append-trail bounded by persistent-commit-endurance + powercut-recovery-verdict, content-addressed not log-file.
- vector-residue-sweeper: AVX/SIMD register silence witness on every run-to-completion switch, enforces residue-silence-verdict.
