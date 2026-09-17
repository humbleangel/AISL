# Brutal combine v1 — new-pairing (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Round-2 reality check applied.

## Merges and kills
- cet-is-return KILLED outright as redundant (one #CP fault type, absorbed into EPT-CET cells + fault taxonomy).
- pku narrowed: intra-cap subgrant only, never outward gate (WRPKRU is unprivileged).
- kmask narrowed: predicate+scrub only (no per-lane faults; AVX512 not universal; throttle budgeted).
- PT/LBR narrowed: filtered budgeted witness (GB/s unfiltered explodes trace budget).
- RAPL narrowed: coarse lease + timer + quorum, never solo enforcer.
- IPI narrowed: fail-closed 2-dissenter ballot over pinned IR endpoints (storms + spoofing kill naive ballot).

## Final merged list (6 survive)
- tsc-delta-slice-only: TSC is interval inside one run, never time; all cross-core skew is dissent vote.
- pku-subgrant-inside-cap: PKU only revokes within an EPT cap arena; saves INVVPID, grants no security.
- kmask-select-scrub: k-masks are predicated select over counted lanes plus mandatory zero-scrub on exit.
- pt-lbr-bounded-witness: PT/LBR is filtered budgeted witness for replay/scrub, never full trace.
- joule-lease-dual-budget: RAPL is coarse lease paired with timer deadline and quorum throttle.
- ipi-pir-quorum-ballot: IPI+PIR is fail-closed 2-dissenter ballot over pinned IR endpoints only.
