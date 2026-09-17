# Swarm review v3 — new-time (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3.

## Collisions resolved
- trapped-replay vs oracle/vtx-replay-identity: TIME owns record/replay mechanism (trap nondeterminism inputs); ORACLE owns verdict.
- trapped-replay vs abundance/scale/observer tracing: capture owned by ABUNDANCE+OBSERVER; TIME owns replay semantics only.
- page-versioned-history vs abundance/substrate/interface: DROPPED from time; COW to ABUNDANCE, storage to SUBSTRATE, commit syntax to INTERFACE.
- resume-not-boot vs constraint/substrate/scale: crash-only rule to CONSTRAINT, NVMe objects to SUBSTRATE, measured boot to SCALE; TIME owns temporal consequence (no boot-time, only resume-epoch).
- epoch-now vs failure/oracle/substrate: crash-correctness to FAILURE, TPM+NVMe mechanism to SUBSTRATE/ORACLE; TIME owns epoch semantics.
- causal-tick-first vs scarcity/scale: tick definition to TIME; budget to SCARCITY; no-tick-scheduler consequence to SCALE.
- wall-as-range vs substrate: range semantics to TIME; enforcement to SUBSTRATE.
- deadlines-as-handlers vs interface/constraint/scarcity: handler semantics to TIME; expression to INTERFACE; run-to-completion rule to CONSTRAINT; joule enforcement to SCARCITY.

## New insights
- No global now: TSC desyncs across cores, APIC per-core, HPET slow, VMX timer differs — time is per-core-local, merge never sync.
- SMI/SMM steals time invisibly: cycles vanish with no log; clock must expose stolen-time as first-class degraded interval.
- Cycles are not seconds: AVX downclock + thermal throttle stretch time; need dual clock (ticks + joules).
- Tickless time: with run-to-completion there is no periodic tick; time exists only as next-deadline.
- Wall is adversary input: TSC/wall can be virtualized, desynced, replayed; only causal tick + epoch trusted.
- Per-lane forked time: million lanes each need own causal clock with vector-merge; global total order unwanted.
- Time as lease: page/object expiry at tick-N gives hardware-enforced temporal capability, reclamation without GC/scheduler.

## Completed sublist v3
- resume-not-boot: boot is just first resume; time never starts at zero.
- causal-tick-first: monotonic causal tick is primary clock, wall is secondary annotation.
- wall-as-range: wall time is interval with error bounds, never a point value.
- trapped-replay: determinism by trapping nondeterminism sources; attestation ceded to oracle.
- deadlines-as-handlers: missed deadline fires control-flow handler, not a type error.
- epoch-now: persistent monotonic epoch surviving power loss; storage/mechanism ceded outward.
- no-global-now: per-core ticks with visible stolen/stretched time (SMI, AVX, thermal); cores merge, never sync.
