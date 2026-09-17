# Swarm review v3 — new-failure (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. FAILURE owns threat models and failure behavior; mechanisms/proofs/rules ceded outward.

## Collisions resolved
- compiler-betrayal vs oracle/pairing/abundance/substrate/scale/organization: FAILURE owns Thompson-1984 threat model; all others own detection/attestation/log mechanism.
- firmware-microcode-betrayal vs observer/scarcity: FAILURE owns SMM/ME/microcode-malicious-or-buggy failure; OBSERVER owns watcher.
- silent-corruption vs substrate/abundance/oracle: FAILURE owns bitflip/rowhammer/rot model; others own checksum/snapshot/verdict.
- power-loss-mid-write vs oracle/constraint/pairing/substrate/time: FAILURE owns torn-write failure; ORACLE owns cut-test proof; CONSTRAINT/SUBSTRATE/TIME own persistence rules.
- side-channel-leak vs pairing/oracle/scarcity/substrate/abundance: FAILURE owns leak-as-failure taxonomy; others own cause, budget, verdict, isolation.
- resource-exhaustion vs scarcity/constraint/pairing/substrate/scale/time/interface: others own budgets and rules; FAILURE owns exhaustion behavior and limp response. Absorbs old degraded-not-dead policy.
- irreversible-misintent vs observer/interface/organization: those own prevention/comparison/gating; FAILURE owns committed-irreversible failure class.
- degraded-not-dead vs constraint/time: direct crash-vs-limp conflict; DROPPED as standalone, crash-only keeps persistence rule, limp folds into resource-exhaustion.

## New insights
- nvme-flush-lie: device ACKs flush but data still in volatile cache/FTL remap; survives lab recovery yet dies on real cut.
- smm-time-theft + tsc-desync: SMM/ME/VM-exit stalls steal cycles with no fault; wall/causal/deadlines silently lie.
- attestation-brittleness: PCR too strict bricks honest upgrade/resume, too loose forges identity; identity itself fails availability.
- errata-at-birth + sibling-sabotage: fab erratum mis-executes despite attestation; SMT sibling actively poisons ports/cache/speculation (Byzantine timing faults, not passive leak).

## Completed sublist v3
- compiler-betrayal: canonical Thompson 1984 toolchain inserts undetectable backdoor, root of bootstrap-trust.
- firmware-microcode-betrayal: SMM/ME/microcode/NVMe-FTL below ring-0 lies or mis-executes, invisible to OS checks.
- silent-corruption: cosmic/rowhammer/NVMe-rot flips bits with no trap, needs content-address + snapshot recovery.
- power-loss-mid-write: honest device plus dead power yields torn pages, defines crash-only correctness.
- nvme-flush-lie: dishonest device reports persisted-but-not, survives naive recovery tests, needs pull-the-plug proof.
- side-channel-leak: speculation/cache/RAPL/PMU/AVX-ports exfiltrate across lanes, distinct from active sabotage.
- resource-exhaustion: OOM/lane/joule/thermal depletion must degrade-not-die, absorbs old degraded-not-dead policy.
- irreversible-misintent: human typo plus AI-agent drift commits unrecoverable page-diff, needs veto/dual-intent gating.
