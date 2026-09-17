# Swarm review v5 — new-scarcity (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 quantitative audit.

## Round-2 verdicts (REAL unless noted)
- STLB ~1536–2048 entries + EPT multiplier up to ~24 refs: REAL. PCID-churn real but small (additive, not equal to walks).
- NAND P/E ~1k QLC / 3k TLC: REAL (endurance is scarcity; QD depth is abundance).
- RAPL ~15uJ units, ~1ms update, PKG scope: REAL WITH CAVEAT (lease/epoch budgets, never per-instruction).
- TPM2 quote ~20–100ms, ~10–50 quotes/s: REAL (bandwidth, not per-tick).
- AVX downclock 100–300MHz transient: REAL (rate-limit, not ban).
- PT up to GB/s/core unfiltered, LBR 32 entries: REAL (unbounded trace is hallucinated abundance).
- TSC desync as clock drift: WEAK — real scarcity is stolen/SMI time + VM-exit skew.
- Privilege-crossing scarcity: CORRECTLY KILLED (syscall ~100ns trivial vs EPT-miss storm).
- Determinism scarcity: KILLED as invented (determinism is design; cost-of-proof is the scarcity).

## Final v5
- review-attention-bandwidth: only human dissent scales safety; all else is machine pre-filter.
- joule-thermal-headroom: RAPL joules + AVX downclock are the hard scale limit, not cores.
- nested-ept-tlb-budget: STLB misses x EPT-multiplier + VPID flush is the per-send tax.
- durable-commit-endurance: P/E cycles + flush-lie make every persist a spent coin.
- quote-attestation-bandwidth: TPM ms-quotes + pinned-microcode measure gate what may execute.
- trace-replay-bandwidth: PT/LBR bytes + dissent-replay prove determinism; unbounded trace is fiction.
- scrub-containment-residue: LLC-ways + TLB + AVX/MDS buffers must be scrubbed on switch; residency is paid in cycles.
