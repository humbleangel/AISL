# Swarm review v4 — new-substrate (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (MPK/PKU+PCID, CET domains, DMA+IR firewall, AES torn-proof pages, pinned-microcode measure, AVX lanes, IPI/RAPL/PMU/deadline mesh) attacked in round 2 for TSC trust (virtualizable — use range), missing CAT (LLC eviction DoS), CET-without-MPK readability, encrypt-then-hash ordering, no-update fallback, SMT sibling-as-attacker.

## Round-1 → Round-2 changes
- TSC demoted to range source; CAT partitions + SMT paired-or-off folded into mesh entry.
- Hash-then-encrypt ordering fixed; CET shadow explicitly PKU-sealed; degraded-to-one-core fallback tied in.

## Final v4
- pku-pcid-page-capabilities: single-address-space isolation without TLB flush; PKU key = type tag, PCID = tlb-reach budget.
- ept-cet-module-domains: EPT partitions plus CET shadow-stack/ENDBR so only measured pages execute with intact control flow.
- iommu-dma-ir-firewall: VT-d covers DMA and interrupt-remapping so devices cannot inject spoofed IPIs.
- nvme-aes-torn-proof-pages: hash-then-encrypt content pages with flush-fence and torn-write detect for powercut-recovery.
- tpm-pinned-microcode-measure: PCR quotes bind microcode/SMM version, SMM excluded, degraded-to-one-core if unpatched.
- avx-lanes-first-vectors: AVX lanes are the thread primitive; scalar is the special case.
- ipi-rapl-pmu-deadline-mesh: noncoherent IPI messaging with RAPL joule caps, PMU speculation meter, TSC-range deadlines, CAT partitions, SMT paired-or-off.
