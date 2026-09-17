# Decision brief 02/28 — module boundary mechanism

RECOMMENDATION: module-is-EPTP-view — module = (EPTP + VPID); call = VMFUNC ECX=target; grant = root-installed temporary EPT alias in callee EPTP only; revoke = root unmap + INVEPT:1 + INVVPID:1 + scrub. Language types + CET/PKU manage intra-cell structure only, never cross-cell authority.

## THINK-1 (from metal)
Module boundary is both type boundary and compromise boundary (Thompson'84: cannot trust language alone). Few strong cores + SMT/speculation: sharing address space (PKU-only, lang-only) leaves Spectre/MDS/L1TF residue — only separate physical view (EPT) removes sibling-readable bytes. Rings/process syscall+CR3+invlpg/IPI path (~1000+ cycles + KPTI + shootdowns) is the wrong price for trusted-compiler internal calls. PKU: WRPKRU ~20 cycles, no TLB flush — tempting, but PKRU is per-thread U-accessible, data-only, X-perm unaffected, no DMA/VT-d binding, bypassable by any ROP/WRPKRU gadget; no integrity vs SMM/ME/microcode. Cannot be sole anchor. EPTP-view: per-module GPA→HPA = true physical confinement, binds VT-d second-stage, works with CET-shadow per view, PMU/LBR per view. Risk: 2D-walk + nested-TLB budget is scarce (N EPTPs × M VPIDs = TLB thrash + clone-fanout memory); revoke needs shootdown + scrub. Tentative: EPTP-view iff switch is truly exitless and revoke provably complete.

## HARDWARE FINDINGS (x86-only, web)
- VMFUNC EAX=0 = EPTP-switching only. ECX selects from 512-entry EPTP-list preconfigured by VMX-root; ECX>=512 → VM-exit reason 59. Loads VMCS.EPTP, no regs/flags changed. Requires secondary controls + VM-function control bit0, else #UD/VM-exit. (Intel SDM 25.5.5, minix86 Vol3 pp.1087-1088)
- Switch does NOT change VPID/PCID. Combined mappings tagged (VPID,PCID,EP4TA). With VPID=1 mappings persist — fast path, no flush. Cannot fault on itself; fault deferred to next GPA use. ECX[15:0] → EPTP-index for #VE if EPT-violation #VE=1. (pp.1088-1089)
- INVEPT/INVVPID are CPL>0 → #GP (root-only). INVEPT: 1=single-EP4TA, 2=global; INVVPID: 0=addr, 1=single-VPID, 2=all, 3=single-retain-globals. INVEPT clears guest-phys+combined for that EP4TA all VPID/PCID; INVVPID:1 clears linear+combined for that VPID. May over-invalidate. (felixcloutier, Vol3 p.1164)
- Posted-IR: pi_desc 64B (PIR[256]+ON+SN+NV+NDST). Sender sets PIR[vec]; notify only if ON==0 AND (URG==1 OR SN==0), then ON=1 + send NV to NDST. Multiple posts coalesce — handler must drain-loop PIR. Spurious PIN on wrong pCPU is arch violation + cost; SN=1 when vCPU unloaded, clear only at IN_GUEST_MODE entry. EPTP-switch alone does NOT switch PID. (KVM APICv Nakajima 2012; posted_intr.h; vmx_vcpu_pi_load/put; 2022/2025 PIR-to-IRR sync patches)

## THINK-2 (after findings)
VMFUNC confirms promise and ceiling. Promise real: exitless module call, VPID-tagged survival — order cheaper than VM-exit/process. Ceiling real: (a) only 512 views, root-provisioned — guest/module cannot create/grant unprivileged; grant/revoke = root update + INVEPT/INVVPID (VM-exit), so data-plane fast, control-plane slow; (b) revoke correctness depends on INVVPID:1 + INVEPT:1 + scrub ordering, else stale combined mapping readable; (c) interrupts/DMA don't follow EPTP automatically — need per-cell PID + VT-d root tables. Collisions load-bearing: nested-TLB-budget caps live modules at tens not thousands; clone-fanout forbids per-object/per-closure EPTP. PKU/lang-only fail Thompson + WRPKRU-unprivileged + no VT-d bind; process/ring pays exit+shootdown per call. Consequence: modules must be FEW LARGE EPTP-cells (substrate), with language/PKU sub-domains inside — not one EPTP per function.

## REASONS
1. Only option with HW physical isolation + exitless switch + VT-d/IR bind.
2. Revoke enforceable (flush semantics explicit) vs PKU revocable by attacker WRPKRU.
3. Matches CPU design intent: VPID exists to make this cheap; PCID/process path exists to make Unix safe, not fast.

## PROVING EXPERIMENT (x86-64, no sim)
1. Latency: VMFUNC vs WRPKRU vs syscall round-trip (PMU cycles, LBR to verify no exit on VMFUNC path). 2. Correctness: secret in A only, temp-grant to B, revoke + INVEPT:1/INVVPID:1, then from B attempt load + speculative load + DMA (VT-d) + IPI-posted to A — must #VE/EPT-violation, no PIR delivery. 3. Budget: scale live EPTPs 2→128, measure 2D-walk misses (DTLB_LOAD_MISSES.WALK, EPT-violation rate) + INVEPT:1 cost; define cap (expect 16–64). 4. Bypass contrast: same test under PKU-only — show WRPKRU gadget reads across domain.

## WHAT WOULD CHANGE MY MIND
Revoke flush+scrub > syscall cost at target call rate; 512-list/TLB collapse forces <8 cells; nested-VMX host denies VMFUNC; or VT-d per-cell tables prove unmanageable vs single table + PKU. Then fall back to process/ring + language types, accept slower calls.
