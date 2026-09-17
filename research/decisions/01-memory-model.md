# Decision brief 01/28 — memory isolation model

RECOMMENDATION: single-address-no-share (one VA, no writable sharing, IPI mesh + immutable content pages).

## THINK-1 (from metal)
x86-64 punishes what Unix-classic does most: mutable sharing + frequent remap. TLB is not coherent — every mmap/mprotect/munmap on shared/writable mappings = IPI shootdown to every core caching it. MESI never sleeps: writable-shared lines ping-pong (RFO + invalidate + refill) even when "sharing" is rare; on few strong cores + SMT + speculation this is lost throughput + timing side-channel surface. Context-switch cost is solvable (PCID/INVPCID); shootdown cost is not. EPT makes multi-AS worse (2-D walk latency + VPID management + VMFUNC/exit costs). Single VA with no writable sharing: page tables become ~static (allocate once, never remap shared), shootdowns → ~zero; cores meet by explicit copy (IPI + immutable content page), turning implicit MESI storms into explicit auditable messages.

## HARDWARE FINDINGS (x86-only, web)
- Local flush cheap, shootdown expensive: INVLPG ~200 cycles; INVPCID single-context ~550 cycles; full flush-all ~376ns Skylake. Shootdown by contrast = several thousand cycles plus locks plus post-flush TLB-miss refill. Sources: LKML Dave Hansen INVPCID measurements; openwall INVPCID-vs-CR4.PGE (376ns vs 539ns); Amit et al. "Don't shoot down TLB shootdowns" §2.2.
- Shootdown = IPI + spin-wait + handler + ack; scales badly (initiator sends IPI, each remote flushes + acks; locks held; masked interrupts delay ack). 129-page flush measured ~25–70k cycles. Sources: Amit et al. 2020 §2.2/§3.1; USENIX ATC'17; kernel-internals.org (`flush_tlb_multi`, 1–5µs/IPI, cross-socket worse).
- Batching/lazy tricks (mmu_gather, idle-core/mm_cpumask filtering, access-bit tracking cutting invalidations up to 98% in some workloads) mitigate but don't cure shared-mutable designs — at up to 9% overhead when mappings never die. Multithreaded shared-AS is the worst case. Sources: ATC'17 access-tracking paper; kernel-internals mmu_gather, lazy-TLB.

Implication: PCID fixes *switch* cost, nothing fixes *shared-mutable shootdown + MESI* except not sharing writably.

## THINK-2 (after findings)
Findings confirm THINK-1: dominant x86 cost is coherent-in-software TLB + coherent-in-hardware MESI on writable-shared pages. Alternatives keep it: per-process paging + shared heap (maximum shootdowns + MESI); EPT-partitioned multi-AS + shared heap (same + nested-walk tax); single-address with tracked sharing (keeps sharing + tracker complexity Thompson-1984 warns about, for a cost you could delete). Single-address-no-share deletes the category: static mappings → no shootdown in steady state; intra-AS isolation via paging R/W + MPK/PKU key switch (WRPKRU, core-local, no IPI) instead of CR3/EPT switch. IPI mesh is then *signaling*, not coherence: small, explicit, rate-limited, auditable.

## REASONS
1. Kills the two x86 taxes at once: ~zero shootdowns (static mappings) and ~zero MESI writable-sharing traffic.
2. Uses the cheap hardware path: PCID for residual clips, MPK/PKU + U/S + R/W for intra-AS isolation (no flush, no IPI), IPI only as explicit message.
3. Smallest TCB (Thompson): no shared-heap tracker, no EPT hypervisor layer, no shootdown-avoidance daemon.
4. Collisions resolved: implements subtraction no-shared-writable; page-capability-MMU becomes grant-once-map + rare batched revoke; pinned-endpoints get stable VAs.
5. Scales with cores: cost grows with explicit messages sent, not with cores caching a shared PTE.

## PROVING EXPERIMENT (x86-64, kill-or-confirm in one day)
Bare 8–64-core box (note microcode rev). A/B with perf/PMU (dtlb walk counters, page-walker cycles, smp_call_function / TLB-flush tracepoints, MESI RFO/invalidate counters): (A) shared-writable ring buffer, N threads, mprotect/write loop; (B) same workload single-VA-no-share (per-core private pages + IPI/queue + immutable page handoff). Vary N=2..max, measure throughput, p99, shootdowns/s, walker cycles, coherence traffic.

## WHAT WOULD CHANGE MY MIND
(i) Message rate in real AISL workloads forces IPI storms as bad as shootdown storms (then bounded tracked-sharing justified); (ii) hardware with cheap broadcast TLB coherence (e.g. AMD INVLPGB globally <500ns cross-core); (iii) MPK/PKU or 4-level paging unusable for needed granularity (then EPT partitions reconsidered).
