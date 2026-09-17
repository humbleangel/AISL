# Brutal combine v1 — new-scale (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Scale owns numbers/endpoints only.

## Merges and kills
- one-core-degraded-proof KILLED as scale-owned (oracle degraded-physics-quorum + failure exhaustion own proof; scale consumes derated numbers).
- fleet-quorum-not-identical KILLED as scale-owned (outward quorum gates own it; scale keeps endpoint set).
- 128 fixed killed (64–128 + PL1/PL2+Tau+Tjmax derate); unbounded one-to-n killed (EPTP-512/PCID-4096/8-views caps); flat INVVPID cost killed (+refill refs); us-resume qualified (cpu-only); PT-always-on killed (4–64MB buffers); RAPL-exact killed (lease + lag).

## Final merged list (6 survive)
- pinned-cap-endpoints-bounded: one-to-n is explicit EPT-cap table with hard fanout cap, not names.
- lanes-counted-derated-ceiling: lanes counted at resume, min-measured-128 then PL1-Tau-Tjmax derated.
- ept-clone-invvpid-tlb-budget: each clone priced in INVVPID-ns plus refill refs against nested-TLB budget.
- resume-us-cpu-only: resume restores cpu state in us-TSC, devices/SMI excluded by construction.
- trace-bytes-per-lane-filtered: scale limited by PTWRITE-filtered bytes-per-lane against 4–64MB buffers.
- joules-rapl-dual-budget-ceiling: sustained scale capped by RAPL-lease plus timer, judged by PMU.
