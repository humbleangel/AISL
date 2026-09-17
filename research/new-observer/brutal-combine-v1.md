# Brutal combine v1 — new-observer (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Round-2 reality check applied (VMEXIT-storm, PT-flood, package-RAPL, 8-bit vectors, fault-only IOMMU, endurance, same-core AVX).

## Merges and kills
- Continuous-poll sentry killed (VMEXIT storm + stale TLB); batched budgeted bind-check only.
- General-trace witness killed (GB/s flood); own-lane bounded witness only.
- Precise-per-lane-joule judge killed (package-granular fiction); lease + dissent only.
- Mesh killed as topology (O(n²) breaks thermal); 2-vote ballot only.
- Payload-inspection killed (IOMMU fault-only fiction); tripwire only.
- Unbounded-trail killed (endurance + flush-lie); capped COW + sparse epoch only.
- Cross-core-sweep killed (architecturally unreadable); same-core exit scrub only.

## Final merged list (6 survive)
- ept-ad-bind-sentry: batched EPT A/D bind-check, no traps, respects TLB budget, faults on unbound page.
- bounded-ptwrite-witness: own-lane PTWRITE only, byte-budgeted, disposable-cell scoped, replay-capped.
- rapl-lease-ipi-ballot: RAPL lease + PMU hint + 2-dissenter IPI vote, dissent-not-fault, fail-closed.
- iommu-fault-firewall-tripwire: DMA/IR fault-only witness, no snoop claim, unfenced-DMA faults.
- endurance-capped-cow-trail: COW-append only, endurance-capped, sparse TPM epoch hash, DRAM-snap/NVMe-commit.
- same-core-avx-scrubber: SMT-off same-core VZERO on exit, kills SIMD residue, nothing cross-core.
