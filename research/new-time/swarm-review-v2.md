# Swarm review v2 — new-time (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- instant-boot: shallow lie on x86 — firmware/DRAM-train/PCIe owns boot time before your code runs, OS can't "instant" it.
- logical-time-first: buzzword carrying Lamport-1978 as new; says nothing about what orders, and pretends wall can be ignored when NVMe/LAPIC/TPM need it.
- replayable-execution: overlaps both logical-time and versioned-time; unimplementable naively on x86 due to RDTSC/RDRAND/SMI/SMT/interrupt nondeterminism.
- deadlines-as-types: Ada/real-time retread; type-level WCET promise is fraud on x86 where SMM stalls, thermal throttle, caches make guarantees unprovable.
- persistent-now: oxymoron branding that overlaps instant-boot + versioned-time; TSC resets and wall jumps on crash so there is no portable "now" to persist.
- versioned-time: git-for-time slogan over linear von Neumann memory; no hardware multiversion clock, all versioning is software overlay, still assumes linear time.

## Completed sublist v2
- resume-not-boot: cold boot abandoned, language boots only by resuming measured NVMe image because firmware time is untouchable.
- causal-tick-first: primary clock is scheduler-step/retired-instruction counter owned by ring0, wall never used for ordering.
- wall-as-range: wall time exposed only as interval-with-error, never point value, because TSC/SMM/NTP/VM-preempt make instants treacherous.
- trapped-replay: VT-x traps RDTSC/RDRAND/CPUID + pinned isolated core makes a deterministic partition; replay re-executes causal log, nondeterminism is explicit effect.
- deadlines-as-handlers: deadline type compiles to timed fallback branch, not guarantee, because SMM/thermal contention makes WCET promises on x86 dishonest.
- epoch-now: present is a crash-consistent NVMe epoch token + TPM monotonic seq you can hold and persist, not gettimeofday float.
- page-versioned-history: history is MMU/EPT dirty-page snapshots keyed to causal tick, time-travel is remap not copy.
