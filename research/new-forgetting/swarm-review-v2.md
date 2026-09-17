# Swarm review v2 — new-forgetting (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- forget-posix: correct enemy but bloated umbrella that swallows files, fork, daemons — split it or it means nothing.
- forget-files-as-truth: strongest entry, but still assumes NVMe blocks must look like bytes-in-a-tree — name the replacement.
- forget-undefined-behavior: too narrow; keeping C/LLVM while banning UB is fantasy — forget the whole C-as-metal bargain.
- forget-backward-compat: naive on x86-64 without fab — ISA/firmware/NVMe reality is mandatory, only userland ABI is disposable.
- forget-daemons-cron-folklore: symptom-level; kill fork/time-sharing/processes or you will reinvent cron in new syntax.
- forget-configure-make-install: shallow tool gripe; real baggage is OS-vs-language-vs-toolchain split, not autotools.

## Completed sublist v2
- forget-posix-fd-text-model: buys one typed invocation path, no syscalls + fds + stdout-text parsing at the intent-metal boundary.
- forget-files-as-truth: buys NVMe as persistent typed-object heap, no re-parsing hierarchical byte streams to know state.
- forget-c-undefined-behavior: buys one defined language from intent to paging/AVX with no nasal demons or optimizer lotteries.
- forget-ring0-monolith: buys protection from language types + MMU/VT-x compartments instead of an all-powerful ring-0 kernel gate.
- forget-fork-daemons-cron: buys core-affine intents that sleep truly dark for power/thermal, no polling processes, signals, or sched folklore.
- forget-outside-toolchain: buys compiler/linker as in-system verified service (TPM-measured), killing trusting-trust and configure-make-install.
- forget-userland-compat-requirement: buys freedom to drop Linux ABI and PC-boot folklore while keeping only x86-64 ISA/driver reality.
