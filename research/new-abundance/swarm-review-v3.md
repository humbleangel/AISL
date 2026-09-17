# Swarm review v3 — new-abundance (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. ABUNDANCE owns cheap-supply patterns only; mechanisms/verdicts ceded.

## Collisions resolved (abundance drops or narrows each)
- vm-per-actor-isolation: SUBSTRATE owns EPT mechanism, DOMAIN owns sandbox use-case; abundance drops it (isolation is cost, not abundance).
- cow-time-travel-snapshots: TIME owns version semantics, SCALE owns clone op, SUBSTRATE owns store; abundance is consumer only.
- measured-bootstrap-chain: SUBSTRATE owns measure, ORACLE owns verdict, ORGANIZATION owns log; abundance drops it.
- selective-hardware-tracing: OBSERVER owns mechanism, SCALE owns honesty policy; abundance keeps no tracer.
- precompute-with-thermal-budget: SCARCITY owns budget, SUBSTRATE owns enforcer, ORACLE owns proof; abundance owns precompute workload only.
- proof-carrying-generation: PAIRING owns binding, ORACLE owns check, ORGANIZATION owns gate; abundance owns cheap proof emission only.
- disposable-specialized-code: INVERSION owns placement, SUBTRACTION owns no-link rule, DOMAIN owns CI instance, SCALE owns fan-out; abundance owns throwaway lifecycle.

## New insights
- EPT-clone abundance: whole-machine fork in ms makes N-version execution and pre-merge trial runs free; never test in place.
- NVMe-capacity abundance: never recompute — content-addressed memo of every pure result, proof, trace, compilation; disk cheaper than joules.
- Address-bit abundance: 64-bit space + monotonic never-reuse allocation kills UAF/reuse, makes snapshots trivial; wasting addresses is correct.
- Idle-lane abundance: AVX/SMT/sibling cores idle during single-task run — spend on shadow execution, proof-checking, scrubbing, re-fuzzing.
- Cold-joule abundance: thermal headroom is time-shiftable — precompute when cold/idle, throttle when hot; RAPL makes it measurable.
- Generation-diversity abundance: generate 3 diverse variants and vote — generation cheap, dissent verdict strong.
- Fault/trap abundance: COW faults, EPT exits, page-version forks cheap enough as normal control; only the verdict is expensive.

## Completed sublist v3
- disposable-specialized-code: generate single-site single-epoch code and discard, no generic retained libraries.
- proof-carrying-generation: every generated artifact emits a cheap checkable certificate, verification stays fast and scarce.
- n-version-generation-vote: generate 2-3 diverse implementations per intent and compare, buying trust with cheap generation.
- idle-core-background-reverify: spend spare cores/lanes continuously re-compiling, re-fuzzing, re-checking live pages.
- content-addressed-memo-cache: NVMe-backed memo of results/proofs/compiles by page hash, never compute twice.
- precompute-with-thermal-budget: spend cold/idle joules precomputing tables and specializations, stop when hot.
- monotonic-snapshot-abundance: never reuse addresses, fork cheap COW branches for trial/replay, mechanism owned by time/scale/substrate.
