# Swarm review v4 — new-failure (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (merge power-loss + flush-lie into one commit path; add stolen-execution-time-skew as separate integrity break, not degraded performance) attacked in round 2 for reliable-IPI assumption, truthful-PMU assumption, errata-vs-corruption conflation, steal-vs-exhaustion boundary.

## Round-1 → Round-2 changes
- Merge confirmed safe (shared oracle verdict + observer trail); steal/skew kept separate (breaks causality and replay-identity with zero fault, unlike bounded exhaustion).

## Final v4
- compiler-betrayal: Thompson 1984 still holds; unmeasured toolchain can inject what source never said.
- firmware-microcode-betrayal: SMM/ME/microcode execute below only-measured-pages-execute; measurement never sees them.
- silent-corruption: fab errata + bit-flip + FTL remap return wrong data with no fault raised.
- commit-persistence-lie: absorbs power-loss-mid-write and nvme-flush-lie; crash-only-persistence fails when flush is physics-interrupted or device-lied.
- side-channel-leak: speculation-is-effect + shared cache + sibling SMT leak across ept-partition-domains.
- resource-exhaustion: absorbs degraded-not-dead; joule-thermal-headroom and commit-endurance run out under bounded-energy-time-only.
- irreversible-misintent: sealed-intent-before-run was wrong; no undo-window saves it.
- stolen-execution-time-skew: SMM/ME/NMI/MCE steal run-to-completion with no fault; TSC desync plus thermal stretch break causal-tick-first and deadlines-as-handlers.
