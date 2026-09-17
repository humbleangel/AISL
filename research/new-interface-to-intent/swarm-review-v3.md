# Swarm review v3 — new-interface-to-intent (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. INTERFACE owns gestures and draft syntax only; checking/enforcement/storage owned outward (Bun rule: proposer never grades itself).

## Collisions resolved
- budget-as-intent vs scarcity/substrate/oracle: DROPPED as owned concept; SCARCITY defines budgets, SUBSTRATE enforces, ORACLE proves. Intent references budgets by name only.
- commit-as-page-diff vs time/substrate/pairing: DROPPED storage claim; TIME+SUBSTRATE own version/diff mechanism. Intent keeps commit gesture if at all.
- demonstrate-in-vmx-cell vs domain/oracle/abundance: DOMAIN/SUBSTRATE own cell mechanism, ORACLE owns replay check. Intent owns demonstrate-gesture only.
- words-to-checked-contracts vs observer/oracle: violates judge independence. Intent proposes UNCHECKED draft only; OBSERVER/ORACLE check/veto. Renamed words-to-unchecked-contracts.
- point-at-live-pages vs substrate/inversion: SUBSTRATE/INVERSION own page mechanism; intent owns deictic pointing gesture.
- forbid-states-first vs constraint/failure: CONSTRAINT owns global forbiddens, FAILURE owns misintent taxonomy; intent owns per-task forbid-declaration syntax.
- veto-examples-in-isolation vs observer/oracle: OBSERVER/ORACLE own veto enforcement; intent owns rule that examples alone never commit.

## New insights
- Sealed proposal missing: seal intent BEFORE run so judge diffs proposal vs emission; without seal comparator is circular.
- Refusal missing: autocomplete guessing is the failure mode; interface needs refuse-ambiguous-intent fault.
- Ranges-not-points missing: metal gives ranges (wall, energy, thermal); intent verbs still take point values — take ranges + handler.
- Allocator/core naming missing: with caller-allocates + pinned tasks, intent naming no allocator/core is unexecutable.
- Review-sized intent missing: human-review-bandwidth demands intents capped in size/complexity.
- Undo-as-intent missing: v1 undo lost in v2; commit without declared undo window invites irreversible-misintent.

## Completed sublist v3
- point-at-live-pages: deictic reference to live MMU/EPT objects, not names/paths; mechanism owned by substrate.
- forbid-states-first: per-task forbidden states declared upfront; global forbiddens owned by constraint.
- demonstrate-not-describe: user shows behavior in a cell; cell itself owned by domain/substrate.
- words-to-unchecked-contracts: prose drafts a contract that only observer/oracle may check; preserves judge independence.
- veto-examples-alone: examples illustrate but never suffice for commit; veto enforced by observer/oracle.
- sealed-intent-before-run: hash-seal intent pre-execution so comparator judges proposal vs emission independently.
- refuse-ambiguous-intent: fault on under-specification instead of guessing; anti-autocomplete.
- undo-window-as-intent: every commit declares its undo/recovery affordance; storage owned by time.
