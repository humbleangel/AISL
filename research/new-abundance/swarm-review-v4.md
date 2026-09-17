# Swarm review v4 — new-abundance (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (keep 6, kill idle-background as folklore) attacked in round 2 for leftover Unix (idle core assumes global-now + free background time), coherent-tally voting, coherent-global memo, unmeasured disposable code cost, uncapped precompute/monotonic vs thermal/powercut.

## Round-1 → Round-2 changes
- Idle/background reverify becomes explicit bounded lease (no free background time).
- N-version vote moves to IPI mesh (no shared tally).
- Memo shards per-core, DRAM-first (no coherent global cache, no infinite endurance).
- Disposable code bound to EPT arena + PCR measure; precompute capped by RAPL lease; snapshots append-only crash-safe.

## Final v4
- disposable-ept-variant-arena: generate per-actor code only inside measured EPT partition, discard on epoch fault; no binaries ever persist.
- proof-carries-or-faults: generated pages execute only with inline proof checked as call, else language trap faults.
- ipi-vote-no-shared-memory: N variants vote via IPI noncoherent messages judged by sibling dissenter, never shared mutable tally.
- per-core-content-memo-shards: memo is per-core content-addressed DRAM shards, dodging SMT/side-channel and TLB/commit costs.
- rapl-capped-precompute-lease: precompute runs only under explicit RAPL joule lease with deadline-handler kill.
- monotonic-append-snapshot-log: snapshots only monotonic NVMe appends replayable after powercut, no overwrite.
- explicit-reverify-no-background: no idle/background reverify; reverify only as bounded run-to-completion job.
