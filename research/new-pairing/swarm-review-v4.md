# Swarm review v4 — new-pairing (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 direction (EPT views, posted sends) attacked in round 2 for reliability lies (APIC coalescing), EPT-switch cost, missing interrupt-remapping, residue survival, alias-measurement drift, powercut-vanishing sends.

## Round-1 → Round-2 changes
- Loss made explicit and bounded everywhere.
- Revoke made physical (flush + scrub).
- Timers bounded, SMI exiled outside the module system.

## Final v4
- module-is-eptp-vpid-view: module is EPT mapping + VPID of same GPA space, switch via VMFUNC not exit; keeps single-address-space while making page-table-is-type enforceable.
- send-is-posted-pir-slot: cross-module/cross-core send is APICv posted-PIR enqueue handled at yield, never preempt; fits run-to-completion + ipi-noncoherent-mesh.
- grant-is-temporary-ept-alias: sharing is time-bounded EPT remap of capability page, not coherent queue; satisfies no-coherent-shared-memory + explicit-allocation.
- timer-is-bounded-deadline-send: TSC/APIC timer fires as queued send with energy-time bound, not async steal; satisfies bounded-energy-time + deadlines-as-handlers.
- device-irq-is-iommu-filtered-send: NVMe/MSI-X IRQ valid only after interrupt-remapping + IOMMU check; removes ambient device authority.
- smi-nmi-is-nonmodule-betrayal: SMI/NMI/ME cannot be modules or sends; treated as firmware-microcode-betrayal.
- revoke-is-vpid-tlb-flush: ungrant requires VPID-invalidate + L1D/BTB scrub; makes residue-silence + speculation-secrecy physical, not policy.
