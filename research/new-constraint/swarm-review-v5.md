# Swarm review v5 — new-constraint (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 reality check (hallucination / executability / sense / coherence).

## Round-2 kills
- Per-epoch TPM-quoting of SMM/ME killed as hallucinated (no ISA path; TPM signs PCRs, not live SMM/ME).
- Microcode-pinned-expiry as enforceable expiry killed (can record revision, cannot prove running ucode).
- NVMe-AES as device torn-proof killed (host-constructed CoW+CRC instead).
- Contradiction fixed: pinned-firmware-trust vs never-trust — resolved by inversion (trust nothing below language).
- Single-address vs disposable-arenas resolved: single VA, multiple EPTP/VPID views.
- Per-page TPM vs microsecond resume resolved: fast local hash, slow TPM epoch anchor.

## Final v5
- explicit-arena-alloc-only: every byte from a named, bounded arena grant; no heap/malloc/sbrk.
- capability-no-ambient-only: authority is PKU-key+EPT-view+PCID only; no fds/paths/root/clock.
- single-va-no-coherence-only: one virtual address, many EPTP/VPID views; no coherent shared mutable, no atomics/locks; cross-core by posted-PIR send + temporary alias.
- run-to-completion-smt-off-only: one thread per core, SMT off, no preempt/steal; timer is bounded deadline-send, stolen TSC is fault.
- crash-only-cow-append-only: never overwrite/delete; CoW + monotonic append snapshot log; host-side torn-proof CRC; volatile explicit.
- measured-pages-execute-anchored-only: X-only if hash in live manifest; fast local check, TPM anchors epoch digest, never per-page TPM.
- smi-is-fault-no-firmware-trust-only: SMM/ME/ucode unmeasurable by construction; SMI/NMI counts as nonmodule betrayal fault with energy/time accounting.
- joule-deadline-bounded-only: every run carries joules+ticks+trace-bytes budget on RAPL/PMU/IPI mesh; exceed or desync aborts as fault.
