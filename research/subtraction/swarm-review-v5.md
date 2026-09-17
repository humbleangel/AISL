# Swarm review v5 — subtraction (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 honesty audit: rings/MESI/TSC/DMA/NMI/SMI persist as hardware fact — only their *use* is deleted.

## Round-2 kills / revisions
- no-rings-no-syscalls revised DISHONEST as written: rings/CPL/NMI persist; only syscall ABI + ring-crossing use deleted.
- no-coherence revised HALF-DISHONEST: MESI/store-buffers/shared-LLC persist; only shared-writable mappings + LOCK/atomics/locks deleted.
- no-ambient-no-heap-no-time split: TSC/APIC/RAPL tick persist; only ambient wall-clock use deleted.
- no-async-no-dma revised: APIC-IPI/IRQ/DMA/NMI/SMI persist; only unfenced DMA + unvetted preemption deleted.
- no-firmware revised: SMM/ME/microcode persist as untrusted guest; only trust + driver-model + legacy ABI reliance deleted.

## Final v5
- no-files-no-fds-no-overwrite: NVMe has only addressed pages; files replaced with caps + COW append-log.
- no-processes-no-fork-no-clone: fork/clone are SW; replaced with EPTP/VPID domain spawn + disposable arena.
- no-syscall-no-ring-crossing: rings persist, crossing deleted; EPT-violation upcalls replace syscall ABI.
- no-unmeasured-execution-no-linking: unmeasured bytes never get X; linker replaced by measured-page manifest.
- no-shared-writable-no-atomics-no-locks: MESI/speculation persist, sharing deleted; posted-PIR-send + per-core shards replace locks.
- no-ambient-no-heap-no-wall-clock-use: TSC ticks persist, ambient use deleted; explicit arenas + causal-tick/deadline replace heap/now.
- no-unfenced-dma-no-unvetted-async: IRQ/DMA/NMI/SMI persist, unfenced use deleted; IOMMU-filtered-send + betrayal-as-fault replace.
- no-legacy-drivers-no-firmware-trust: SMM/ME/microcode persist as untrusted guest; drivers replaced by pinned vocabulary + measured-expiry.
