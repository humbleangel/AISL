# Swarm review v2 — new-scale (additive, non-destructive)

Original v1 sublist kept as folders. This file adds the swarm audit.

## Judgment of v1
- single-task-image: shallow — says "one" but keeps process/image split, doesn't force language==kernel in ring-0.
- few-tasks-few-cores: not an extreme, just SMP Linux status quo; carries scheduler+per-process-paging assumption.
- many-tiny-tasks: real stress but framed as threads, still assumes stacks+TLB per task instead of lanes/continuations.
- whole-machine-clone: vague — EPT remap vs copy vs NVMe snapshot untestable without bandwidth/latency bound.
- minimal-image-size: byte-fetish, still assumes von Neumann linear image + UEFI cruft instead of what metal mandates.
- maximal-trace-scale: untestable God's-eye log, ignores SMT/speculation observer effect and where trace is stored.

## Completed sublist v2
- single-task-no-scheduler: one task owns all paging/rings, teaches whether language runtime can BE the OS.
- pinned-task-per-core: N tasks pinned, no migration, teaches shared-cache/TLB-shootdown cost as language placement.
- million-lanes-not-tasks: 1M continuations on few strong cores, teaches stacks/processes die, AVX lanes + single address space win.
- ept-whole-machine-clone: fork full machine via VT-x EPT + NVMe snapshot, teaches persistence vs RAM is fake.
- minimal-measured-boot: smallest TPM-measured 64-bit image that enables paging, teaches what metal actually requires.
- maximal-honest-trace: billion-event causal trace under SMT/speculation, teaches observability ceiling and lying tracers.
- all-cores-avx-thermal-limit: all cores AVX-turbo vs halted, teaches power/thermal as first-class language scale limit.
