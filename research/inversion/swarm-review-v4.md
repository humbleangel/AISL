# Swarm review v4 — inversion (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (keep 6, add speculation-only-by-invitation + persistence-is-default) attacked in round 2 for budget overflow (9 entries), pairing-duplicate faults entry, firmware-is-foundation leftover, who-defines vs what-metal-is distinction, missing speculation-default and volatile-default truths.

## Round-1 → Round-2 changes
- faults-are-control-flow dropped as pairing-duplicate; errors-only-as-faults covers the philosophy.
- kernel-is-guest broadened to everything-below-language-is-guest (SMM/ME/firmware/microcode included).
- language-defines-traps vs metal-is-vocabulary kept as distinct (who-defines vs what-metal-is).

## Final v4
- language-defines-traps: language assigns meaning to every trap/fault/VMEXIT; CPU only emits events.
- errors-only-as-faults: no errno/return-code/in-band errors; all nonlinear failure is a fault continuation.
- everything-below-language-is-guest: kernel, drivers, SMM/ME/firmware/microcode all run as EPT-confined guests of the language host.
- code-follows-data: never haul pages to code; generate/disposable-specialize code where the live pages already are.
- upcalls-only-no-downcalls: code never calls down to kernel/services; lower layers only upcall into pre-allocated handlers.
- metal-is-vocabulary-not-drivers: MMU/EPT/IOMMU/AVX/PMU/RAPL/TSC/TPM/APIC are language words, never abstracted behind driver layers.
- speculation-only-by-invitation: nothing speculates/shares-predictor by default; language explicitly marks speculation-allowed regions.
- persistence-is-default-volatile-is-explicit: pages survive powercut unless annotated ephemeral; volatility must be requested.
