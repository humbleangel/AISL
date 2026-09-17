# Brutal combine v1 — subtraction (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Subtraction owns deletions only; replacements live elsewhere.

## Merges and kills
- no-legacy-drivers-no-firmware-trust KILLED as standalone (folded into unfenced-DMA entry + SMI-fault + TPM-pin).
- All entries renamed to honest use-deletions: rings/MESI/TSC/DMA/NMI/SMI/LOCK persist as hardware fact — only crossing/sharing/trust/use deleted.
- Bloated triples split honestly (ambient vs heap vs wall-clock kept visible in one name by budget compression).

## Final merged list (7 survive)
- no-files-no-fds-no-overwrite: deletes fd/file/overwrite abstraction; persistence owned by COW-append elsewhere.
- no-fork-no-clone-no-process-image: deletes process/fork; execution owned by cells/views/resume elsewhere.
- no-syscall-no-ring-cross-use: rings persist, only crossing/use deleted; upcall/fault path owned elsewhere.
- no-unmeasured-exec-no-link-use: deletes unmeasured exec/link; measured-anchor owned elsewhere.
- no-shared-writable-no-atomic-lock-use: MESI/LOCK persist, only shared-writable/atomic/lock use deleted.
- no-ambient-no-heap-no-wall-use: deletes ambient/heap/wall-interpretation; caps/arena/tick-delta owned elsewhere.
- no-unfenced-dma-no-unvetted-async-no-firmware-trust-use: DMA/SMI/ME/microcode persist as adversary; only unfenced/unvetted/trust deleted.
