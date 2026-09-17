# Swarm review v5 — new-interface-to-intent (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 theater audit.

## Round-2 kills / revisions
- Pointing-at-live-pages via debug regs (DR0–3) KILLED as theater: 4 regs, process-scoped, no domains. Replaced: temporary EPT alias + PKU key + PCID/VPID.
- TPM-seal-whole-page-before-every-resume KILLED: ~10s ms quote kills microsecond resume. Revised: TPM pins root/microcode only; sentry checks hash-diff + append-log + torn-proof.
- Unbounded demo-cell PT replay KILLED: bounded by bytes-per-lane + memo shards + joule lease.
- Phantom unanimous veto KILLED: fail-closed (no quorum in deadline = no run); posted-PIR votes only.
- Sealed-diff + causal-undo overlap SPLIT: seal = integrity gate before resume; undo = bounded causal index after.

## Final v5
- reference-is-ept-cap-not-string: point must be MMU capability to live page (eptp,vpid,perm,deadline); never filename/prompt noun.
- forbid-pages-before-permit-code: page-types via EPT+PKU+CET trap first; no allowlist code runs.
- demo-in-disposable-cell-with-bounded-pt: EPT-clone arena + PT/LBR trace capped by bytes-per-lane and joule lease, or it didn't happen.
- proposer-drafts-checker-judges-never-same: Bun rule in metal — draft cell unreadable-writable by judge, judge domain isolated, store only by append-log.
- veto-only-by-live-ipi-quorum-fail-closed: no quorum in deadline = no run; votes by posted-PIR with no shared memory.
- measure-diff-before-resume-tpm-pins-root: TPM pins microcode+log root only; sentry checks hash-diff pre-resume in microseconds.
- ambiguity-faults-outward-not-retries: unclear intent is outward oracle fault vector, never prompt-error loop inside cell.
- undo-is-bounded-cow-causal-index: undo = N-entry CoW snapshot index on send-tick only; eviction by endurance/joule cap.
