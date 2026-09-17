# Swarm review v2 — new-failure (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- compiler-betrayal: right instinct, too narrow — stops at compiler, ignores microcode/firmware/SMM/ME/VT-x underneath that can betray identically.
- silent-corruption: junk-drawer bucket — conflates RAM flip, DMA smash, NVMe rot which need different check points, still assumes linear memory just works.
- power-loss-mid-write: correct but Unix-brained — treats crash-consistency as fsync afterthought, not language primitive.
- side-channel-leak: dangerously understated — on SMT + shared cache + speculation, leakage is default hardware behavior, not edge case.
- resource-exhaustion: vague and carries Unix OOM-killer lottery assumption — undetectable-by-design until something random dies.
- human-misintent: vaguest, unfalsifiable — without reified intent there is nothing to detect, diff, or undo.

## Completed sublist v2
- compiler-betrayal: design for diverse double-compilation from tiny hand-audited seed first, so no binary toolchain is ever trusted blindly.
- firmware-microcode-betrayal: measure/pin microcode+firmware+TPM first, so OS+language treats everything below ring-0 as hostile input, not ground truth.
- silent-corruption: checksum/capability-guard every RAM-to-NVMe boundary first, so bit-flip/DMA/rot is detected at use, not trusted via flat address space.
- power-loss-mid-write: make atomic persistent commit a language verb first, so torn NVMe writes are impossible to express, not patched with fsync.
- side-channel-leak: make timing/cache/speculation isolation the default domain first, so sharing few cores never leaks by accident.
- resource-exhaustion: make memory/cores/cache quota explicit in language first, so exhaustion fails loudly at allocation, never via surprise killer.
- irreversible-misintent: reify intent as inspectable reversible object first, so wrong human command is preview/undo, not raw metal mutation.
- degraded-not-dead: design for AVX-downclock/thermal-throttle first, so power limits degrade performance predictably instead of breaking timing assumptions.
