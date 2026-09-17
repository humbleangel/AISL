# Brutal combine v1 — new-constraint (additive, non-destructive)

Refiner lineage (swarm-review-v2…v5) kept. This file force-merges against all 15 other v5s: fuse duplicates, kill weak entries. Round-2 reality check applied (MESI/SMI/RAPL/TSC/TPM honesty).

## Merges and kills
- Arena+caps merged (arena IS the cap system); RTC+no-share merged (RTC unsound with sharing).
- Nested-TLB/trace/quote sub-budgets killed as absorbed into the one lease.
- Honesty narrowing: "no-coherence" means no-shared-writable (MESI persists); SMI means detect-quarantine (cannot block); lease means estimate (RAPL is advisory); single-VA vs arenas resolved as one VA, many views; per-page TPM vs microsecond resume resolved as local hash + epoch anchor.

## Final merged list (5 survive)
- arena-cap-cow-append-only: explicit arenas are the only caps; no heap/fds/ambient/overwrite; persistence is COW append with endurance cap.
- single-va-no-share-rtc-only: one VA, no writable sharing/atomics/locks, SMT-off run-to-completion, tick-on-send only.
- measured-exec-fault-upcall-only: no unmeasured exec/link; faults/upcalls only crossings; CET+EPT+TPM pinned, diff-before-resume.
- smi-detect-quarantine-no-firmware-trust-only: firmware/ME/SMM untrusted; SMI/fault skew is dissent then quarantine; DMA/IR fenced.
- joule-tick-lease-deadline-only: dual timer+joule lease bounds all work; RAPL/PMU judge, wall is outward signed range only.
