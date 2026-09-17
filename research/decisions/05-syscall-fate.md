# Decision brief 05/28 — syscall fate (item-defining)

RECOMMENDATION: no-syscall-use — set EFER.SCE=0. Any SYSCALL/SYSRET faults (#UD vector 6, IDT 0xE, DPL0). No STAR/LSTAR, no DPL3 IDT gate, no syscall table. All user→kernel requests are faults: #UD catch + guard-page/EPT-violation upcall with language-typed message in shared page.

## THINK-1 (from metal)
What persists cannot be wished away: CPL0/CPL3 + CS[1:0], 256-entry IDT, SYSCALL opcode bytes, NMI/SMI, EPT. What AISL deletes is crossing convention, not rings. Metal already gives a delete-switch: IA32_EFER.SCE. Replacement crossing comes from persisting metal: #PF/#GP/#UD via IDT + EPT-violation VMEXIT, both trapping to CPL0 with stack-switch via TSS.RSP0/IST. Thompson warning: trap-and-emulate full POSIX reintroduces the compromised ABI you claim to delete; narrow table reintroduces STAR/LSTAR + DPL3 gate management. Tension ("don't fight CPU" — SYSCALL is the CPU-optimized crossing) resolves because cost is no longer dominated by entry opcode and ABI surface is the enemy, not cycles.

## HARDWARE FINDINGS (x86-only, web)
- SYSCALL enable → #UD: if CS.L≠1 or EFER.LMA≠1 or EFER.SCE≠1 then #UD. In 64-bit, SCE=0 makes every SYSCALL/SYSRET fault vector 6. Sources: felixcloutier syscall; shell-storm x86doc; KVM vmx.h (SYSCALL #UDs if SCE=0).
- SYSCALL cost: bare round-trip ~78–99 cycles (Ryzen loop; kernel_entry_benchmark: Syscall 99, Interrupt Gate 610, Call Gate 473). With Linux entry + Spectre mitigations (pipeline drain, BHB clear, RSB untrain, IBRS): ~1400 cycles for clock_gettime vs ~157 via vDSO. Sources: codingconfessions 2025-09-16; blitz/kernel_entry_benchmark.
- EPT/VMEXIT cost: VMCALL-forced VMEXIT ~1600–1700 cycles pinned; EPT-violation same class — order-of-magnitude above bare SYSCALL, comparable to mitigated syscall. Source: yshalabi VMExits.
- IDT long mode: only 0xE interrupt gate (clears IF) and 0xF trap gate; 16-byte descriptors; SS:RSP always pushed, 16B-aligned; task gates removed; LIDT ring-0 only; INT n from CPL3 requires DPL≥CPL else #GP. Sources: OSDev IDT; Intel Vol3A; kernel-internals.org.
- CPL checks: CS[1:0]=CPL; gate DPL checked for software INT; handler CS.DPL + TSS switch enforces CPL0. SYSCALL bypasses IDT entirely (STAR/LSTAR/SFMASK, no stack switch) — hence SW mitigations. NMI/SYSRET fragility (guest-controlled stack; Linux IRET fallback) disappears with no SYSRET. SMI invisible regardless — fewer entry paths = fewer states SMM interrupts mid-transition.

## THINK-2 (after findings)
SCE=0 trick resolves "don't fight CPU": disabling SYSCALL is architected use, not hack; illegal-opcode-trap is free. Perf objection inverts: bare SYSCALL is 10–20× cheaper than EPT-VMEXIT, but real syscall is no longer bare — with mitigations (~1400c) the EPT path (~1600c) is at parity, and AISL deletes high-frequency syscalls by design (shared rings, no POSIX byte I/O), so residual upcalls are rare. Security: SYSCALL preserves attacker-controlled ABI number + args + STAR targets + return-predictor state (Retbleed/Spectre surface); #UD/EPT path has no syscall-number namespace, no STAR to corrupt, forces language-typed decode first. Alternatives fail honesty: narrow table keeps STAR/LSTAR/SFMASK + DPL logic; full POSIX keeps C ABI; trap-and-emulate keeps SYSCALL bytes plus emulator (largest TCB) — all collide with no-posix-no-c-abi and upcalls-only. Risk to prove: EPT-as-IPC coupling (isolation fault = service dispatch) must demux simply; NMI/double-fault IST paths must work without SYSCALL.

## REASONS
1. One bit deletes whole ABI class; nothing to version, gate, or mitigate.
2. Reuses mandatory handlers (IDT+#PF/EPT) — fewest mechanisms.
3. NMI/SWAPGS fragility deleted with SYSRET.
4. Cost affordable iff upcalls are control-plane-only — which the AISL driver model already requires.

## PROVING EXPERIMENT (bare metal or KVM, ~200 LOC)
Boot test kernel, LIDT with #UD + #PF handlers, clear SCE, zero LSTAR/STAR. CPL3 probe: SYSCALL must land in #UD handler with fault RIP=0F05; SYSRET same; assert no CPL0 entry via STAR path. Implement 3 services fault-only (yield, log, page-grant) via guard-page touch + shared ring. Measure RDTSC fault-upcall vs SCE=1 round-trip with/without mitigations; count MSRs + IDT DPL3 gates eliminated and ROP gadgets in handler. Pass = traps correct, parity (<2× mitigated syscall), zero DPL3 gates.

## WHAT WOULD CHANGE MY MIND
Hot-loop driver needing <500ns synchronous kernel assist with no batching/mapping alternative (GATE wins on physics); load-bearing binary trapping >10k #UD/s (narrow GATE N≤8 allow-listed RIPs + audit counter until it dies); or NMI/#DF/SMI correctness requiring STAR path.
