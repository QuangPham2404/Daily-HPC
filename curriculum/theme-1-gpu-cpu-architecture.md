# Theme 1 — GPU + CPU-GPU Architecture

**Theme ID:** `GPU`  
**Priority:** P1  
**Purpose:** Build a transferable mental model of modern GPU architecture and heterogeneous CPU-GPU systems, from first principles to current HPC accelerator nodes.

## Scope

This curriculum focuses on **hardware structure and architectural behavior**:

- why CPUs and GPUs are architected differently;
- how GPU work is created, scheduled, and executed;
- compute pipelines and specialized matrix hardware;
- GPU and CPU memory hierarchies;
- CPU↔GPU data movement and coherence;
- PCIe and scale-up GPU interconnects;
- multi-GPU node topology;
- heterogeneous packaging, chiplets, HBM, and coherent CPU-GPU systems;
- how to read an architecture/specification sheet and reason about theoretical hardware capability.

Detailed distributed programming and scale-out communication (`MPI`, `NCCL`, `NVSHMEM`, RDMA, InfiniBand, multi-node collectives) belong primarily to **Theme 2 — Distributed / Multi-GPU HPC**.

Detailed profiling and performance-tuning methodology belong primarily to **Theme 4 — Performance Engineering**.

## Progression Rule

Follow the curriculum in order unless there is a strong reason to revisit an earlier topic.

A daily volume should normally combine **2–4 adjacent, tightly related topics** into one coherent lesson. Dense topics may occupy a full volume by themselves.

---

# 1. Processor Architecture Foundations

### GPU-1.1 — What a processor actually does
Instructions, data, execution units, registers, memory, and control.

### GPU-1.2 — CPU vs GPU design goals
Latency-oriented CPUs versus throughput-oriented GPUs; why each architecture spends transistor area differently.

### GPU-1.3 — Forms of parallelism
Instruction-level parallelism (ILP), data-level parallelism (DLP), thread-level parallelism (TLP), and task-level parallelism.

### GPU-1.4 — Scalar, vector, SIMD, SIMT, and SPMD
How these execution models differ and how GPU programming maps onto the hardware.

### GPU-1.5 — Latency versus throughput
Why a device can have high operation latency yet achieve enormous aggregate throughput.

### GPU-1.6 — Basic hardware performance quantities
Clock frequency, operations per cycle, execution width, FLOP/s, bandwidth, latency, and utilization.

---

# 2. GPU Execution Model

### GPU-2.1 — Host, device, and kernels
How CPU code launches work onto an accelerator.

### GPU-2.2 — Threads and hierarchical work decomposition
Threads, thread blocks/workgroups, grids, and their hardware-independent meaning.

### GPU-2.3 — Warps, wavefronts, and subgroups
NVIDIA warps, AMD wavefronts, Intel subgroups, and why GPUs execute groups of threads together.

### GPU-2.4 — The GPU compute building block
Streaming Multiprocessors (SMs), Compute Units (CUs), Xe-cores, and the common architectural ideas behind them.

### GPU-2.5 — Warp/wavefront scheduling
Ready warps, issue logic, scoreboarding, stalls, and how GPUs choose useful work every cycle.

### GPU-2.6 — Latency hiding
Why GPUs keep many threads resident and switch among them instead of relying mainly on large out-of-order cores.

### GPU-2.7 — Branch divergence and predication
What happens when threads in the same execution group take different control-flow paths.

### GPU-2.8 — Synchronization within the GPU
Barriers, execution scope, ordering, and why synchronization has architectural cost.

---

# 3. GPU Compute Datapath

### GPU-3.1 — Arithmetic pipelines
Integer, floating-point, load/store, special-function, and control pipelines.

### GPU-3.2 — Floating-point execution
FP64, FP32, TF32-like formats, FP16, BF16, FP8-class formats, and why different precisions have different throughput.

### GPU-3.3 — Fused multiply-add (FMA)
Why FMA is central to scientific and matrix computation and how FLOP counts are derived.

### GPU-3.4 — Vector/SIMD execution inside a GPU core
How execution lanes process a warp/wavefront instruction.

### GPU-3.5 — Matrix and tensor execution units
Tensor Cores, AMD Matrix Cores, Intel XMX-class hardware, and why matrix engines differ from conventional FP pipelines.

### GPU-3.6 — Matrix instruction dataflow
Tiles, operands, accumulators, and the basic hardware structure behind matrix-multiply acceleration.

### GPU-3.7 — Precision versus throughput versus accuracy
Why low-precision hardware exists and what architectural trade-offs enable its high throughput.

### GPU-3.8 — Deriving theoretical peak compute
Compute peak FLOP/s from clocks, unit counts, operations per instruction, and precision.

---

# 4. GPU Memory Hierarchy

### GPU-4.1 — Why GPUs need a hierarchy
The latency/bandwidth/capacity trade-off and why no single memory technology can satisfy all three.

### GPU-4.2 — Registers
Per-thread state, register files, capacity, bandwidth, and why register demand affects hardware residency.

### GPU-4.3 — Shared memory / LDS / local memory
Programmer-managed on-chip memory and its role in data reuse and cooperation.

### GPU-4.4 — L1 cache
Local caching close to execution units and its interaction with load/store traffic.

### GPU-4.5 — L2 cache
The chip-wide cache, coherence point, and its role between compute units and external memory.

### GPU-4.6 — HBM and device memory
High Bandwidth Memory organization, stacks, channels, controllers, capacity, and bandwidth.

### GPU-4.7 — Memory transactions and coalescing
How individual thread accesses become hardware memory transactions.

### GPU-4.8 — Shared-memory bank conflicts
Banks, simultaneous accesses, serialization, and conflict-free layouts.

### GPU-4.9 — Cache locality and access patterns
Spatial locality, temporal locality, reuse, stride, and why layout changes hardware behavior.

### GPU-4.10 — Memory latency versus memory bandwidth
Why these are different limits and how each affects a GPU workload.

### GPU-4.11 — Atomics and memory ordering
Hardware support for concurrent updates, contention, scopes, and consistency.

### GPU-4.12 — Deriving theoretical memory bandwidth
Use memory rate, bus/channel width, stacks, and controllers to reason about peak bandwidth.

---

# 5. Resource Residency and GPU Scheduling

### GPU-5.1 — What can reside on an SM/CU
Threads, warps/wavefronts, blocks/workgroups, registers, and shared memory.

### GPU-5.2 — Occupancy
What occupancy actually measures and why maximum occupancy is not the same as maximum performance.

### GPU-5.3 — Register pressure
How compiler register allocation can reduce resident work or cause spilling.

### GPU-5.4 — Shared-memory capacity constraints
How per-block shared-memory use limits simultaneous blocks.

### GPU-5.5 — Block/workgroup scheduling
How work is assigned across compute units and why blocks are designed to execute independently.

### GPU-5.6 — Concurrent kernels
When multiple kernels can share GPU resources and what hardware resources constrain concurrency.

### GPU-5.7 — Hardware queues and asynchronous engines
Compute queues, copy/DMA engines, and the architectural basis for overlapping operations.

---

# 6. CPU Architecture for Heterogeneous HPC

### GPU-6.1 — CPU core architecture in contrast to a GPU
Out-of-order execution, branch prediction, speculative execution, caches, and low-latency optimization.

### GPU-6.2 — CPU cache hierarchy
L1/L2/L3 caches, cache lines, locality, and coherence.

### GPU-6.3 — CPU memory controllers and DRAM
How CPU cores reach system memory and where bandwidth comes from.

### GPU-6.4 — Sockets and NUMA
Why memory access cost depends on which CPU socket owns the memory.

### GPU-6.5 — PCIe root complexes and system topology
How CPUs, GPUs, NICs, and other devices attach to a server.

### GPU-6.6 — CPU cores, NUMA nodes, GPUs, and NIC locality
Why physical placement matters in an HPC node.

### GPU-6.7 — Host memory types
Pageable memory, pinned/page-locked memory, and why DMA changes the transfer path.

### GPU-6.8 — Interrupts, DMA, and device-driven data movement
How accelerators move data without the CPU copying every byte itself.

---

# 7. CPU↔GPU Memory and Data Movement

### GPU-7.1 — Discrete CPU-GPU memory model
Separate host RAM and GPU VRAM/HBM and the consequences of separate address spaces.

### GPU-7.2 — PCI Express
Lanes, generations, switches, bandwidth, latency, and why PCIe can become a bottleneck.

### GPU-7.3 — Host-to-device and device-to-host transfers
The physical path data follows across a discrete accelerator system.

### GPU-7.4 — Pinned versus pageable transfers
Why pinning host memory changes DMA behavior and achievable transfer performance.

### GPU-7.5 — Unified Virtual Addressing
One virtual address model across host and devices versus physically unified memory.

### GPU-7.6 — Managed / Unified Memory
Page migration, demand paging, placement, and the difference between convenience and physical data location.

### GPU-7.7 — Zero-copy and mapped host memory
When a GPU accesses host memory directly and why avoiding a copy is not automatically faster.

### GPU-7.8 — Memory coherence between CPUs and accelerators
Coherent versus non-coherent systems, ownership, visibility, and synchronization.

### GPU-7.9 — IOMMU, address translation, and device memory access
How devices translate and access memory safely in a modern system.

---

# 8. Multi-GPU Scale-Up Architecture

### GPU-8.1 — Why multiple GPUs need dedicated interconnects
When PCIe is insufficient for tightly coupled accelerator workloads.

### GPU-8.2 — GPU peer-to-peer memory access
One GPU accessing another GPU's memory without staging data through host RAM.

### GPU-8.3 — NVIDIA NVLink
Point-to-point GPU connectivity, link aggregation, generations, and architectural purpose.

### GPU-8.4 — NVSwitch and switched GPU fabrics
How a switch changes topology and enables dense all-to-all scale-up connectivity.

### GPU-8.5 — AMD Infinity Fabric / xGMI-class connectivity
AMD's approach to GPU-GPU and heterogeneous scale-up communication.

### GPU-8.6 — Other accelerator scale-up fabrics
Intel Xe Link-class systems and the general architectural ideas shared across vendors.

### GPU-8.7 — Interconnect topology
Direct links, rings, meshes, fully connected fabrics, switched fabrics, hop count, and path contention.

### GPU-8.8 — Bisection bandwidth and oversubscription
How topology limits aggregate traffic even when individual links are fast.

### GPU-8.9 — Reading a multi-GPU topology diagram
Reason from GPUs, CPUs, NICs, PCIe switches, NUMA nodes, and high-speed GPU links.

### GPU-8.10 — Scale-up versus scale-out
The architectural boundary between communication inside a node/system and communication across nodes.

---

# 9. Packaging and Heterogeneous System Architecture

### GPU-9.1 — Monolithic dies versus chiplets and tiles
Why modern processors are increasingly composed from multiple dies.

### GPU-9.2 — Advanced packaging
Interposers, bridges, 2.5D/3D packaging, die-to-die links, and their role in HPC accelerators.

### GPU-9.3 — HBM packaging
Why HBM is placed close to compute dies and how this enables extreme bandwidth.

### GPU-9.4 — Discrete accelerators versus integrated CPU-GPU systems
Separate devices compared with tightly integrated heterogeneous processors.

### GPU-9.5 — Coherent CPU-GPU systems
Architectures where CPUs and GPUs share a more tightly coupled memory/coherence model.

### GPU-9.6 — Memory pooling and coherent expansion
The architectural motivation behind CXL-class memory and coherent device interconnects.

### GPU-9.7 — Power delivery and thermal limits
TDP, power envelopes, cooling, frequency behavior, and why peak capability depends on system design.

### GPU-9.8 — System balance
Matching compute throughput, memory bandwidth, interconnect bandwidth, CPU capability, and I/O.

---

# 10. Modern GPU Architectural Features

### GPU-10.1 — Asynchronous data movement
Hardware mechanisms that move data while computation proceeds.

### GPU-10.2 — Hardware-assisted tensor/tile movement
Modern mechanisms for feeding matrix engines efficiently without excessive instruction overhead.

### GPU-10.3 — Thread-block/workgroup clustering
Architectural support for cooperation beyond a single traditional thread block.

### GPU-10.4 — Distributed/shared on-chip memory mechanisms
How newer GPUs expand cooperation and data sharing across nearby compute blocks.

### GPU-10.5 — Hardware synchronization primitives
Barriers, asynchronous transactions, producer-consumer synchronization, and execution pipelines.

### GPU-10.6 — GPU partitioning and isolation
MIG-class partitioning, hardware resource isolation, and implications for HPC sharing.

### GPU-10.7 — Reliability features for HPC
ECC, fault detection, error reporting, and why large accelerator systems require stronger reliability mechanisms.

---

# 11. Architecture Families and Evolution

This module should emphasize **why architectural features evolved**, not memorization of product specifications.

### GPU-11.1 — NVIDIA GPU architecture evolution
Trace major changes in compute, memory, matrix hardware, and scale-up connectivity across generations.

### GPU-11.2 — AMD CDNA architecture evolution
Compute Units, Matrix Cores, chiplets, HBM, Infinity Fabric, and heterogeneous integration.

### GPU-11.3 — Intel HPC GPU architecture
Xe-HPC/Xe-class execution, vector/matrix engines, tiles, HBM, and scale-up connectivity.

### GPU-11.4 — Cross-vendor terminology mapping
SM ↔ CU ↔ Xe-core; warp ↔ wavefront ↔ subgroup; shared memory ↔ LDS/SLM; tensor/matrix engines.

### GPU-11.5 — Current-generation accelerator comparison
Compare current HPC accelerators by architectural principles rather than marketing numbers.

### GPU-11.6 — HPC node architecture case studies
Study representative NVIDIA-, AMD-, and Intel-based accelerator nodes and identify their CPU, GPU, memory, NIC, and interconnect topology.

---

# 12. Reading Hardware Like an HPC Engineer

### GPU-12.1 — Reading a GPU specification sheet
Distinguish capacity, bandwidth, compute throughput, link bandwidth, clock, and architectural limits.

### GPU-12.2 — Calculate theoretical peak FLOP/s
Derive peak performance for several precisions from architectural information.

### GPU-12.3 — Calculate theoretical memory bandwidth
Relate HBM organization and transfer rate to aggregate bandwidth.

### GPU-12.4 — Compute-to-memory balance
Compare peak FLOP/s with memory bandwidth and form an initial expectation of workload behavior.

### GPU-12.5 — Link and topology bandwidth calculations
Aggregate link bandwidth, directional bandwidth, hop count, and topology constraints.

### GPU-12.6 — Reconstruct a server topology
Use hardware diagrams and system information to determine CPU↔GPU↔NIC relationships.

### GPU-12.7 — From specification to hypothesis
Given a workload and an architecture, predict likely compute, memory, transfer, or topology bottlenecks before profiling.

---

# Completion Goal

After completing this theme, the reader should be able to:

1. Explain **why GPU architecture differs from CPU architecture** and what workloads benefit from each.
2. Trace a GPU kernel from **thread hierarchy → scheduling → execution pipelines → memory accesses**.
3. Explain the major levels of **GPU and CPU memory hierarchy** and their latency/bandwidth trade-offs.
4. Reason about **register use, shared memory, residency, occupancy, and latency hiding** from hardware structure.
5. Explain how data physically travels between **CPU RAM, GPU HBM, peer GPUs, and scale-up interconnects**.
6. Read a heterogeneous server topology and identify important **NUMA, PCIe, GPU, and NIC relationships**.
7. Compare NVIDIA, AMD, and Intel accelerator architectures using **common architectural concepts rather than vendor terminology**.
8. Derive basic theoretical limits for **compute throughput, memory bandwidth, and interconnect bandwidth**.
9. Read a modern GPU architecture announcement, whitepaper, or technical article and understand **what changed, why it matters, and which workloads are likely to benefit**.
10. Use architecture knowledge as the foundation for later study of **distributed communication, HPL/HPL-MxP, numerical kernels, and performance engineering**.
