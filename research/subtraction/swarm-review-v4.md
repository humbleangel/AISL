# Swarm review v4 — subtraction (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (heap/clock/atomics/drivers/legacy still implied — 5 gaps) attacked in round 2 for hidden merges (ambient+heap+clock hide three scarcities), undeleted SMM/ME/legacy/driver model, missing no-overwrite (NVMe tear), missing no-debug-halt-world.

## Round-1 → Round-2 changes
- Clock/heap split from authority into own entries.
- Whole legacy PC + driver model + firmware trust explicitly deleted.
- No-overwrite folded into files entry; no-debug folded into steal entry.

## Final v4
- no-files-no-fds-no-overwrite: only persistent-content-pages + nvme-append-trail exist; no streams, no truncate; powercut is the only commit.
- no-processes-no-fork-no-clone: only pinned run-to-completion lanes; no fork, no signals, no invisible preemption.
- no-async-steal-no-unfenced-dma: interrupts are send + DMA only via iommu-dma-firewall; never steal a lane.
- no-syscall-boundary-no-ring-crossing: syscall-is-call in single-address-space; no trap cost; privilege-crossing-budget stays zero.
- no-unmeasured-exec-no-linking: only-measured-pages-execute + pcr-equality-verdict; no binaries, no dynamic linking, no JIT bytes.
- no-coherent-shared-no-atomics-no-locks: no-coherent-shared-mutable-memory; mesh is ipi-noncoherent-mesh; MESI is transport, not semantics.
- no-ambient-no-heap-no-clock: no-ambient-authority + explicit-allocation + no-global-now; no root/env/cwd/malloc-gc/sleep-wall-now; deadlines-as-handlers with power-is-capability.
- no-legacy-no-drivers-no-firmware-trust: no real-mode/PIC/PIT/VGA/option-ROM, no driver model, no SMM-ME-microcode correctness dependence; metal vocabulary of APIC/NVMe/TPM/EPT/IOMMU only.
