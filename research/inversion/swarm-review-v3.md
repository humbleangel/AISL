# Swarm review v3 — inversion (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. INVERSION owns direction-flips and philosophy; mechanisms ceded outward.

## Collisions resolved
- language-defines-traps vs pairing/syscall-is-call vs subtraction vs constraint: authority-flip stays in INVERSION; call mechanism to PAIRING; removal to SUBTRACTION; ABI ban to CONSTRAINT.
- caller-always-allocates vs constraint/explicit-allocation-only: duplicate. CONSTRAINT owns. DROPPED from inversion.
- faults-are-control-flow vs pairing/oracle/observer/time: INVERSION owns philosophy (fault is normal path); PAIRING owns continuation mechanism; ORACLE/OBSERVER own verdict/witness; TIME owns replay/deadline use.
- kernel-is-guest vs substrate/observer/abundance/forgetting/scale: INVERSION owns direction-flip (guest-first); SUBSTRATE owns EPT mechanism; OBSERVER owns sentry; ABUNDANCE owns per-actor use; FORGETTING owns ring0 removal; SCALE owns whole-clone use.
- pages-are-the-heap vs substrate/pairing/time/interface: duplicate. SUBSTRATE owns page primitive; PAIRING owns type/persistence; TIME owns versioning; INTERFACE owns UX. DROPPED from inversion.
- code-follows-data vs abundance/substrate: INVERSION owns ordering-flip (data selects code); ABUNDANCE owns generation mechanism; SUBSTRATE owns vector primitive.
- errors-only-as-faults vs pairing/failure/time: INVERSION owns channel-flip (no in-band codes); PAIRING owns delivery; FAILURE owns taxonomy; TIME owns deadline use.

## New insights
- Upcalls-only: app never calls down; kernel only resumes typed continuations up. No downcall path at all.
- Drivers-are-keywords: MMU/AVX/RAPL/TPM/PMU/VT-x are language verbs with types; cpuid-shaped code disappears.
- Volatility-is-cache: all pages persistent by default; DRAM is write-back cache; power loss is no event, resume is the only boot.
- Preemption-is-polling: CPU never asynchronously preempts; language polls hardware at typed safe-points.
- Return-never-happens: kernel never returns, only resumes; success and error unify as resuming different continuations.

## Completed sublist v3
- language-defines-traps: app language, not kernel, names trap vectors as typed signatures; Thompson authority flip.
- faults-are-control-flow: page/protection fault is the normal call path, not the exception path.
- kernel-is-guest: tiny EPT host holds partitions; all OS/app code runs as guest; ring0 monolith inverted.
- code-follows-data: data layout and page type select specialized code; no generic code fetching alien data.
- errors-only-as-faults: no return-code channel; wrong intent traps to a typed fault continuation.
- upcalls-only-no-downcalls: eliminates downcall direction; kernel only resumes upward, never invoked downward.
- metal-is-vocabulary-not-drivers: AVX/MMU/RAPL/TPM/PMU exposed as checked language verbs, zero driver layer.
