# Swarm review v4 — new-forgetting (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (7 hardware-lie candidates) attacked in round 2 for repeat-vs-new check (coherence-cost and scalar-default are programming defaults, not v3 repeats; flush-lie is device lie, not file abstraction; firmware-betrayal is below-OS, distinct from ring0-monolith; preemption-mechanism distinct from cron-folklore; page-granularity new; global-now + delete uncovered), entropy/RNG and single-binary-generality cut as weaker.

## Round-1 → Round-2 changes
- Names made folder-safe, each sharpened to one metal truth.

## Final v4
- forget-coherence-is-free: unlearn threads-share-memory-cheaply; only message-passing over noncoherent mesh, coherence is budgeted exception.
- forget-scalar-default: unlearn one-instruction-stream CPU; lanes are the thread, scalar is degenerate vector of width 1.
- forget-flush-means-durable: unlearn fsync-equals-safe; NVMe can lie and power can cut mid-write, only crash-only append + recovery verdict counts.
- forget-firmware-is-trusted-ground: unlearn below-ring0-is-benign; SMM/ME/microcode are liars to be quoted, measured, sentried, never trusted.
- forget-preemption-anywhere: unlearn kernel-may-steal-my-core; per-core run-to-completion only, interrupts are messages not steals.
- forget-byte-heap: unlearn infinitely-splittable malloc heap; allocation is whole pages with types in page tables, nothing smaller exists.
- forget-single-global-now: unlearn one true wall clock; TSCs drift, only causal-tick first and wall-as-range, replay needs trapped identity.
- forget-overwrite-and-delete: unlearn storage-can-be-erased-in-place; persistent pages are content-addressed and monotonic, past is abandoned never mutated.
