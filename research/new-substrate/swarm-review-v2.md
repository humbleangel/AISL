# Swarm review v2 — new-substrate (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- mmu-as-primitive: restates 80-year flat linear memory, 4KB pages treated as neutral, not as typed enforceable primitive.
- vt-x-guests: a fence not ground — VMs are second-order isolation theater, not something a language can stand on directly.
- nvme-persistence: shallow block worship — LBA + files re-imports location-addressing, hides crash atomicity the language needs.
- tpm-measured-boot: sidecar not substrate — attestation chip verifies boot, gives no memory/compute model to build on.
- simd-vectors-first: right instinct, shallow slogan — still scalar-control + vector-assist unless scalar becomes the degenerate case.
- content-addressed-store: only anti-von-Neumann idea here, but dangling software emulation with no link to pages/NVMe/TPM.

## Completed sublist v2
- page-capability-mmu: page tables as typed unforgeable capabilities, language pointers are MMU grants, not integers.
- ept-partition-domains: VT-x EPT + 4 rings as language-level actors, replaces Unix process/ring duality with hardware partitions.
- nvme-persistent-objects: NVMe namespaces as single-level crash-atomic object heap, no files/LBAs visible to language.
- tpm-measured-bootstrap: hand-checked measured chain from reset to self-hosting compiler, answers Thompson 1984 directly.
- simd-vectors-first: AVX vectors are the default numeric type, scalar is length-1, uses the CPU instead of fighting it.
- content-addressed-pages: pages and NVMe extents named by hash, dedup/verify free, breaks location-equals-identity assumption.
- cache-core-mesh: few strong cores + shared L3/SMT/side-channels as explicit locality/fence primitive, no flat thread illusion.
- time-energy-budget: invariant-TSC + APIC + RAPL/thermal as scheduling base, language reasons about finite hot clocks, not infinite CPU.
