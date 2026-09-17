# Swarm review v2 — new-constraint (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- few-strong-cores-only: restates hardware fact, bans nothing; still assumes threads + coherent shared memory.
- no-implicit-allocation: strongest of six but narrow; bans hidden malloc while leaving stack/paging/interrupts implicit.
- cooperative-scheduling-only: picks the 1960s side of an old debate; keeps the Unix thread+scheduler model intact.
- capabilities-only-access: 1966 idea relabeled; unenforceable unless bound to paging/rings/VT-x, says nothing about how.
- fixed-memory-budget: overlaps no-implicit-allocation; counts bytes while ignoring pages/cache-lines/power as real budgets.
- no-posix-layer: purely negative nostalgia-ban; forbids fd/fork names but permits the same syscalls renamed.

## Completed sublist v2
- explicit-allocation-only: every byte/page/arena named at creation site, kills hidden GC/malloc/closure-alloc and forces budget types.
- hardware-bound-capabilities-only: authority must compile to paging+rings+VT-x entries, not software checks, kills ambient root/user split.
- no-posix-no-c-abi: no fds, fork, signals, libc linkage; forces new bootstrap and syscall ISA instead of renamed Unix.
- single-address-space-only: one virtual space for all code, forces language-level isolation and kills per-process paging assumption.
- run-to-completion-per-core-only: no blocking, no preemptive slices, core-pinned handlers; forces event/deadline language, kills thread scheduler.
- no-coherent-shared-mutable-memory: cores share only immutable or transferred pages via message, kills false-sharing/SMT/Spectre-by-construction.
- crash-only-persistence: NVMe is the memory, TPM-measured resume, no clean shutdown; forces persistent language state, kills volatile-vs-file split.
