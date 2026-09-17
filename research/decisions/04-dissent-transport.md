# Decision brief 04/28 — observer dissent transport

RECOMMENDATION: IPI-dissent-mesh (amended: one vector per live core, OR-veto, fail-closed timeout). Dissent by x2APIC IPI vector number only, no shared memory; quorum = live-core vectors seen.

## THINK-1 (from metal)
CPU offers exactly one cheap async poke: IPI, payload = 8-bit vector only. No sender ID, no ack, no reliability. IRR semantics: same-vector pending = coalesced/lost. So "counting votes over IPI" fights the CPU; "OR-veto over IPI" rides it: one surviving poke suffices to block. Few strong cores (8–64) → per-core vector allocation fits in user vectors (32–255); many-core would not — metal says keep N small. Shared-memory ballot is countable/auditable but pays MESI + polling + speculation surface even when dissent is rare; dissent should be rare → common case must cost ~zero (no-shared-memory signal). Full-mesh agree = O(N²) IPIs = storm = thermal/C-state churn + jitter, violating scarcity budgets. Single-watcher = SPOF + Thompson-trust problem. Fail-closed timeout mandatory regardless: IPI has no sender-detectable delivery guarantee (x2APIC drops even Delivery Status polling), so silence must mean block, not consent.

## HARDWARE FINDINGS (x86-only, web)
- x2APIC IPI: ICR is single MSR 0x830 (vs xAPIC ICR2+ICR MMIO); SELF-IPI register dedicated; WRMSR completion implies delivery posted to fabric. Illegal vector ≤0x0F sets Send-Illegal-Vector; Delivery Status polling removed. Logical ID 32-bit. Source: Intel x2APIC Spec §2.3–2.4.
- Posted interrupts: PID = PIR[255:0] + ON + SN + NV + NDST. Rule: set PIR bit; notify only if ON==0 AND (URG==1 OR SN==0), then set ON. Multiple PIR bits coalesce into ONE notification; handler must drain-loop PIR. Spurious PIN on wrong pCPU is arch violation + cost; SN=1 when vCPU unloaded/host, clear SN only at IN_GUEST_MODE entry. EPTP-switch alone does NOT switch PID. Sources: Intel TDX Interrupt Virtualization Spec §2.4; Feng Wu VT-d Posted Interrupts slides; VT-d Spec §9.10–9.11.
- VT-d IRTE: per-interrupt remap vs post (PST bit → Descriptor Address + Virtual Vector + URG). CPU-to-CPU ICR IPIs do NOT traverse IRTE; IRTE governs DMA/IOAPIC/MSI. So IPI-dissent is unauthenticated by HW. Sources: VT-d Spec; VT-d Posted Interrupts backup.
- Latency: bare-metal IPI <1µs; synthetic ~420 cycles min issuer, ~1300–1600 cycles mean issuer+remote; virtualized ~10µs guest-IPI; virtual IPI ~5230–11557 cycles; IPI-virtualization cuts ~22% (xAPIC)/~16% (x2APIC), unicast physical exitless, multicast still exits. Sources: QNX Guest IPIs doc; Intel Community IPI cost thread (Vyukov); virt-jitter-lab Hyper-V report; LWN IPI-virtualization + KVM unittest table; HotCloud Gleaner paper.

## THINK-2 (after findings)
Lossy coalescing is architectural at BOTH levels (bare APIC IRR + virtual PIR/ON). Any design counting IPI arrivals is wrong on metal — count distinct vectors/bits, not notifications; OR-semantics. Per-core dissent vector rescues the anchor: distinct PIR/IRR bits do NOT coalesce each other away; only repeats of same vector do. So quorum = "live-core vectors seen" + "2-dissenter" both survive iff vector = sender identity, not reason code. PID/IRR are HW-owned memory, not OS ballot — no MESI polling cost, acceptable. IRTE finding kills "remapping makes IPI-dissent safe": no HW spoof-protection on CPU IPI — mitigation is procedural (fail-closed + live-core mask from x2APIC presence + rare use; rogue core can only veto/DoS, never forge consent — safe direction under fail-closed). Thermal/determinism collision real: each IPI yanks remote out of C-state/user flow — answer: dissent IPIs only on dissent (rare), never heartbeat; liveness via existing scheduler clock.

## REASONS
1. Don't fight CPU: rare-event veto maps exactly to what IPI does cheaply — one surviving poke blocks.
2. Coalescing-safe iff per-sender vectors + drain-bits-not-count-interrupts; satisfies 2-dissenter vote with 2 distinct vectors.
3. Zero common-case cost (no ballot polling/MESI); avoids N² storm and single-watcher SPOF/Thompson trust.
4. Failure direction safe: loss/silence/spoof all resolve to block under timeout fail-closed.

## PROVING EXPERIMENT (bare metal, x2APIC, N=8–32, no hypervisor)
Assign vector 0x40+i per core; handler records IRR vector + TSC, EOIs. Tests: (a) single dissent latency p50/p99 (expect <2µs bare); (b) all-N simultaneous dissent — verify N distinct vectors all observed after drain despite one notification; (c) same-core 100x repeat storm — demonstrate coalescing loss (count < sent) proving OR-not-count; (d) kill/mask one core — verify timeout fires and action blocks (fail-closed); (e) C-state + TSC jitter on victim to price thermal/determinism cost. Pass bar: (b)+(d) 100%, (c) shows loss but veto held, (e) within budget because dissent rare.

## WHAT WOULD CHANGE MY MIND
(b) loses distinct vectors under storm (IRR overflow/PIR race beyond one drain loop); p99 IPI > observer timeout budget forcing false-blocks; audit/replay demands persistent ballot (then shared-memory ballot wins as slow-path complement); or N outgrows user vectors.
