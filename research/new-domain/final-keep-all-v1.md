# Final keep-all v1 — new-domain (additive, non-destructive)

v1 + v2…v5 + brutal-combine-v1 kept. Unions v5 FINAL (6) with brutal FINAL (5). Domain owns buyer-facing shapes only.

## Round 1 — shape verdicts (buyer? deployable? order? leakage? duplicate?)
- dmaless-signing-vault → signing-vault KEEP, beachhead #1: CA/firmware/registry/custody/OTA pay high for quorum ceremony + audit; tolerates slow; control plane over existing HSMs (not HSM-replace); ceremony-sign + automated-sign modes; no-export/no-single-open rules.
- disposable-ci-worker KEEP, #2: platform/SOC2/foundation volume; secret-zeroization + no cross-job persistence + attested clean start; explicit content-addressed cache imports (no ambient persistence); warm-pool SLA without dirty reuse.
- tpm-sealed-inference-node → sealed-inference-node KEEP, #3: hospital/bank/sovereign/EU-AI-Act premium; CPU-node sealing + attested fleet dashboard (GPU memory out of scope — no hallucination); sealed migration/upgrade policy + human-readable attestation bundle.
- disposable-agent-cell KEEP, #4: enterprise agent platform per-task premium; capability manifest first-class (files/net/tools, deny-by-default, logged checkpoint export); least-privilege scoped secrets + output review (no prompt-injection magic claim); distinct from CI (interactive+tools vs batch).
- sealed-recovery-escrow KEEP, #5: continuity/risk insurance; no-open-alone-or-secretly (quorum + delay + transparency log); quorum recovery drill + social recovery; offline/air-gap copy + append-only (refuse delete).
- ept-fuzz-partition → isolated-fuzz-lab RESURRECTED, #6 specialist: MSRC/chip-vendor/browser/EDR/red-team/government pay for containment + deterministic crash replay; harness import (run guest blobs unmodified); rapid-retry vs clean-slate budget split; mechanism-sharing with agent-cell does NOT merge buyers (different buyer/metric/motion: betrayals-found + containment-uptime + replay-fidelity vs task-success).
- lockstep-dissent-pair: reconsidered, DECLINED as SKU (same-image dual gives no Thompson diversity; x86 nondeterminism fights lockstep; duplicates quorum inside vault/escrow) — kept as dual-approval policy note inside vault+escrow; revisit only on heterogeneous second stack.
- Drops confirmed: powercut rig, joule node, trust rig outward (lab gear/telemetry/vague, no buyer workflow); generic sandbox recast into ci-worker + agent-cell.

## Round 2 — buyer attacks + fixes (excerpted)
- "Have HSM/KMS already" → control-plane + ceremony + audit positioning; automated-sign path so devs don't bypass.
- "Firecracker free" → compliance artifact (zeroization + attestation), high-risk jobs first; cache-by-hash keeps speed honest.
- "Firmware update bricks sealed keys" → sealed migration/upgrade + escrow backup + degraded read-only; scope honesty on GPU.
- "Isolation breaks agent utility" → capability manifest + scoped secrets + logged export; blast-radius + audit promise, never injection-proof.
- "Escrow = honeypot/subpoena" → cannot-open-alone-or-secretly + jurisdictional spread; loss-of-quorum answered by drills + social recovery.
- "Have KVM rigs already" → harness import + containment/replay value; throughput via retry-vs-slate budget split.
- Mechanism scrub applied: no DMA/IOMMU/TPM/EPT/VT-x in buyer names or one-line whys; disposable/sealed retained as observable properties.

## Final (6, beachhead order)
- signing-vault: quorum-ceremony signer unstealable even by malicious peripherals; slow-tolerant for roots/releases.
- disposable-ci-worker: fresh attested worker per CI job; no secret leaks, no backdoor persistence; volume for untrusted PRs.
- sealed-inference-node: regulated inference sealed to measured boot with auditor-readable attestation; unseals only when known-good.
- disposable-agent-cell: per-task ephemeral cell for tool-using agents; explicit capability manifest, logged export; blast radius contained.
- sealed-recovery-escrow: quorum-plus-delay encrypted backup no single admin/vendor can open alone or secretly; drill-gated.
- isolated-fuzz-lab: standalone specialist rig detonating hostile drivers/firmware with containment + deterministic crash replay.
