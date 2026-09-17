# Final keep-all v1 — subtraction (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (8) with brutal FINAL (7). Subtraction owns deletions only; honest use-deletion wording throughout (silicon persists, use deleted).

## Round 1 — one by one
- no-files-no-fds-no-overwrite KEEP+FUSE: file/fd/pathname/VFS are software, fully deletable; physics note — storage cells rewritable, language forbids in-place overwrite semantics (immutable/versioned objects, measured extent caps).
- no-processes-no-fork-no-clone KEEP+FUSE: pid/address-space multiplicity is policy; single domain + language compartments + capability spawn replace fork. Kept distinct from syscall entry (identity vs transition).
- no-syscall-no-ring-crossing KEEP+FUSE: rings/CPL/IDT persist; run CPL0 single domain, never emit SYSCALL, trap illegal opcodes. Honest rename: no-emission/use, not no-hardware.
- no-unmeasured-exec-no-linking KEEP+FUSE: strongest Thompson survivor; CPU fetches anything, so forbid unmeasured fetch + runtime dlopen/PLT; static measured image + reproducible toolchain + TPM pin allowed; residual trust in root admitted.
- no-shared-writable-no-atomics-no-locks KEEP+FUSE: MESI/LOCK persist; delete shared-writable alias + general LOCK emission; private-writable + one vetted ownership-handoff allowed.
- no-ambient-no-heap-no-wall-clock-use KEEP+FUSE (triple kept bundled by laziness): ambient authority → capabilities; ambient malloc → arenas/ownership; wall dependence → monotonic/no-time. Repaired against "no allocation" misread.
- no-unfenced-dma-no-unvetted-async KEEP+FUSE: DMA/IRQ/NMI/SMI persist; deny unfenced descriptors + unvetted gates; default-deny + bounded vetted gates + SMI-as-fault.
- no-legacy-drivers-no-firmware-trust RESURRECTED (repaired): brutal fold rejected with record — fenced-DMA/SMI-fault/TPM-pin cover behavior, but only explicit deletion is grep-auditable and blocks the "fenced legacy" loophole (parser complexity, SMM callbacks, blind ACPI trust). Repaired wording: no-legacy-driver-code-use-no-firmware-trust-use (minimal vetted drivers allowed; firmware measured/contained, never trusted).

## Round 2 — attacks + fixes (each: files/processes/rings/exec/atomics/ambient/DMA/firmware attacks answered)
- Storage persists → abstraction ≠ medium; measured extents replace VFS. PASS.
- MMU persists → multiplicity is policy; single domain replaces. PASS.
- Traps persist → crossing is policy; CPL0 + vetted gates replace syscalls. PASS.
- Fetch persists → unmeasured path deleted; static measured image replaces. PASS.
- MESI persists → sharing deleted; private + one handoff replaces. PASS.
- TSC/DRAM persist → ambient/wall dependence deleted; caps + arenas + monotonic replace. PASS.
- DMA/async persist → unfenced/unvetted deleted; IOMMU + vetted gates + SMI-fault replace. PASS.
- Firmware persists → trust/inclusion deleted; minimal drivers + measured-contained firmware replace. PASS.
- Coherence: single-domain + no-syscall + no-process cohere; caps + arenas cohere; vetted gates cohere; measured root cohere. 8 within 6-10.

## Final (8, honest use-deletions)
- no-files-fds-overwrite-use: delete file/fd/pathname/overwrite use; measured extent caps + immutable versioned objects.
- no-processes-fork-clone-use: delete process/pid/multiplicity use; single domain, capability spawn, never fork/clone.
- no-syscall-ring-transition-use: rings persist, crossing/use deleted; vetted fault gates, illegal-opcode-trap syscalls.
- no-unmeasured-exec-runtime-linking-use: delete unmeasured fetch + runtime link/dlopen; static measured image + TPM pin.
- no-shared-writable-atomics-locks-use: MESI/LOCK persist, sharing deleted; private-writable + one vetted handoff only.
- no-ambient-heap-wallclock-use: delete ambient authority, ambient malloc, wall dependence; caps + arenas + monotonic.
- no-unfenced-dma-unvetted-async-use: DMA/async persist, unfenced/unvetted deleted; IOMMU fence + vetted bounded gates + SMI-fault.
- no-legacy-drivers-firmware-trust-use [RESURRECTED]: delete legacy driver inclusion + firmware trust; minimal vetted drivers, SMI-as-fault, measured-pinned ME/microcode/ACPI.
