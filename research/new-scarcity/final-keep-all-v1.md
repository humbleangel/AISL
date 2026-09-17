# Final keep-all v1 — new-scarcity (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (7) with brutal FINAL (6). Scarcity owns what-is-scarce.

## Round 1 — one by one (all 6 keepers pass duplicate/hallucination/incoherence; scrub held for trial)
- review-attention-bandwidth KEEP: Bun scarcest; metered as reviewer-minutes/queue-age/escaped-defects; automation shifts, never eliminates (Thompson trust root).
- joule-thermal-headroom KEEP: RAPL PKG + AVX downclock; headroom = usable joules before throttle; batching/scalar-fallback/duty-cycle design.
- nested-ept-tlb-budget KEEP: STLB 1536–2048 + ~24x EPT walk + PCID 4096namespace pressure; metered via walk-duration/violation counters.
- durable-commit-endurance KEEP: QLC ~1k / TLC ~3k P/E; commit rate × bytes × WAF meters spend; group-commit/wear-aware structures.
- quote-attestation-bandwidth KEEP: 10–50 qps per TPM; forces batching/caching/chaining; quote+trace+trail jointly consumed.
- trace-replay-bandwidth KEEP: PT GB/s vs LBR-32; filtered PT + LBR-on-hot-path + snapshot policy; sampling IS the budgeting decision.
- scrub-containment-residue: resurrection trial — candidate meters tested one by one (DRAM zeroing bandwidth, residue window, cache-pollution, SSD TRIM wear, ECC scrub rate, clear-deadline): every one routes to an existing keeper (joule/TLB/commit) or names no capacity. Name fuses mechanism+goal+hazard, violating one-scarce-thing. No independent meter in context. FAILS. Kill upheld; revisit only on dedicated scrub-engine counter evidence.

## Round 2 — attacks + fixes
- Human attention not system resource? Rejected — Bun stipulated scarcest; metered operationally.
- Joules infinite from wall? Rejected — PKG-scope + downclock prove finite headroom.
- Bare-metal single-ASMO makes nesting moot? Rejected — budget prices the choice to avoid nesting.
- Cloud hides P/E? Rejected — TBW billed; physics persists.
- Software attestation suffices? Rejected — spoofable; root quotes bottleneck refresh.
- Tracing optional? Rejected — replay/forensics/audit require it; PT GB/s proves bound needed.
- Scrub resurrection paths all fail (bandwidth→joule, window→no capacity, pollution→TLB, TRIM→commit, ECC→wrong purpose, deadline→SLO). Kill stands with record.

## Final (6)
- review-attention-bandwidth: human verification minutes bound safe change rate (Bun scarcest).
- joule-thermal-headroom: RAPL PKG energy + AVX downclock cap burst compute before throttle.
- nested-ept-tlb-budget: STLB + ~24x EPT walk + PCID 4096 price isolation/nesting via translation misses.
- durable-commit-endurance: QLC ~1k / TLC ~3k P/E caps lifetime commits/TBW; forces batching.
- quote-attestation-bandwidth: TPM quote 20–100ms caps fresh attestations at ~10–50/sec; forces caching/chaining.
- trace-replay-bandwidth: PT GB/s unfiltered floods vs LBR 32 shallow; bounds always-on replay fidelity vs cost.
- Explicit non-entry: scrub-containment-residue NOT resurrected (no independent meter; spend owned by joule/TLB/commit; resurrect only on dedicated counter evidence).
