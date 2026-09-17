# Swarm review v3 — new-constraint (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3 (cross-checked against all other 15 v2s, new insights licensed).

## Collisions resolved
- explicit-allocation-only vs inversion/caller-always-allocates: INVERSION owns mechanism (caller pays). CONSTRAINT keeps the ban form.
- hardware-bound-capabilities-only vs substrate/page-capability-mmu vs pairing/power-is-capability vs scarcity/privilege-crossing-budget: SUBSTRATE owns HW format, PAIRING owns metaphor, SCARCITY owns budget. CONSTRAINT keeps only no-ambient-authority rule.
- no-posix-no-c-abi vs subtraction + forgetting posix entries: full overlap. DROPPED from constraint; SUBTRACTION owns deletions, FORGETTING owns mental-model purge.
- single-address-space-only vs subtraction/no-processes vs scale/single-task-no-scheduler: CONSTRAINT owns SAS invariant; SUBTRACTION owns API deletion; SCALE owns task-count consequence.
- run-to-completion-per-core-only vs scale/time/subtraction: CONSTRAINT owns preemption ban; SCALE owns placement; TIME owns deadline handlers.
- no-coherent-shared-mutable-memory vs subtraction/scarcity: CONSTRAINT owns coherence invariant; SUBTRACTION owns API ban; SCARCITY owns residency/secrecy budgets.
- crash-only-persistence vs substrate/pairing/oracle/failure: CONSTRAINT owns crash-only rule; SUBSTRATE owns format; ORACLE owns proof; FAILURE owns fault case.

## New insights
- W-xor-X + IOMMU rule missing: DMA and RWX pages bypass language without no-execute-without-measure, no-DMA-without-capability.
- Speculation/SMT sharing unbanned: store-buffers, SMT siblings, AVX throttle are cross-intent channels no v2 constraint forbids.
- Energy/time unbounded: RAPL/PMU make cycles+joules countable, yet v2 allows unbounded handlers; thermal throttle becomes invisible preemption.
- Thompson-1984 gap: unmeasured microcode/firmware means CPU lies about what executed; unmeasured pages must not execute.
- Replay unforced: trapped-replay assumes replay possible, but no constraint forces RDTSC-equality, FP determinism, sized loops.

## Completed sublist v3
- explicit-allocation-only: every byte charged to caller at creation, no fault-time malloc/GC path to hide cost.
- no-ambient-authority-only: no access without presented token, HW format ceded to substrate/page-capability-mmu.
- single-address-space-only: one 64-bit space for all code/data, process deletion ceded to subtraction/no-processes.
- run-to-completion-per-core-only: handler on a core cannot be preempted, placement ceded to scale, deadlines ceded to time.
- no-coherent-shared-mutable-memory: cores pass pages/messages, never MESI-shared RW, API ban ceded to subtraction.
- crash-only-persistence: RAM is cache, NVMe atomic append-diff is truth, format ceded to substrate, test ceded to oracle.
- only-measured-pages-execute: no X bit without TPM/EPT measurement, closes Thompson/microcode trust hole.
- bounded-energy-time-only: every handler declares cycle+joule cap enforced by PMU/RAPL, makes thermal/speculation visible.
