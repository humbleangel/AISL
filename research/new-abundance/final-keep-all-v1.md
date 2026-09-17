# Final keep-all v1 — new-abundance (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (7) with brutal FINAL (6). Abundance owns cheap-supply patterns only.

## Round 1 — one by one
- resume-cow-arena-8views + brutal 8max-capped → fused resume-cow-arena-8views-max-capped (views + max + cap; memory cap + view-count cap; fail/collapse-oldest on exceed).
- proof-or-fault-bounded-check + brutal with-selective-reverify → fused (bounded + conditional + fail-closed; proof carries cycle/joule budgets; reverify predicate explicit).
- two-dissenter-ipi-vote + brutal 3live-fail-closed → fused ipi-ballot-cheap-3live-two-dissenter-fail-closed (threshold + live bound + fail-closed; rare control-plane only).
- per-core-memo-no-share + brutal bytecapped → fused (noshare avoids coherence; byte cap avoids OOM; lookup is cheap generation-compare, never full reverify).
- joule-leased-precompute + brutal dual-budget → fused (cycle+joule; droppable, never critical path; writes only per-core memo).
- dram-snap-cheap-nvme-commit-leased + brutal endurance-capped → fused dram-snap-cheap-nvme-commit-leased-endurance-capped (snap cheap; commit leased/batched; flush-lie aware; keep nvme explicit).
- explicit-reverify-on-use: DO NOT RESURRECT STANDALONE — category error (verification tax as supply), duplicate scope (unconditional vs selective predicates would diverge), storm risk on loaded cores. Preserved as folded condition inside proof entry. Full reasons recorded; expand IU-2 predicate fields instead if underspecified later.

## Round 2 — attacks + fixes (false abundance? executability? sense? coherence?)
- COW resume cheap until write-fault storm/TLB shootdown/memory blowup → capped views + memory cap + collapse-oldest; arena vs snap distinguished (execution resume vs data persistence).
- Proof-check DoS via expensive proof → bounded budgets + fault on exceed + fail-closed; never store in proposer.
- IPI cheap for N voters → ≤3 live, rare control-plane, fail-closed; per-use IPI forbidden (memo stays noshare).
- Memo infinite hits → byte cap + LRU/ring + scrub; duplication accepted under 128 ceiling.
- Precompute saves joules → dual-budget + lease expiry + droppable; RAPL lease + maxtick, degraded-proof path.
- NVMe commit cheap → leased/batched/endurance-capped + flush-lie fault handling; snap path never blocks on NVMe.
- Cross-coherence: no two entries claim same supply (views, memo, precompute, snaps distinct); gate/vote/lease conditions reference outward owners. 6 within 6-10.

## Final (6)
- resume-cow-arena-8views-max-capped: COW resume abundant only with ≤8 EPTP views and memory cap, else TLB/shootdown lie.
- proof-or-fault-bounded-check-with-selective-reverify: abundant supply trustworthy only with cycle/joule-bounded check + conditional reverify, fail-closed.
- ipi-ballot-cheap-3live-two-dissenter-fail-closed: IPI ballot cheap only as rare ≤3-live ballot with two-dissenter-to-fault and fail-closed.
- per-core-memo-noshare-bytecapped: memo hits abundant only per-core with no sharing and byte cap.
- joule-leased-precompute-dual-budget: precompute abundant only as droppable lease enforced against cycle + joule budgets.
- dram-snap-cheap-nvme-commit-leased-endurance-capped: DRAM snap cheap, NVMe commit leased/batched/endurance-capped, flush-lie aware.
