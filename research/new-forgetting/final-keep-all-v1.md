# Final keep-all v1 — new-forgetting (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (8+1 candidate) with brutal FINAL (8 confirmed). Forgetting = diagnosis, never bans.

## Round 1 — belief verdicts (folklore exists? metal truth real? unlearnable?)
- forget-coherence-is-free KEEP: pthreads/mutex folklore vs TSO + MESI/RFO cost + NUMA; single-owner default. Deepened: cost + TSO, not just correctness.
- forget-scalar-default KEEP: K&R/autovectorizer folklore vs 4–16x SIMD + superscalar + front-end energy; bulk default, scalar explicit slow path.
- forget-flush-means-durable KEEP: write/fflush folklore vs store-buffer/L1/WPQ/disk-cache/FTL/GC layers; journal + COW + CLWB/SFENCE/FUA + dir-sync. Dual of #8 (durability vs erasure — same copies, opposite goals; keep both with cross-link).
- forget-firmware-is-trusted-ground KEEP: below-OS trusted folklore vs SMM/ME/UEFI/DMA/SPI; measure/lock/IOMMU; toolchain folded inside as upward Thompson instance (same shape, no duplicate folder).
- forget-preemption-anywhere KEEP: threads-anywhere folklore vs NMI/#PF/IPI landing anywhere; safepoints/masked sections; language-level yield is the AISL win.
- forget-byte-heap KEEP: flat malloc folklore vs paging/TLB/NUMA/ownership; arenas/zones/lifetimes; distinct from lanes (#2) and sharing (#1).
- forget-single-global-now KEEP: one-now() folklore vs TSC offset/migration/NTP steps; monotonic + causal clocks; distinct from data order (#1).
- forget-overwrite-and-delete KEEP: rm-means-gone folklore vs FTL/journal/swap/DSE copies; crypto-erase + explicit scrub; dual of #3.
- forget-speculation-is-invisible PROMOTE to 9th: pre-2018 sequential-ISA folklore vs Spectre/Meltdown/MDS transient residue; committed-vs-transient distinct from #1; MFENCE≠LFENCE (folding would cause wrong mitigation); constant-time + barriers + isolation.
- Candidates: entropy (defer-reserve as 10th if early-boot crypto; overlaps #4/#7), single-binary (fold into #2 as dispatch example), toolchain (fold into #4 as upward instance). All with written reasons; no weak dilution.

## Round 2 — attacks + fixes + dedup vs subtraction bans
- "TSO strong so coherence free" → TSO still reorders (SB) + RFO cost; diagnosis mentions visibility AND price.
- "Compiler autovectorizes" → fragile on control/gather/aliasing; bulk default stands.
- "fsync durable on modern SSD" → volatile caches + torn + dir-fsync missed; crash-injection proof stands.
- "Secure Boot solves firmware" → keys/measurer/SMM gaps; verify-per-boot stands.
- "Linux preempts fine" → kernel uses preempt_disable/spinlock-irqsave/RCU; safepoints stand.
- "malloc fine / GC solves" → TLB/NUMA/lock/UAF first-order; arenas stand.
- "NTP good enough" → migration/steps/skew break order; monotonic/causal stands.
- "Overwrite enough for non-NSA" → stolen/RMA/swap/DSE routine; crypto-erase stands.
- "Spectre fixed in hardware" → partial/model-specific/new variants; language hardening stands.
- Dedup rule enforced: every entry diagnosis-verbed (believe/costs/leaks/survives), never ban-verbed; cross-links to ban layer, never restates it. Pairwise inter-forgetting sweep: no merges (committed-vs-transient, durability-vs-erasure, lanes-vs-locality all distinct).

## Final (9)
- forget-coherence-is-free: x86 TSO + MESI makes sharing visible-late and costly; single-owner beats transparent sharing.
- forget-scalar-default: x86 wins via SIMD/superscalar throughput; bulk default beats scalar-loop default.
- forget-flush-means-durable: CPU/disk/FTL caches make flush about visibility not power-safe durability; journals + fences + dir-sync needed.
- forget-firmware-is-trusted-ground: SMM/ME/UEFI/DMA (+ toolchain per Thompson) persist below reinstall; measure/lock/IOMMU beats assumed trust.
- forget-preemption-anywhere: interrupts/NMI land between any insns; explicit safepoints/masked sections beat invisible scheduler.
- forget-byte-heap: paging/TLB/NUMA/ownership make flat malloc a lie; arenas/local/bulk-reclaim beat anonymous heap.
- forget-single-global-now: TSC/NTP skew, no global clock; monotonic + causal clocks beat one true now.
- forget-overwrite-and-delete: FTL/journal/swap/DSE keep copies after overwrite/unlink; crypto-erase + explicit scrub beat naive zero/delete.
- forget-speculation-is-invisible: discarded transient execution stays timing-readable; constant-time + LFENCE/isolation beats ISA-only reasoning.
- Reserved: forget-entropy-is-plentiful (defer to 10th on early-boot crypto need).
