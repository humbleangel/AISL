# Brutal combine v1 — new-substrate (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Substrate owns hardware primitives only.

## Merges and kills
- PKU-alone killed (WRPKRU unprivileged — dies without run-to-completion + no-syscall + no-unmeasured-exec).
- EPT-auto-gives-CET killed (guest CET + shadow pages + INVVPID/INVEPT must be explicit, costed).
- Allowlist-DMA-by-default killed (default dmaless, vetted DMA only, faults are dissent/witness).
- Hash-then-encrypt killed (encrypt-then-MAC; flush-lie assumed; torn-loss = reverify fault).
- TPM-secures-firmware killed (TPM measures only; firmware/SMM pinned adversary; sparse epochs).
- No-flush domains killed (targeted flush required); wall-clock killed (delta+lease+range only); RDRAND-as-seed killed (mix + health-check).
- No new primitives: all 7 tightened to fail-closed, leased, counted, scrubbed, re-verified forms.

## Final merged list (7 survive)
- pku-pcid-subgrant-arena: intra-VA subgrant only, PCID-tagged, CET-backed, zeroed on exit.
- ept-cet-vpid-disposable-cell: run-to-completion cell with explicit CET + INVVPID-costed clone.
- iommu-ir-deny-default-firewall: dmaless default, vetted DMA only, faults as witness.
- nvme-cow-encrypt-then-mac-leased: COW-append, encrypt-then-MAC, flush-lie assumed, endurance-leased reverify.
- tpm-measure-only-no-trust-pin: sparse epoch measure, SMI/firmware as fault, expiry-pinned microcode.
- avx-counted-zero-scrubbed-lanes: 128-resident ceiling, kmask-select, same-core zero, residue faults.
- ipi-joule-deadline-ballot-mesh: RAPL lease + TSC-delta + PMU judge, IPI dissent vote, no wall clock.
