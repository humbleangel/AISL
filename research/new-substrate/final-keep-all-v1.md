# Final keep-all v1 — new-substrate (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (7) with brutal FINAL (7) pair-by-pair; every token preserved. Substrate owns hardware primitives only.

## Round 1 — pair fusion
- pku-pcid-cap-pages + pku-pcid-subgrant-arena → pku-pcid-cap-subgrant-arena-pages: cap-check + delegation + arena lifetime + page granularity + 16-key/4096-PCID ceilings + WRPKRU-gate + INVPCID still required (no-flush fiction killed).
- ept-cet-vpid-cells + ept-cet-vpid-disposable-cell → ept-cet-vpid-disposable-cells: plural cells + disposable default; EPT≠CET-auto fix (explicit enable + shadow pages); INVEPT/INVVPID on teardown.
- iommu-dma-ir-firewall + iommu-ir-deny-default-firewall → iommu-dma-ir-deny-default-firewall: DMA scope + deny-default + IR separate; ATS disabled/allow-listed; IOTLB/IE invalidate on unmap, quarantine before free reuse.
- nvme-aes-cow-pages + nvme-cow-encrypt-then-mac-leased → nvme-aes-cow-encrypt-then-mac-leased-pages: AES-NI + COW + encrypt-then-MAC order-fix + epoch lease/rekey; mixed-entropy nonces; keys scrubbed.
- tpm-measured-firmware-pin + tpm-measure-only-no-trust-pin → tpm-measure-only-no-trust-firmware-pin: PCR extend + pin digests + seal policy + external verify; RDRAND/TPM-RNG never sole; sparse re-measure.
- avx-lanes-scrubbed-vectors + avx-counted-zero-scrubbed-lanes → avx-counted-zero-scrubbed-lanes: lane budget ceiling + zero-scrub on every switch incl. fault paths; vzeroupper/XRSTOR gate; vectors in why-line.
- ipi-rapl-pmu-deadline-mesh + ipi-joule-deadline-ballot-mesh → ipi-rapl-pmu-joule-deadline-ballot-mesh: sensors + joule unit + deadline + sparse-epoch ballot + capped events; ballot sparse (per 10ms/miss), mesh degree-bound; scheduler policy above composes primitives.

## Round 2 — attacks + fixes
- Cap = hardware capability fiction → fixed: software cap table + vetted trampoline + binary scan + generation recycle.
- EPT auto CET / VPID isolation fiction → fixed: explicit enable + shadow pages + invalidates.
- IOMMU-on = safe fiction → fixed: ATS/PASID + invalidate + fault log + deny-default.
- AES = durability fiction → fixed: order-fix + nonce/epoch/lease + rekey/quarantine.
- TPM pin = unhackable fiction → fixed: measure-only + external verify + rotation.
- Scrub-once fiction → fixed: every-switch scrub + budget ceiling + #UD fallback.
- RAPL/PMU precise + ballot consensus fiction → fixed: filtered estimate + capped events + sparse ballot + backoff.
- No kills, no merges beyond pairwise fuse; all 7 survive with fixes as scope notes.

## Final (7)
- pku-pcid-cap-subgrant-arena-pages: PKU keys + PCID tags for page-granular caps delegable via narrowing subgrant inside arena lifetime.
- ept-cet-vpid-disposable-cells: EPT isolation + explicit CET shadow-stack/IBT + VPID tags with disposable teardown by default.
- iommu-dma-ir-deny-default-firewall: VT-d DMA remap + Interrupt Remapping tables default-deny with IOTLB/IE invalidate on unmap.
- nvme-aes-cow-encrypt-then-mac-leased-pages: NVMe COW pages with AES encrypt-then-MAC order-fix and epoch lease/rekey, mixed-entropy nonces.
- tpm-measure-only-no-trust-firmware-pin: TPM PCR-measured firmware pinned to expected digests with external verify + seal policy, never local trust.
- avx-counted-zero-scrubbed-lanes: AVX lane budget ceiling with zero-scrub on every switch including fault paths.
- ipi-rapl-pmu-joule-deadline-ballot-mesh: IPI mesh with TSC-deadline, RAPL/PMU sensors as joule accounting, sparse-epoch ballot vote.
