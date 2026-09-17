# Swarm review v3 — new-organization (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. ORGANIZATION owns people-policy only; mechanisms/verdicts/bots owned outward.

## Collisions resolved
- oracle-gated-merge vs oracle verdicts: ORACLE owns all verdicts; org keeps gate policy only.
- attested-bootstrap-log (5-way): SUBSTRATE owns measuring, ORACLE owns verdicting, OBSERVER owns trailing/quoting; org owns publication rule only.
- ringed-trust-lanes vs substrate/pairing/scale: SUBSTRATE owns EPT enforcement, PAIRING owns lane semantics, SCALE owns count; org drops hardware lanes.
- machine-adversary-first vs observer bots: OBSERVER owns bots; org owns default-on policy only.
- sandbox-classroom-track vs domain/interface: DOMAIN owns use-cases, INTERFACE owns demo cell; org owns people quarantine only.
- dual-intent-core vs single-image-owner (internal): split-brain risk, contradicts Thompson single-trust-root + Bun 1-human lesson. single-image-owner WINS; duality DROPPED.

## New insights
- Review-bandwidth is the MMU of organization: page humans out of code review into intent-diff review; code diff is the slow path.
- Oracle independence must be organizational: separate human, separate machine, separate seed, no shared toolchain, else Thompson colludes across gate.
- Single owner needs crash-only succession: TPM-sealed intent escrow, not co-owners; single image dies with one bus factor otherwise.
- No-fork discipline: forbid long-lived forks/branches; only ephemeral EPT-clone lanes that die on merge/revert.
- Org mirrors hardware: pinned-owner-per-lane like pinned-task-per-core, no manager-scheduler; revert-is-cheap culture from COW snapshots.
- Swarm thermally batched: joule-thermal-headroom limits parallel agent lanes, not headcount.

## Completed sublist v3
- single-image-owner: one human holds whole-image intent, only defense against compiler-betrayal collusion.
- independent-oracle-gate: no merge without verdict from separate human+machine+seed; references oracle verdicts only.
- adversarial-review-default: every lane needs dissenter sign-off; policy only, bots live in observer.
- public-append-only-log: every intent and merge published, never rewritten; tech lives in substrate/observer.
- sandbox-classroom-track: untrusted humans only in disposable VM-cell lanes with no privilege path.
- intent-diff-first: humans approve intent diffs before code exists to stretch human-review-bandwidth.
- escrowed-successor: owner intent sealed for succession so single ownership survives loss without splitting trust.
