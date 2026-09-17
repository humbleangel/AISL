# Swarm review v5 — inversion (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 direction-flip audit.

## Round-2 kills / revisions
- everything-below-language-is-guest KILLED as worded: SMM SMRAM-locked, ME separate microcontroller, microcode below VMX — cannot be EPT-contained. Replaced by adversary-model (measured, pinned, dissented).
- upcalls-only-no-downcalls KILLED as worded: SMI never VMEXITs/upcalls; NMI unreliable under theft. Scoped to trappable-crossings-only; untrappable = betrayal/fault.
- language-defines-traps PARTIAL: CPU keeps NMI/SMI vectors; language owns meaning of trappable faults only.
- metal-is-vocabulary PASSED WITH SCOPE: pinned endpoint micro-drivers remain; vocabulary = page-types + bounded sends, not zero code.
- speculation-only-by-invitation PASSED WITH SCOPE: silicon speculation unstoppable; default-fence compiler + gated invitation regions.
- New licensed insight: unflippable-down-is-witnessed-as-theft — where CPU wires direction, inversion flips from redirection to detection.

## Final v5
- language-defines-trappable-faults: CPU keeps NMI/SMI vectors; language owns meaning of all it can trap via EPT-sentry.
- errors-only-as-faults: no error returns; violation or hardware residue becomes fault/send, never value.
- below-language-is-pinned-adversary: SMM/ME/firmware never guests, only measured, pinned, dissented, treated as betrayal source.
- trappable-crossings-are-upcalls-only: every interceptable crossing is posted-send up; untrappable theft is stolen-time fault, never downcall.
- code-follows-data: execute by temporary EPT-alias at data, never by coherent sharing or driver copy.
- speculation-only-by-invitation: fenced by default; speculative region requires explicit language gate + PMU crosscheck.
- persistence-is-default-volatile-is-explicit: CoW-append durable by default; volatile/scratch only via explicit page-type.
