# Swarm review v3 — new-scale (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. SCALE owns numbers/endpoints only; rules and mechanisms ceded.

## Collisions resolved
- single-task-no-scheduler vs subtraction/constraint: rule owned outward; SCALE owns endpoint N=1.
- pinned-task-per-core vs constraint/subtraction: no-preemption rule owned outward; SCALE owns N=cores.
- million-lanes-not-tasks vs pairing/substrate: definition to PAIRING, vectors to SUBSTRATE; SCALE owns count ~1M.
- ept-whole-machine-clone vs substrate/abundance: EPT to SUBSTRATE, snapshot/actor to ABUNDANCE; SCALE owns fan-out number.
- minimal-measured-boot vs substrate/abundance/time: TPM/chain to SUBSTRATE/ABUNDANCE, resume semantics to TIME; SCALE owns size/time number.
- maximal-honest-trace vs abundance/observer: mechanism/policy to ABUNDANCE, witness/trail to OBSERVER; SCALE owns bytes/sec bound.
- all-cores-avx-thermal-limit vs scarcity/pairing/observer/substrate: budget to SCARCITY, abstraction to PAIRING, judgment to OBSERVER; SCALE owns stress extreme.

## New insights
- Vertical-only cloning: missing horizontal scale — same measured image on N physical machines with identical PCRs.
- Downward scale missing: bookend always up; missing degraded-to-1-core / 1-page survival extreme.
- Coherence worst-case missing: all-cores-one-cacheline storm complements no-sharing ideal; quantifies mesh/TLB/IPI ceiling.
- Fault-path throughput missing: faults-per-second storm extreme for mmu-fault-verdict path.
- Energy floor missing: minimal joules-per-intent (RAPL-measured cost of one intent).

## Completed sublist v3
- scale-one-to-n-pinned: merges scheduler-less endpoints into one measurable axis 1..core-count, rule ceded outward.
- million-lanes-count: SIMD width extreme as pure count, lane definition to pairing, vectors to substrate.
- clone-fanout-count: whole-machine clone as 100-way fan-out number, EPT to substrate, CoW to abundance.
- minimal-resume-measured: smallest measured resume bytes/microseconds, TPM/chain to substrate/abundance, resume semantics to time.
- maximal-trace-bytes-honest: largest lossless trace bytes/sec bound, mechanism to abundance, trail to observer.
- all-cores-avx-thermal-ceiling: all-cores AVX at RAPL/thermal ceiling as power demonstrator, budget to scarcity.
- degraded-to-one-core: new downward extreme — system still runs with N-1 cores lost, quantifies few-strong-cores survival.
- fleet-identical-measured: new horizontal extreme — same measured image attested on N physical machines.
