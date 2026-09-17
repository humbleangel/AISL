# Decision brief 16/28 — ownership model

RECOMMENDATION: single-owner-with-drill, noncustodial. 1 human holds expiring lease; auto-revoke; no self-renew — renewal requires successor + m-of-n escrow quorum issuing new lease. Named successor (1) + m-of-n escrow (e.g. 3-of-5) holds PolicyAuthorize key shares + lease-grant authority, never daily sign authority. Quarterly quarantine restore drill on isolated x86-64.

## THINK-1 (trilemma)
Bus-factor=1 is fatal if lease is perpetual/custodial/self-renewable (classic single-owner dies, gets coerced, loses key → AISL unbootable/unreleasable). Split-brain is worse: dual-core co-ownership = 2 divergent PCR policies, 2 sealed blobs, disagreement on release — on x86 you don't fight the CPU (one machine = one TPM, one NV index, one PCR set; two owners = fork). Bandwidth is the limit (Thompson + Bun: 1 human + swarm + independent oracle; committee/rotating ownership multiplies reviewers per release; review bandwidth scarce; extra approvers = latency + diffused responsibility). Ownerless automation removes the human decider entirely — violates premise. Want: one decider at a time, unable to persist, unable to self-perpetuate, replaceable without the incumbent.

## HARDWARE FINDINGS (TPM seal/unseal + PCR mechanics, web)
- Seal = bind secret to PCR values; unseal only if current PCRs match seal-time values. Foundation for measured boot / disk key protection.
- PCRs brittle: BIOS update, kernel/bootloader patch changes PCRs → seal_pcr blob fails to unseal. Expected behavior.
- Fix: PolicyAuthorize + signing key — TPM-resident ECC/RSA key re-authorizes new PCR values after legitimate update; secret survives without re-seal. Production pattern for updating systems. If signing key regenerated/lost, blob un-unsealable — key + blob kept together.
- Alternative: seal_nv — secret lives entirely in TPM NVRAM under PCR policy, no external blob (store/read/delete lifecycle).
- Flow (tpm2-tools/Heads): trial session → tpm2_policypcr [+ policauthvalue] → tpm2_create/nvdefine with policy digest → later policy session reconstructed → tpm2_unseal. Heads seals DUK/TOTP to PCRs 0–7, pre-computes future PCR4/6 via calcfuturepcr; entering recovery extends PCR4 and permanently kills unseal for that boot.
- Hard limits: sealed keys cannot be extracted/copied, machine-bound, no backup by design; TPM Clear destroys all; BIOS update may invalidate; counter in NVRAM for rollback protection.
- Sources: wolfSSL 2026-05-05 TPM 2.0 Sealing Policies; wolfTPM seal README; tpm2-tools tpm2_policypcr.1; Heads doc/tpm.md; TCG TPM 2.0 Part1 Architecture.
- Implication: don't escrow TPM itself. Escrow the PolicyAuthorize signing key + NV auth + successor definition off-machine via m-of-n. TPM enforces locality; paper/people enforce recovery.

## THINK-2 (after findings)
Single TPM = single trust root per machine; non-exportability makes co-ownership/committee custody theater — you can't have 3 people "hold" the same unexportable key; only n people holding shares that reconstruct authorization. Collisions: vs quorum-gate (gate lives in escrow/successor activation, not daily release — daily = 1 owner fast; death/loss = m-of-n + named successor slow but possible); vs successor-escrow (named successor + m-of-n is lease-granter after expiry, no standing power → no split-brain); vs review-bandwidth (1 lease-holder = 1 review slot). Lease tick+joule+epoch + auto-revoke + no self-renew is the anti-Thompson trick: even compromised owner can't make backdoor permanent; expiry enforced by epoch + TPM PolicyCounterTimer/monotonic + quarantine image refusing expired lease.

## REASONS
Don't fight CPU: one TPM/one PCR set → one active owner. Bandwidth: 1 reviewer, swarm works, oracle checks. Liveness without split-brain: expiry + successor solves bus-factor without dual-active.

## PROVING EXPERIMENT (quarterly operability drill, ~1hr, pass/fail)
Simulated owner-loss: declare owner dead, revoke/expire lease. On air-gapped spare x86-64, m-of-n reconstruct, install quarantine AISL image, attempt unseal with sealed blob + reconstructed auth. Success = boot + release signed by successor lease within <X hours, no incumbent contact. Fail = blob un-unsealable (lost authkey, PCR drift unhandled, NV-only secret with no backup) → fix escrow/update procedure before next quarter. Log tick+joule+epoch at each step.

## WHAT WOULD CHANGE MY MIND
Drill fails 2× consecutively due to PCR fragility → switch seal from seal_pcr to seal_policy_auth mandatory, or add dual-machine escrow-live replica. Successor + quorum colludes or can't assemble in time → dual-core with strict partition (one signs releases, one signs leases only). Review load proves >1 human needed → rotating ownership with single-active lease (time-division, never co-active).
