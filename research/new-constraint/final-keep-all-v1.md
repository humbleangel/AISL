# Final keep-all v1 — new-constraint (additive, non-destructive)

v1 folders + swarm-review-v2…v5 + brutal-combine-v1 kept. This file unions v5 FINAL (8) with brutal-combine-v1 FINAL (5). Mandate: keep everything possible; drop only proven duplicates. Full reasoning below is the saved memory.

## Round 1 — every entry judged
- explicit-arena-alloc-only KEEP: software discipline (bulk lifetimes, page-table reclaim), no new MMU claimed. Executable: arena vs malloc microbench, PMU TLB trend, no IPI on free.
- capability-no-ambient-only KEEP: discipline + enforcement assist (paging/EPT/MPK/CET), not CHERI fiction. Executable: deputy test, use-without-cap faults.
- single-va-no-coherence-only KEEP WITH FIX: single-VA real (one page table, PCID stays); literal "no-coherence" is hallucination (MESI/snoop persist). Fixed to no-share: sharded per-core + message passing, HITM/shootdown counters prove it.
- run-to-completion-smt-off-only KEEP: pinned bounded RTC, SMT off via BIOS/MSR; NMI/SMI/thermal still preempt → paired with SMI quarantine. Executable: nosmt boot, pin, blocking-call audit, jitter histogram.
- crash-only-cow-append-only KEEP: no graceful shutdown, only crash+recovery; CoW+append with CRC+epoch+FUA beats flush-lie; bounded GC documented. Executable: power-pull recovery test.
- measured-pages-execute-anchored-only KEEP WITH REFINEMENT: hash at load, EPT NX + CET enforcement, TPM async anchor (ms latency → never per-exec sync). Fused with brutal fault-upcall: violations become bounded re-entrant upcalls.
- smi-is-fault-no-firmware-trust-only KEEP WITH FIX: SMI cannot be blocked/trapped precisely; best-effort TSC-gap detect + epoch/lease quarantine, false positives accepted. Fused with brutal detect-quarantine wording.
- joule-deadline-bounded-only KEEP WITH REFINEMENT: RAPL advisory + hard tick-deadline kill; TSC needs sync discipline. Fused with brutal tick-lease (APIC IPI ok few cores).
- Brutal arena-cap-cow-append-only: PROVEN CONJUNCTION-DUPLICATE — drop name, keep coverage (testability wants allocator/authority/persistence separate).
- Brutal single-va-no-share-rtc-only: PROVEN DUPLICATE with honesty fix — drop name, fold no-share wording into single-va entry.
- Brutal measured-exec-fault-upcall-only: REFINEMENT — fuse (bounded re-entrant upcall path).
- Brutal smi-detect-quarantine: REFINEMENT — fuse (detect + quarantine, SMI undetectable-block noted).
- Brutal joule-tick-lease: REFINEMENT — fuse (ticks + revocable leases).

## Round 2 — attacks + fixes
- Arena hardware fiction? No — worded as discipline. PASS.
- Caps fiction (MPK≠caps)? Fixed: software caps + gate trampoline + binary scan. PASS.
- Single-VA-no-share fiction? Fixed to no-share while MESI persists; KASLR/PCID feasible. PASS.
- RTC absolute? Fixed: discipline + SMT off + SMI quarantine + leases expire via bounded upcall. PASS.
- Crash-CoW vs flush-lie/GC? Fixed: CRC+epoch+FUA, bounded GC, wear noted. PASS.
- Measured-upcall vs RTC? Fixed: measure-at-load, NX/EPT/CET enforce, async attest, bounded upcall; AVX masks irrelevant. PASS.
- SMI detect/block fiction? Fixed: best-effort timing detect, quarantine only, TSC sync required. PARTIAL but honest. PASS.
- Joule billing fiction? Fixed: RAPL advisory + hard tick deadline. IPI lease ok few cores. PASS.
- Coherence: arena-bulk vs CoW-GC, single-VA vs caps (MPK/EPT not tables), RTC vs upcall/lease/deadline — all resolved via bounded upcalls, tick leases, bounded GC. SMI breaks RTC → quarantine restores epoch. PASS.
- Count: 13 names → 8 after proven conjunction-duplicates fused. No genuine insight lost. PASS.

## Final (8)
- explicit-arena-alloc-only: all memory from explicit arenas with bulk lifetime; keeps TLB/PCID locality, avoids per-object shootdowns.
- capability-no-ambient-only: all authority via unforgeable software caps enforced by paging/EPT/MPK/CET; no ambient uid/root/env.
- single-va-no-share-only: one VA to avoid flush/IPI plus no mutable sharing so MESI persists but stays idle.
- run-to-completion-smt-off-only: pinned bounded RTC tasks with SMT off; kills speculation leakage and scheduler jitter.
- crash-only-cow-append-only: crash-only recovery over CoW append with CRC/epoch; survives NVMe flush-lie, spreads P/E wear.
- measured-exec-fault-upcall-only: only load-measured pages executable via NX/EPT/CET with async TPM anchor; violations as bounded upcalls.
- smi-detect-quarantine-no-firmware-trust-only: trust no SMM/ME/microcode; best-effort TSC-gap detect with epoch/lease quarantine.
- joule-tick-lease-deadline-only: ticks from synced TSC plus revocable leases with advisory RAPL estimates and hard deadline kills.
