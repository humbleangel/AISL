# Decision brief 13/28 — reference form

RECOMMENDATION: EPT-cap references. Adopt: reference = (eptp, vpid, gpa-base, len, rwxU/S, deadline-epoch) to live page. Ban filename/prompt-noun/path/fd/hash as dereferenceable.

## THINK-1 (from metal)
CPU enforces exactly one reference that cannot be forged in software: linear → guest-phys → host-phys walk + TLB tag. Everything else — string path, int fd, hash — is data the CPU will happily misinterpret. Thompson 1984: if reference lives above MMU, attacker lives below it. Candidates: (1) Path/string: needs parser + resolver + namespace on every use — TOCTOU by construction; prompt-noun worst case (attacker controls the string). (2) fd int table: one global integer namespace, ambient, forgeable, no perms attached, confused-deputy target; subtraction no-files-no-fds already kills it. (3) Content-hash only: good for immutability, zero for liveness/mutability/perms/revoke — can't express RW page until t or device DMA window; hash says what, never who-may-write-where-when. (4) EPT-cap (eptp+vpid+perm+deadline): the walker already checks it. Point = live MMU capability, not name. Pairs with page-table-is-type + page-capability-MMU: reference = PTE slice; revoke = clear PTE/shootdown; delegate = copy PTE with subset perms + shorter deadline. Bun rule: language proposes, human grades — don't let runtime grade its own strings.

## HARDWARE FINDINGS (web, metal only)
- SLAT/EPT since Nehalem: guest-phys treated as host-virt, hardware walks nested tables; unrestricted guest requires EPT; MBEC (Kaby Lake) splits EPT execute into user-execute + supervisor-execute. Source: Wikipedia SLAT (live search throttled, fell back to fetch).
- x86 debug regs DR0–3: 4 linear-address regs only; DR7 enables/type/len; CPL0-only MOV; local enables cleared on task switch. Source: Wikipedia debug registers.
- IOMMU (Intel VT-d / AMD-Vi): maps device-visible-virt → host-phys with own page tables; blocks DMA attacks; required for safe guest device passthrough. Source: Wikipedia IOMMU.
- TLB untagged until 2008 Nehalem VPID + AMD ASID; Westmere 12-bit PCID retains entries across CR3; INVVPID/INVPCID selective flush; Linux 4.14+ uses PCID. Source: Wikipedia TLB.
- Intel SDM Vol.3C known constants (confirm before RTL): EPT PTE bits 0/1/2 = R/W/X, 000 = not-present → EPT violation exit; EPTP = physical root + memtype + walk-length; VPID 16-bit (0 reserved); VT-d second-level tables mirror EPT perms + fault on unmapped DMA.

## THINK-2 (after findings)
DR0–3 confirms "theater" claim: 4 regs, linear not physical, task-scoped, debug-only (#DB), CPL0 — cannot name cross-domain pages, carry perms/deadline, or cover DMA. VPID/PCID confirms cap needs vpid: without tag every domain switch = full flush (slow or shared-entry temptation). IOMMU confirms device refs must be same shape (CPU-only cap leaves DMA hole): device cap = IOMMU second-level entry with same R/W + deadline, no separate device-path type. EPT R/W/X (+MBEC U/S-X) confirms perms belong IN reference, not sidecar ACL — CPU already ANDs guest-PTE with EPT perms; violation traps. Strings/fds/hashes get zero walker help.

## REASONS
Don't fight CPU: only form the walker enforces — forging string/fd/hash still faults; forging EPT-cap requires CPL0 page-table write. Revoke/delegate free: narrow perms or shorten deadline = new cap; revoke = clear leaf + INVEPT+INVVPID (+ IOTLB invalidate for device). Other forms need GC/ACL server. Least code: ~1 struct + walker + fault handler — no parser, no global fd table, no hash-store + revocation overlay. Collisions resolved: satisfies subtraction no-files-no-fds; executable form of page-table-is-type and page-capability-MMU.

## PROVING EXPERIMENT
Build 2 domains A/B, different EPTP+VPID sharing host page P mapped R-only in B. B attempts: (a) string-spoof "/P", (b) fd-guess 7, (c) replay stale hash of P, (d) store to EPT-cap R-only. PASS if a–d all trap (EPT violation/misconfig, never silent success), and after hypervisor clears B leaf + INVEPT, B's cached TLB entry also faults (no stale use). FAIL if any non-cap path reaches P. Device variant: NIC with VT-d table lacking P attempts DMA → DMAR fault, while CPU cap with W still works for CPU only.

## WHAT WOULD CHANGE MY MIND
Workload where EPT-cap cost (INVEPT/INVVPID/IOTLB shootdown rate) exceeds string/fd lookup AND survives adversarial TOCTOU test without adding equivalent revocation server — then the server IS the reference system and we renamed it.
