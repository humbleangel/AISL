# Final keep-all v1 — new-pairing (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (7) with brutal FINAL (6). Full reasoning saved.

## Round 1 — one by one
- tsc-is-delta KEEP: invariant TSC fixes frequency, not offset/desync/migration/VM-offset. Delta-only via LFENCE+RDTSC + AUX/seqno discard policy. Fused with brutal slice-only.
- pku-is-subgrant KEEP: WRPKRU unprivileged (SDM) — inside-cap bug-containment only, never outward gate; combine with separate address spaces/EPT for adversaries. Fused with brutal never-outward.
- cet-is-return KEEP — brutal kill REJECTED as over-aggressive: CET-SS/IBT + #CP vector 21 with distinguishing error codes is real; nothing else covers backward edge; optionality applies equally to PKU/AVX512/PT/RAPL. Resurrected with return-scope + #CP-decode + fallback requirement.
- kmask-is-select KEEP: k0 reserved, 7 usable; predicate-only, masked lanes suppress faults; compiler zeroing by default (merging needs live-old proof). Fused with brutal scrub.
- pt-lbr-is-witness KEEP: PT GB/s flood + ToPA overflow + LBR-32 overwrite → bounded truncated witness API returning (events, truncated:bool). Fused with brutal bound+policy.
- rapl-is-lease KEEP: model-based ms-granularity advisory with wrap; dual budget (cumulative joules + rate watts) with hysteresis. Fused with brutal dual-budget.
- ipi-pir-is-ballot KEEP: 256-bit PIR coalescing loses counts; set-bit ballot + quorum poll + re-arm loop with VT-d IR; PIR as wakeup hint, authoritative state in memory ballot. Fused with brutal quorum threshold.
- Brutal slice-only/subgrant/scrub/bounded/dual-budget/quorum entries: all FUSED as narrowings, none standalone-duplicate.

## Round 2 — attacks + fixes (hallucination verdicts)
- TSC globally synced? NO — desync/C-states/VM-offset documented; slice-only + AUX-discard stands.
- WRPKRU privileged? NO — unprivileged per SDM; never-outward stands; CFI/seccomp cannot fix, separate domains required.
- CET exists with #CP decode? YES — return-scoped, ENDBR/IBT out of scope noted, fallback required.
- k-mask per-lane faults? NO — predicate-only confirmed; zero-by-default stands.
- PT lossless? NO — GB/s flood confirmed; bounded+overflow-bit stands.
- RAPL precise meter? NO — advisory confirmed; dual-budget + wrap handling stands.
- PIR counted queue? NO — coalescing confirmed; hint+memory-ballot+quorum stands.
- Cross-coherence: 7 distinct HW units (time, rights, return, vector, witness, energy, agreement), no duplicates. Count 7 fits 6-10.

## Final (7)
- tsc-delta-slice-only: TSC desyncs/VM-offsets forbid wall time; AUX-checked delta inside slice only.
- pku-subgrant-inside-cap: WRPKRU unprivileged so PKU only subdivides inside held cap, never outward gate.
- cet-is-return: CET-SS hardware-checks returns via #CP; resurrected — no other entry covers backward edge.
- kmask-select-scrub: k-masks are fault-suppressing predicates; zero masked lanes to avoid leaks.
- pt-lbr-bounded-witness: PT/LBR floods and drops; usable only as bounded truncated witness with overflow flag.
- joule-lease-dual-budget: RAPL coarse advisory needs cumulative+rate dual lease with wrap handling, not precise meter.
- ipi-pir-quorum-ballot: posted-PIR 256-bit coalescing loses counts; PIR as hint plus memory ballot with quorum.
