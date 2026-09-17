# Decision brief 07/28 — cheap-execution substrate pattern

RECOMMENDATION: resume-COW-arenas. Variants = microsecond CPU-only resumes of measured COW pages via VMFUNC, ≤8 live EPTPs, TLB refill in budget, DRAM snap always, NVMe commit leased.

## THINK-1 (metal first)
Don't fight CPU = variants must be an MMU operation, not an OS operation. Execution is gVA→gPA→hPA; worst miss is multiplicative; resume re-warming from DRAM-cached PTEs is µs, from devices/NVMe is ms. fork() copies page tables, CR3 write flushes, shootdown = IPI + spinwait — fanout scales with cores not variants. Container snapshot: same TLB problem + no HW boundary. Full-VM migration: copies memory, ms-scale — wrong for generation-scale use. S3/S0ix resume: kernel thaws devices; even if CPU state is MWAIT-next-instruction, device resume dominates — so AISL "resume" must mean CPU-ONLY (registers + EPTP selector + demand-fault), never device model. NVMe: per-variant flush is endurance + latency lie — snap in DRAM free, commit leased/verified. Bun lesson: generation cheap ⇒ bottleneck is verification — substrate must produce measured pages so verifier has something scarce to check.

## HARDWARE FINDINGS (x86-only, web)
- EPTP list = 512: 4KB list, 512×8B EPTPs, ECX≥512 → VM exit. VMFUNC(EAX=0) loads EPTP=list[ECX] if valid. VPID/PCID unchanged. (Intel SDM Vol3 24.6.14, 25.5.5.3, 27.5.6)
- VMFUNC raw cost ~109–160 cycles: 109–147c + save/restore (MARS/IEEE-SP-Mag 2023); ~160c vs CR3 ~300c (EPTI ATC'18); ~122c KVM test (lkml 2015). At 2.5–3GHz ≈ 40–60ns raw; full cross-domain 150–300ns credible.
- TLB is per-EPTP but global flushes leak: each EPT has own combined mapping; CR3 write and INVLPG flush ALL EPTs' TLBs (EPTI Table 1). Switching cheap, but COW CR3/INVLPG in one arena evicts others — must count.
- INVPCID/INVVPID costs: INVLPG ~200c, INVLPG+INVPCID ~550c (Hansen lkml 2017); full INVPCID type2 ~376ns Skylake vs CR4.PGE toggle ~539ns; PCID saves ~100ns/switch but shootdown = thousands of cycles + IPI + lock + delayed-ACK in driver (Amit EuroSys'20; LWN 671299).
- Nested miss = 24 refs (4-level), 35 refs (5-level): g*(n+1)+n (Ahn ISCA'12; Gandhi MICRO'14; Agile Paging). PWC+NTLB hide average, worst still DRAM-chained.
- S3/S0ix resume = ms not µs: e.g. XPS13 suspend 1366ms, resume 769ms kernel device resume (sleepgraph); S0ix target enter/exit <2s. CPU MWAIT resume is next-instruction; devices make it ms. Proves resume-µs must be CPU-only.

## THINK-2 (after findings)
Arch allows 512 EPTPs; recommend ≤8 live for TLB physics (each live EPTP owns combined TLB + PWC footprint; beyond ~8, refill approaches 24-ref/miss storm and VMFUNC saving vanishes). Base arena on 2M EPT pages (collapse nested levels); variant diff on 4K COW, measured. fork/container lose on fanout (pagetable copy + shootdown IPIs); migration loses by 3 orders (ms copy for µs task). Only EPTP-switch keeps hot path to one unprivileged instruction with no VM-exit and no device thaw. NVMe commit leased: DRAM COW snap = EPT alias + RO bit; durable commit pays flush + write-amplification per variant — only on verify-pass.

## REASONS
Only primitive at ~10² cycles with no exit, no IPI, no device thaw. TLB tagging lets 8 arenas stay warm; others parked (EPTP invalidated, pages retained in DRAM). 2M base + 4K COW bounds 24-ref risk to diff set. Matches Thompson/Bun: trust base once, measure diff cheap, verify scarce before paying NVMe. Reject fork-clone (shootdown scales with cores), container snapshot (no HW isolation, same flush cost), live-migration-as-primitive (ms copy for µs task).

## PROVING EXPERIMENT (1 host, KVM, perf)
Base arena 128MB, EPT 2M. N=1/8/64 variants: VMFUNC + touch 0/16/256 COW pages. Measure VMFUNC cycles, time-to-first-useful-instruction, dtlb walk_duration, LLC misses in 10µs window. Compare vs fork()+exec and vs S3/mem_sleep resume. Pass: p50 CPU-only resume <10µs, p99 <50µs at N=8; walk cycles flat 1→8, cliff >8 justifies cap; zero NVMe writes per uncommitted variant.

## WHAT WOULD CHANGE MY MIND
Measured COW working set >~1–4MB makes refill > fork (walk_duration crossover); isolation forces per-variant INVVPID/full flush (kills per-EPTP TLB win); variant needs device DMA/re-enumeration (resume becomes ms by definition); verifier can't use measurements to skip work (generation-cheap premise fails — pick simplest fork).
