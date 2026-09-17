# Swarm review v2 — inversion (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- app-defines-syscalls: directionally right but vague; overlaps fully with os-is-a-library, keeps syscall-table assumption intact.
- caller-always-allocates: strongest of the six, but stated as calling-convention trivia instead of killing the heap illusion.
- interrupts-are-messages: renaming, not inversion; microkernels already did this, still worships async preemption.
- os-is-a-library: stale (exokernel/unikernel 90s), just link-order change, not master-slave reversal; ignores VT-x which allows real reversal.
- data-loads-code: vaguest entry; on von Neumann x86-64 data can't load code without-fetch, so slogan without mechanism.
- errors-only-in-band: fashionable, not inverted; denies metal reality where NMI/MCE/#PF are irreducibly out-of-band.

## Completed sublist v2
- language-defines-traps: compiler emits the syscall/IDT dispatch table, kernel only enforces — intent defines the gate, not the servant.
- caller-always-allocates: allocation is an RSP bump plus page rights at the call site; kills hidden callee allocators and shared-heap contention on few strong cores.
- faults-are-control-flow: #PF/#GP/NMI/timer/syscall all enter one language match on the IDT — no fast-path vs slow-path, the fault is the call.
- kernel-is-guest: runtime sits in VT-x root, legacy OS/drivers run deprivileged as guests — the old master becomes the sandboxed servant.
- pages-are-the-heap: no malloc over linear memory; MMU page tables and NX/RW bits are the allocator and type system.
- code-follows-data: fault pulls code to the core/cache holding the data, not data dragged to code — serves SMT/shared-cache reality and power limits.
- errors-only-as-faults: no in-band return codes; every error travels as a fault message via the same IDT path, matching how metal already reports MCE/NMI.
