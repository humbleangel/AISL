# Swarm review v2 — new-abundance (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- generated-code-is-cheap: true but shallow — tokens are cheap, trusted artifacts aren't; without proof it restates the Bun liability.
- exhaustive-testing: carries 80-year test-by-running assumption; state explosion makes exhaustive a fantasy, unaffordable on power-limited metal.
- whole-system-snapshots: naive RAM-dump framing; unaffordable without COW/EPT-dirty-tracking/dedup to NVMe.
- precompute-everything: slogan that fights caches/thermals; unbounded precompute is just pollution with an eviction problem.
- trace-everything: unaffordable and self-defeating on speculative SMT cores — drowns in data and widens side-channels.
- parallel-verification: overlaps exhaustive-testing, vague; x86 has few strong contended cores, so only embarrassingly-parallel offline checking is actually cheap.

## Completed sublist v2
- disposable-specialized-code: generate per-site/AVX/paging-layout variants at install/boot and discard, never ship one generic binary.
- proof-carrying-generation: every generated blob enters rings only with machine-checkable proof + fuzz corpus; unverified bytes don't run.
- cow-time-travel-snapshots: use EPT-dirty-bits + COW to NVMe to snapshot per-syscall-boundary; reboot becomes pointer-rewind.
- vm-per-actor-isolation: use VT-x/EPT to give each language actor its own microVM; fork cost is a page-table clone.
- selective-hardware-tracing: use Intel-PT/PMU/VT-x-exits to trace only cross-ring/cross-page attestation events, never every fetch.
- measured-bootstrap-chain: re-do Thompson bootstrap under TPM/emulation with each stage hashed; hand-asm -> assembler -> compiler -> OS is attested.
- precompute-with-thermal-budget: precompute only hot tables/kernels to NVMe with power- and cache-aware eviction, not everything.
