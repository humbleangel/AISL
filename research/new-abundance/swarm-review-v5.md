# Swarm review v5 — new-abundance (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 abundance audit (few SMT cores, nested-walk tax, endurance, flush-lie).

## Round-2 kills / bounds
- Unbounded EPT-clone fanout KILLED (nested-miss storm + shootdowns): 2–8 live EPTP views + microsecond resume.
- Infinite NVMe append KILLED (endurance + torn-proof + flush-lie): DRAM COW abundant, durable commits leased/sparse.
- Idle-lane/background folklore KILLED (no idle on loaded few cores; snoopers violate containment): leased precompute + explicit reverify only.
- N-voter IPI mesh KILLED for N>3: 1 proposer + max 2 dissenters.
- Infinite memo KILLED: per-core byte-capped shards + scrub.
- Free proof-check KILLED: every proof carries cycle/joule bound.

## Final v5
- resume-cow-arena-8views: variants are microsecond resumes of measured COW pages, not clones; 8 live EPTPs max, TLB cost counted.
- proof-or-fault-bounded-check: every generated artifact carries proof with cycle/joule bound; no proof or over-budget is a fault, not retry.
- two-dissenter-ipi-vote: one proposer, max two dissenter cores via posted-PIR sends, no shared memory, dissent replays.
- per-core-memo-no-share: content-hash memo sharded per core with AVX scrub, never coherent, byte-capped for cache residency.
- joule-leased-precompute: precompute only under RAPL joule lease with deadline-send; no background, no idle folklore.
- dram-snap-cheap-nvme-commit-leased: DRAM COW snapshots abundant with hash trail; NVMe durable commits sparse, leased, flush distrusted.
- explicit-reverify-on-use: no background verifier; reverify fires explicitly on resume/receive with PMU/speculation crosscheck.
