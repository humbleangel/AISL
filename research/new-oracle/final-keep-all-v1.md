# Final keep-all v1 — new-oracle (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (7) with brutal FINAL (6). Oracle = verdicts only, mechanisms ceded outward.

## Round 1 — one by one
- outward-quorum-gate-verdict KEEP: Thompson consequence — all inward physics degraded (TPM slow, PMU lie-able, PT drops, RAPL estimates, TSC relative), so verdict gates on outward N-of-M epoch attestations. Fused with brutal batched joule-epoch fail-closed qualifiers.
- dissenter-replay-verdict KEEP narrowed: full deterministic replay from PT is hallucination (lossy/dropping); verdict on dissenter-presented bounded filtered slice only. Fused with brutal bound.
- speculation-tripwire-verdict KEEP as veto-only: PMU-positive ⇒ convict is hallucination (spoofable/SMT-contaminated); tripwire fires veto/fail-closed, never proves clean. Fused with brutal qualifier.
- scrub-trail-verdict KEEP NARROWED — brutal kill recorded as kill-of-duty not kill-of-verdict: oracle never scrubs (EPT doesn't scrub caches/BTB/AVX); oracle gates on observer-presented trail quorum per epoch (presence, coverage, signatures, epoch-bound, fail-closed).
- microcode-pinned-expiry-verdict KEEP verbatim: pinned rev + epoch/delta expiry, fail-closed on stale; TPM ms-latency forces batched per-epoch checks.
- causal-page-bind-verdict → fused into causal-cite-verdict: bind-verb is mechanism hallucination (EPT action by oracle); cite-only (presence/shape of page/epoch cite) is the verdict. Bind lineage superseded, coverage kept.
- degraded-physics-quorum-verdict KEEP: explicit degraded-mode quorum policy (conservative thresholds, flagged commits); distinct from normal-path gate.

## Round 2 — attacks + fixes
- Precise-joule/wall in gate? Killed — joule-epoch buckets + delta only.
- Per-op synchronous TPM quote? Killed — batched epoch gating, cached quotes, fail-closed on missing/late.
- Lossless PT replay? Killed — bounded filtered slice with hash chain + epoch binding; missing bytes = fault.
- Tripwire conviction? Killed — veto-only asymmetry retained.
- Oracle scrubbing inwardly? Killed — trail attestations checked, never measured inward.
- Microcode truth from self-report? Killed — quoted rev vs pinned allowlist + epoch/delta expiry.
- Bind pages? Killed — cite shape check only, no TLB/EPT action.
- Degraded detection inward? Killed — degradation signals presented outward, oracle applies degraded policy.
- Cross-coherence: gate (normal) vs degraded (policy) vs scrub-trail (hygiene) vs replay-slice (dissent admission) vs tripwire (veto) vs microcode-expiry (substrate) vs cite (anchoring) — 7 distinct scopes, no duplicates, all verdict-only. Count 7 fits.

## Final (7)
- outward-quorum-gate-verdict: batched joule-epoch fail-closed gate on outward N-of-M epoch attestations only.
- dissenter-replay-verdict: verdict admitting/rejecting dissenter-presented bounded filtered-PT slice only.
- speculation-tripwire-verdict: veto-only verdict on outward tripwire signal, never proof of clean.
- scrub-trail-verdict: gate-verdict on observer-presented scrub trail, oracle never scrubs.
- microcode-pinned-expiry-verdict: verdict that TPM-quoted rev is pinned and unexpired by epoch/delta, fail-closed on stale.
- causal-cite-verdict: cite-only shape check, binds nothing, fused from causal-page-bind-verdict.
- degraded-physics-quorum-verdict: policy verdict for commit under outward-flagged degraded sensors.
