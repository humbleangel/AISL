# Swarm review v3 — subtraction (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. Rule: SUBTRACTION owns deletions (negative mechanism); replacements live elsewhere.

## Collisions resolved
- no-files vs forgetting/substrate/constraint/pairing: SUBTRACTION owns negative (no hierarchy/paths/fds); FORGETTING owns mental break; others own replacement.
- no-processes vs constraint/scale/forgetting: SUBTRACTION owns deletion (fork/exec/daemons); CONSTRAINT/SCALE own what runs instead; FORGETTING owns habit.
- no-async-signals + no-invisible-preemption vs inversion/time/constraint: SUBTRACTION owns merged negative (nothing arrives unasked); INVERSION/TIME own replacement control-flow.
- no-binaries-no-linking vs forgetting/abundance/pairing: SUBTRACTION owns removal (no ELF/loader); ABUNDANCE/PAIRING own generative replacement.
- no-syscall-boundary vs pairing/constraint/scarcity/forgetting/inversion: SUBTRACTION owns gate deletion; PAIRING owns call replacement; SCARCITY owns crossing budget.
- no-posix-libc: DROPPED from subtraction, ceded to CONSTRAINT (rule) + FORGETTING (habit); subtraction keeps concrete mechanisms.
- no-shared-memory-threads vs constraint/pairing/scale/scarcity: SUBTRACTION owns deletion (no pthreads); CONSTRAINT owns rule; PAIRING/SCALE own lane replacement.

## New insights
- Legacy PC still trusted: real-mode, BIOS/SMM, PIC/PIT/VGA, legacy ports are invisible ring--1 TCB + speculation backdoor; subtract whole platform, boot UEFI-measured + VT-x only.
- Interrupts + DMA steal the core: NVMe/NIC/timer IPIs break run-to-completion and determinism/energy budgets; subtract interrupt-steal, poll-only fenced queues.
- Unix identity is ambient authority: uid/gid/root, mode bits, setuid, open sockets/DMA contradict hardware-bound capabilities; subtract users + ambient IO.
- Demand-swap/overcommit lies on persistent NVMe: transparent paging hides persistence cost, breaks crash-only + RAPL accounting; subtract swap, persistence explicit/versioned.

## Completed sublist v3
- no-files-no-fds: no hierarchy, paths, fds, or text-as-truth lookup; pages addressed directly.
- no-processes-no-fork: no fork/exec/wait/daemons/cron; one pinned task per core, cloned whole via EPT.
- no-async-steal: no signals, IPIs, device interrupts, or invisible preemption; faults and deadlines are synchronous continuations.
- no-syscall-boundary: no trap gate or user/kernel split; cross-domain is a checked call with caller-allocated pages.
- no-binaries-no-linking: no ELF, shared libs, or runtime loader; only generated, measured, disposable code.
- no-shared-memory-threads: no pthreads over coherent mutable memory; lanes share immutable pages or message explicitly.
- no-legacy-no-ambient-authority: no BIOS/SMM/legacy IO, no uid/root/mode-bits, no ambient sockets/DMA; all IO is explicit polled capability queues.
