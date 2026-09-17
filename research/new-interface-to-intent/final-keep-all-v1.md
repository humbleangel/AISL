# Final keep-all v1 — new-interface-to-intent (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (8) with brutal FINAL (6). Bun rule enforced throughout.

## Round 1 — one by one
- reference-is-ept-cap-not-string + brutal ept-cap-references-only → keep V5 (explicit NOT-STRING names the killed theater; int-handle-backed-by-string-table and DR0-3 pointing both killed).
- forbid-pages-before-permit-code + brutal +measured → forbid-pages-before-permit-measured-code (ordering + evidence; alias mappings forbidden; never W+X; outward checker verdict before permit completes).
- demo-in-disposable-cell-with-bounded-pt + brutal reordered twin → keep V5 demo-first (stresses action constraint); fresh EPTP view + zeroed pages + bounded ToPA + fault-on-overflow.
- proposer-drafts-checker-judges-never-same + brutal bun-split → proposer-drafts-checker-judges-never-same-bun-split (hardness + provenance; binary contains zero verdict branches; per-gesture principal logging).
- veto-only-by-live-ipi-quorum-fail-closed + brutal fused half → keep SPLIT (live APIC ACK set required; timeout = deny; no unanimous claim without live set).
- measure-diff-before-resume-tpm-pinned-root + brutal twin → keep (CPU fast hash of dirty diff vs RAM-pinned root, microseconds; TPM pins once at create; outward compare).
- ambiguity-faults-outward-not-retries + brutal fused half → keep SPLIT (single attempt; no retry counter; evidence-bearing outward fault).
- undo-is-bounded-cow-causal-index + brutal fused half → keep SPLIT (bounded pool + hash-chained index; overflow faults outward).

## Fusion decisions (explicit, mandated)
- Veto+ambiguity SPLIT BACK: different triggers (explicit no vs undecidable), different evidence (ACK set vs fault record), different recovery (deny vs escalate), different forensics codes; fusion would lose precision and risk deny-on-ambiguity liveness loss or escalate-on-veto safety loss. No CPU savings from fusion.
- Seal+undo SPLIT BACK: different time domains (synchronous microsecond gate vs asynchronous history), different bounds (diff-bytes vs pool+index-length), different TPM interaction; fusion invites history-walk on hot path, per-entry TPM theater, skip-diff-after-undo coherence fallacy.

## Round 2 — attacks + fixes (theater? holder+judge? Bun? coherence?)
- String-table-backed int handles → unforgeable kernel EPT handle + generation + perms + EPT-present dereference; shootdown on revoke.
- mprotect-flag-only forbid / alias凭空 / W+X convenience / build-time-only measure → EPT forbid + IPI shootdown + INVEPT, then hash, then X-only; evidence outward; no aliases.
- Reused-page cells / unbounded PT ring / host-visible demo → fresh zeroed EPTP view, no host mappings, small explicit ToPA bound + fault, draft-evidence outward.
- Same-principal split theater / cached verdicts / shared-memory coherence → distinct domains/cores/principals, zero verdict branches in interface, per-gesture never-same logging.
- Phantom veto / wall-clock quorum / self-veto → N-of-M live ACKs in microsecond budget, logged APIC IDs, timeout = deny.
- Per-resume TPM seal (20–100ms) → pin-once + fast diff; resumer never self-approves.
- Retry-with-jitter / wall-window peek → zero retries; single attempt + outward fault with evidence.
- Full-heap snapshot / unbounded log → EPT write-fault COW + bounded pool + causal index + shootdown + scrub; resume-diff and undo keep separate bounds.
- Cross-coherence: no coherent heap anywhere; no trusted firmware (CPU+EPI+TPM-root only); no wall-windows; cells isolated.

## Final (8)
- reference-is-ept-cap-not-string: hardware EPT cap only; kills string-authority forgery.
- forbid-pages-before-permit-measured-code: EPT forbid+shootdown before measured permit; stops TOCTOU.
- demo-in-disposable-cell-with-bounded-pt: ephemeral cell with scrub + bounded PT; kills heap/wall/unbounded-trace theater.
- proposer-drafts-checker-judges-never-same-bun-split: Bun hardness; interface drafts gestures only, never judges same gesture.
- veto-only-by-live-ipi-quorum-fail-closed: live APIC ACK quorum proves veto; kills phantom veto; timeout denies.
- measure-diff-before-resume-tpm-pinned-root: microsecond diff vs RAM-pinned root; TPM pins once; kills per-resume seal theater.
- ambiguity-faults-outward-not-retries: single attempt faults outward with evidence; kills retry-hiding self-grade.
- undo-is-bounded-cow-causal-index: bounded EPT COW + hash-chained index rewinds without coherent-heap snapshot.
