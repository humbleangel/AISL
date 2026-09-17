# Swarm review v5 — new-pairing (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 reality check.

## Round-2 kills
- Per-lane-thread fiction killed (k-masks predicate only, no per-lane faults/MMUs).
- Userspace VMFUNC killed (VMX non-root ring0 only); per-VPID BTB scrub precision killed; TPM-as-MMU killed; RAPL-as-fault killed (advisory counter + coarse limit).
- PIR counter/queue language killed (256-bit coalescing bitmap, lossy slots).
- Revoke fixed: BOTH INVEPT+INVVPID, not one.

## Final v5
- tsc-is-delta: TSC desyncs so language exposes only deltas/deadlines, never now.
- pku-is-subgrant: PKU keys carve sub-capabilities inside one EPT view without TLB flush.
- cet-is-return: CET shadow stack makes continuations unforgeable faults, not values.
- kmask-is-select: k-masks predicate lanes; no per-lane faults, no lane-as-thread fiction.
- pt-lbr-is-witness: PT/LBR trace is the replayable proof that carries or faults.
- rapl-is-lease: RAPL joules bound execution where time cannot be trusted (lease, not policeman).
- ipi-pir-is-ballot: posted-PIR coalesces so votes are lossy slots requiring re-post; no shared-memory quorum.
