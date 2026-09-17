# Swarm review v2 — new-domain (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- local-dev-machine: worst beachhead — demands GPUs/WiFi/browsers/IDEs/POSIX, fights 80-year ecosystem head-on and loses.
- build-farm-worker: shallow — "Linux but faster boot", still assumes Unix processes/toolchain, names no language win.
- edge-appliance: vague to meaninglessness — names no device, no metal advantage, just "appliance" wish.
- ai-agent-sandbox: strongest of the six — disposable hostile code needs capabilities not Unix users, but underspecified.
- recovery-rescue-image: good instinct — owns bare metal, minimal drivers, boot-time dominates — but niche, doesn't prove language.
- classroom-bootstrap: bad first — teaching needs stability/docs, experimental fragility inverts Thompson trust moral.

## Completed sublist v2
- ai-agent-sandbox: per-task VT-x + MMU capability jail spawns in ms and dies clean, no container/Linux CVE surface.
- disposable-ci-worker: single NVMe image boots straight to language runtime, PR code runs cap-confined with no persistent state to poison.
- recovery-rescue-image: owns rings+NVMe+TPM directly, fast-boot measured image repairs untrusted disks without trusting them.
- tpm-sealed-inference-node: single-image + TPM measured boot + AVX/core-pinning serves vectors with no scheduler/syscall noise or Linux supply chain.
- offline-signing-vault: capability-only I/O + TPM seal proves private keys never left 1 machine, airgap enforced by no network stack existing.
- vt-x-fuzz-lab: AISL as thin hypervisor harness reboots crashed guests in ms and partitions shared cache/SMT to kill speculation leaks.
- trust-bootstrap-rig: one fixed x86-64 box proves hand-bytes -> tiny assembler -> self-host from nothing, Thompson moral as demo.
