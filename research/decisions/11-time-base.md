# Decision brief 11/28 — time base

RECOMMENDATION: causal-tick as specified. Tick u64 advanced only on send; recv = max(local,remote)+1. Never a shared counter, never wall-derived. TSC only as same-slice delta (LFENCE-bracketed, same TSC_AUX/core, abort on migration/SMI_COUNT change). Wall only outward (signed range, never used in order()/expire()). Slice enforcement via per-core TSC-deadline (MSR 0x6E0), re-armed locally.

## THINK-1 (from metal)
CPU does not give ONE time: TSC is per-logical-processor, APIC TSC-deadline is per-core, SMI steals cycles invisibly, VMX-preemption stretches guest time non-uniformly. Any OS pretending a single 64-bit number orders everything fights the CPU (Thompson: hardware lies, don't trust it). Global wall-clock (NTP/RTC) imports jumps/leaps/VM-skew into core ordering → non-deterministic. TSC-as-timestamp-everywhere compares unsynchronized counters → false before/after. Hybrid logical+wall still lets wall infect causality on ingest. What metal gives cheaply: per-core monotonic delta (same core, short interval), per-core deadline wakeup, causal fact (message sent → received). Natural anchor: order = sends (Lamport tick), duration = local delta, wall = untrusted exterior display only.

## HARDWARE FINDINGS (x86-only, web)
- Invariant-TSC ≠ synchronized: constant rate + doesn't stop on halt — NOT guaranteed in sync across cores. Intel: "TSCs are not guaranteed to be synchronized although OS usually tries at boot." WRMSR IA32_TIME_STAMP_COUNTER (0x10) applies to one logical processor only; cross-core sync "difficult." Linux tsc_sync.c boot sync-test, warp → mark TSC unstable, turn off TSC clock. Sources: Intel Community TSC-sync thread; Linux tsc.c/tsc_sync.c.
- TSC_ADJUST (0x3B) per-thread: detects BIOS/SMM wreckage. SMM save/restore TSC to "hide" stolen cycles → slow desync, caught via ADJUST mismatch. All threads in package required same ADJUST; cross-package may differ after hotplug (Gleixner 8-patch series). Source: LKML x86/tsc TSC_ADJUST series.
- RDTSC/RDTSCP ordering: RDTSC not serializing, can execute early. RDTSCP waits for prior instructions, not subsequent. SDM rule: LFENCE;RDTSC = order prior loads; MFENCE;LFENCE;RDTSC = + stores visible; RDTSC;LFENCE = block subsequent. Sources: felixcloutier rdtsc/rdtscp; rdtsc.rs docs.
- APIC TSC-deadline, per-core one-shot: IA32_TSC_DEADLINE MSR 0x6E0, LVT Timer mode 0b10, IRQ when TSC ≥ deadline. Per-local-APIC, not global. CPUID.01H:ECX[24]. In xAPIC mode LVTT + DEADLINE writes need MFENCE serialization. Needs ARAT or stops in deep C-states. Sources: OSDev APIC Timer; Linux apic.c; Dreamportdev notes.
- SMI_COUNT MSR 0x34: counts SMIs since boot. SMI untrappable, but inferrable: delta SMI_COUNT + TSC vs APERF/MPERF gap = stolen time. Sources: NCC Group / Fox-IT Rendezvous with SMI; LKML perf SMI_COUNT patch; smackerelofopinion Detecting SMIs.
- HPET: one shared main counter ≥10MHz + up to 32 comparators (normally 3+), single monotonic domain independent of per-core skew. Slow MMIO, BIOS often doesn't route interrupts, legacy-replacement steals IRQ0/8. Good slow watchdog, not fast path. Sources: kernel timers/hpet, virt/kvm/timekeeping; Intel PCH datasheet HPET.

## THINK-2 (after findings)
Hardware's only trustworthy primitives are local-delta + local-deadline + counted-theft. Cross-core TSC compare is architecturally unsound (ADJUST/warp evidence). Global ordering must be software-causal, not hardware-temporal. TSC demoted to same-slice stopwatch (LFENCE-bracketed, same AUX, abort on migration/SMI_COUNT change). APIC-deadline demoted to per-core slice enforcer, never global scheduler. HPET demoted to rare cross-check. Wall relegated outward: sign a range [earliest,latest], never ingest for ordering. Satisfies pairings: tsc-is-delta, determinism-budget, stolen-time-skew (SMI → mark slice tainted, don't correct tick).

## REASONS
Only option that doesn't fight desync/SMI/VMX. Global-wall and TSC-everywhere both assume a hardware global now x86 does not provide. Hybrid re-admits wall jumps into causal order.

## PROVING EXPERIMENT (no new infra)
1. 2-core ping-pong, 10M iters: compare cross-core TSC timestamps vs tick order; assert ≥1 inversion/warp or AUX-migration under load + injected WRMSR ADJUST offset on one core → tick order stable, TSC order flips. 2. Same-slice delta test: LFENCE;RDTSC loop with SMI-count sampler; inject SMI-heavy IPMI load → naked RDTSC delta jitters/misorders, bracketed+SMI-gated delta either tight or explicitly tainted. 3. Deadline test: arm per-core deadlines with skewed TSCs → each core fires locally correct, global firing order disagrees with tick order → proves deadline can't be global clock.

## WHAT WOULD CHANGE MY MIND
Measured guarantee that invariant-TSC stays ≤20ns synced across all sockets/packages under SMI + C-state + VMX load with TSC_ADJUST locked (or hardware TSC-reliable + ART), plus trappable/stamped SMI. Then hybrid logical+bounded-wall becomes safe. Absent that: keep causal-tick.
