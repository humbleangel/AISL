# Decision brief 10/28 — scale unit

RECOMMENDATION: pinned-endpoints. Scale unit = explicitly counted EPT-capable endpoint, pinned to PCID/VPID/lane/TLB/power/trace budget at create, hard fanout cap = min(512 EPTP, 4096 PCID, TLB-resident, sustained-AVX power, PT-buffer). Never threads/processes/replicas.

## THINK-1 (from metal)
CPU does not see threads, processes, pods. It sees: 1× architectural context per logical CPU (CR3+PCID+VPID+EPTP), fixed TLB capacity, shared execution/cache per core, fixed power/trace envelopes. Scaling lies if it counts software abstractions. Only countable hard things: residency slots surviving a switch without flush + refill storm. Hypothesis: scale = min(EPTP-list, PCID-live, TLB-resident, lane/power/trace-sustainable) pinned at admission, not scheduled elastically. Thread-pool assumes switch is cheap; elastic assumes creation is free; lane-count assumes width = independence — all three fight the CPU.

## HARDWARE FINDINGS (x86-only, web)
- EPTP list = 512: VMFUNC EPTP-switching selects from 512 EPTPs configured by root; no VM-exit, no TLB flush on switch; ~160 cycles (same mappings). Haswell+. Sources: USENIX ATC'18 Hua EPTI; ATC'22 Gu EPK; Intel VT docs.
- PCID = 12-bit = 4096, PCID0 reserved; tagged TLB skips full flush; recycling + INVPCID management past ~4095 live spaces. Sources: Intel SDM; kernel PTI docs; grsecurity TLB-invalidation.
- Switch cost: MOV to CR3 ~100–300 cycles even with PCID; VMFUNC ~160c vs CR3 ~300c in guest; direct pipe context-switch ~3.8µs old; modern fast-path 150–300ns best-case + microseconds indirect (cache/TLB refill). INVVPID/INVEPT flush derived mappings regardless of VPID/PCID. Sources: ExpCS07; kernel pti.txt; ATC'18.
- TLB-miss up to 24 refs virtualized (4-level guest + 4-level EPT = 2D walk); native 4 refs; Skylake "hundreds of cycles" on miss despite paging-structure caches. Perf: dtlb_*_walk_duration/completed. Sources: UT-Austin ISCA'17 POM-TLB; Ahn ASPLOS 2020 DMT; Bhargava 2008.
- Topology: package > core > thread; thread = logical CPU = scheduling unit; SMT/HT shares execution engine, caches, D-TLB/I-TLB, ROB — 2 threads ≠ 2 cores, contended throughput sublinear, can regress. Sources: kernel x86/topology; Intel HT guides; NASA Westmere HT study.

## THINK-2 (after findings)
Caps are real and nested: 512 EPTP is hard admission cap; 4096 PCID is hard tag cap but effective cap far lower (L1 64–128, STLB ~1.5–2K — live > resident = thrash + walk storm up to 24 refs/miss). VMFUNC proves the CPU's preferred scale primitive (pre-pinned EPT, no-exit/no-flush switch, per-EPT TLB context); CR3-switch proves opposite (flushes all EPT TLBs). SMT proves thread-pool fallacy (scheduler sees 2 CPUs, CPU sees 1 contended core). Threads = multiplexing fiction; lanes = width not isolation; elastic spawn = unbounded PCID/EPTP/TLB debt. Only pinned-endpoint counts what CPU must retain. Collision resolved: abundance arenas may over-provision memory but never tags/residency; scarcity TLB/joule budgets and resume metrics become min() inputs to fanout cap.

## REASONS
Don't fight CPU: VMFUNC/EPT-TLB is the only no-flush scale path — use it as unit. Collapse impossible by construction: admission fails instead of thrashing all tenants. One mechanism (Thompson): isolation + accounting + scheduling key in same endpoint ID.

## PROVING EXPERIMENT (2 configs, same socket, HT on, EPT on)
A: thread-pool 64→2048 threads, shared spaces, random 4K touch. B: N pinned endpoints (N = cap), each own EPTP+PCID, VMFUNC switch, same touch. Measure throughput, perf dtlb walk_duration/completed, VMFUNC vs CR3 cycles, shootdowns (/proc/interrupts TLB, trace_tlb_flush). Pass = A collapses past residency (walk/exit explodes) while B flat to cap then clean admission-refuse. Fail = A matches B.

## WHAT WOULD CHANGE MY MIND
Measured CR3+refill ≤ VMFUNC+no-flush at >512 live contexts on target SKU; STLB proves residency for ≫512 active EPTPs without walk inflation; or SMT proves linear isolation under AVX/PT pressure.
