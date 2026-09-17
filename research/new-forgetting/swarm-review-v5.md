# Swarm review v5 — new-forgetting (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 folklore-vs-truth audit (all 9 candidates PASS as real folklore with real metal truth; speculation added; heap+overwrite merged to stay <=8).

## Round-2 verdicts
- coherence-is-free PASS (pthreads default vs MESI/TLB-walk cost). scalar-default PASS (C scalar vs 8–16x AVX lanes). flush-means-durable PASS (fsync vs NVMe lie + torn pages). firmware-is-trusted-ground PASS (invisible-trusted vs SMI theft/ME DMA/mutable microcode). preemption-anywhere PASS (Unix preempt vs residue/scrub blowup). byte-heap PASS (malloc vs whole typed pages). single-global-now PASS (clock_gettime vs TSC drift + stolen time). overwrite-and-delete PASS (erase vs flash remanence/wear). speculation-is-invisible PASS as NEW (straight-line ISA vs Spectre/Meltdown + PMU crosscheck + sweeper).
- Executability caveat: scalar cannot be fully forgotten (control stays scalar) — phrase as default. Coherence/flush/firmware bans reframed as beliefs with cost-defaults, not erasures.

## Final v5
- forget-coherence-is-free: default is posted IPI + per-core shards; coherence is measured cost, not medium.
- forget-scalar-default: lanes are the CPU, scalar is control exception; count SIMDs, not cores.
- forget-flush-means-durable: FLUSH lies and power tears; only CoW + torn-proof pages + crash-trail persist.
- forget-firmware-is-trusted-ground: below-ring0-is-benign is false; SMM/ME/microcode are liars to be quoted, measured, sentried.
- forget-preemption-anywhere: kernel-may-steal-my-core is false; one run-to-completion thread per core with bounded deadline-sends.
- forget-byte-heap: infinitely-splittable malloc heap is false; allocation is whole pages with types in page tables.
- forget-single-global-now: one true wall clock is false; TSCs drift, only causal-tick first and wall-as-range.
- forget-overwrite-and-delete: storage-can-be-erased-in-place is false; persistent pages are content-addressed and monotonic.
