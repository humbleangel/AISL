# Swarm review v5 — new-failure (additive, non-destructive)

v1 folders + v2/v3/v4 files kept. Two-round protocol with round-2 physical-reality audit + MECE-by-detector check.

## Round-2 verdicts
- compiler-betrayal REAL (Thompson 1984, reproducible). firmware-microcode-betrayal REAL (SMM rootkits, ME, semantic microcode change). silent-corruption REAL (rowhammer, bit-flip, FDIV/JCC errata, retention). commit-torn-loss REAL (power mid-write, torn 4K, CoW race). device-flush-lie REAL (volatile-DRAM ACK, FTL map loss). side-channel-leak REAL (Spectre/Meltdown, Prime+Probe, RAPL, PMU, AVX ports). SMM steal / TSC desync REAL (10s–100s us invisible).
- SMT sibling sabotage REAL as attacker, KILLED as standalone mode (manifests via side-channel + stolen-time + exhaustion).
- irreversible-misintent KILLED as failure mode (FOLKLORE as physics — no metal trigger; interface property once sent).
- commit vs flush-lie boundary CONFIRMED: torn-loss = language/OS atomicity fails with honest device; flush-lie = device ACKs durability never provided. Separated by test (honest-RAM cut vs lying-FTL sim).

## Final v5
- compiler-betrayal: binary lies about source; only diverse replay catches it.
- firmware-microcode-betrayal: SMM/ME/microcode executes unmeasured code below language.
- silent-corruption: fab errata + bit-flip + FTL remap return wrong data with no fault raised.
- commit-torn-loss: crash leaves half-persisted snapshot despite honest device.
- device-flush-lie: NVMe ACKs flush it did not persist in FTL.
- side-channel-leak: speculation/cache/RAPL/PMU/AVX leaks across domains.
- resource-exhaustion: allocation/lane/joule/thermal budget denied or depleted.
- stolen-time-skew: SMM/ME/NMI/MCE steal run-to-completion with no fault; TSC desync plus thermal stretch break causal tick and deadlines.
