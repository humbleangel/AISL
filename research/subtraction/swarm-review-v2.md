# Swarm review v2 — subtraction (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- no-file-hierarchy: shallow — keeps files-as-bytes, just flattens tree; still assumes NVMe = named blobs not persistent typed memory.
- no-processes: right target, vague — rename to tasks/actors with private address spaces and nothing was removed.
- no-signals: trivial — hated Unix wart, not architectural; hardware interrupts/APIC still force async delivery somewhere.
- no-dynamic-linking: shallow — static ELF is the same compile-link-load pipeline; still carries Thompson 1984 trust chain.
- no-user-kernel-split: dishonest on x86-64 — rings/NMI/SMM/page-faults persist; removable as ABI, unremovable as hardware fact.
- no-libc-shim: overlapping consequence, not removal — if OS=language there is no C to shim.

## Completed sublist v2
- no-files: NVMe as persistent typed objects/extents, no paths/hierarchy/fds; kills stringly namespace between intent and metal.
- no-processes: single 64-bit address space + language-isolated tasks pinned to cores; kills fork/exec, per-process page tables, TLB-shootdown tax.
- no-async-signals: HW interrupts stay in tiny stub, surface only as typed messages/sync exceptions; kills reentrant handler hell.
- no-binaries-no-linking: no ELF/so/loader; one live image built from source intent by hand-seeded bootstrap; kills linker and Thompson trust gap.
- no-syscall-boundary: no user/kernel ABI; language-safe code calls metal as functions, rings+paging used only for fault containment.
- no-posix-libc: no fds/errno/mmap/brk/sockets-as-files; pages, queues, NVMe extents exposed directly as language types.
- no-shared-memory-threads: no pthreads+locks over shared caches/SMT; cores communicate by message-passing, speculation side-channels off by default.
- no-invisible-preemption: no preemptive timesharing illusion; tasks yield with scheduler hints, fitting few strong cores + power/thermal limits.
