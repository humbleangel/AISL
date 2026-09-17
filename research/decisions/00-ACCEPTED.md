# ACCEPTED — convergence record (28 decisions)

Status: decided. The 28 subagent recommendations below are accepted as the converged vision.
Grilling record: the 10 load-bearing calls were put to the owner one by one; all 10 confirm the
briefs' recommendations with no modification. No conflicts between grilling and briefs.

## Item-defining (16) — all confirmed by grilling where applicable
1. Memory model → single-address-no-share. [grilled: confirmed]
2. Modules → module-is-EPTP-view (VMFUNC, few large cells, PKU inside only).
3. Trust gate → outward-quorum-gate, batched joule-epochs, fail-closed.
4. Dissent transport → per-core-vector IPI mesh, OR-veto, fail-closed timeout.
5. Syscalls → DELETE unconditionally (SCE=0, no tripwire). [grilled: confirmed]
6. Crossings → upcalls-only; SMI/NMI hostile down-preemption, never IPC. [grilled as part of 5/9]
7. Cheap execution → resume-COW-arenas (≤8 live EPTPs, CPU-only microseconds, NVMe commit leased).
8. First scarcity → review-bandwidth-first (machines pre-filter, humans merge diffs only).
9. Capability primitive → PTE-key + PKRU + PCID + CET, arena-scoped.
10. Scale unit → pinned-endpoints, cap = min(EPTP, PCID, TLB-resident, power, trace).
11. Time base → causal-tick (send-only; TSC same-slice delta; wall outward-range). [grilled: confirmed]
12. Thompson detector → page-pin + diverse double-compilation, compiler/microcode pins separate.
13. Reference form → EPT-cap tuples (eptp, vpid, perms, deadline); strings/fds/hashes never dereferenceable.
14. Sharing doctrine → coherence-is-cost (single-owner default; sharing is measured).
15. First beachhead → signing-vault-first. [grilled: confirmed as VAULT-FIRST image]
16. Ownership → single-owner-with-drill, noncustodial, m-of-n escrow. [grilled: confirmed]

## Cross-cutting (12)
17. Syscall fate → DELETE (parity with mitigated syscalls; Thompson surface minimal).
18. Time composition → tick-primary; TSC annotates, APIC+RAPL kill, TPM sparsely checkpoints.
19. Trust root → MEASURED-ONLY, never PIN (even offline). [grilled: confirmed]
20. Persistence → COW-append rule + leased commits; journaled-in-place <1% exception. [grilled: confirmed]
21. SMI posture → DISSENT pure, no local fault threshold. [grilled: confirmed]
22. Speculation scope → invitation-windows (closed default, bounded unfenced regions). [grilled: confirmed]
23. Replay bound → adaptive-down-from-tight, fail-closed (4MiB single-ToPA atomic unit).
24. TLB budgets → 8-live cap + measured refill (INVPCID discipline, 2M-by-proof).
25. First image → VAULT-FIRST (golden UKI + PCR 7/11 before any fleet). [grilled: confirmed]
26. Seed language → MINIMAL-ASM hex0→M0 with diverse second seed, no fallback. [grilled: confirmed]
27. Merge pipeline → machine-gate → intent-diff → quorum-last, all fail-closed, no bypass.
28. Escrow → m-of-n 3-of-5 + NV-deadman + public veto window; owner holds zero shares.

## Grilling record (10/10, owner-decided, no deferrals)
single-address-no-share · DELETE unconditionally · tick-primary · measured-only (incl. offline) ·
COW-append-only as rule · VAULT-FIRST · minimal-asm, no fallback · single-owner (halt-on-life accepted) ·
dissent pure · invitation-windows.
