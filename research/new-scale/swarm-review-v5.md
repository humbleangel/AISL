# Swarm review v5 — new-scale (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 quantitative audit.

## Round-2 kills / bounds (measured numbers)
- Million parallel lanes KILLED: 64 (AVX2x8) to 128 (AVX512x8) FP32 lanes/cycle/socket real; 1M = ~7800x time-slice, billed not parallel.
- Unbounded clone fanout KILLED: VMFUNC EPTP list 512 hard ceiling; PCID 4096 tags; per-domain tables ~12KB; switch ~150–300ns, INVVPID ~200–500ns, refill up to 24 refs/miss. Honest fanout: tens, low hundreds max.
- Unbounded honest trace KILLED: PT TIP 6–8B, worst-case GB/s/core vs shared ~20GB/s DRAM; typical buffer 4–64MB/core. Per-lane byte budget, overflow = fault.
- Microsecond resume CONDITIONAL PASS: regs+EPTP+VPID+CR3 <0.5us + refill 1–3us true iff no device/IOMMU/NVMe reinit (S0ix ms, S3 100–500ms, NVMe 10s ms).
- Sustained all-cores-AVX KILLED: PL1/PL2 + Tau + Tjmax + license downclock; joules/s is the unit.

## Final v5
- pinned-endpoints-one-to-n: scale is counted explicit endpoints, never threads/processes/forks.
- lanes-counted-128-resident-ceiling: only 64–128 SIMD lanes exist per socket; rest is billed time-slice.
- ept-clone-fanout-with-invvpid-cost: fanout capped at 512 EPTP list / 4096 PCID; every clone bills flush plus nested-walk refill.
- resume-us-cpu-state-only: microseconds true only for regs plus EPTP plus VPID restore; devices excluded.
- trace-bytes-per-lane-budget: every lane gets a PT byte budget; overflow is fault, not drop.
- joules-as-scale-limit-rapl: sustained scale is RAPL joules per second; AVX downclock proves watts beat cores.
- one-core-degraded-proof: every image must demonstrate full run pinned to one core.
- fleet-quorum-not-identical: TSC plus microcode plus thermals make identical impossible; N-of-M diverse quorum decides.
