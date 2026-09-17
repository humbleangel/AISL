# Swarm review v4 — new-oracle (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 (isolated domain, dissent replay, PMU verdicts) attacked in round 2 for meter-trust (microcode can spoof PMU/RAPL), seed monoculture (two x86 seeds share ISA), replay-vs-thermal falsity, same-socket fate-sharing, majority-vs-truth confusion, verdict expiry.

## Round-1 → Round-2 changes
- Microcode pinned with expiry; verdicts expire on version/thermal change.
- Quorum diversified across physics (cold/hot/degraded), not just seeds.
- Verdicts bind live pages + causal tick, never files/wall-now.
- Meters cross-checked against each other.

## Final v4
- isolated-domain-verdict: oracle lives in EPT-partitioned ring-minus-one guest, never in target address space, else self-judgment.
- dissenter-core-replay-verdict: same intent re-run on pinned sibling core via APIC-IPI with SMT-sibling idle; identity requires two cores agree.
- speculation-pmu-crosscheck-verdict: PMU/LBR speculation count is part of verdict and PMUs cross-compared; microcode-spoofed counters diverge.
- pcid-avx-scrub-verdict: residue-silence extended to PCID/TLB, AVX regs, LBR/PT buffers; verdict fails if scrub not witnessed on fault-return.
- microcode-pinned-expiry-verdict: PCR quote must pin microcode+ME/SMM version in public log; verdict expires on version/thermal-throttle change.
- live-page-causal-bind-verdict: verdict seals content-hash of measured live pages + causal-tick/epoch range; no files/fds/wall-now replay.
- diverse-physics-quorum-verdict: three runs across cold/hot core + SMT-off degraded-to-one-core; catches thermal/microcode common-mode liar.
