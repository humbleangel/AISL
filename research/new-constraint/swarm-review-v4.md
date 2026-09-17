# Swarm review v4 — new-constraint (additive, non-destructive)

v1 folders + v2/v3 files kept. Two-round protocol: round-1 draft attacked in round 2 for unenforceable absolutes (SMM/NMI can't be disabled), hidden merges, NVMe tear hole, NMI/SMI scope, unstated W^X.

## Round-1 → Round-2 changes
- no-hidden-execution restated as pinned-quoted-fenced (not absent).
- Address+coherence merge kept explicit in name.
- Crash rule hardened to copy-on-write/content-pages-only.
- Run-to-completion scoped to one-hardware-thread + synchronous traps.
- W^X folded into measured-pages.

## Final v4
- explicit-allocation-only: no malloc-folklore; every page/lane/joule named at intent time, PMU/RAPL accountable.
- no-ambient-authority-only: no root/fd/POSIX ambient; every access is hardware-bound capability.
- single-address-no-coherence-only: one address space, no coherent mutable sharing; cores meet only via IPI mesh + content pages.
- run-to-completion-one-thread-per-core-only: one hardware thread per core, SMT off, no async steal; interrupts arrive as explicit messages.
- crash-only-copy-on-write-persistence: never overwrite live state; powercut-safe append of content pages, replay resume-not-boot.
- only-measured-pages-execute: W-xor-X pages execute only if TPM-quoted; closes CPU-visible Thompson hole.
- no-hidden-execution-pinned-firmware-only: SMM/ME/microcode version pinned and PCR-quoted per epoch, SMI fenced; closes Ring-minus-2 hole.
- bounded-energy-time-only: every intent carries joule/tick cap enforced by PMU/RAPL; overrun is fault-handler, not slowdown.
