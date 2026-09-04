# THEMES.md — Daily HPC Theme Registry

This file is the high-level bookkeeping index for the Daily HPC curriculum.

It defines:
- the five curriculum themes;
- their priority and purpose;
- the boundary between themes;
- the curriculum file associated with each theme;
- the default weekly theme schedule.

Detailed topic order belongs in the individual curriculum files.  
Daily operating rules belong in `AGENTS.md`.  
Completed coverage is tracked in `PROGRESS.md`.

---

# 1. Theme Overview

| Theme ID | Theme | Priority | Curriculum file |
|---|---|---:|---|
| `GPU` | GPU + CPU-GPU Architecture | P1 | `curriculum/theme-1-gpu-cpu-architecture.md` |
| `DIST` | Distributed / Multi-GPU HPC | P1 | `curriculum/theme-2-distributed-multigpu-hpc.md` |
| `HPL` | Dense Linear Algebra + HPL/HPL-MxP | P1 | `curriculum/theme-3-dense-linear-algebra-hpl-hpl-mxp.md` |
| `PERF` | Performance Engineering | P2 | `curriculum/theme-4-performance-engineering.md` |
| `SYS` | Broader HPC Systems | P3 | `curriculum/theme-5-broader-hpc-systems.md` |

Priority meaning:
- **P1** — Core focus for the current learning objective.
- **P2** — Essential supporting knowledge for analyzing and optimizing HPC workloads.
- **P3** — Breadth and wider systems awareness.

Priority does **not** change curriculum order inside a theme.

---

# 2. Theme 1 — GPU + CPU-GPU Architecture

**Theme ID:** `GPU`  
**Priority:** P1

## Purpose

Build a strong architectural mental model of modern GPUs, CPUs, and heterogeneous CPU-GPU systems.

The goal is to understand how hardware structure determines execution behavior, memory behavior, data movement, and theoretical performance.

## Main coverage

- processor architecture fundamentals;
- CPU versus GPU design goals;
- SIMT/SIMD and GPU execution;
- warps, wavefronts, SMs, CUs, and scheduling;
- floating-point and matrix/tensor execution units;
- GPU memory hierarchy;
- registers, shared memory, caches, and HBM;
- occupancy and hardware residency;
- CPU caches, NUMA, memory controllers, and PCIe roots;
- CPU-GPU data movement;
- unified memory and coherence;
- PCIe, NVLink, NVSwitch, xGMI, and scale-up fabrics;
- heterogeneous packaging, chiplets, HBM, and coherent CPU-GPU systems;
- reading accelerator specifications and estimating theoretical limits.

## Boundary

Use this theme primarily for **hardware architecture and local system structure**.

Move to:
- `DIST` for MPI, NCCL, NVSHMEM, RDMA, InfiniBand, collectives, and multi-node communication;
- `HPL` for dense linear algebra and HPL/HPL-MxP algorithms;
- `PERF` for profiling and optimization methodology.

---

# 3. Theme 2 — Distributed / Multi-GPU HPC

**Theme ID:** `DIST`  
**Priority:** P1

## Purpose

Build a first-principles understanding of communication across GPUs, processes, nodes, NICs, and network fabrics.

The goal is to understand the actual data paths, synchronization, communication algorithms, and topology effects behind distributed HPC performance.

## Main coverage

- distributed-memory fundamentals;
- processes, ranks, messages, latency, and bandwidth;
- point-to-point communication;
- MPI concepts and collectives;
- collective algorithms;
- communication performance models;
- GPU-aware MPI;
- host-staged communication;
- RDMA and GPUDirect RDMA;
- NICs, InfiniBand, RoCE, and HPC network fabrics;
- NCCL;
- NVSHMEM and one-sided communication;
- communication/computation overlap;
- communication progress;
- rank, CPU, GPU, NUMA, and NIC affinity;
- topology-aware communication;
- multi-node communication hierarchy;
- scientific communication patterns;
- HPL/HPL-MxP communication behavior;
- diagnosing fast paths, fallback paths, skew, and communication bottlenecks.

## Boundary

Use this theme primarily for **communication mechanisms and distributed execution**.

Move to:
- `GPU` for local GPU/CPU architecture;
- `HPL` for LU, matrix algorithms, numerical precision, and HPL/HPL-MxP mathematics;
- `PERF` for profiling and experiment design.

---

# 4. Theme 3 — Dense Linear Algebra + HPL/HPL-MxP

**Theme ID:** `HPL`  
**Priority:** P1

## Purpose

Build the mathematical and algorithmic foundation needed to understand and optimize HPL and HPL-MxP.

The goal is to connect dense linear algebra, floating-point arithmetic, low-precision computation, distributed LU, and algorithmic optimization to real HPC execution.

## Main coverage

- vectors, matrices, and linear systems;
- matrix multiplication;
- computational complexity and FLOP counts;
- floating-point arithmetic and precision;
- BLAS Levels 1, 2, and 3;
- GEMV, GEMM, TRSM, and related kernels;
- Gaussian elimination;
- LU factorization;
- pivoting and numerical stability;
- blocked LU;
- panel factorization and trailing matrix updates;
- data reuse and arithmetic intensity;
- distributed matrices;
- process grids and block-cyclic distribution;
- parallel LU;
- HPL algorithm structure and parameters;
- mixed precision;
- iterative refinement;
- HPL-MxP;
- low-precision kernels;
- communication-avoiding algorithms;
- matrix shapes and remainder effects;
- critical-path reasoning;
- numerical correctness and residuals;
- algorithm-to-hardware mapping;
- hypothesis-driven HPL/HPL-MxP optimization.

## Boundary

Use this theme primarily for **mathematics, numerical algorithms, and HPL/HPL-MxP algorithmic structure**.

Move to:
- `GPU` for accelerator hardware internals;
- `DIST` for communication libraries and network mechanisms;
- `PERF` for profiling, experiments, benchmarking methodology, and general performance diagnosis.

---

# 5. Theme 4 — Performance Engineering

**Theme ID:** `PERF`  
**Priority:** P2

## Purpose

Build a disciplined methodology for understanding why an HPC workload performs as it does and how to optimize it using evidence.

The goal is to replace blind tuning with measurement, modeling, hypothesis formation, controlled experiments, and validation.

## Main coverage

- performance metrics;
- wall time, throughput, latency, utilization, and efficiency;
- variability and reproducibility;
- theoretical and sustained limits;
- Roofline analysis;
- strong and weak scaling;
- Amdahl and Gustafson reasoning;
- CPU profiling;
- GPU profiling;
- profiler timelines;
- GPU kernel metrics;
- memory bottlenecks;
- launch overhead and kernel granularity;
- concurrency and overlap;
- CPU affinity and NUMA;
- GPU and NIC affinity;
- communication performance diagnosis;
- load imbalance and rank asymmetry;
- critical-path analysis;
- benchmarking methodology;
- parameter sweeps;
- controlled experiment design;
- performance-counter interpretation;
- HPL/HPL-MxP phase analysis;
- performance portability;
- end-to-end optimization workflow.

## Boundary

Use this theme primarily for **how to measure, diagnose, test, and optimize**.

The underlying mechanism being analyzed may belong to another theme:
- architecture mechanism → `GPU`;
- communication mechanism → `DIST`;
- numerical/algorithmic mechanism → `HPL`.

---

# 6. Theme 5 — Broader HPC Systems

**Theme ID:** `SYS`  
**Priority:** P3

## Purpose

Build broad systems awareness around the core architecture, communication, numerical, and optimization themes.

The goal is to understand complete HPC environments and keep up with wider developments in supercomputing research and industry.

## Main coverage

- cluster and supercomputer architecture;
- compute, login, and service nodes;
- schedulers and resource management;
- Slurm and PBS;
- Linux and HPC operating-system concepts;
- modules, software environments, and reproducibility;
- containers and Apptainer;
- storage and parallel filesystems;
- MPI-IO, HDF5, NetCDF, and ADIOS;
- compilers and optimization;
- HPC programming models;
- performance portability;
- representative scientific workloads;
- sparse linear algebra;
- CFD and OpenFOAM;
- FFT workloads;
- resilience and checkpointing;
- power and energy efficiency;
- cooling and physical infrastructure;
- monitoring and operations;
- HPL, HPCG, STREAM, TOP500, and Green500 ecosystem;
- AI/HPC convergence;
- emerging accelerators;
- CXL, chiplets, and memory-centric systems;
- scientific workflow systems;
- HPC security;
- research/conference landscape;
- reading HPC news and papers critically.

## Boundary

Use this theme for **broader systems context and awareness**.

When a topic becomes deeply architectural, communication-specific, numerical, or optimization-oriented, continue detailed treatment in the corresponding core theme.

---

# 7. Default Weekly Theme Schedule

The default schedule is defined in `AGENTS.md` and reproduced here only for bookkeeping.

| Day | Theme ID | Theme |
|---|---|---|
| Monday | `GPU` | GPU + CPU-GPU Architecture |
| Tuesday | `DIST` | Distributed / Multi-GPU HPC |
| Wednesday | `HPL` | Dense Linear Algebra + HPL/HPL-MxP |
| Thursday | `PERF` | Performance Engineering |
| Friday | `GPU` | GPU + CPU-GPU Architecture |
| Saturday | `DIST` | Distributed / Multi-GPU HPC |
| Sunday | `SYS` | Broader HPC Systems |

The schedule intentionally gives two days each to:
- `GPU`
- `DIST`

because accelerator architecture and distributed communication are especially important to the current learning objective.

`HPL` receives one dedicated day, while related HPL/HPL-MxP examples may also appear naturally in `GPU`, `DIST`, and `PERF`.

---

# 8. Cross-Theme Learning Model

The themes are separated for curriculum organization, but the underlying knowledge is connected.

```text
                 GPU
          hardware architecture
                /   \
               /     \
              v       v
           DIST      HPL
       communication  algorithms
              \       /
               \     /
                v   v
                 PERF
          measurement + optimization
                   |
                   v
                  SYS
         wider HPC system context
```

Examples of legitimate cross-theme connections:

- GPU memory hierarchy (`GPU`) → GEMM arithmetic intensity (`HPL`) → Roofline analysis (`PERF`);
- NVLink/NVSwitch (`GPU`) → NCCL collectives (`DIST`) → communication profiling (`PERF`);
- LU panel factorization (`HPL`) → panel broadcast (`DIST`) → rank skew diagnosis (`PERF`);
- supercomputer architecture (`SYS`) → node topology (`GPU`) → scale-out fabric (`DIST`).

Cross-theme references are encouraged.

However, only the topics explicitly selected from the scheduled curriculum should be marked as covered in `PROGRESS.md`.

---

# 9. Curriculum Progression Principle

Each theme should progress:

```text
foundations
    ↓
core mechanisms
    ↓
architecture / algorithm structure
    ↓
system behavior
    ↓
advanced techniques
    ↓
real systems / research / optimization
```

The project intentionally starts from foundational material even though the reader already has practical HPC experience.

The objective is not merely to recognize tools or terminology, but to build a durable mental model that supports future work in:

- GPU and accelerator architecture;
- distributed HPC;
- HPL/HPL-MxP optimization;
- numerical computing;
- scientific computing;
- performance engineering;
- HPC research.

---

# 10. Bookkeeping Rules

This file is an overview only.

Do not use `THEMES.md` to record daily completion.

Use:

```text
PROGRESS.md
```

for completion state.

Use:

```text
past-volumes/
```

for generated Daily HPC coverage files.

Use the individual curriculum files to determine exact topic IDs and ordering.

If the user changes:
- theme priorities;
- theme names;
- scope;
- weekly schedule;
- curriculum files;

update this file so it remains consistent with `AGENTS.md` and `curriculum/`.
