# Decision brief 06/28 — crossing direction (upcalls-only)

RECOMMENDATION: trappable-crossings-are-upcalls-only. No syscalls/downcalls as language primitives. Voluntary crossing = fault up to pinned handler (resume = context restore, never typed return-down). SMI/NMI/MCE/INIT/ME/microcode = hostile down-preemption class: pinned stub records + resumes, never dispatches language call.

## THINK-1 (from metal, pre-research)
x86 never asks permission downward. Faults/interrupts/VMEXITs always vector UP to a pinned descriptor/table (IDT entry, VMCS host state, SMRAM entry). There is no silicon primitive for "lower layer calls a service in upper layer and waits." Downward transfer is always preemption/theft (SMI/NMI/INIT/MCE steal the core, save minimal state elsewhere). It does not preserve caller continuation semantics. So "syscall/downcall" is software fiction: SYSCALL/INT/VMCALL look like calls down but silicon treats them as voluntary faults trapping UP to a more privileged handler. Alternatives die here: BIOS/UEFI runtime services require trusting callee below to return faithfully (silicon gives callee SMI/NMI/RSM powers caller can't revoke); capability-gated hybrid still lets lower invoke upward with continuation expectations (NMI/SMI lossiness breaks exactly-once); full bidirectional IPC assumes symmetric trap — silicon is asymmetric by design.

## HARDWARE FINDINGS (x86-only, web)
- VMEXIT reasons wired in silicon: 32-bit exit reason in VMCS, basic reason low 16 bits enumerated (Appendix C): exception/NMI, external interrupt, I/O SMI vs Other SMI (SMM VM exits), RSM, VMCALL. VMX non-root VMCALL/privileged insns/events cause VM exits to VMM, replacing ordinary behavior. Pin-based controls (external-interrupt exiting, NMI exiting) + exception bitmap (bit=1 → VM exit, else IDT). Sources: Intel SDM Vol 3C VMX chapters; Table C-1.
- SMI/SMRAM non-trappable theft: SMI via SMI#/APIC only, unmaskable by CLI/IF, independent of IDT, takes precedence over NMI/debug; CPU waits for instruction boundary, saves context to SMRAM, all cores rendezvous; exit only via RSM (0F AAh), RSM outside SMM → #UD; invalid SMRAM → shutdown. Dual-monitor is the sole exception, still SMM-world entry, not trappable guest event. Sources: OpenSecurityTraining IntroBIOS SMM deck (Intel SDM-derived); Intel SDM Ch.34; KVM SMM/RSM threads.
- NMI: vector 2, NMI pin or APIC delivery-mode NMI; unmaskable by IF; servicing blocks further NMIs until IRET; IRET unblocks even on fault; edge-triggered single latch — concurrent NMIs coalesce/lost; NMI exiting=1 → VM exit else IDT 2. Virtual-NMIs/NMI-window only virtualize readiness. Sources: Intel SDM Vol 3A §6.7/6.7.1; Vol 3C.
- IDT mechanics: task/interrupt/trap gates; vector = index; gates invoked like CALL-gate call in current task context; interrupt gate clears IF, trap gate doesn't; no downward parameter passing — handler inherits interrupted context, returns via IRET. Sources: Intel SDM Vol 3A §6.8.2.

## THINK-2 (after findings)
Confirmed, sharpened: silicon has exactly one honest crossing — event → indexed pinned handler with saved context. VMEXIT reason register, IDT vector, SMRAM save map are the same pattern at three rings. None returns a typed value; resume is restore, not return-value. SMI worse than assumed: dedicated SMM VM exits visible only inside the SMM world — guest code cannot trap/mask/gate/capability-check it; any model treating SMI as slow upcall is false. NMI unfit as upcall for opposite reason: IDT-vectored but unreliable (edge, one-deep latch, IRET-unblock footguns Linux works around) — must be idempotent log-and-continue only, never IPC.

## REASONS
Matches IDT/VMCS/SMRAM direction; one crossing shape = auditable TCB; removes Thompson-hiding spot (no "trusted helper below" to subvert return path).

## PROVING EXPERIMENT (distinguishes anchor)
Enumerate all crossing opcodes/paths in AISL spec. For each, force-fire under contention: nested NMI during handler, SMI during VMCALL-upcall window, double-NMI coalesce, RSM-outside-SMM. Anchor predicts: upcall path preserves exactly-once typed delivery (counter + payload intact); preemption path shows loss/reorder (NMI latch drops second event) and theft with no VMEXIT/IDT trace (SMI). If any downcall/IPC path survives with exactly-once typed return under SMI+NMI storm, anchor is wrong.

## WHAT WOULD CHANGE MY MIND
Silicon evidence of a maskable, trappable, exactly-once downward invoke with typed return that SMI cannot preempt and NMI cannot coalesce — a new VM-exit reason + IDT-independent gate with hardware queue depth >1. Absent that, keep upcalls-only.
