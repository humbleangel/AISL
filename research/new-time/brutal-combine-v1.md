# Brutal combine v1 — new-time (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s.

## Merges and kills
- No KILLs; all 7 survive as fused (each already split cleanly across time/substrate/oracle/observer/scarcity owners in v4).
- SMI-as-fault revised to dissent (untrappable — inference only).
- TPM-per-tick revised to sparse anchor (ms latency + wear).
- Wall-instant revised to signed range (virtualizable, desynced).
- Deadline-instant revised to joule-budget handler (thermal stretch).

## Final merged list (6 survive, fused)
- resume-restores-logical-maxtick-cow-only: resume restores versioned logical tick from COW arena, never TSC/wall.
- tick-on-send-no-shared-now: only send advances time, per-core memo, no global clock/coherence.
- lfenced-same-slice-tsc-delta-with-skew-as-dissent: LFENCED delta same pinned slice only; AVX/SMI/NMI skew goes to IPI dissent vote/replay.
- outward-signed-wall-range-only: wall banned for ordering; outward quorum-signed range with fail-closed gate.
- dual-budget-apic-deadline-plus-rapl-lease: APIC TSC-deadline bounds hang, conservative RAPL joule lease bounds burn.
- sparse-tpm-epoch-hash-trail: cheap DRAM hash trail, sparse TPM anchor/quote to survive NV slowness and endurance.
