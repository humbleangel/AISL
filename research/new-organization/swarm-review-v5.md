# Swarm review v5 — new-organization (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 operability audit (lease expiry, escrow unseal, promotion criteria made concrete).

## Round-2 fixes (all three interim FAILs repaired)
- Lease expiry: joule-lease not human-countable → expiry = min(causal-tick-count, RAPL joule counter, hash-epoch) with signed-wall-range display; automatic crash-only revoke, no command, no self-renew.
- Escrow unseal: no who/when → named successor + m-of-n shares (3-of-5 outward quorum, none is owner) + unseal only on logged expiry/death/veto-proof + quarterly quarantine restore drill.
- Classroom promotion: no bar → graduation = survive dissenter-replay + diverse-physics quorum + bounded joule replay trace + intent-diff-only; image never downcalls.
- Non-custodial owner: proposer-drafts-never-checks-nor-stores — owner proposes diff, never runs check, never holds log/escrow keys; all votes posted IPI slots.
- Human-last capped: machines replay/dissent first; humans judge survivors under per-diff joule+time budget (uncapped = denial-of-review).

## Final v5
- joule-epoch-leased-owner-noncustodial: one human holds expiring lease (tick+joule+epoch, auto-revoke, no self-renew); proposes diffs but never checks, stores log, or holds escrow.
- outward-physics-quorum-gate: merges pass only on m-of-n outward oracle verdicts from diverse physics/domains, never owner-local check.
- machine-dissent-first-human-last-capped: machines replay/dissent first on every diff; humans judge only survivors under per-diff joule+time budget.
- intent-diff-only-merge: only sealed intent diff with comparator proof merges; rollback is causal snapshot index.
- silent-attested-append-log: every decision appends attested hash-chained entry silently (residue-swept); reads explicit, history never overwritten.
- quarantined-classroom-no-downcall-with-graduation-bar: learners execute only in EPT-quarantined cells with no downcall from image; graduation only by surviving replay+dissent+quorum.
- tpm-sealed-successor-deadman-drill: named successor sealed m-of-n, unseals only on logged expiry/death/veto-proof, proven by quarterly quarantine restore drill.
