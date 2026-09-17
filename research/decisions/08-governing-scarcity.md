# Decision brief 08/28 — first governing scarcity

RECOMMENDATION: review-bandwidth-first. All machine verdicts (tests, oracle diverse rebuild, PT-scoped trace, RAPL delta, static proof) pre-filter to intent-diff-only; human merges diffs only; oracle gates merge asynchronously; machine dissent forces re-review, never auto-merge. Subordinate order: review-bandwidth > translation-budget > joules > commit-endurance.

## THINK-1 (economics + metal)
AISL = OS+lang one thing. Thompson 1984: can't trust the toolchain you review with. Bun lesson: 1 human + swarm works iff independent oracle + strict pre-filter, because human review-minutes are the only non-renewable, non-parallelizable resource. Joules, TLB/EPT cycles, SSD program-cycles are renewable/substitutable (buy power, huge pages/PCID/INVLPGB batching, wear-level/dedupe logs). Human attention has no fix. "Don't fight CPU" cuts FOR review-first: x86-64 already gives cheap machine-verdict hardware (PMU/RAPL/PT/EPT-A/D) — make machines burn cheap cycles to save expensive human minutes. Alternatives fail as FIRST governor: joules-first governs scheduling only under hard power cap (general dev is throughput/latency bound); translation-budget-first is real but second-order (fix with design, not govern by it); commit-endurance-first slowest to bind (only if you log everything forever). Collisions resolved: machine-dissent-first, intent-diff-only-merge, oracle-gating are the MECHANISM implementing review-first, not rival scarcities.

## HARDWARE FINDINGS (metal meters only, web)
- RAPL read cost via perf-events ≈ sub-µs (0.54µs laptop / 0.50µs AMD / 0.74µs Intel server); powercap sysfs 2–6µs; MSR /dev/cpu/N/msr 0.54–4.4µs. Naive 1kHz userspace tools: +0.25% to +46.75% time, +40% energy; lean tools ≈ baseline. At ≥10ms interval ≈0.4% overhead stable; at 0.5–1ms: 0.9–2.4% + 50% stale reads. Rule: sample RAPL ≥10ms via perf-events. Sources: Raffin/Trystram IEEE TPDS 2024; Stoico EASE 2026; Dauner HotCarbon 2026.
- Intel PT: HW trace <5% typical (2–20% range), 100–300MB/s/CPU up to ~GB/s bursts; decode compute-heavy (10ms system-wide on 32-core = MBs raw → 100s MB text). Must scope/filter: single-range, address-filter, TNT/CYC throttling, last-10ms ring. Cannot trace-always-everything. Sources: Intel SDM Vol3 Ch36 via perfwiki; Dorsal PolyMTL 2022; nanotrace 2026.
- TPM2 Quote/attest: slow by design, single-digit sigs/sec. fTPM: CreatePrimary 3433ms, Quote 28ms — reuse key = 100× win; Merkle batch 64–4096 → 1350–8174/s. Fleet ECDSA-P256: p50 200ms, p95 600ms. Implication: no per-diff synchronous Quote/load; cache resident AIK, async/background Quote, batch/Merkle. Sources: attested-inference-receipts; Stian Kri 2024; Chrome TPM study; tpmdd-devel 2017.

## THINK-2 (after findings)
Metal confirms review-first FEASIBLE iff pre-filter obeys budgets: RAPL via perf-events @10–100ms = free; PT scoped/ring = affordable, unscoped = bandwidth/decoding DoS on the review pipeline itself; TPM per-merge = death, cached+batched/async = OK. Subordinate order stands: translation second (EPT/TLB flush + PT bandwidth + RAPL syscall path are the taxes making machine verdicts expensive), joules third (metering now proven cheap when disciplined), endurance last (binds only if full logs kept — which PT finding forbids).

## REASONS
Only human minutes are non-renewable and Thompson-relevant (trust). Metal shows machines CAN pay the filter tax for ~<1–5% if disciplined, buying order-of-magnitude human leverage. No alternative binds first on general x86-64 server.

## PROVING EXPERIMENT (kill/confirm in 2 weeks)
Swarm A/B on real queue, n≥200 diffs/arm: (A) review-first+pre-filter, (B) joules-first scheduler, (C) no pre-filter human-direct. Metrics: human-min/merged-diff (primary), time-to-merge p50/p95, machine precision/recall vs oracle, host overhead (%CPU, RAPL @10ms vs 1kHz, PT MB/s + decode s, TPM ms/merge). Prove: A wins human-min/merged-diff ≥2× with overhead <5% CPU, <100MB/s PT, TPM off critical path. Kill if: A overhead >10%, queue wait < review time (human-min not bottleneck), or oracle false-negative > human catch rate.

## WHAT WOULD CHANGE MY MIND
Sustained power-cap throttling where RAPL forces clock-drop dominating merge latency → joules-first; EPT/PT bandwidth saturates (dTLB-misses, EPT violations, PT overflow >1%) despite scoping → translation-first; commit-endurance/TPM latency forces sync gate (>100ms/merge, no batching gain) → oracle-gating becomes governor.
