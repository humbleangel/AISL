# Swarm review v3 — new-substrate (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. SUBSTRATE owns hardware primitives only; policy/typing/budgets/verdicts ceded.

## Collisions resolved
- page-capability-mmu vs constraint vs pairing: CONSTRAINT owns rule, PAIRING owns semantics (PTE = type); SUBSTRATE keeps MMU/TLB/PCID enforcement mechanism.
- ept-partition-domains vs abundance/inversion: SUBSTRATE owns EPT mechanism; ABUNDANCE owns use-policy; INVERSION owns structural claim.
- nvme-persistent-objects vs pairing/constraint/inversion: PAIRING owns equation, CONSTRAINT owns crash rule, INVERSION owns slogan; SUBSTRATE owns NVMe mechanism (namespaces/zones/append).
- content-addressed-pages vs inversion/subtraction: SUBTRACTION owns negation of files, INVERSION owns pages=heap slogan; SUBSTRATE owns content-hashing mechanism.
- tpm-measured-bootstrap vs oracle/abundance/organization/domain: ORACLE owns verdicts, ABUNDANCE owns chain, ORGANIZATION owns log, DOMAIN owns rig; SUBSTRATE keeps TPM primitive (extend/seal/quote).
- simd-vectors-first vs pairing/scale: PAIRING owns lane=thread equation, SCALE owns count; SUBSTRATE owns AVX register/layout primitive.
- cache-core-mesh vs scarcity/constraint/scale: CONSTRAINT owns no-coherence rule, SCARCITY owns budgets, SCALE owns pinning; SUBSTRATE owns physical mesh description.
- time-energy-budget: CEDED ENTIRELY — SCARCITY owns joule budget, PAIRING owns power equation, TIME owns deadlines, OBSERVER owns judgment.

## New insights
- IOMMU/VT-d missing: without DMA firewall, page capabilities are bypassable by devices; DMA-capability is load-bearing.
- MPK/PKU missing middle: protection-keys give intra-address-space light domains with no TLB flush between MMU and EPT weights.
- APIC/IPI as IPC: with no coherent shared memory, cross-core substrate must be IPI/MSI-X message mesh.
- PMU/LBR/PEBS as ports: observers need hardware trace ports; substrate exposes them, never judges.
- CET + AES-NI + RDRAND missing: shadow-stack/ENDBR for intent-integrity, AES/RDRAND as in-silicon crypto/random anchor.
- Firmware/SMM as hostile device: microcode/UEFI/SMM quarantined behind EPT/IOMMU, never trusted base.
- Sensors only: invariant-TSC + TSC-deadline + RAPL are raw sensors; budgets live in scarcity/time.

## Completed sublist v3
- page-capability-mmu: MMU+PCID/TLB as sole capability enforcement engine, policy and typing ceded outward.
- ept-partition-domains: EPT as hardware partition/clone mechanism, isolation policy ceded to abundance.
- iommu-dma-firewall: VT-d DMA remapping as mandatory MMU complement so devices cannot bypass capabilities.
- persistent-content-pages: merged content-hashed page/object store spanning RAM and NVMe zones.
- tpm-seal-quote-primitive: narrowed to PCR-extend/seal/quote hardware primitive, chain/verdict/log ceded outward.
- simd-vectors-first: AVX/SIMD registers and contiguous-vector layout as first-class data substrate.
- ipi-noncoherent-mesh: APIC-IPI mesh over explicitly non-coherent cores; cache described only as hostile shared residency.
