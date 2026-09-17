# Final keep-all v1 — new-scale (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (8) with brutal FINAL (6). Scale owns numbers/endpoints only.

## Round 1 — one by one
- pinned-endpoints-one-to-n + brutal bounded → pinned-cap-endpoints-bounded: cap = min(512 EPTP, 4096 PCID, lanes, TLB/power/trace budgets); pinned = resident, verified by counting.
- lanes-counted-128 + brutal derated → lanes-counted-derated-ceiling: resident max 128 (64–128/socket) derated to sustained by PL1/PL2+Tau+Tjmax; per-socket counted via RAPL + thermal + sustained-frequency observation.
- ept-clone-fanout + brutal tlb-budget → ept-clone-invvpid-tlb-budget: per-clone switch 150–300ns + INVVPID 200–500ns (named type) + refill up to 24 refs/miss; fanout ≤512, PCID ≤4096.
- resume-us + brutal identical → resume-us-cpu-only: regs/EPTP/PCID/SIMD only, devices excluded (ms); TSC-measured, pinned, no device reinit on fast path.
- trace-bytes + brutal filtered → trace-bytes-per-lane-filtered: hardware PT filtering + per-lane budget fitting 4–64MB buffers; overflow policy (drop with counter, never silent corrupt).
- joules + brutal dual → joules-rapl-dual-budget-ceiling: joules over window + watts with Tau + Tjmax distance; throttle/reduce on exceed.
- one-core-degraded-proof: KILLED-AS-PROOF, CANDIDATE demo — oracle owns correctness proof; scale can own degraded numbers demo.
- fleet-quorum-not-identical: KILLED-AS-QUORUM-PROOF, CANDIDATE count — oracle owns agreement proof; scale can own heterogeneous endpoint math.

## Round 2 — attacks + fixes (fantasy numbers? counters? sense? coherence?)
- Bounded-without-formula → min-formula + resident verification added.
- 128-always → per-socket + derate method added.
- Free fanout → INVVPID type + TLB-miss counters + PCID budget added.
- Full-system us resume → cpu-only scope + TSC method added.
- Full trace at scale → filter + per-lane budget + drop-counter added.
- Joules-alone → dual budget + throttle action added.
- Degraded/full-fleet fantasy → degraded numbers required; identical-fleet forbidden; disclaimers added.

## Resurrection decisions (explicit, mandated)
- one-core-degraded-scale-demo RESURRECTED: oracle proves degraded safe; scale demonstrates degraded bounded (lanes, resume, trace, joules, endpoint cap on 1 core). Without it scale implies bounds hold only at full socket. Renamed proof→demo with disclaimer.
- fleet-heterogeneous-endpoint-count RESURRECTED: oracle proves quorum agreement; scale supplies heterogeneous per-node caps summed (never N×128). Without it identical-fleet fantasy stands with no owner. Renamed quorum→count with disclaimer.

## Final (8)
- pinned-cap-endpoints-bounded: one-to-n is explicit EPT-cap table with hard fanout cap, not names.
- lanes-counted-derated-ceiling: resident ceiling max 128 derated to sustained by PL1-PL2-Tau-Tjmax, per-socket counted.
- ept-clone-invvpid-tlb-budget: each clone priced in INVVPID-ns plus refill refs against nested-TLB budget.
- resume-us-cpu-only: resume microseconds for CPU state only; devices excluded.
- trace-bytes-per-lane-filtered: PT worst-case contained by hardware filtering + per-lane budget fitting 4–64MB buffers.
- joules-rapl-dual-budget-ceiling: scale limited by dual RAPL ceiling (joules + watts/Tau/Tjmax).
- one-core-degraded-scale-demo: scale demonstrator (not oracle proof) showing bounds still hold degraded on one core.
- fleet-heterogeneous-endpoint-count: scale count (not quorum proof) summing per-node derated caps; never assume identical fleet.
