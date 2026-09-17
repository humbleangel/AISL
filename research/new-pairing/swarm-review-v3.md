# Swarm review v3 — new-pairing (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3.

## Collisions resolved
- syscall-is-call vs subtraction/no-syscall-boundary: SUBTRACTION owns deletion, PAIRING owns replacement pairing.
- fault-is-continuation vs inversion/faults-are-control-flow + errors-only-as-faults: INVERSION owns principle, PAIRING owns continuation-capture mechanism.
- page-table-is-type vs substrate/page-capability-mmu vs constraint/hardware-bound-capabilities-only: SUBSTRATE owns enforcement, CONSTRAINT owns restriction, PAIRING owns type-identity (walk == check).
- heap-is-persistent vs substrate/inversion/constraint: SUBSTRATE owns store, INVERSION owns structure, CONSTRAINT owns rule; PAIRING cedes this one.
- build-is-attestation vs oracle/substrate/abundance/organization (5-way): ORACLE owns verdict, SUBSTRATE owns mechanism, ABUNDANCE owns chain artifact; PAIRING keeps generative side only.
- speculation-is-effect vs scarcity/failure/observer: FAILURE owns leak, SCARCITY owns budget, OBSERVER owns detection, PAIRING owns effect-typing.
- power-is-capability (6-way): SCARCITY owns headroom, SUBSTRATE owns mechanism, ORACLE/OBSERVER own proof/measure; PAIRING owns capability-token form.
- lane-is-thread vs substrate/simd-vectors-first vs scale/million-lanes-not-tasks: SUBSTRATE owns vectors, SCALE owns count, PAIRING owns lane==thread abstraction.

## New insights
- ept-is-module-system: EPT partition as language module boundary; enter/exit == import/call.
- interrupt-is-message-send: APIC IPI / VT-x exit as the only inter-core message primitive, no queues, no shared memory.
- avx-mask-is-capability: AVX-512 k0-k7 masks as predicate capabilities; lane predication == permission.
- nvme-queue-is-channel: NVMe submission/completion pair as language channel; persist == send.
- tlb-is-type-cache: TLB as memoized type-derivation cache; shootdown == revocation broadcast.
- thermal-is-backpressure: RAPL/thermal throttle as typed backpressure value, not fault; hot core yields lanes.

## Completed sublist v3
- syscall-is-call: call instruction is the only trap; no libc stub, no number table.
- fault-is-continuation: page/VT-x fault captures delimited continuation the handler resumes or abandons.
- page-table-is-type: PTE permission+domain is the type; walk success == typecheck.
- speculation-is-effect: speculative/secret-dependent ops require effect annotation, else forbidden.
- lane-is-thread: one AVX lane is one language thread; mask is its scheduler.
- power-is-capability: joules/thermal headroom is a spendable unforgeable token, not a counter.
- ept-is-module: EPT root switch is module import; cross-domain call is EPT transition.
- interrupt-is-send: IPI is language send; arrival is message receipt, never preemption.
