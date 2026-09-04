# Theme 2 — Distributed / Multi-GPU HPC

**Theme ID:** `DIST`  
**Priority:** P1  
**Purpose:** Build a first-principles understanding of communication in multi-GPU and multi-node HPC systems, from basic message-passing concepts to modern GPU-aware communication stacks used by distributed scientific workloads.

## Scope

This curriculum focuses on:

- why distributed-memory communication is necessary;
- point-to-point communication and synchronization;
- MPI concepts and collectives;
- GPU-aware MPI and CUDA-aware communication;
- RDMA and GPUDirect RDMA;
- NCCL and GPU collectives;
- NVSHMEM and one-sided GPU communication;
- network fabrics, NICs, switches, and topology;
- scale-up versus scale-out communication;
- communication/computation overlap;
- rank placement, locality, and communication paths;
- how these ideas appear in HPL, HPL-MxP, HPCG, and other distributed workloads.

Detailed GPU execution and local GPU architecture belong primarily to **Theme 1 — GPU + CPU-GPU Architecture**.

Detailed HPL/LU algorithm structure belongs primarily to **Theme 3 — Dense Linear Algebra + HPL/HPL-MxP**.

Detailed profiling and tuning methodology belong primarily to **Theme 4 — Performance Engineering**.

## Progression Rule

Follow the curriculum in order unless there is a strong reason to revisit an earlier topic.

A daily volume should normally combine **2–4 adjacent, tightly related topics** into one coherent lesson. Dense topics may occupy a full volume by themselves.

---

# 1. Distributed Computing Foundations

### DIST-1.1 — Why one processor is not enough
Problem size, memory capacity, throughput, and the need to distribute work.

### DIST-1.2 — Shared-memory versus distributed-memory systems
One address space versus separate address spaces and why communication becomes explicit.

### DIST-1.3 — Processes, ranks, threads, and devices
The basic execution entities used in distributed HPC systems.

### DIST-1.4 — Data partitioning
How large arrays, matrices, and meshes are divided across processes and accelerators.

### DIST-1.5 — Communication versus computation
Why distributed performance depends on both useful work and data movement.

### DIST-1.6 — Latency and bandwidth
Startup cost versus sustained transfer rate.

### DIST-1.7 — Message size and communication regimes
Why small messages are usually latency-sensitive and large messages bandwidth-sensitive.

### DIST-1.8 — Synchronization
Why distributed workers must sometimes wait and how synchronization creates exposed idle time.

---

# 2. Point-to-Point Communication

### DIST-2.1 — Send and receive
The simplest distributed communication model.

### DIST-2.2 — Blocking communication
What it means for a process to wait for an operation to complete.

### DIST-2.3 — Non-blocking communication
Initiation versus completion and how communication can proceed concurrently with computation.

### DIST-2.4 — Matching messages
Source, destination, tag, communicator, and why correct matching matters.

### DIST-2.5 — Eager versus rendezvous protocols
Why communication libraries use different transfer protocols depending on message size.

### DIST-2.6 — Buffering and copying
Application buffers, internal transport buffers, and where extra copies can occur.

### DIST-2.7 — Progress
Who actually advances a non-blocking communication operation and why progress behavior matters.

### DIST-2.8 — Deadlock
How communication dependencies can stall a distributed program.

---

# 3. MPI Foundations

### DIST-3.1 — What MPI is
The message-passing programming model, standard, and implementation ecosystem.

### DIST-3.2 — MPI communicators
Groups of processes and isolated communication contexts.

### DIST-3.3 — MPI ranks
Logical process identifiers versus physical hardware placement.

### DIST-3.4 — MPI datatypes
How MPI describes structured memory regions.

### DIST-3.5 — MPI point-to-point operations
Send, receive, synchronous variants, and non-blocking variants.

### DIST-3.6 — MPI completion semantics
Requests, waits, tests, and what “complete” actually means.

### DIST-3.7 — MPI process placement
How ranks map to sockets, NUMA nodes, GPUs, and NICs.

### DIST-3.8 — MPI implementations
Open MPI, MPICH, vendor MPI stacks, and why implementations differ while sharing the MPI API.

---

# 4. Collective Communication

### DIST-4.1 — What a collective is
Communication involving a group of ranks rather than a single pair.

### DIST-4.2 — Broadcast
One-to-many data distribution.

### DIST-4.3 — Reduce
Many-to-one aggregation using an associative operation.

### DIST-4.4 — All-reduce
Global aggregation with the result delivered to all participants.

### DIST-4.5 — Scatter and gather
Distributing and collecting distinct data segments.

### DIST-4.6 — All-gather
Collecting distributed segments so every rank receives the full result.

### DIST-4.7 — Reduce-scatter
Combining reduction and distribution.

### DIST-4.8 — All-to-all
Every rank communicating distinct data to every other rank.

### DIST-4.9 — Barrier
Explicit synchronization and why barriers often expose load imbalance.

### DIST-4.10 — Collective algorithms
Linear, tree, binomial-tree, ring, recursive-doubling, and hierarchical strategies.

### DIST-4.11 — Collective cost models
How latency, bandwidth, message size, and number of ranks determine cost.

---

# 5. Communication Performance Models

### DIST-5.1 — The alpha-beta model
Model communication time as startup latency plus bytes divided by bandwidth.

### DIST-5.2 — Serialization time
Why even an ideal link needs finite time to transmit a message.

### DIST-5.3 — Software overhead
Runtime, protocol, memory-copy, and synchronization costs beyond raw link performance.

### DIST-5.4 — Effective bandwidth versus link bandwidth
Why measured application bandwidth is lower than advertised physical bandwidth.

### DIST-5.5 — Injection bandwidth
How fast an endpoint can inject traffic into the communication fabric.

### DIST-5.6 — Contention
Multiple flows competing for links, switches, NIC resources, or memory bandwidth.

### DIST-5.7 — Hop count
Why path length matters in switched fabrics and scale-up topologies.

### DIST-5.8 — Bisection bandwidth
How topology limits aggregate communication even when individual links are fast.

### DIST-5.9 — Strong and weak scaling communication effects
Why communication becomes more dominant as work is divided across more ranks.

---

# 6. GPU-Aware Communication Foundations

### DIST-6.1 — The traditional host-staging path
GPU memory → host memory → network → host memory → GPU memory.

### DIST-6.2 — Why host staging is expensive
Extra copies, PCIe traffic, synchronization, and CPU involvement.

### DIST-6.3 — GPU-aware communication
The idea that communication libraries can recognize GPU-resident buffers directly.

### DIST-6.4 — CUDA-aware MPI and equivalent models
Passing GPU pointers directly to MPI and what the library must do internally.

### DIST-6.5 — GPU memory registration
Why a NIC or RDMA stack must map and pin memory before direct access.

### DIST-6.6 — Registration caches
Why repeated memory registration can be expensive and how communication stacks mitigate it.

### DIST-6.7 — IPC paths inside a node
How processes on the same node may communicate through GPU peer access or shared transport mechanisms.

### DIST-6.8 — Choosing communication paths
How libraries select shared memory, peer-to-peer, RDMA, TCP, or other transports depending on topology and capability.

---

# 7. RDMA and GPUDirect RDMA

### DIST-7.1 — Remote Direct Memory Access
How one endpoint accesses remote memory without CPU-mediated copying of every byte.

### DIST-7.2 — RDMA verbs and queue pairs
The basic NIC work-submission model behind many high-performance transports.

### DIST-7.3 — Memory registration in RDMA
Pinned memory, memory keys, protection, and why registered regions are needed.

### DIST-7.4 — DMA versus RDMA
Local device-driven memory transfer versus remote device-to-memory transfer.

### DIST-7.5 — GPUDirect RDMA
Direct NIC access to GPU memory and the communication path it removes.

### DIST-7.6 — GPU↔NIC topology
Why PCIe root complex placement and locality can affect GPUDirect performance.

### DIST-7.7 — Peer memory and kernel/driver support
The role of drivers and memory-access mechanisms in enabling NIC access to GPU memory.

### DIST-7.8 — Fast path versus fallback path
How to reason about a direct GPU↔NIC path versus host-staged communication.

### DIST-7.9 — GPUDirect Async and evolving direct-communication models
Reducing CPU involvement further by allowing devices to initiate or coordinate communication operations.

---

# 8. High-Performance Network Hardware

### DIST-8.1 — Network Interface Cards
NICs, ports, queues, DMA engines, and offload capabilities.

### DIST-8.2 — HPC switches
How switched fabrics connect many compute nodes.

### DIST-8.3 — InfiniBand
The architecture, transport model, link speeds, and why it is common in HPC.

### DIST-8.4 — Ethernet for HPC
RoCE and other approaches to low-latency/high-bandwidth Ethernet-based communication.

### DIST-8.5 — Network topology
Fat tree, Clos, dragonfly, torus, mesh, and why topology affects performance.

### DIST-8.6 — Adaptive routing
How switches choose paths to reduce congestion.

### DIST-8.7 — Oversubscription
What it means when aggregate endpoint bandwidth exceeds available upstream bandwidth.

### DIST-8.8 — Congestion
Hot spots, queue buildup, head-of-line blocking, and traffic interference.

### DIST-8.9 — Network offload
Collective offload, SHARP-like operations, and moving work into the network.

---

# 9. NCCL and GPU Collectives

### DIST-9.1 — Why GPU collectives need specialized libraries
The performance gap between generic process communication and GPU-aware collective execution.

### DIST-9.2 — NCCL programming model
Communicators, ranks, GPU buffers, streams, and collective calls.

### DIST-9.3 — Ring all-reduce
Reduce-scatter plus all-gather and why ring algorithms achieve high bandwidth.

### DIST-9.4 — Tree-based collectives
Reducing latency using hierarchical communication paths.

### DIST-9.5 — Algorithm selection
How message size, rank count, and topology influence collective algorithm choice.

### DIST-9.6 — Protocol selection
Different low-latency/high-throughput transport protocols inside GPU collective libraries.

### DIST-9.7 — Intra-node NCCL
Use of NVLink, NVSwitch, PCIe, and GPU peer access.

### DIST-9.8 — Inter-node NCCL
Use of GPU-direct networking, NICs, and hierarchical communication.

### DIST-9.9 — Topology-aware collective construction
How communication libraries map logical algorithms onto physical links.

### DIST-9.10 — Multiple NICs and rail-aware communication
How traffic is distributed across more than one network interface.

### DIST-9.11 — NCCL versus MPI
Different programming models, strengths, and typical roles in GPU HPC applications.

---

# 10. NVSHMEM and One-Sided GPU Communication

### DIST-10.1 — One-sided communication
Why a remote memory operation differs from explicit send/receive.

### DIST-10.2 — PGAS
Partitioned Global Address Space and the idea of globally accessible distributed memory.

### DIST-10.3 — Symmetric memory
Why NVSHMEM uses similarly allocated objects across processing elements.

### DIST-10.4 — Processing elements
The NVSHMEM execution model and its relationship to GPU processes/ranks.

### DIST-10.5 — Put and get
Remote writes and reads.

### DIST-10.6 — Atomics
Remote atomic operations and synchronization.

### DIST-10.7 — Signals, fences, and quiet
Ordering and completion in one-sided GPU communication.

### DIST-10.8 — Device-initiated communication
Why allowing GPU kernels to initiate communication can remove CPU coordination from the critical path.

### DIST-10.9 — NVSHMEM versus MPI/NCCL
When one-sided communication, collectives, or explicit messages are the better abstraction.

### DIST-10.10 — Communication-computation coupling
How NVSHMEM enables finer-grained overlap and irregular communication patterns.

---

# 11. Communication and Computation Overlap

### DIST-11.1 — Exposed versus hidden communication
The difference between total communication time and communication that actually delays the application.

### DIST-11.2 — Asynchronous execution
Host-side and device-side operations that proceed without immediate blocking.

### DIST-11.3 — Non-blocking MPI overlap
Starting communication early and doing useful work before waiting.

### DIST-11.4 — CUDA streams and communication
How GPU work queues interact with communication operations.

### DIST-11.5 — Double buffering
Alternating buffers so one chunk is processed while another transfers.

### DIST-11.6 — Pipelining
Breaking large work/data into chunks to overlap sequential stages.

### DIST-11.7 — Communication chunk size
The trade-off between more overlap and more protocol/startup overhead.

### DIST-11.8 — Progress engines and asynchronous progress
When communication advances without the application repeatedly calling into the runtime.

### DIST-11.9 — Dependency chains
Why communication cannot be hidden when later computation genuinely depends on arriving data.

### DIST-11.10 — Critical-path communication
Distinguishing communication that can be overlapped from communication that directly extends runtime.

---

# 12. Rank Placement, Affinity, and Topology

### DIST-12.1 — Logical rank placement
Mapping rank IDs to nodes, sockets, GPUs, and NICs.

### DIST-12.2 — GPU affinity
Ensuring each process uses the intended accelerator.

### DIST-12.3 — CPU affinity
Binding CPU helper/progress threads and MPI processes to suitable CPU cores.

### DIST-12.4 — NUMA affinity
Keeping host memory and CPU execution close to the correct devices.

### DIST-12.5 — NIC affinity
Matching ranks and GPUs to the nearest network interface.

### DIST-12.6 — Topology-aware rank ordering
Why different rank numbering can change communication paths.

### DIST-12.7 — Multi-rail placement
Using multiple NICs effectively without creating cross-socket or cross-root-complex traffic.

### DIST-12.8 — Locality versus balance
Why the best mapping may trade perfect locality for balanced use of network resources.

---

# 13. Multi-Node Communication Hierarchy

### DIST-13.1 — Scale-up and scale-out layers
GPU-GPU links inside a node/system versus the network between nodes.

### DIST-13.2 — Hierarchical collectives
First communicate locally, then across nodes, then locally again.

### DIST-13.3 — Local versus remote bandwidth
Why the communication hierarchy is often highly asymmetric.

### DIST-13.4 — GPU↔GPU paths across nodes
Trace a message from source GPU through local fabric, NIC, network, remote NIC, and destination GPU.

### DIST-13.5 — Multiple communication domains
CPU memory, GPU memory, NVLink/NVSwitch, PCIe, NIC, switch fabric, and their interaction.

### DIST-13.6 — Cross-socket penalties
Why a GPU using a remote NIC or CPU memory may take a slower path.

### DIST-13.7 — Node-level versus cluster-level bottlenecks
Distinguish local PCIe/NVLink issues from network/fabric limitations.

---

# 14. Communication Patterns in Scientific Computing

### DIST-14.1 — Nearest-neighbor communication
Stencil and domain-decomposition workloads.

### DIST-14.2 — Halo exchange
Why PDE solvers repeatedly communicate boundary data.

### DIST-14.3 — Global reductions
Residual norms, convergence checks, and synchronization-heavy algorithms.

### DIST-14.4 — Broadcast-heavy algorithms
Distributing shared data from one owner to many ranks.

### DIST-14.5 — All-to-all-heavy workloads
Transpose-based methods, FFTs, and distributed reshuffling.

### DIST-14.6 — Irregular communication
Sparse matrices, graphs, particles, and nonuniform communication.

### DIST-14.7 — Bulk synchronous parallelism
Alternating compute and communication phases.

### DIST-14.8 — Asynchronous communication models
Reducing global synchronization and allowing ranks to progress independently.

---

# 15. Communication in HPL and HPL-MxP

### DIST-15.1 — Distributed matrix ownership
How a block-cyclic matrix layout determines which rank owns each panel/block.

### DIST-15.2 — Panel broadcast
Why factorized panel data must be distributed to other ranks.

### DIST-15.3 — Row and column communication
How process-grid geometry creates distinct communication directions.

### DIST-15.4 — Broadcast algorithm choices
Trees, rings, MPI collectives, GPU-aware collectives, and topology effects.

### DIST-15.5 — Communication versus trailing GEMM
Why HPL attempts to overlap communication with high-throughput matrix updates.

### DIST-15.6 — Panel critical path
Why panel work and its communication can starve otherwise fast GPUs.

### DIST-15.7 — Chunking
Breaking panel communication into smaller pieces to change overlap and synchronization behavior.

### DIST-15.8 — MPI versus NCCL roles in HPL-MxP
Why an application may use more than one communication library.

### DIST-15.9 — Rank asymmetry
How uneven communication paths or progress can create different timelines across ranks.

### DIST-15.10 — Multi-node topology effects on HPL-MxP
Why process grids and rank placement must be re-evaluated when moving from single-node to multi-node systems.

---

# 16. Communication Diagnostics and Reasoning

### DIST-16.1 — Draw the expected data path
Before measuring anything, write the physical path data should take.

### DIST-16.2 — Identify fast path and fallback path
Direct GPU↔NIC communication versus host-staged or indirect alternatives.

### DIST-16.3 — Separate latency, bandwidth, and synchronization problems
Three different communication bottlenecks that require different fixes.

### DIST-16.4 — Small-message versus large-message behavior
Interpret benchmark results based on transfer regime.

### DIST-16.5 — Local versus remote tests
Use intra-node and inter-node comparisons to isolate the failing layer.

### DIST-16.6 — Pairwise communication reasoning
Test specific GPU↔GPU, GPU↔NIC, and node↔node paths to reveal topology asymmetry.

### DIST-16.7 — Collective benchmark reasoning
Understand what collective bandwidth and bus bandwidth represent.

### DIST-16.8 — Application communication traces
Recognize communication bursts, waiting, synchronization, overlap, and rank skew in timelines.

### DIST-16.9 — Build a communication bottleneck hypothesis
Use architecture, message size, topology, and timing evidence to predict the likely limiting layer.

---

# 17. Modern Distributed GPU Systems

This module should emphasize principles and changing system trends rather than memorizing a single vendor's product line.

### DIST-17.1 — GPU supernodes
Systems with dense scale-up GPU fabrics before the scale-out network boundary.

### DIST-17.2 — Network-attached GPU clusters
The common multi-node accelerator-cluster model.

### DIST-17.3 — NIC offload and smart networking
Increasing communication functionality inside the NIC/DPU.

### DIST-17.4 — In-network collectives
Reducing communication overhead by performing aggregation inside the fabric.

### DIST-17.5 — GPU-initiated networking
Architectural trends toward removing CPU involvement from communication control.

### DIST-17.6 — Coherent fabrics and memory-centric systems
Emerging models where device communication becomes increasingly memory-like.

### DIST-17.7 — Current multi-GPU system case studies
Study representative NVIDIA-, AMD-, and Intel-based systems and identify their scale-up and scale-out communication hierarchy.

---

# 18. Reading Distributed Systems Like an HPC Engineer

### DIST-18.1 — Read a topology diagram
Identify nodes, sockets, GPUs, NICs, switches, and communication domains.

### DIST-18.2 — Estimate transfer time
Use latency and bandwidth models for a given message size.

### DIST-18.3 — Estimate collective cost
Apply simple models to broadcast, all-reduce, and other collectives.

### DIST-18.4 — Predict likely bottlenecks from message size
Determine whether latency, bandwidth, or synchronization is likely dominant.

### DIST-18.5 — Predict rank-placement consequences
Reason about locality before running an experiment.

### DIST-18.6 — Reconstruct a GPU↔GPU path
Describe each hardware/software layer involved in intra-node and inter-node communication.

### DIST-18.7 — From communication symptoms to hypotheses
Turn poor scaling, idle GPUs, rank skew, or low bandwidth into a small set of testable explanations.

---

# Completion Goal

After completing this theme, the reader should be able to:

1. Explain the difference between **shared-memory and distributed-memory execution** and why explicit communication is needed.
2. Distinguish **latency, bandwidth, serialization, synchronization, and contention** as separate performance effects.
3. Understand the basic MPI model: **ranks, communicators, point-to-point operations, non-blocking communication, and collectives**.
4. Explain how major collective algorithms such as **rings and trees** work and why their performance depends on message size and topology.
5. Trace the difference between a **host-staged GPU communication path** and a **GPUDirect RDMA fast path**.
6. Explain the roles of **MPI, NCCL, and NVSHMEM** and why a distributed GPU application may use several of them.
7. Understand the purpose of **RDMA, NIC memory registration, InfiniBand/RoCE, switches, and network topology**.
8. Reason about **communication/computation overlap**, exposed communication, pipelining, and chunking.
9. Map ranks onto **CPUs, NUMA nodes, GPUs, and NICs** while considering locality and balance.
10. Trace communication hierarchically across **GPU fabric → PCIe → NIC → network → remote GPU**.
11. Recognize common scientific communication patterns such as **halo exchange, reductions, broadcasts, and all-to-all**.
12. Explain the communication structure of **HPL/HPL-MxP**, including panel broadcasts, process-grid effects, overlap, and rank asymmetry.
13. Use topology and benchmark evidence to form a **testable communication bottleneck hypothesis** instead of treating “communication” as one undifferentiated problem.
