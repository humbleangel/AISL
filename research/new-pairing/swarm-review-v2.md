# Swarm review v2 — new-pairing (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- language-is-syscall: shallow sugar that still worships the ring boundary; if OS==language there is no syscall left to be.
- types-are-capabilities: 1970s reheat; erasure + revocation + TOCTOU make it unmergeable without hardware tags.
- scheduling-is-control-flow: async/await already did this; still pretends N:M threads hide few cores + shared cache + SMT.
- allocator-is-pager: fuses two wrappers around the same linear-memory lie, never questions volatile vs NVMe split.
- one-toolchain-boot-to-app: not a pairing, just monorepo romanticism; real-mode/firmware/TPM boot != app ABI.
- intent-is-contract: unfalsifiable fluff overlapping types-are-capabilities; unenforceable on metal without runtime tax.

## Completed sublist v2
- syscall-is-call: ring-cross trap deleted, CPL0/3 becomes a type-checked function call in one address space, not INT/SYSCALL boundary.
- fault-is-continuation: IDT/#PF/#GP/NMI vectors are typed language continuations, no asm stubs, hardware faults directly resume code.
- page-table-is-type: PTE permission bits fused with language type ID, allocator emits typed pages MMU enforces against confusion/use-after-free.
- heap-is-persistent: malloc/mmap/files fused into NVMe-backed single-level pointers stable across reboot, no volatile vs file split.
- build-is-attestation: compiler output fused with TPM measured boot, type/capability proofs extend PCRs so boot checks semantics not just hashes.
- speculation-is-effect: SMT/speculation treated as language effect, secret-dependent paths need capability or typechecker inserts fences.
- power-is-capability: RAPL/thermal budget as linear consumable type, over-budget closures refuse to compile/schedule, physics visible to code.
- lane-is-thread: AVX lanes + few strong cores replace OS threads, data-parallel closures scheduled to SIMD lanes not scheduler queues.
