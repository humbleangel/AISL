# Swarm review v4 — new-time (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (epoch-now reintroduces global now; page-versioning dropped breaks replay/recovery; stolen-time mechanism missing; deadlines unbounded under stretch; wall range unsigned) attacked in round 2 for resume-restores-wall impossibility, untrappable SMI, slow TPM anchors, unenforceable wall deadlines, unreplayable wall, global epoch.

## Round-1 → Round-2 changes
- Tick-restore split from wall-reanchor; TSC demoted to intra-slice delta; stolen divergence raised as fault continuation; wall readable only as signed range; deadline fused with energy; TPM sparse + hash-chain trail.

## Final v4
- resume-restores-tick-not-wall: persistent image restores causal tick only; wall is re-anchored fresh as a new range after powercut.
- causal-tick-on-send-only: tick advances only when carried in IPI send, never via shared counter, honoring no-coherent-shared-memory.
- tsc-is-delta-not-now: desyncing TSC allowed only for intra-slice deltas on one lane, forbidden as timestamp or ordering source.
- stolen-time-is-fault: APIC-vs-TSC-vs-VMX divergence raises a continuation handler, turning SMI/thermal stretch into control flow.
- wall-is-signed-range-only: wall readable only as [earliest,latest] from NVMe trail plus quorum/TPM anchor, never a point value.
- deadline-is-joule-budget: handlers bound by ticks plus RAPL joules, since thermal stretch makes wall-instant deadlines unenforceable.
- sparse-epoch-with-hash-trail: TPM seals rare per-domain epochs, NVMe hash-chain links ticks between, surviving flush-lie with bounded endurance.
