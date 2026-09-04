# Theme 5 — Broader HPC Systems

**Theme ID:** `SYS`  
**Priority:** P3  
**Purpose:** Build broad systems-level understanding of how real HPC environments are assembled and operated around compute kernels: supercomputer architecture, schedulers, storage, programming models, compilers, scientific workloads, resilience, energy efficiency, emerging accelerators, and the HPC research/industry ecosystem.

## Scope

This curriculum focuses on:

- how complete HPC systems are structured beyond the processor level;
- cluster and supercomputer architecture;
- batch scheduling and resource management;
- operating systems, containers, and software environments;
- parallel filesystems and storage;
- compilers and performance-portable programming models;
- representative scientific workloads;
- resilience and fault tolerance;
- power, cooling, and Green500-style efficiency;
- system-level benchmarking;
- emerging accelerators and heterogeneous systems;
- AI/HPC convergence;
- how to interpret major supercomputer announcements, research papers, and industry developments.

Detailed GPU and CPU-GPU architecture belongs primarily to **Theme 1 — GPU + CPU-GPU Architecture**.

Detailed multi-node communication belongs primarily to **Theme 2 — Distributed / Multi-GPU HPC**.

Detailed HPL/HPL-MxP mathematics and numerical algorithms belong primarily to **Theme 3 — Dense Linear Algebra + HPL/HPL-MxP**.

Detailed profiling and optimization methodology belongs primarily to **Theme 4 — Performance Engineering**.

## Progression Rule

Follow the curriculum broadly in order, but this theme is intentionally more flexible than Themes 1–4.

A daily volume should normally combine **2–4 adjacent, tightly related topics** into one coherent lesson. Because this theme also tracks wider HPC developments, the agent may occasionally revisit a module when a major system, architecture, software stack, or research result makes it especially relevant.

---

# 1. HPC Systems Foundations

### SYS-1.1 — What makes a computer an HPC system
Performance, scale, concurrency, memory capacity, interconnect, storage, and workload characteristics.

### SYS-1.2 — Workstation, server, cluster, and supercomputer
How these systems differ in organization and scale.

### SYS-1.3 — Compute nodes
CPUs, accelerators, memory, NICs, local storage, and management interfaces.

### SYS-1.4 — Login, service, and compute nodes
Why production clusters separate user interaction from heavy computation.

### SYS-1.5 — Cluster fabric
The networks that connect nodes for computation, storage, and management.

### SYS-1.6 — Head nodes and management infrastructure
Provisioning, monitoring, authentication, scheduling, and orchestration.

### SYS-1.7 — System topology
How node-level and cluster-level hierarchy combine into one machine.

### SYS-1.8 — Capability versus capacity computing
Very large individual jobs versus many smaller jobs.

---

# 2. Supercomputer Architecture

### SYS-2.1 — Building blocks of a supercomputer
Compute cabinets, nodes, switches, storage, cooling, and power.

### SYS-2.2 — Homogeneous versus heterogeneous systems
CPU-only systems versus accelerator-heavy architectures.

### SYS-2.3 — Scale-up versus scale-out design
Large tightly connected nodes versus many distributed nodes.

### SYS-2.4 — Fat nodes versus thin nodes
Memory-rich/high-accelerator nodes compared with smaller replicated nodes.

### SYS-2.5 — System balance
Compute, memory bandwidth, network bandwidth, storage, and power must scale together.

### SYS-2.6 — Machine topology
Dragonfly, torus, fat-tree/Clos, mesh, and other large-system organizations.

### SYS-2.7 — Supercomputer case-study method
Identify compute node, accelerator, memory, interconnect, storage, power, and target workloads.

### SYS-2.8 — Why benchmark leadership does not imply universal superiority
Different systems are optimized for different workloads and constraints.

---

# 3. HPC Resource Management

### SYS-3.1 — Why clusters need schedulers
Shared expensive hardware must be allocated fairly and efficiently.

### SYS-3.2 — Jobs and resource requests
Nodes, CPUs, GPUs, memory, wall time, queues, and partitions.

### SYS-3.3 — Batch versus interactive jobs
Production execution versus short development sessions.

### SYS-3.4 — Queueing
Why a job may wait even when some hardware appears idle.

### SYS-3.5 — Scheduling policies
FIFO, priority, fair-share, reservations, backfilling, and quality of service.

### SYS-3.6 — Gang scheduling and tightly coupled jobs
Why distributed workloads often require all allocated resources simultaneously.

### SYS-3.7 — Resource fragmentation
How unused gaps can appear when job shapes do not fit available nodes.

### SYS-3.8 — Utilization versus user latency
The scheduler trade-off between keeping hardware busy and minimizing wait time.

---

# 4. Slurm, PBS, and Batch Systems

### SYS-4.1 — Batch scheduler architecture
Controller, execution daemons, accounting database, and worker nodes.

### SYS-4.2 — Slurm concepts
Partitions, jobs, steps, tasks, nodes, and resource flags.

### SYS-4.3 — PBS concepts
Queues, resource lists, jobs, execution hosts, and job scripts.

### SYS-4.4 — Resource specification
Requesting CPUs, GPUs, memory, nodes, and wall time correctly.

### SYS-4.5 — Job launch
How `srun`, `mpirun`, `mpiexec`, or launcher wrappers interact with allocations.

### SYS-4.6 — Scheduler environment variables
How jobs discover node lists, ranks, and allocated resources.

### SYS-4.7 — Reservations and exclusive nodes
When reproducibility or large distributed jobs require isolated resources.

### SYS-4.8 — Job arrays
Efficiently run many related independent experiments.

---

# 5. Operating Systems in HPC

### SYS-5.1 — Linux as the dominant HPC operating environment
Processes, files, devices, users, and permissions.

### SYS-5.2 — Processes and threads
Execution units managed by the operating system.

### SYS-5.3 — Virtual memory
Address spaces, pages, page tables, and mappings.

### SYS-5.4 — Huge pages
Why larger pages can reduce translation overhead.

### SYS-5.5 — CPU scheduling
How the OS schedules runnable threads onto cores.

### SYS-5.6 — Kernel versus user space
Where system services, drivers, and applications execute.

### SYS-5.7 — Device drivers
How operating systems expose GPUs, NICs, storage, and accelerators to software.

### SYS-5.8 — Syscalls and runtime overhead
When transitions into the kernel become relevant to performance.

---

# 6. Software Environments and Reproducibility

### SYS-6.1 — System software stack
OS, drivers, runtimes, libraries, compilers, MPI, application.

### SYS-6.2 — Environment modules
Why HPC systems use module systems to expose multiple software versions.

### SYS-6.3 — ABI compatibility
Why compiled libraries and runtimes must agree at binary interfaces.

### SYS-6.4 — Dependency management
Libraries, paths, environment variables, and version pinning.

### SYS-6.5 — Reproducible environments
Record software versions and configuration as part of the experiment.

### SYS-6.6 — Bare metal versus containers
What changes and what still depends on the host.

### SYS-6.7 — Driver/runtime boundary
Why containers can carry user-space libraries but still rely on host kernel drivers.

### SYS-6.8 — Environment drift
How updates can change performance or behavior without source-code changes.

---

# 7. Containers in HPC

### SYS-7.1 — Why containers are useful in HPC
Portable environments without full virtual-machine overhead.

### SYS-7.2 — Image versus runtime
Immutable filesystem image versus running container process.

### SYS-7.3 — Apptainer/Singularity model
Why HPC containers differ from typical Docker workflows.

### SYS-7.4 — GPU passthrough
How host devices and GPU drivers become visible inside containers.

### SYS-7.5 — Network and RDMA passthrough
Why high-performance networking must be correctly exposed inside containers.

### SYS-7.6 — MPI inside versus outside containers
Different integration models and compatibility constraints.

### SYS-7.7 — Bind mounts
Expose host filesystems and project directories inside containers.

### SYS-7.8 — Performance pitfalls
Missing devices, mismatched libraries, CPU affinity, shared-memory limits, and fallback communication paths.

---

# 8. HPC Storage Foundations

### SYS-8.1 — Storage hierarchy
Registers/cache/HBM/DRAM/local SSD/shared filesystem/archive.

### SYS-8.2 — Local versus shared storage
Node-local scratch compared with cluster-wide filesystems.

### SYS-8.3 — Throughput versus IOPS
Large sequential transfers versus many small operations.

### SYS-8.4 — Metadata
File creation, directories, stat operations, permissions, and why metadata can become a bottleneck.

### SYS-8.5 — Block versus file storage
Different storage abstractions and access patterns.

### SYS-8.6 — Storage latency
Why small-I/O workloads behave differently from bandwidth-heavy workloads.

### SYS-8.7 — Burst buffers and caching tiers
Use fast intermediate storage to absorb temporary high-I/O demand.

### SYS-8.8 — Data lifecycle
Input, scratch, checkpoints, output, archival storage, and deletion.

---

# 9. Parallel Filesystems

### SYS-9.1 — Why a single fileserver does not scale
Bandwidth and metadata bottlenecks.

### SYS-9.2 — Striping
Distribute file data across multiple storage targets.

### SYS-9.3 — Lustre
Metadata servers, object storage servers, targets, and client access.

### SYS-9.4 — IBM Spectrum Scale / GPFS
Distributed filesystem concepts and cluster-wide storage.

### SYS-9.5 — Weka-class high-performance storage
Modern distributed NVMe and data-path ideas.

### SYS-9.6 — Parallel I/O
Multiple ranks reading/writing concurrently.

### SYS-9.7 — Small-file problem
Why many tiny files can overwhelm metadata systems.

### SYS-9.8 — I/O contention
Many jobs competing for shared storage bandwidth.

### SYS-9.9 — File-per-rank versus shared-file patterns
Trade-offs in parallel scientific applications.

---

# 10. Parallel I/O Libraries

### SYS-10.1 — Why applications use parallel I/O libraries
Raw POSIX I/O often does not scale well across many ranks.

### SYS-10.2 — MPI-IO
Collective and independent I/O through MPI.

### SYS-10.3 — Collective buffering
Aggregate small requests into larger efficient transfers.

### SYS-10.4 — HDF5
Structured scientific datasets, metadata, and parallel access.

### SYS-10.5 — NetCDF
Self-describing scientific data and parallel variants.

### SYS-10.6 — ADIOS
High-performance I/O and in situ data movement.

### SYS-10.7 — I/O aggregation
Use selected ranks or nodes to concentrate writes.

### SYS-10.8 — I/O performance reasoning
Match application access pattern to filesystem and library behavior.

---

# 11. Compilers for HPC

### SYS-11.1 — What a compiler does
Front end, intermediate representation, optimization, code generation, and linking.

### SYS-11.2 — Optimization levels
What `-O1`, `-O2`, `-O3`, and architecture-specific flags broadly change.

### SYS-11.3 — Vectorization
Transform loops into SIMD instructions.

### SYS-11.4 — Loop transformations
Unrolling, interchange, fusion, fission, blocking, and software pipelining.

### SYS-11.5 — Inlining
Trade function-call overhead against code size.

### SYS-11.6 — Auto-parallelization
When compilers can expose parallelism automatically.

### SYS-11.7 — Profile-guided optimization
Use runtime behavior to guide compilation.

### SYS-11.8 — Link-time optimization
Optimize across translation-unit boundaries.

### SYS-11.9 — Compiler reports
Understand why loops did or did not vectorize or optimize.

---

# 12. HPC Programming Models

### SYS-12.1 — Serial programming as the baseline
Single process and single execution stream.

### SYS-12.2 — OpenMP
Shared-memory parallelism using threads and directives.

### SYS-12.3 — MPI
Distributed-memory process communication.

### SYS-12.4 — CUDA and HIP
Explicit GPU programming models.

### SYS-12.5 — SYCL
Single-source heterogeneous programming.

### SYS-12.6 — OpenACC
Directive-based accelerator programming.

### SYS-12.7 — Kokkos
Performance-portable abstractions for scientific kernels.

### SYS-12.8 — RAJA and related abstraction layers
Portable loop and execution-policy frameworks.

### SYS-12.9 — Hybrid programming
MPI + OpenMP, MPI + CUDA/HIP, and multi-model applications.

---

# 13. Performance Portability

### SYS-13.1 — Why portable code can still perform differently
Hardware differs in execution width, memory hierarchy, and parallelism.

### SYS-13.2 — Functional portability versus performance portability
Running everywhere is not the same as running efficiently everywhere.

### SYS-13.3 — Abstraction cost
What may be lost when hiding hardware-specific details.

### SYS-13.4 — Backend specialization
Use common source code with architecture-specific implementations.

### SYS-13.5 — Tuning spaces
Thread/block shapes, vector width, tile size, memory placement, and work decomposition.

### SYS-13.6 — Auto-tuning
Search for architecture-specific parameter choices automatically.

### SYS-13.7 — Domain-specific languages
Use workload structure to generate specialized code.

### SYS-13.8 — Portable performance as co-design
Balance abstraction, compiler quality, library support, and hardware awareness.

---

# 14. Scientific Workload Taxonomy

### SYS-14.1 — Dense linear algebra
High arithmetic intensity and regular memory access.

### SYS-14.2 — Sparse linear algebra
Irregular memory access and indirect indexing.

### SYS-14.3 — Structured grids and stencils
Local-neighbor computation with halo exchange.

### SYS-14.4 — Unstructured meshes
Irregular data structures and communication patterns.

### SYS-14.5 — FFTs
Global data rearrangement and all-to-all communication.

### SYS-14.6 — Particle methods
Neighbor search, force computation, and dynamic data movement.

### SYS-14.7 — Monte Carlo methods
Large numbers of statistically independent samples.

### SYS-14.8 — Graph workloads
Irregular traversal and low arithmetic intensity.

### SYS-14.9 — Machine learning workloads
Dense tensor computation combined with large-scale collective communication.

---

# 15. Sparse Linear Algebra

### SYS-15.1 — Sparse matrices
Store only nonzero entries.

### SYS-15.2 — Sparse matrix formats
CSR, CSC, COO, ELL, and why layout depends on access pattern.

### SYS-15.3 — SpMV
Sparse matrix-vector multiplication and its low arithmetic intensity.

### SYS-15.4 — Irregular memory access
Indirect indexing, poor locality, and cache inefficiency.

### SYS-15.5 — Load imbalance in sparse computation
Rows can contain very different numbers of nonzeros.

### SYS-15.6 — Sparse solvers
Iterative methods and preconditioning at a conceptual level.

### SYS-15.7 — Sparse GPU challenges
Why GPUs excel less naturally at irregular workloads than dense GEMM.

### SYS-15.8 — Communication in distributed sparse systems
Ghost values, halo exchange, and global reductions.

---

# 16. CFD and PDE Workloads

### SYS-16.1 — Discretizing physical equations
Turn continuous PDEs into algebraic systems.

### SYS-16.2 — Structured versus unstructured meshes
Different memory and communication behavior.

### SYS-16.3 — Domain decomposition
Split the physical domain across processes.

### SYS-16.4 — Halo/ghost regions
Duplicate boundary information required by neighboring subdomains.

### SYS-16.5 — Sparse linear solves
Why solver performance often dominates CFD runtime.

### SYS-16.6 — Preconditioning
Transform a difficult iterative solve into an easier one.

### SYS-16.7 — Global reductions
Residual norms and convergence checks as synchronization points.

### SYS-16.8 — OpenFOAM as an HPC workload
Meshes, sparse operators, solvers, MPI decomposition, and typical bottlenecks.

---

# 17. FFT and Spectral Workloads

### SYS-17.1 — Discrete Fourier Transform
Transform data between spatial/time and frequency domains.

### SYS-17.2 — Fast Fourier Transform
Reduce complexity from \(O(n^2)\) to \(O(n \log n)\).

### SYS-17.3 — Local FFT computation
Highly optimized regular numerical kernels.

### SYS-17.4 — Distributed FFT decomposition
Slab and pencil decompositions.

### SYS-17.5 — Transposes
Why distributed FFTs require large data redistribution.

### SYS-17.6 — All-to-all communication
The scale-out bottleneck in many FFT applications.

### SYS-17.7 — FFT libraries
FFTW, cuFFT, rocFFT, oneMKL FFT, and vendor optimization.

### SYS-17.8 — Compute versus communication scaling
Why FFTs can become network-bound at large scale.

---

# 18. HPC Resilience and Fault Tolerance

### SYS-18.1 — Why failures matter at scale
More components increase the probability that something fails during a long job.

### SYS-18.2 — Mean time between failures
System reliability as a statistical property.

### SYS-18.3 — Checkpoint/restart
Periodically save state so failed jobs can resume.

### SYS-18.4 — Checkpoint overhead
Writing large application state can consume time and storage bandwidth.

### SYS-18.5 — Incremental and asynchronous checkpointing
Reduce the cost of saving application state.

### SYS-18.6 — Algorithm-based fault tolerance
Use mathematical redundancy rather than only full checkpoints.

### SYS-18.7 — Hardware error detection
ECC, parity, link errors, and device health monitoring.

### SYS-18.8 — Resilient distributed runtimes
Recover or continue when a node/process fails.

---

# 19. Power and Energy Efficiency

### SYS-19.1 — Power versus energy
Instantaneous watts versus total joules consumed.

### SYS-19.2 — Performance per watt
Useful work delivered for a given power budget.

### SYS-19.3 — Dynamic versus static power
Switching activity versus leakage/background power.

### SYS-19.4 — DVFS
Trade frequency, voltage, power, and performance.

### SYS-19.5 — Power caps
Operate hardware under facility or system limits.

### SYS-19.6 — Energy-to-solution
A faster run can consume less total energy even at higher power.

### SYS-19.7 — Green500
Benchmark-oriented view of supercomputer energy efficiency.

### SYS-19.8 — Performance-energy co-optimization
Optimize both time-to-solution and energy-to-solution.

---

# 20. Cooling and Physical Infrastructure

### SYS-20.1 — Why cooling matters to HPC
Compute density converts electrical power into heat.

### SYS-20.2 — Air cooling
Traditional datacenter airflow and its density limits.

### SYS-20.3 — Direct liquid cooling
Move heat efficiently from high-power processors.

### SYS-20.4 — Immersion cooling
Submerge hardware in dielectric fluid.

### SYS-20.5 — Rack power density
Why modern accelerator systems reshape datacenter design.

### SYS-20.6 — Thermal throttling
Hardware reduces clocks when thermal limits are exceeded.

### SYS-20.7 — Facility-level efficiency
Cooling, power conversion, and non-compute energy overhead.

### SYS-20.8 — Compute architecture and infrastructure co-design
System packaging, cooling, and power delivery constrain achievable performance.

---

# 21. System Monitoring and Operations

### SYS-21.1 — Why clusters need observability
Failures and performance issues occur across many components.

### SYS-21.2 — Node health
CPU, memory, GPU, temperature, power, and device status.

### SYS-21.3 — GPU telemetry
Utilization, memory use, clocks, ECC, thermals, and power.

### SYS-21.4 — Network telemetry
Link state, errors, congestion, counters, and traffic rates.

### SYS-21.5 — Filesystem telemetry
Capacity, bandwidth, metadata load, and failure state.

### SYS-21.6 — Scheduler/accounting data
Job history, utilization, queue wait, and fair-share behavior.

### SYS-21.7 — Logs
Kernel, driver, scheduler, application, and service logs.

### SYS-21.8 — From telemetry to diagnosis
Correlate system events with application symptoms.

---

# 22. HPC Benchmark Ecosystem

### SYS-22.1 — Why benchmarks exist
Compare systems using standardized workloads.

### SYS-22.2 — HPL
Dense floating-point throughput and TOP500 relevance.

### SYS-22.3 — HPCG
Sparse/irregular computation and memory/network stress.

### SYS-22.4 — STREAM
Sustained memory bandwidth.

### SYS-22.5 — OSU and communication microbenchmarks
Latency, bandwidth, and collectives.

### SYS-22.6 — Graph500
Graph traversal and irregular-memory performance.

### SYS-22.7 — MLPerf and AI-oriented benchmarks
Training/inference throughput and system-level AI performance.

### SYS-22.8 — Benchmark suites versus real applications
Why no single benchmark characterizes every workload.

### SYS-22.9 — Benchmark gaming
How systems can be tuned aggressively for a benchmark without being universally better.

---

# 23. TOP500, Green500, and System Rankings

### SYS-23.1 — What TOP500 measures
Ranking systems primarily through HPL performance.

### SYS-23.2 — Rmax and Rpeak
Measured HPL performance versus theoretical peak.

### SYS-23.3 — Efficiency
Interpret \(R_{max}/R_{peak}\).

### SYS-23.4 — Green500
Performance per watt ranking.

### SYS-23.5 — Why ranking position is incomplete
Application mix, memory, network, storage, reliability, and usability also matter.

### SYS-23.6 — Reading a TOP500 system entry
Processor, accelerator, interconnect, core count, performance, and power.

### SYS-23.7 — Generational trends
Track changes in accelerators, interconnects, power, and architectural balance.

### SYS-23.8 — Benchmark rankings as architectural evidence
Use ranking data to ask why certain system designs perform well.

---

# 24. AI and HPC Convergence

### SYS-24.1 — Why AI and HPC increasingly share infrastructure
Both require accelerators, high-bandwidth memory, fast networks, and large-scale scheduling.

### SYS-24.2 — Similarities between AI training and scientific HPC
Dense tensor operations, collectives, parallel I/O, and large memory footprints.

### SYS-24.3 — Differences between AI and classical HPC
Precision, numerical requirements, workload regularity, runtime systems, and application goals.

### SYS-24.4 — Mixed workloads
Operate simulation, data analytics, and AI on the same infrastructure.

### SYS-24.5 — AI for Science
Use machine learning inside scientific discovery and simulation workflows.

### SYS-24.6 — Surrogate models
Replace or augment expensive numerical simulations with learned approximations.

### SYS-24.7 — In situ AI
Analyze or steer simulations while data remains close to the compute system.

### SYS-24.8 — Foundation models for scientific domains
Large models trained on scientific data, code, or physical systems.

---

# 25. Emerging Accelerators

### SYS-25.1 — Why accelerators keep appearing
Specialization can improve performance and energy efficiency.

### SYS-25.2 — GPUs
General throughput accelerators with increasingly specialized matrix hardware.

### SYS-25.3 — AI accelerators
TPUs, NPUs, wafer-scale engines, and matrix-centric designs.

### SYS-25.4 — FPGAs
Reconfigurable data paths and domain-specific pipelines.

### SYS-25.5 — Vector processors
Explicit wide-vector execution and their continued relevance.

### SYS-25.6 — Dataflow architectures
Execute according to data dependencies rather than conventional instruction streams.

### SYS-25.7 — Wafer-scale systems
Use extremely large silicon area to reduce off-chip communication.

### SYS-25.8 — Choosing an accelerator
Match workload structure, precision, memory behavior, programmability, and ecosystem.

---

# 26. Chiplets, CXL, and Memory-Centric Systems

### SYS-26.1 — Why chiplets matter at system scale
Modularity, yield, heterogeneous integration, and package-level communication.

### SYS-26.2 — Die-to-die interconnects
High-bandwidth communication inside a package.

### SYS-26.3 — CXL concepts
Cache coherence, memory expansion, and device interconnect over PCIe-class links.

### SYS-26.4 — Memory expansion
Attach additional memory capacity beyond processor-local DRAM/HBM.

### SYS-26.5 — Memory pooling
Share memory resources across multiple hosts or devices.

### SYS-26.6 — Tiered memory
HBM, DRAM, CXL-attached memory, local SSD, and other levels.

### SYS-26.7 — Data placement
Choose which tier should hold which data.

### SYS-26.8 — Memory-centric HPC
Shift some system design emphasis from compute throughput toward data placement and movement.

---

# 27. Workflow Systems and Scientific Pipelines

### SYS-27.1 — Beyond a single job
Real scientific campaigns often consist of many dependent tasks.

### SYS-27.2 — Workflow DAGs
Represent simulations, preprocessing, analysis, and postprocessing as dependencies.

### SYS-27.3 — Ensemble workloads
Run many related simulations with different parameters.

### SYS-27.4 — High-throughput computing
Optimize total completed tasks rather than one tightly coupled job.

### SYS-27.5 — Workflow engines
Automate dependencies and execution across cluster resources.

### SYS-27.6 — Data movement between stages
Intermediate datasets can dominate workflow cost.

### SYS-27.7 — In situ and in transit processing
Analyze simulation data before writing everything to storage.

### SYS-27.8 — End-to-end time-to-science
Optimize the whole scientific workflow, not only the compute kernel.

---

# 28. HPC Security and Multi-User Systems

### SYS-28.1 — Why HPC security differs from a personal workstation
Large shared systems contain valuable data and expensive infrastructure.

### SYS-28.2 — Authentication and authorization
Users, groups, SSH, directory services, and access control.

### SYS-28.3 — Privilege separation
Why normal users should not need administrative access.

### SYS-28.4 — Software supply chain
Risks from packages, containers, scripts, and dependencies.

### SYS-28.5 — Multi-user isolation
Prevent one job from interfering with or accessing another.

### SYS-28.6 — Network segmentation
Separate management, storage, compute, and external traffic.

### SYS-28.7 — Secrets and credentials
API keys, SSH keys, tokens, and secure handling.

### SYS-28.8 — Security versus usability
Strong controls must still support scientific productivity.

---

# 29. HPC Research and Conference Landscape

### SYS-29.1 — Major HPC conferences
SC, ISC, IPDPS, PPoPP, HPDC, and related venues.

### SYS-29.2 — Architecture conferences relevant to HPC
ISCA, MICRO, HPCA, ASPLOS, and related systems/architecture venues.

### SYS-29.3 — Numerical and scientific computing venues
Where algorithms, solvers, and scientific software are published.

### SYS-29.4 — Vendor technical conferences
Architecture and software announcements from NVIDIA, AMD, Intel, and others.

### SYS-29.5 — National laboratories
Why DOE and other national labs are central to HPC research and procurement.

### SYS-29.6 — Academic HPC centers
University research groups and computing centers as sources of systems work.

### SYS-29.7 — Preprints versus peer-reviewed papers
How to treat arXiv and unpublished results.

### SYS-29.8 — Follow the citation trail
Move from an announcement or article to the original technical paper or documentation.

---

# 30. Reading HPC News Critically

### SYS-30.1 — Separate marketing from architecture
Ask what technically changed.

### SYS-30.2 — Peak numbers versus real workload performance
Theoretical capability is not sustained application performance.

### SYS-30.3 — Precision matters
A PFLOP/s number is meaningless without knowing FP64, FP32, FP16, FP8, integer, or tensor conditions.

### SYS-30.4 — System scale matters
Single-device results and full-system results answer different questions.

### SYS-30.5 — Benchmark choice matters
HPL, HPCG, MLPerf, application benchmarks, and microbenchmarks measure different properties.

### SYS-30.6 — Compare against the correct predecessor
Generational improvements require equivalent workload and configuration.

### SYS-30.7 — Identify the actual innovation
Compute core, memory, packaging, interconnect, cooling, software, algorithm, or system integration.

### SYS-30.8 — Ask who benefits
Map the development to workload characteristics rather than assuming universal improvement.

---

# 31. Reading HPC Papers Critically

### SYS-31.1 — Identify the research question
What specific limitation or opportunity is being addressed?

### SYS-31.2 — Understand the baseline
What existing method or system is the work compared against?

### SYS-31.3 — Identify the mechanism
Architecture, algorithm, runtime, communication, compiler, or storage change.

### SYS-31.4 — Understand the evaluation setup
Hardware, software, problem sizes, datasets, and cluster scale.

### SYS-31.5 — Inspect metrics
Runtime, throughput, scaling, energy, memory, accuracy, or utilization.

### SYS-31.6 — Look for ablations
Which component of the proposed technique actually creates the improvement?

### SYS-31.7 — Check generality
Does the result apply to one workload or a wider class?

### SYS-31.8 — Convert the paper into a reusable lesson
Extract the principle rather than memorizing the implementation.

---

# 32. From System Announcement to Technical Understanding

### SYS-32.1 — Step 1: identify the node architecture
CPU, GPU/accelerator, memory, and local interconnect.

### SYS-32.2 — Step 2: identify scale-out networking
NICs, switches, network topology, and expected bandwidth.

### SYS-32.3 — Step 3: identify storage
Filesystem, bandwidth, capacity, and data-management strategy.

### SYS-32.4 — Step 4: identify power and cooling
Understand physical constraints and efficiency.

### SYS-32.5 — Step 5: identify software environment
Programming models, libraries, compiler stack, and scheduler.

### SYS-32.6 — Step 6: identify target workloads
What science or computation is the machine designed to accelerate?

### SYS-32.7 — Step 7: compare with previous generation
Ask what architectural or system bottleneck the new design attempts to remove.

### SYS-32.8 — Step 8: form performance hypotheses
Predict which workloads should benefit and where new bottlenecks may appear.

---

# Completion Goal

After completing this theme, the reader should be able to:

1. Explain how a complete HPC system is assembled from **compute nodes, interconnects, storage, schedulers, software, power, and cooling**.
2. Understand why clusters use **batch schedulers**, and interpret the core ideas behind Slurm/PBS resource allocation and job launch.
3. Explain the role of **Linux, drivers, containers, modules, compilers, and runtime libraries** in an HPC software stack.
4. Understand the fundamentals of **parallel storage, Lustre/GPFS/Weka-class systems, MPI-IO, HDF5, and I/O bottlenecks**.
5. Compare major **HPC programming models** and understand the idea of performance portability.
6. Recognize the computational and communication structure of representative scientific workloads such as **dense linear algebra, sparse linear algebra, CFD, FFTs, particle methods, and AI training**.
7. Explain why resilience, checkpointing, power, cooling, and observability become increasingly important at large scale.
8. Interpret **TOP500, Green500, HPL, HPCG, STREAM, communication microbenchmarks, and application benchmarks** in context.
9. Understand broad trends in **AI/HPC convergence, chiplets, CXL, emerging accelerators, and memory-centric systems**.
10. Read major HPC announcements and distinguish **technical innovation from marketing**.
11. Read research papers and identify the **problem, mechanism, baseline, evaluation, and transferable principle**.
12. Place new developments into the larger HPC system stack instead of viewing processors, networks, storage, software, and workloads in isolation.
