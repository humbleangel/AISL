# Swarm review v3 — new-scarcity (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. SCARCITY owns what-is-scarce; mechanisms/proofs/threats ceded outward.

## Collisions resolved
- bootstrap-trust: DROPPED from scarcity (trust is property/threat, not scarce good); owned by ORACLE+SUBSTRATE (mechanism/proof) and FAILURE (threat).
- joule-thermal-headroom: SCARCITY owns the scarce good; others own mechanism/proof/use. Kept, scoped.
- determinism-budget: TIME owns clocks/replay mechanism; SCARCITY owns predictability-as-budget. Kept with that split.
- shared-cache-residency: SUBSTRATE owns mesh HW; CONSTRAINT/SUBTRACTION own prohibition; SCARCITY owns residency-as-budget. Kept.
- speculation-secrecy: FAILURE owns leak threat; PAIRING owns effect semantics; ORACLE/OBSERVER own verdict/witness; SCARCITY owns secrecy-as-budget. Kept.
- privilege-crossing-budget: SUBTRACTION/CONSTRAINT own removal; PAIRING/INVERSION own new mechanism; SCARCITY owns residual crossing cost. Kept.
- human-review-bandwidth: canonical scarce good (Bun lesson); ORGANIZATION owns spending process; INTERFACE owns spending efficiency. Kept in scarcity.

## New insights (unnamed scarcities)
- TLB reach scarcer than pages: ~2k TLB entries + up to 24 nested EPT refs per miss bound every domain split.
- Persistent commits not free: NVMe P/E cycles, queue depth, powercut-atomic window bound the persistent heap.
- Quote/measurement bandwidth tiny: TPM signs in ms, PCRs few, attestation log bounded; what gets measured must be budgeted.
- Exits/shootdowns are mesh round-trips: VMEXIT, EPT violation, TLB-shootdown IPI, fault-continuation burn core-mesh/DRAM trips.
- AVX steals GHz not just joules: 512-bit lane downclocks whole package via RAPL license; frequency-headroom distinct from energy.
- Honest trace self-perturbs: PMU/LBR/PT ports + drain bandwidth compete with measured workload.

## Completed sublist v3
- human-review-bandwidth: scarcest good per Bun lesson, canonical definition all gating/spending processes point to.
- joule-thermal-headroom: joules + AVX downclock headroom bound all-cores compute; includes frequency-license slice.
- determinism-budget: timing-predictability as spendable budget, distinct from time's clocks/replay mechanisms.
- shared-cache-residency: L1/L2/LLC occupancy + DRAM bandwidth per pinned core, the price of lane/thread placement.
- speculation-secrecy: secret-keeping under SMT/speculation as budget, distinct from leak threat and silence verdict.
- privilege-crossing-budget: residual fault/VMEXIT/shootdown cost after syscall boundary removal.
- tlb-reach-walk-budget: TLB entries + nested MMU/EPT walk cycles, the hidden tax on pages-as-types and domain splits.
- persistent-commit-endurance: NVMe P/E cycles + atomic-commit rate, the finite price of crash-only persistent heap.
