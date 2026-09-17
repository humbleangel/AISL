# Swarm review v2 — new-scarcity (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- human-review-bandwidth: keep — only true new scarcity here, Bun lesson holds, rest depend on it.
- verified-trust: bloated abstraction, not a resource; overlaps review, renames Thompson 1984 problem without a budget.
- power-thermal-budget: real but stale framing; x86-64 scarcity is joules-then-throttle, not watts on a label.
- timing-predictability: wish, not resource; assumes fetch-loop determinism the metal already killed via SMT/speculation/shared cache.
- cache-locality: carries linear-memory assumption; scarce thing is shared-cache residency + interconnect, not abstract locality.
- side-channel-free-execution: negative goal, not economizable; overlaps timing, impossible fully-free on speculative x86 — budget secrecy, not purity.

## Completed sublist v2
- human-review-bandwidth: economize by minimal surface + generated-code quarantine, every line must earn human eyes.
- bootstrap-trust: economize by tiny hand-audited seed + reproducible build + TPM-measured boot, never re-verify the world.
- joule-thermal-headroom: economize by AVX/boost-aware codegen and idle-by-default, joules spent only to retire intent.
- determinism-budget: economize by pinning, partitioning, and no-speculation fast paths where jitter costs correctness.
- shared-cache-residency: economize by single-owner data placement and streaming discipline, treat LLC/TLB misses as faults.
- speculation-secrecy: economize by secret-free speculation by default, fences/flushes/domains only where secrets live.
- privilege-crossing-budget: economize by OS+language in one ring/domain, zero syscall/VM-exit/TLB-shootdown in hot paths.
