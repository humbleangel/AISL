# Swarm review v4 — new-scale (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (million-lanes dishonest on x86 — ~128 real AVX lanes; fleet-identical impossible under microcode drift) attacked in round 2 for linear-speedup/K8s folklore, free-clone assumption, global-now, identical-fleet, minimal-size fallacy.

## Round-1 → Round-2 changes
- Cores vs SIMD lanes split; fanout carries TLB/IPI cost; trace made per-lane budget; identical-fleet replaced by equivalence-quorum; zero-core-persisted added below one-core; joules made the scale unit.

## Final v4
- pinned-endpoints-one-to-n: scale owns endpoint count only; N pinned run-to-completion images, no scheduler co-scaling.
- simd-lanes-counted-not-cores: count AVX lanes separately from cores; few x86 cores never equal million tasks.
- ept-clone-fanout-with-tlb-cost: fanout number must include PCID/TLB-shootdown and IPI-mesh hop cost.
- resume-not-boot-microseconds: resume latency measured; minimal means microseconds from persistent-content-pages.
- trace-bytes-per-lane-budget: maximal trace bounded per-lane by PT bandwidth and NVMe endurance, honest by cap.
- thermal-joules-as-scale-limit: RAPL joules/thermal headroom is the scale ceiling, not core count.
- one-core-degraded-proof: downward scale proved by running full task degraded to one core with SMT off.
- fleet-equivalence-quorum-not-identical: horizontal scale is PCR-equivalence class + quorum verdict, never bit-identical fleet.
