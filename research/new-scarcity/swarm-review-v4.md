# Swarm review v4 — new-scarcity (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (drop privilege-crossing as legacy ~zero; rename secrecy to containment-budget) attacked in round 2 for overlapping replay-killers (units), unowned IPI/SMM disturbance, missed PCID-churn/AVX-downclock/DRAM-move costs, overbroad attestation pipe.

## Round-1 → Round-2 changes
- Nested-TLB-walk explicit with EPT-multiplier + PCID-churn from disposable code.
- Attestation kept as one bandwidth (verifier consumes quote+trace+trail jointly).
- IPI/SMM steal folded into determinism-replay; DRAM-move into cache-residency; endurance+latency kept together (recovery verdict prices both).

## Final v4
- human-review-bandwidth: only non-generable gate per Bun lesson; all proofs/mechanisms ceded to oracles.
- joule-thermal-headroom: RAPL/thermal + all-cores-avx-thermal-ceiling makes power-is-capability the hard bound.
- determinism-replay-budget: trapped-replay + causal-tick-first spendable ticks; SMM/ME-steal and IPI-disturbance charged here.
- shared-cache-residency: shared-L3/SMT + lane-is-thread + code-follows-data makes residency and DRAM-move the owned good.
- speculation-containment-budget: speculation-is-effect reframed from property to spendable fence/mask/partition budget judged by pmu-speculation-judge.
- nested-tlb-walk-budget: single-address-space + ept-is-module gives 2D-walk multiplier plus PCID-churn from disposable-code; page-table-is-type prices it.
- persistent-commit-endurance: crash-only-persistence + nvme-flush-lie + power-loss-mid-write makes P/E-cycles and flush-latency one finite commit good.
