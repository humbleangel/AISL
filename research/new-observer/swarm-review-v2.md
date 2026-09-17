# Swarm review v2 — new-observer (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- human-intent-signoff: click-through theater, assumes human can judge machine effects from language text.
- independent-verifier: empty word without separate metal root; same CPU, same blind spots, overlaps all others.
- adversarial-agent: vague red-team label; software adversary on shared-cache SMT is observable and gameable.
- operator-traceback: 80-year syslog thinking; forgeable post-hoc text with no metal binding.
- boot-witness: stale secure-boot replay; attests loader hash once, blind to runtime intent-to-metal drift.
- replica-dissent: datacenter Byzantine import; 3x cost on few strong cores, local diverse re-execution wins.

## Completed sublist v2
- intent-emit-comparator: sees language AST and emitted x86 bytes in one address space, impossible when OS and language are split.
- ring-minus-one-sentry: VT-x/EPT watcher the system cannot touch, answers Thompson 1984 with a metal outside the compiler.
- pmu-power-judge: uses perf-counters/RAPL/thermal as physics ground truth, watts and misses cannot lie like logs can.
- page-fault-witness: uses MMU accessed/dirty bits and EPT violations as proof of what memory was truly touched.
- nvme-append-trail: write-once persistent journal that survives power loss and judges after crash, not volatile dmesg.
- tpm-intent-quote: TPM quotes intent-hash plus pagetable-hash together, binding human will to live metal state.
- sibling-core-dissenter: second physical core re-emits critical intent diversely and compares, replica dissent without network.
