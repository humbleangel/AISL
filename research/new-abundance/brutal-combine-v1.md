# Brutal combine v1 — new-abundance (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Abundance owns cheap-supply patterns only.

## Merges and kills
- 100s-variant fanout killed (nested-walk storm): 2–8 live views + microsecond resume.
- Infinite NVMe append killed (endurance + flush-lie): DRAM COW abundant, durable commits leased/sparse.
- Idle/background folklore killed (no idle on loaded few cores): leased precompute + explicit reverify only.
- N-voter mesh killed for N>3: 1 proposer + max 2 dissenters, fail-closed.
- Infinite memo killed: per-core byte-capped shards + scrub.
- Free proof-check killed: every proof carries cycle/joule bound.
- explicit-reverify-on-use killed as standalone (mis-owned verification); folded as tripwire condition (Bun lesson).

## Final merged list (6 survive)
- resume-cow-arena-8max-capped: COW arena snap resumes cpu-state-only under explicit-alloc + INVVPID TLB charge; 8 EPTP ceiling.
- proof-or-fault-bounded-check-with-selective-reverify: check bounded in bytes-joules-cycles or trappable-fault; proposer never checks; reverify by tripwire-bind only.
- ipi-ballot-cheap-3live-fail-closed: cheap PIR ballot transport for ≤3 live voters; verdict owned by outward quorum; skew counts as dissent.
- per-core-memo-noshare-bytecapped: per-core memo hits only, no share/atomics/locks, byte-capped + AVX-scrubbed + code-follows-data.
- joule-leased-precompute-dual-budget: precompute inside RAPL lease + timer-joule dual budget on maxtick; void under degraded-physics quorum.
- dram-snap-cheap-commit-leased-endurance-capped: DRAM COW snap cheap; NVMe commit only as leased AES-COW append surviving flush-lie and torn-loss.
