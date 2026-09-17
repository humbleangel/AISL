# Swarm review v5 — new-oracle (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 reality check. Oracle = bounded pure function over sealed inputs; verdicts only.

## Round-2 kills
- isolated-domain-verdict killed as oracle guarantee (self-attestation loop; isolation is mechanism ceded outward).
- PMU-as-proof killed (SMT-contaminated, SMM-invisible, PT drops; tripwire/veto only, never approve-by-equality).
- Per-send synchronous TPM quote killed (~100ms+ latency; quote at resume/seal, cache, carry expiry).
- Bit-identity replay killed (AVX/thermal/SMT); replaced by identity-with-envelope + diverse physics.
- RAPL-as-exact-proof killed (estimate, not joule truth); capped-lease + torn-proof instead.

## Final v5
- outward-quorum-gate-verdict: oracle votes only, enforcement ceded outward; gate tallies sealed votes, never executes.
- dissenter-replay-verdict: one dissenting core replays sealed trace under replay budget; mismatch is veto, match never proves absence.
- speculation-tripwire-verdict: PMU/LBR/PT as lossy veto-only tripwire for speculation leakage, never proof of cleanliness.
- scrub-trail-verdict: verdicts only that PCID/VPID+AVX scrub trail is present on measured pages before resume, not that silence holds.
- microcode-pinned-expiry-verdict: pinned microcode+PCR measured at resume, cached quote, all verdicts expire on revision mismatch or epoch end.
- causal-page-bind-verdict: live page valid only bound to causal send-tick hash trail; unbound replay is fault.
- degraded-physics-quorum-verdict: quorum requires agreement across cold/hot/degraded joule-thermal states, equivalence not identity.
