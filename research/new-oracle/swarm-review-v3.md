# Swarm review v3 — new-oracle (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. Oracle keeps verdicts only; all mechanisms ceded.

## Collisions resolved
- hand-seed-double-compile vs failure/scarcity/forgetting: FAILURE owns threat, SCARCITY owns economics, FORGETTING owns stance; ORACLE owns verdict procedure.
- tpm-pcr-verdict (heaviest collision, 8 items): SUBSTRATE owns mechanism, OBSERVER owns live quote, PAIRING owns semantics, ORGANIZATION owns log, DOMAIN owns use-case; ORACLE keeps PCR-equality check.
- mmu-fault-verdict vs observer/pairing/inversion/substrate: SUBSTRATE owns mechanism, OBSERVER owns witness, PAIRING/INVERSION own semantics; ORACLE keeps behavior-matches-spec verdict.
- vtx-replay-identity vs time/substrate/interface/domain/abundance: TIME owns replay mechanism, SUBSTRATE owns EPT, INTERFACE/DOMAIN own labs; ORACLE keeps identity-equality verdict.
- cache-silence-verdict vs observer/scarcity/substrate/failure/pairing: FAILURE owns threat, SCARCITY owns budgets, SUBSTRATE owns mesh, OBSERVER owns live dissent; ORACLE keeps post-slice no-residue check.
- rapl-energy-budget-proof: PAIRING owns semantics, SUBSTRATE owns budget object, SCARCITY owns headroom, OBSERVER owns online judge; ORACLE keeps offline proof-check.
- nvme-powercut-recovery vs substrate/pairing/constraint/observer/failure: SUBSTRATE owns objects, CONSTRAINT owns rule, OBSERVER owns trail, FAILURE owns threat; ORACLE keeps pull-plug verdict.

## New insights
- Off-machine placement: verdicts running on the judged machine are circular; oracle must live in ring-minus-one / trust-bootstrap-rig.
- Liar-detector missing: RAPL/PMU/NVMe-flush can lie under throttle/FUA-ignore; need cross-check vs wall-range + diode + read-back.
- Determinism envelope missing: bit-identity is false on AVX/thermal/SMT; need allowed-nondeterminism envelope verdict.
- Judge-the-judge missing: verdicts themselves must be content-addressed pages with hash replay.
- EPT-vs-MMU coherence missing: nested paging can diverge silently; no item checks MMU==EPT identity.
- Irreversibility gate missing: destructive page-diff needs pre-commit recoverability check via COW snapshots.

## Completed sublist v3
- hand-seed-quorum-verdict: two independent hand seeds must bit-match to break Thompson bootstrap circle.
- pcr-equality-verdict: check-only quoted PCRs equal expected measurement, all chain/mechanism ceded.
- fault-behavior-verdict: hardware MMU fault trace must match contracts-as-spec, witness ceded to observer.
- replay-identity-verdict: VT-x trapped replay must preserve actor identity hash, replay mechanism ceded to time.
- residue-silence-verdict: post-slice sibling-core scan must show no cache/spec residue, live judging ceded to observer.
- energy-proof-check: offline RAPL+PMU proof verifies against time-energy-budget, online judging ceded to observer.
- powercut-recovery-verdict: mid-write powercut must recover to last crash-consistent page version only.
- liar-crosscheck-verdict: RAPL/PMU/NVMe-flush claims cross-checked against wall-range and read-back to catch lying hardware.
