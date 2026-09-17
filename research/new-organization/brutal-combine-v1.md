# Brutal combine v1 — new-organization (additive, non-destructive)

Refiner lineage kept. Force-merged against all 15 other v5s. Organization owns people-policy only, rewritten to strip mechanism names.

## Merges and kills
- tpm-sealed-successor folded into leased ownership (succession is ownership lifecycle, not second item).
- All entries rewritten in policy-only language (lease, noncustodial, silent-until-fault, drill-cadence); TPM/RAPL/EPT mechanism names stripped to budget/quorum/bar/drill references.
- Caps added everywhere: human-last capped, diff-size capped, log read-on-fault-only, graduation checklist, named successor + drill cadence.

## Final merged list (6 survive)
- leased-noncustodial-ownership-with-successor-drill: leased epoch ownership, org never holds keys, named successor + deadman drill cadence.
- outward-live-quorum-gate-fail-closed: no outward act without live quorum, fail-closed, skew is dissent.
- machine-dissent-first-human-last-capped-review: machine dissent/replay first, human last with daily cap.
- intent-diff-only-merge: merge small intent-diffs only, ambiguity faults outward, proposer never checks/stores.
- silent-append-only-log-on-fault-only: silent append-only trail, read only on fault/drill, endurance-capped.
- quarantined-classroom-with-graduation-bar: train in disposable cell with no downcall, graduate via checklist or stay.
