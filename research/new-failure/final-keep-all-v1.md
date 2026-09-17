# Final keep-all v1 — new-failure (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (8) with brutal trigger-boundary names (8). 8 modes MECE by detector.

## Round 1 — mode fusion (V5 breadth + brutal trigger-boundary)
- compiler-betrayal + measured-page-pin: Thompson/miscompile/UB-elision/target-skew breadth + diverse-rebuild page-hash pin gate (static + JIT). U1 pin depends on U2 pin integrity (documented dependency).
- firmware-microcode-betrayal + smi-fault-pin: full ring -3..-1 actor set + SMI-count/time + microcode-rev + fault-semantics pins; cross-vote (no single-source trust).
- silent-corruption + scrub-replay-quarantine: CPU/mem/FTL silent wrong-data + scrub→replay-classify→quarantine pipeline (poison retained for forensics).
- commit-torn-loss + cow-causal-fault: atomicity/causality tear on honest devices + COW no-overwrite-until-complete + causal-DAG fault matrix (finite edges).
- device-flush-lie + leased-commit-verdict: durability-ACK lie (contract lie incl. honest volatility) + lease window + power-cycle proof; ACK-vs-media test.
- side-channel-leak + invitation-only-tripwire: Spectre/Meltdown/Probe/RAPL/PMU/AVX breadth + default-deny + canary/statistical tripwire (MFENCE≠LFENCE noted).
- resource-exhaustion + joule-deadline-abort: all budgets (mem/FD/thread/disk/joule/time) + numeric cap + deterministic abort (OOM-killer replaced).
- stolen-time-skew + tsc-dissent-vote: SMI/NMI/hypervisor/IRQ steal + TSC skew + time+progress vote (APERF/MPERF ratio); detect-and-degrade, never restore.

## Commit vs flush-lie boundary CONFIRMED (double dissociation)
- TEST A (torn, honest device): multi-part commit + crash at every causal edge → replay must show all-old/all-new. Single-sector verdict passes → failure is torn, not lie.
- TEST B (lie, trivial atomicity): single-sector FUA + power-cut + verdict read → fail means ACK lied. COW replay clean → failure is lie, not tear.
- A fails while B passes and vice versa → separate modes mandatory. FTL-remap triage: no-power-loss + scrub dissent = silent-corruption; post-ACK power-cycle dissent = flush-lie.

## Irreversible-misintent: FINAL BURIAL (7 reasons recorded)
No metal trigger/sensor; reducible to U4/U5/U7/U1; confuses UX regret with metal contract; SMT-fold precedent; unreproducible oracle; would break MECE/count; mitigations (COW undo, lease confirm) live atop retained modes. Revisit only on architectural intent/undo primitive with sensor. SMT-sabotage likewise folded as attacker tactic.

## Round 2 — attacks + fixes
- Measurer trusts firmware → U1 depends on U2 explicitly; defense in depth.
- JIT unpinnable → measure-then-execute gate for dynamic pages.
- SMI/MSR spoofed → cross-vote (time + behavior oracle), never single source.
- Scrub lumps 3 physics → kept deliberately: same detector (redundant compare), subtype tags in log.
- COW vs cache-reorder → barriers/epochs + causal-DAG bound.
- Verdict reads served by liar → power-cycle proof + replica vote; lease bounds window.
- Invitation impossible on shared x86 → least-privilege + isolation-by-default + canary/statistical tripwire.
- Abort narrows budgets → joule/deadline are instances; all budgets share quota+abort discipline.
- Vote freezable → external/wall witness + progress ratio + safe-mode degrade.
- Pairwise detector-disjointness verified across all 8 (build/load, below-OS allowance, content scrub, crash replay, power-cycle verdict, probe tripwire, budget trip, time vote).

## Final (8)
- compiler-betrayal-measured-page-pin: Thompson toolchain lie caught by page-hash fault before execute.
- firmware-microcode-betrayal-smi-fault-pin: SMM/ME/microcode as pinned adversary; SMI is fault and rev-expiry faults.
- silent-corruption-scrub-replay-quarantine: errata/bit-flip/FTL wrong-data caught by scrub plus dissenter replay and quarantine.
- commit-torn-loss-cow-causal-fault: crash mid-append caught by COW causal-index gap on resume.
- device-flush-lie-leased-commit-verdict: flush-ACK distrusted; leased commit plus quorum read-back proves lie.
- side-channel-leak-invitation-only-tripwire: Spectre/RAPL/PMU/AVX leaks contained by SMT-off plus tripwire and scrubbed sweep.
- resource-exhaustion-joule-deadline-abort: depletion fires dual timer-plus-joule budget to bounded abort, no retry.
- stolen-time-skew-tsc-dissent-vote: SMI steal/TSC skew is IPI dissent vote on delta, never wall-clock fault.
