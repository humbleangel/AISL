# Decision brief 03/28 — primary trust gate

RECOMMENDATION: outward-quorum-gate (batched joule-epoch, fail-closed). Verdict exists only as live-IPI quorum acknowledgment within epoch + signed wall-range epoch binding. No local pass. No cached verdict reuse outside epoch.

## THINK-1 (from metal)
Thompson 1984: local toolchain/CPU can lie to itself — AISL cannot self-attest from inside the same compromised domain. TPM is off-CPU slow MCU over SPI/LPC: good root, terrible fast path; per-operation quotes would stall throughput. PMU/RAPL/PT/EPT/TSC are all CPU-local and attacker-visible (PMU polluted by SMT neighbor; RAPL is power-model output not a meter; PT is lossy compressor with internal FIFO; EPT controls translation not residency; TSC is delta counter not wall time). So local measurement = hint, never verdict; verdict must exist OUTSIDE the measured node. Bun lesson: proposer never grades itself; review bandwidth (quote bandwidth + quorum bandwidth) is scarcest, so batch it. Oracle discipline: keep verdicts (pass/fail per epoch), cede mechanisms outward. This eliminates before measuring: single local verifier (self-grading Thompson violation) and per-op TPM quote (fighting TPM physics).

## HARDWARE FINDINGS (x86-only, web)
- TPM2 Quote cost/throughput: discrete TPMs are slow embedded RISC + accelerators; fTPM study measured large variance, RSA keygen >10s on early tablets (MSR-TR-2015-84). Nuvoton NPCT7xx policy assumes 1,000 cmds/s max including auth — per-op quoting at machine op rate impossible. Infineon SLB9672: sleep after 50ms inactivity; SPI timing + MCU sign = tens of ms per asymmetric sign (consistent with 20–100ms Quote band). TPM clock ms granularity, rollback-safe flag only, persists ≤2^22 ms (~70 min) — not wall time (MS TPM 2.0 Time.c + fTPM paper §8.1.1).
- RAPL granularity/limits: updated every 1ms; 32-bit accumulator; default ESU 15.3µJ; socket wrap ~9 min at 240W 2P; kernel accumulates via 100s thread (kernel.org amd_energy docs). Model-based estimate, not measurement (Sandy Bridge+ µarch-event energy model); systematic workload bias + Hyper-Threading contradictions (Hähnel, Weaver, E-Team ATC'17, Hackenberg). Domains = package/cores/uncore/DRAM — no per-thread breakdown; MSR kernel-only. SMT co-tenant moves attribution; per-op joule billing from RAPL alone is spoofable.
- PT bandwidth/drop: hundreds of MB/s per core, decode 100–1000× slower than collect; ToPA buffers limited (up to 4MiB per entry) with interrupts to chain; truncation + PSB resync (perf-intel-pt man page). Drops architecturally normal: OVF on internal-buffer overflow; errata document SILENT loss (ADL031 CYC byte drop without OVF; ICL027 CBR dropped/delayed; ICL028 TIP/FUP dropped; OVF itself lost if overflow precedes TraceStop; PSB+ lost on disable race). ECI/perf docs: CPU branches faster than disk drains → loss expected on dense code. Implication: PT cannot be a complete per-op gate; absence of packet ≠ absence of execution.

## THINK-2 (after findings)
Per-op TPM gating dies on latency math (20–100ms × op rate = serial stall); batching mandatory. Statistical anomaly gating dies on Thompson + RAPL/PT facts (estimate-only + SMT pollution + silent drops = attacker-controlled signal). Single local verifier dies on Bun + Thompson (same CPU grades itself). Only outward-quorum survives: verdict off-node, scarce quotes batched into joule-epochs, live liveness (IPI quorum not TSC), signed wall-range binding (not TPM ms-clock, not TSC delta). Fail-closed converts PT/RAPL loss from "attacker hides in gap" into "gap = no verdict = halt." Cost is real (quorum availability, signing infra, quote budgets) but payable; alternatives' failures are fatal.

## REASONS
1. Amortizes TPM physics: one quote covers N ops/joules per epoch; quote-bandwidth scarcity becomes tunable batch parameter.
2. Neutralizes local spoof: PMU/RAPL/PT are hints feeding epoch accounting, never decisive; forging one node's counters doesn't forge quorum liveness.
3. Handles loss correctly: PT drop / RAPL wrap / missed IPI → fail-closed (no verdict), not fail-open.
4. Time done right: wall-range signer + live quorum replaces TSC-delta and TPM ms-clock for freshness.

## PROVING EXPERIMENT (falsifying)
1. Calibrate on target SKU: TPM2_Quote latency distribution (n=500, idle + loaded) + max sustained quotes/s; RAPL 1ms update + ESU; PT MB/s + OVF rate on branch-dense workload with SMT antagonist. 2. Attack: SMT co-tenant skews victim RAPL/PMU >X%; PT drop induced via dense branches; show single-local-verifier and statistical gate both pass bad epoch. 3. Gate test: outward-quorum with 100ms–1s joule-batch epoch; show (a) throughput sustained vs per-op gate collapse, (b) same attack yields no-quorum → halt, (c) replayed prior-epoch verdict rejected by wall-range check.

## WHAT WOULD CHANGE MY MIND
TPM Quote p99 <1ms sustained on deployed discrete TPMs; or proof of per-thread SMT-isolated non-model RAPL; or proof of lossless full-rate PT with no silent-drop errata; or proof quorum liveness cannot meet epoch deadline under real partitions — then fall back to halt-and-alert, never to local self-grade.
