# Decision brief 15/28 — first beachhead

RECOMMENDATION: signing-vault-first. Single anchor SKU: custody/CA/firmware buyers pay high per key for IOMMU-walled no-DMA cell with TPM-sealed pages + quorum ceremony; tolerates slow.

## THINK-1 (buyer reality + readiness + cash)
Buyer reality: only custody/CA/firmware-signing buyers already have a budget line labelled "keys": $35–55k/appliance, $130–180k for HA+offline root, or $25–40k/yr CloudHSM HA. They pay per-key heavily because loss = existential. CI buyers pay per-minute, hate slow, churn fast. Inference buyers regulated but want GPUs, not x86-64 CPU cells. Agent-cell buyers hype, no procurement path. Technical readiness: AISL's current strengths (IOMMU-walled no-DMA cell + TPM-sealed pages + quorum ceremony) ARE a signing vault — no oracle/fleet attestation service needed day 1; one air-gapped-ish box + human ceremony tolerates PCR-drift pain. CI-worker/inference-node both require fleet-scale attestation policy maintenance (golden PCR versioning, IMA appraisal, vTPM trust story) not ready. Cash flow: vault = few units × high margin funds OS work; CI = many × razor margin needing volume AISL can't supply yet. Thompson logic: slow deterministic signing is exactly what x86-64 + language-level isolation does well.

## HARDWARE/MARKET FINDINGS (web, directly relevant only)
- HSM price umbrella high and durable: Thales Luna $20–50k/device; HA+DR ~$80–150k hw + 15–20%/yr support; small-CA 5-yr TCO ~$345k on-prem. Cloud: AWS CloudHSM ~$1.45–1.50/hr (~$13.1k/yr/device), HA 2+1 region ~$39k/yr device-only, $60–130k/yr with net/ops. FIPS 140-3 L3 adds 20–50% premium. Sources: evertrust.io 2026-06; axelspire.com 2026-02; esign.ai 2026-08.
- HSM-as-a-Service $1.8B (2026) → $8.36B (2035), 18.6% CAGR; finance leads (PCI DSS 4.0, SWIFT CSP); CC-lab queues 14–18mo concentrate pricing power. Source: markwideresearch.com 2026-05.
- Supply-chain pain funds pitch: avg supply-chain breach $4.91M (IBM 2025), longest lifecycle 267 days; 15% of breaches, 30% involve third party (Verizon DBIR 2025); SolarWinds: 18k trojaned signed updates, <100 actively exploited + 9 federal agencies, vendor paid ~$0 fine — cost landed on customers. Sources: databreachcost.com; IBM Cost of Breach 2025; deepstrike.io.
- Attestation ops reality favours vault: Linux TPM attestation fails at PCR drift (kernel/initrd/UKI update breaks golden values); IMA-measure ≠ IMA-appraisal (measure detects, doesn't prevent); log-replay gaps; vTPM inherits provider into TCB. Strong programs version expected PCRs with the image. Vault tolerates this (few pinned images, human ceremony, PolicyAuthorize for upgrades); fleet CI/inference does not. Sources: techzeph.com 2026-04; Keylime/Azure attestation docs; arxiv 2609.05011; liamcvw.com 2026-07.

## THINK-2 (after findings)
Wedge confirmed: (a) price umbrella leaves room for $50–100k AISL vault cell undercutting Thales pair + FTEs while beating CloudHSM on sovereignty (DORA, no CSP concentration); (b) $4.91M/267-day supply-chain cost + SolarWinds/XZ memory makes "signed build lied" the sale — quorum + sealed pages + no-DMA is a direct answer, CI speed is not; (c) attestation fragility kills fleet-first (CI/inference need versioned-PCR infrastructure AISL lacks; vault needs exactly one pinned image + ceremony AISL has). Cash: 2–3 vault design-wins fund the attestation service later beachheads need.

## REASONS
Only buyer who pays high-per-key, tolerates slow/deterministic, and buys exactly what AISL is today. Domain keeps buyer-facing shapes only; vault shape minimal (seal, quorum-sign, attest-one-box).

## PROVING EXPERIMENT (60 days)
Offer 3 custody/CA/firmware prospects a paid pilot: migrate one offline root / release-signing key into AISL cell. Success = quorum ceremony completes, TPM-sealed pages survive reboot, customer auditor accepts attestation log in lieu of one HSM. Price test: willing ≥$40k/yr or ≥$25k + support. Falsifying: 0/3 convert, auditor rejects ("not FIPS/EAL, no procurement path"), or PCR/upgrade handling needs >1 FTE per customer.

## WHAT WOULD CHANGE MY MIND
(a) Two auditors in writing require FIPS 140-3 L3 cert before any paid pilot → pivot to disposable-ci-worker (no cert gate, volume proves attest pipeline); (b) CI buyer prepays ≥$200k/yr for disposable attested builders → cash overrides; (c) sealed-inference LOI with GPU-TEE requirement AISL can't meet on x86-64 → confirms vault, kill inference-node until hardware changes.
