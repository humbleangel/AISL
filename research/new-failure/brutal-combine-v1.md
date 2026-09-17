# Brutal combine v1 — new-failure (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Failure owns threat models and failure behavior; 8 modes MECE by detector.

## Merges and kills
- SMT-sabotage-as-mode folded (attacker, not mode — manifests via side-channel + stolen-time + exhaustion).
- irreversible-misintent already dead (interface property once sent, no metal trigger).
- commit vs flush-lie boundary hardened: torn-loss = language/OS atomicity fails with honest device (kill-mid-append test); flush-lie = device ACKs durability never provided (ACK-then-power-cut-readback test).
- compiler-pin vs microcode-pin split: separate expiry paths.
- Scrub-trail vs tripwire vs IPI-dissent witnesses split, no shared witness.

## Final merged list (8 survive, trigger-boundary names)
- compiler-betrayal-measured-page-pin: Thompson toolchain lie caught by page-hash fault before execute, microcode-pin kept separate.
- firmware-microcode-betrayal-smi-fault-pin: SMM/ME/microcode as pinned adversary, SMI is fault and rev-expiry faults.
- silent-corruption-scrub-replay-quarantine: errata/bit-flip/FTL-remap wrong-data caught by scrub plus dissenter replay and causal bind.
- commit-torn-loss-cow-causal-fault: crash mid-append caught by COW causal-index gap on resume, overwrite forbidden.
- device-flush-lie-leased-commit-verdict: flush-ACK distrusted, leased NVMe commit plus quorum read-back proves lie.
- side-channel-leak-invitation-only-tripwire: Spectre/RAPL/PMU/AVX leaks contained by SMT-off plus tripwire and per-core scrubbed sweep.
- resource-exhaustion-joule-deadline-abort: depletion fires dual timer-plus-joule budget to bounded abort, no retry.
- stolen-time-skew-tsc-dissent-vote: SMI steal/TSC skew is IPI dissent vote on delta, never wall-clock fault.
