# Swarm review v5 — new-time (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 clock-reality audit (Intel SDM Vol3 18.7, TSC_ADJUST).

## Round-2 kills / revisions
- SMI trappable/blockable KILLED as fiction: SMI enters SMM invisibly; only inference via TSC-vs-APERF + MSR_SMI_COUNT. stolen-time-is-fault revised to stolen-skew-is-dissent.
- VMX-preemption timer as general OS timer KILLED: non-root only; bare-metal equivalent is APIC deadline.
- Per-send precise joule accounting KILLED: RAPL package-coarse; downgraded to estimate + cap.
- Fast per-tick TPM KILLED: ms latency + wear; sparse resume/attest only.
- NMI trappable PARTIAL: catchable via IDT/guest-host but still steals; same betrayal treatment.
- Dual budget REQUIRED: timer stops hang, joules stop burn; either alone fails (joule alone can't bound low-power infinite loop).

## Final v5
- resume-restores-maxtick-not-wall: crash-CoW reloads max causal tick; wall never persisted, re-attested as range.
- tick-on-send-only-no-shared-clock: Lamport increment on posted-send only, never shared atomic, per-core state.
- tsc-delta-same-slice-only: LFENCED RDTSC start/end on one core/run, delta for skew only, never now, never cross-core compare.
- stolen-skew-is-dissent-not-fault: SMI untrappable; TSC-APERF/SMI_COUNT skew triggers dissent-replay verdict, not synchronous fault.
- wall-is-outward-signed-range-only: wall arrives only as quorum-signed [early,late], never syscall, never stored as point.
- dual-budget-deadline-timer-plus-joules: APIC/VMX-preemption timer bounds hang plus RAPL/PMU joule cap bounds burn; exceeded = bounded-deadline-send fault.
- sparse-tpm-epoch-hash-trail: TPM NV counter + PCR extend only at resume/attest; per-snapshot hash chain in NVMe torn-proof log between.
