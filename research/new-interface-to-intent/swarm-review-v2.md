# Swarm review v2 — new-interface-to-intent (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- constrained-prompt: teletype English with a soft filter, uncheckable unless a non-LLM checker rejects it.
- examples-as-spec: classic PBE, radically underdetermined, AI overfits and grades its own coverage.
- sketch-then-tighten: vague two-phase waterfall, still assumes file/edit/compile loop, no judge for "tight enough."
- state-you-can-point-at: best of the six but still symbolic debugger worship, points at names not pages/rings/registers.
- words-to-contracts: right instinct, fatal if same model writes words and contracts, green means self-consistent not correct.
- undoable-commands: not an intent interface at all, Unix shell-history safety property smuggled into the list.

## Completed sublist v2
- point-at-live-pages: intent names real pages/registers/rings via MMU+debug regs, verifier re-reads same hardware loci, no symbol drift.
- forbid-states-first: human states what must never happen as page-permission/ring invariants, CPU fault is the independent judge.
- demonstrate-in-vmx-cell: sketch replaced by live run in VT-x/EPT cell with Intel PT trace, replayable evidence instead of prose.
- words-to-checked-contracts: words only enter if they compile to a separate machine-checked predicate the proposer cannot edit.
- veto-examples-in-isolation: examples are kill-rules batch-run in an isolated cell via SIMD, any fail vetoes the candidate.
- budget-as-intent: cycles/cache-misses/energy from perf counters are part of the spec, hardware measures pass/fail.
- commit-as-page-diff: every intent lands as NVMe CoW snapshot producing an inspectable page-diff, TPM-attested and abortable by fault.
