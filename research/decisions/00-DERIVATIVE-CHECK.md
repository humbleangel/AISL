# DERIVATIVE CHECK — are the remaining ~87 entries consequences of the 28?

Method: for each of the 16 research items, take its final-keep-all list, mark the entries
identical to (or directly named by) an accepted decision as CORE, and show every other entry
deriving from a CORE entry or from another accepted decision via the ownership/collision
resolutions recorded in the brutal and final rounds. A derivative is legitimate iff removing
its parent would remove its reason to exist. Verdict per item below.

## new-constraint (8) — CORE: single-address-no-share, run-to-completion, crash-only-COW, measured-exec, SMI-detect, joule-lease
- explicit-arena-alloc-only ← single-address-no-share + capability primitive (decision 9): arenas are how single-VA memory is granted without a heap.
- capability-no-ambient-only ← decision 9 + Thompson-pin (12): authority must be HW-bound or Thompson hides in ambient paths.
- measured-exec-fault-upcall-only ← Thompson-pin (12) + upcalls-only (6): measurement enforced at the only crossing shape allowed.
- smi-detect-quarantine ← SMI-dissent (21): detect is the local half of vote-based posture.
- joule-tick-lease ← time composition (18) + speculation scope (22): every run needs both a clock bound and an energy bound.
Verdict: 5/5 derivative. No independent axiom. PASS.

## new-pairing (7) — CORE: module-is-EPTP-view, tsc-delta, pku-subgrant, cet-return, kmask-select, pt-lbr-witness, joule-lease, ipi-ballot
All 7 are the accepted pairing set itself (decision 2 refined it to EPTP/VPID mechanics; the rest are
fusions already owned: tsc-delta ← time (11); joule-lease ← budgets (18); ballot ← dissent (4)).
Verdict: pairing is fully absorbed into decisions 2, 4, 11, 18. No residue. PASS.

## new-oracle (7) — CORE: outward-quorum-gate
- dissenter-replay, speculation-tripwire, scrub-trail, microcode-expiry, causal-cite, degraded-quorum ← all are
  outward-quorum-gate applied to six evidence types (replay slice, PMU signal, scrub trail, microcode rev,
  page cite, degraded physics). Remove the gate and none has a consumer.
Verdict: 6/6 derivative of decision 3. PASS.

## new-observer (7) — CORE: IPI-dissent-mesh
- ept-ad-poll, ptwrite-witness, joule-time-judge, iommu-witness, cow-trail, avx-sweeper ← all are
  sensor instantiations feeding the dissent mesh + quorum gate (decisions 3, 4). Each exists to supply
  one evidence type the gate consumes. Remove gate+mesh and they observe for nobody.
Verdict: 6/6 derivative of decisions 3–4. PASS.

## subtraction (8) — CORE: no-syscall-use + honest use-deletions
- no-files/fds, no-processes/fork, no-unmeasured-exec, no-shared-writable, no-ambient/heap/wall, no-unfenced-DMA,
  no-legacy-trust ← each deletes exactly the complement of one accepted positive (caps, arenas, measured-exec,
  single-VA, upcalls, fenced-DMA, pinned-adversary). They are the negative image of decisions 1, 5, 6, 9, 12.
Verdict: 7/7 derivative (negation is derivation). PASS.

## inversion (7) — CORE: upcalls-only + pinned-adversary + leased-COW + fenced-default
- language-maps-faults, errors-as-cow-faults, code-follows-pinned-data ← all are upcalls-only (6) restated at
  three layers (trap taxonomy, error channel, placement). Fenced-default ← speculation scope (22).
  Leased-COW ← persistence (20). Pinned-adversary is itself decision 6's companion.
Verdict: 6/6 derivative of decisions 6, 20, 22. PASS.

## new-abundance (6) — CORE: resume-COW-arenas
- proof-bounded-check, ipi-ballot-cheap, per-core-memo, joule-leased-precompute, dram-snap/nvme-commit ← all are
  resume-arena economics: what is cheap given COW resume (snapshots, memos, precompute under lease) and what
  stays scarce (proof, votes, commits — each capped by decisions 3, 4, 18, 20).
Verdict: 5/5 derivative of decisions 7 + caps. PASS.

## new-scarcity (6) — CORE: review-bandwidth-first + measured budgets
- joule-headroom, nested-TLB, commit-endurance, quote-bandwidth, trace-replay ← all are the metered budgets that
  make review-first executable (decision 8 requires machine pre-filtering to be cheap; each budget prices one
  filter). Remove review-first and they are unowned numbers.
Verdict: 5/5 derivative of decision 8. PASS.

## new-substrate (7) — CORE: page-capability-MMU + module cells + measured boot
- pku-pcid, ept-cet-vpid, iommu-ir, nvme-aes-cow, tpm-pin, avx-lanes, ipi-rapl-pmu-mesh ← each is one hardware
  primitive implementing capability-MMU (9), module views (2), measured boot (12/25), or budget sensing (18).
  None states policy; all are mechanism instantiations of accepted decisions.
Verdict: 7/7 derivative. PASS.

## new-scale (8) — CORE: pinned-endpoints + measured caps
- lanes-ceiling, clone-fanout-cost, resume-us, trace-budget, joule-ceiling ← all are the min() inputs to the
  pinned-endpoint admission formula (decision 10). Degraded-demo + fleet-count ← the two edge cases of the
  same formula (N=1, N=fleet). Remove the cap and no number has a use.
Verdict: 7/7 derivative of decision 10. PASS.

## new-time (6) — CORE: causal-tick + dual-budget + sparse-epoch
- resume-maxtick, tick-on-send, tsc-delta-slice, skew-as-dissent, wall-range, epoch-trail ← the six faces of
  decision 11/18: order (tick), measure (delta), theft handling (dissent), exterior (range), kill (budgets),
  attest (epochs). Each is one clause of the time composition.
Verdict: 6/6 derivative of decisions 11 + 18. PASS.

## new-failure (8) — CORE: Thompson-pin + pinned-adversary + dissent + budgets
- All 8 modes are threat instantiations of accepted detectors: compiler→page-pin (12), firmware→TPM-pin (12/19),
  corruption→scrub/replay (3/4), torn/flush→COW/lease (20), side-channel→fenced-default (22), exhaustion→budgets
  (18/8), skew→dissent (21). Each mode exists because its detector was decided.
Verdict: 8/8 derivative. PASS. (irreversible-misintent stays buried: no detector, no entry.)

## new-interface-to-intent (8) — CORE: EPT-cap refs + Bun-split + quorum veto + pinned resume
- Each gesture entry is one clause of decisions 13 (references), 27 (pipeline), 3 (quorum): point, forbid,
  demo-cell, draft-never-check, live-veto, sealed-diff, outward-ambiguity, bounded-undo. Remove the pipeline
  and no gesture has a judge; remove caps and no gesture has a holder.
Verdict: 8/8 derivative of decisions 13 + 27 + 3. PASS.

## new-forgetting (9) — CORE: coherence-is-cost (+8 folklores as curriculum)
- The 8 folklores + speculation are the pedagogical duals of accepted bans: each names the belief whose
  removal a subtraction/constraint decision enforces. Coherence→single-VA (1), scalar→lanes (10),
  flush→COW (20), firmware→pinned-adversary (6), preemption→RTC (1), heap→arenas (1/9), now→tick (11),
  overwrite→append (20), speculation→fenced-default (22).
Verdict: 9/9 derivative (curriculum, not axioms). PASS.

## new-domain (6) — CORE: signing-vault-first + beachhead order
- ci-worker, inference-node, agent-cell, escrow, fuzz-lab ← each is a later beachhead position in the order
  decision 15/25 establishes (volume → regulated → interactive → insurance → specialist), reusing the same
  mechanisms (cells, quorum, sealing) proven by the vault.
Verdict: 5/5 derivative of decisions 15 + 25. PASS.

## new-organization (7) — CORE: single-owner + pipeline + escrow
- Quorum-gate, dissent-first, intent-merge, silent-log, classroom, successor ← each is one clause of decisions
  16, 27, 28: gate policy, order policy, artifact policy, attention policy, quarantine policy, continuity policy.
  Remove owner/pipeline/escrow and no clause has an enforcer.
Verdict: 6/6 derivative of decisions 16 + 27 + 28. PASS.

## Overall verdict
~87 non-core entries checked against the 28 accepted decisions. All derive: as mechanism
instantiations, negative images, metered budgets, evidence types, curriculum duals, later beachheads,
or pipeline clauses of a decided core. Zero orphan axioms found. The three resurrection/burial calls
(legacy deletion, fuzz-lab SKU, misintent burial) were already decided in their rounds and are
consistent with the 28. Convergence is closed: 28 decide, the rest follows.
