# Swarm review v3 — new-forgetting (additive, non-destructive)

v1 folders + v2 file kept. This file adds the cross-swarm v3. FORGETTING owns cultural diagnosis (what habit to unlearn); bans live in CONSTRAINT/SUBTRACTION, replacements elsewhere.

## Collisions resolved
- forget-posix-fd-text-model vs constraint/subtraction/pairing: full collision. Cede ban to CONSTRAINT/SUBTRACTION, replacement to PAIRING. Forgetting DROPS this entry.
- forget-files-as-truth vs subtraction/pairing/inversion/substrate: full collision. SUBTRACTION owns removal, SUBSTRATE/PAIRING own replacement. Forgetting DROPS this entry.
- forget-c-undefined-behavior vs inversion: partial. INVERSION owns mechanism (traps/faults); FORGETTING keeps cultural diagnosis (C permissiveness unlearned), narrowed.
- forget-ring0-monolith vs inversion/substrate/scarcity: INVERSION/SUBSTRATE own replacement, SCARCITY owns budget; FORGETTING keeps folklore critique (ring0+root omnipotence).
- forget-fork-daemons-cron vs subtraction/scale: fork/process ban to SUBTRACTION, scheduler replacement to SCALE; FORGETTING keeps background-time folklore (daemons/cron/wallclock).
- forget-outside-toolchain vs oracle/pairing/abundance/subtraction: SUBTRACTION owns binary ban, ORACLE/PAIRING own attested replacement; FORGETTING keeps cultural half (configure-make-install, external GCC, LD_PRELOAD).
- forget-userland-compat-requirement vs constraint/subtraction: adjacent not duplicate. They own specific bans; FORGETTING owns meta-license (the demand for compat itself). Kept.

## New insights (unlisted baggage)
- Ambient Unix authority: uid/gid/chmod/setuid/sudo/cwd/env/PATH/umask as integers, not MMU/EPT/TPM capabilities.
- errno + return-code errors: -1/errno/stringly errors vs metal truth (trap/fault/continuation).
- Wallclock-everywhere: gettimeofday/NTP/timeouts assumed free and true vs causal-tick, wall-as-range, deadlines-as-handlers.
- Energy-free + speculation-invisible: spin/poll/busy-loop blind to SMT/speculation/RAPL/thermal/cache residency.
- Boot/init + debug folklore: BIOS/GRUB/init/systemd-runlevels and ptrace/gdb/core-dump/printf/signals vs resume/snapshot, vm-cell demo, honest trace.
- Hierarchy + dotfile folklore beyond files: /, /etc, /usr, dotfiles, PATH lookup vs content-addressed pages/objects.
- Sockets-select + pthreads-share-everything as mental default vs lanes-per-core, run-to-completion, no-coherent-sharing.

## Completed sublist v3
- forget-ambient-unix-authority: unlearning uid/gid/setuid/chmod/sudo/cwd/env buys hardware-bound capabilities only.
- forget-c-permissiveness-and-errno: unlearning UB plus errno-codes buys trap-defined language where faults are control flow.
- forget-ring0-and-root-monolith: unlearning omnipotent ring0/root buys EPT-partitioned kernel-as-guest with privilege budget.
- forget-background-time-folklore: unlearning daemons/cron/timeouts/wallclock buys causal-tick with deadlines-as-handlers, no invisible preemption.
- forget-outside-unmeasured-toolchain: unlearning configure-make-install/external-GCC/dynamic-linking buys build-is-attestation from hand-seed.
- forget-energy-and-speculation-invisibility: unlearning spin/poll/free-execution buys power-as-capability under joule/thermal budget.
- forget-boot-and-debug-folklore: unlearning BIOS/init/systemd plus ptrace/gdb/signals buys resume-not-boot with vm-cell demonstration and honest trace.
- forget-compat-as-requirement: unlearning backward-compat/ABI-stability buys license for single-image perfection.
