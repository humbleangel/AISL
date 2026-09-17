# Swarm review v5 — new-substrate (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 hardware-existence audit.

## Round-2 verdicts (all 6 claimed primitives REAL, narrowed, none killed)
- MPK/PKU REAL: WRPKRU/RDPKRU, 16 keys, PTE bits 62:59, per-logical-CPU (SMT siblings diverge); U/S still applies.
- CET REAL: shadow-stack + ENDBR, needs VMM enable + save/restore; EPT alias must preserve shadow page type.
- CAT REAL: RDT COS/CBM LLC partition; Xeon-focused, coarse, LLC-only (not L1/L2), isolation not security.
- Interrupt-remapping REAL: VT-d IRTE + queued invalidation; BIOS must enable; filters IRQ source only.
- AES-NI REAL: computes only; no torn-proof/durability (hash-then-encrypt order fixed to encrypt-then-MAC).
- RDRAND REAL BUT DISTRUSTED: stuck/failed history, microcode-dependent; never sole entropy, mix + health-check.
- KILLED FICTION: no-flush domains (targeted flush required); EPT-gives-CET (explicit enable); hash-then-encrypt; TPM-pins-microcode-expiry (measure-only + allowlist-reboot); flush-means-durable.

## Final v5
- pku-pcid-cap-pages: PKRU+PCID-tagged PTE keys are the only refs; revoke is targeted INVVPID.
- ept-cet-vpid-cells: module is EPTP+VPID view with CET shadow/ENDBR explicitly enabled; fault on violation.
- iommu-dma-ir-firewall: all DMA fenced by IOMMU, all IRQs remapped and treated as untrusted send.
- nvme-aes-cow-pages: 4K COW append-only with AES-GCM encrypt-then-MAC; flush is hint, not proof.
- tpm-measured-firmware-pin: TPM measures boot/microcode/SMM; SMI/NMI is non-module betrayal fault.
- avx-lanes-scrubbed-vectors: lanes counted not cores; AVX state scrubbed on cell exit, throttle budgeted.
- ipi-rapl-pmu-deadline-mesh: IPI posted-PIR vote with no shared memory; RAPL/PMU/CAT bound joule-time; TSC delta-only.
