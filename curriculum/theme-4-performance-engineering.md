# Theme 4 — Performance Engineering

**Theme ID:** `PERF`  
**Priority:** P2  
**Purpose:** Build a rigorous, repeatable methodology for analyzing and optimizing HPC applications by measurement rather than intuition, from basic performance metrics to profiling, bottleneck classification, experiment design, and end-to-end optimization workflows.

## Scope

This curriculum focuses on:

- how to define and measure performance correctly;
- wall time, throughput, latency, utilization, and scaling;
- compute-bound, memory-bound, latency-bound, synchronization-bound, and communication-bound behavior;
- Roofline and related performance models;
- CPU and GPU profiling;
- kernel analysis;
- occupancy and launch behavior;
- memory traffic and cache behavior;
- NUMA and affinity;
- communication/computation overlap;
- rank imbalance and critical-path analysis;
- benchmark design and reproducibility;
- parameter sweeps and experimental methodology;
- performance-counter interpretation;
- hypothesis-driven optimization;
- reading profiler timelines and translating symptoms into mechanisms;
- performance portability and optimization across different hardware.

Detailed GPU architecture belongs primarily to **Theme 1 — GPU + CPU-GPU Architecture**.

Detailed distributed communication concepts belong primarily to **Theme 2 — Distributed / Multi-GPU HPC**.

Detailed HPL/HPL-MxP algorithm structure belongs primarily to **Theme 3 — Dense Linear Algebra + HPL/HPL-MxP**.

## Progression Rule

Follow the curriculum in order unless there is a strong reason to revisit an earlier topic.

A daily volume should normally combine **2–4 adjacent, tightly related topics** into one coherent lesson. Dense topics may occupy a full volume by themselves.

---

# 1. Performance Engineering Foundations

### PERF-1.1 — What performance engineering is
Optimization as a cycle of measurement, modeling, hypothesis, intervention, and validation.

### PERF-1.2 — Correctness before performance
Why an incorrect or unstable result invalidates optimization.

### PERF-1.3 — Wall-clock time
The simplest end-to-end performance metric and what it includes.

### PERF-1.4 — Throughput
Work completed per unit time.

### PERF-1.5 — Latency
Time required to complete one operation, stage, request, or dependency.

### PERF-1.6 — Utilization
Fraction of available hardware resources doing useful work.

### PERF-1.7 — Efficiency
Useful performance relative to a reference limit such as theoretical peak or ideal scaling.

### PERF-1.8 — Bottleneck
The resource or dependency that currently limits overall performance.

---

# 2. Measuring Work Correctly

### PERF-2.1 — Define the unit of work
Iterations, FLOPs, cells, elements, requests, tokens, or other meaningful workload units.

### PERF-2.2 — Time per operation versus operations per second
Two equivalent but differently useful views.

### PERF-2.3 — Warm-up effects
JIT compilation, cache population, memory allocation, and initialization overhead.

### PERF-2.4 — Steady-state measurement
Distinguish setup time from repeated compute behavior.

### PERF-2.5 — End-to-end versus kernel-only timing
Why local optimization can fail to improve total runtime.

### PERF-2.6 — Inclusive versus exclusive time
Whether a stage's time includes time spent inside subroutines.

### PERF-2.7 — Synchronization and timing
Why unsynchronized GPU timing can produce misleading measurements.

### PERF-2.8 — Timer overhead and resolution
Understand the measurement tool itself.

---

# 3. Variability and Reproducibility

### PERF-3.1 — Run-to-run variance
Why repeated executions are not identical.

### PERF-3.2 — Mean, median, minimum, and maximum
When each summary statistic is useful.

### PERF-3.3 — Standard deviation and coefficient of variation
Quantify stability and relative noise.

### PERF-3.4 — Outliers
Distinguish real system events from ordinary variation.

### PERF-3.5 — Number of repetitions
Trade measurement confidence against experiment cost.

### PERF-3.6 — Environmental control
CPU frequency, GPU clocks, thermal state, background jobs, filesystem traffic, and node sharing.

### PERF-3.7 — Reproducible experiment records
Hardware, software versions, environment variables, launch command, parameters, and results.

### PERF-3.8 — Baselines
Why every optimization experiment needs a clearly defined comparison point.

---

# 4. Performance Limits

### PERF-4.1 — Theoretical peak
Maximum hardware throughput under ideal conditions.

### PERF-4.2 — Sustained peak
Performance achievable by a highly optimized representative kernel.

### PERF-4.3 — Application efficiency
Measured performance divided by an appropriate peak or baseline.

### PERF-4.4 — Compute limit
When arithmetic execution capacity is the dominant constraint.

### PERF-4.5 — Memory bandwidth limit
When data movement to/from memory dominates.

### PERF-4.6 — Latency limit
When dependency or startup cost dominates.

### PERF-4.7 — Communication limit
When inter-device or inter-node data movement dominates.

### PERF-4.8 — Synchronization limit
When workers spend significant time waiting for each other.

### PERF-4.9 — Serial fraction
Work that does not scale with additional parallel resources.

---

# 5. Roofline Model

### PERF-5.1 — Arithmetic intensity
Operations performed per byte transferred from a chosen memory level.

### PERF-5.2 — Roofline axes
Performance versus arithmetic intensity.

### PERF-5.3 — Compute roof
The flat region limited by peak arithmetic throughput.

### PERF-5.4 — Bandwidth roof
The sloped region limited by memory bandwidth.

### PERF-5.5 — Ridge point
Where the dominant theoretical bottleneck changes from bandwidth to compute.

### PERF-5.6 — Operational intensity in practice
Estimate bytes moved and FLOPs performed by a kernel.

### PERF-5.7 — Hierarchical Roofline
L1, L2, HBM/DRAM, and other memory levels as separate bandwidth roofs.

### PERF-5.8 — Roofline limitations
Why dependencies, latency, instruction mix, occupancy, and communication may violate simple Roofline expectations.

### PERF-5.9 — Use Roofline for hypotheses
Convert a performance point into a likely optimization direction.

---

# 6. Amdahl, Gustafson, and Scaling

### PERF-6.1 — Speedup
Compare optimized or parallel runtime against a baseline.

### PERF-6.2 — Amdahl's Law
How a serial fraction limits strong scaling.

### PERF-6.3 — Gustafson's Law
Why larger problems can use more processors efficiently.

### PERF-6.4 — Strong scaling
Fixed global problem size with increasing resources.

### PERF-6.5 — Weak scaling
Increase problem size proportionally with resources.

### PERF-6.6 — Parallel efficiency
Speedup divided by resource-count increase.

### PERF-6.7 — Superlinear speedup
Why cache/memory effects can occasionally produce more-than-linear behavior.

### PERF-6.8 — Scaling collapse
When communication, synchronization, or serial work overtakes useful compute.

---

# 7. Profiling Fundamentals

### PERF-7.1 — Instrumentation versus sampling
Measure every event versus periodically observe execution state.

### PERF-7.2 — Timeline profiling
View when CPU threads, GPU kernels, copies, and communications execute.

### PERF-7.3 — Statistical profiling
Identify hot functions without tracing every event.

### PERF-7.4 — Hardware performance counters
Read low-level events from CPUs, GPUs, memory systems, and NICs.

### PERF-7.5 — Tracing overhead
Why detailed profiling can perturb the program.

### PERF-7.6 — Profiling scope
System-level, process-level, thread-level, kernel-level, and instruction-level analysis.

### PERF-7.7 — Start broad, then zoom in
Use coarse profiling to identify the important region before collecting expensive detail.

### PERF-7.8 — Correlation versus causation
A high counter value is evidence, not automatically the root cause.

---

# 8. CPU Profiling

### PERF-8.1 — CPU hotspots
Identify functions consuming the most CPU time.

### PERF-8.2 — Instructions per cycle
Measure how much useful instruction throughput the CPU achieves.

### PERF-8.3 — Front-end stalls
Instruction fetch, decode, and branch-delivery limitations.

### PERF-8.4 — Back-end stalls
Execution-unit and memory-system delays.

### PERF-8.5 — Branch prediction
Mispredictions and control-flow cost.

### PERF-8.6 — Cache misses
L1/L2/L3 behavior and memory-latency consequences.

### PERF-8.7 — Memory bandwidth
Measure DRAM traffic and saturation.

### PERF-8.8 — CPU vectorization
SIMD utilization and missed vectorization opportunities.

### PERF-8.9 — Thread scaling
Detect lock contention, serial regions, and memory-bandwidth saturation.

### PERF-8.10 — CPU profiling tools
perf, VTune, vendor tools, and system-level profilers.

---

# 9. GPU Profiling Foundations

### PERF-9.1 — Kernel duration
Measure how long individual GPU kernels run.

### PERF-9.2 — Kernel launch overhead
Why very small kernels can become launch-bound.

### PERF-9.3 — GPU utilization
What aggregate utilization means and what it does not tell you.

### PERF-9.4 — SM/CU utilization
How much compute hardware is active.

### PERF-9.5 — Memory utilization
How heavily the memory system is used.

### PERF-9.6 — Copy-engine utilization
Identify host-device or peer-transfer activity.

### PERF-9.7 — Kernel concurrency
Determine whether multiple kernels overlap.

### PERF-9.8 — CPU-GPU gaps
Idle periods caused by host orchestration, dependencies, synchronization, or insufficient queued work.

### PERF-9.9 — GPU profiling tools
Nsight Systems, Nsight Compute, rocprof/rocprofiler, Intel GPU profiling tools, and related vendor utilities.

---

# 10. Reading GPU Timelines

### PERF-10.1 — Kernel sequences
Recognize repeated computational phases.

### PERF-10.2 — Gaps between kernels
Identify launch delay, dependencies, synchronization, or CPU-side work.

### PERF-10.3 — Transfer-compute overlap
Determine whether data movement is hidden behind computation.

### PERF-10.4 — Multiple streams
Understand concurrent queues and dependency relationships.

### PERF-10.5 — Host synchronization
Recognize points where the CPU waits for GPU completion.

### PERF-10.6 — Device synchronization
Recognize barriers and dependent kernels.

### PERF-10.7 — Rank-to-rank timeline comparison
Find skew, idle time, and asymmetric progress.

### PERF-10.8 — Critical-path reconstruction
Identify the sequence of operations that determines total runtime.

---

# 11. GPU Kernel Analysis

### PERF-11.1 — Instructions and instruction mix
Floating-point, integer, memory, control, and tensor/matrix instructions.

### PERF-11.2 — Achieved occupancy
Actual active-warps/resource residency compared with architectural maximum.

### PERF-11.3 — Eligible versus active warps
Why occupancy alone does not guarantee useful execution.

### PERF-11.4 — Warp stall reasons
Memory dependencies, execution dependencies, barriers, instruction fetch, and other stall classes.

### PERF-11.5 — Branch divergence
Measure control-flow inefficiency.

### PERF-11.6 — Register pressure
Relate register allocation to occupancy and spilling.

### PERF-11.7 — Shared-memory use
Capacity, bandwidth, and bank-conflict behavior.

### PERF-11.8 — Tensor/matrix-core utilization
Measure specialized matrix-engine activity.

### PERF-11.9 — Instruction throughput
Compare achieved pipeline issue rates with architectural limits.

### PERF-11.10 — Kernel bottleneck classification
Synthesize counters into compute-, memory-, latency-, or occupancy-limited behavior.

---

# 12. GPU Memory Analysis

### PERF-12.1 — Global-memory throughput
Measure HBM/device-memory traffic.

### PERF-12.2 — Load/store efficiency
Useful bytes relative to actual memory transactions.

### PERF-12.3 — Memory coalescing
Detect fragmented access patterns.

### PERF-12.4 — L1 cache behavior
Hit rate, local reuse, and access locality.

### PERF-12.5 — L2 cache behavior
Shared reuse and traffic to external memory.

### PERF-12.6 — Shared-memory bank conflicts
Identify serialized accesses inside on-chip memory.

### PERF-12.7 — Memory latency
Long dependency chains versus throughput saturation.

### PERF-12.8 — Memory bandwidth saturation
Distinguish high utilization from inefficient access.

### PERF-12.9 — Register spilling
Detect unexpected traffic caused by insufficient registers.

### PERF-12.10 — Memory bottleneck remedies
Change data layout, tiling, reuse, access order, fusion, or working-set size.

---

# 13. Kernel Launch and Granularity

### PERF-13.1 — Launch-bound workloads
When kernel invocation overhead is a significant fraction of runtime.

### PERF-13.2 — Small-kernel inefficiency
Insufficient work to occupy the GPU.

### PERF-13.3 — Kernel fusion
Combine adjacent operations to reduce launches and memory traffic.

### PERF-13.4 — Kernel splitting
When separating work improves overlap or specialization.

### PERF-13.5 — Batch size
Increase independent work to improve hardware utilization.

### PERF-13.6 — Work granularity
Fine-grained versus coarse-grained decomposition.

### PERF-13.7 — Persistent kernels
Keep work resident on the GPU to reduce repeated launch and setup overhead.

### PERF-13.8 — CUDA Graphs and equivalent launch-reduction mechanisms
Reduce host launch overhead for repeated dependency graphs.

---

# 14. Concurrency and Overlap

### PERF-14.1 — Independent work
Concurrency requires operations without unresolved dependencies.

### PERF-14.2 — CPU-GPU overlap
Run host work while the GPU executes.

### PERF-14.3 — Compute-copy overlap
Use asynchronous transfers and copy engines.

### PERF-14.4 — Compute-compute overlap
Run kernels concurrently when hardware resources permit.

### PERF-14.5 — Communication-computation overlap
Hide network or peer communication behind useful work.

### PERF-14.6 — Double buffering
Alternate workspaces to pipeline stages.

### PERF-14.7 — Pipeline depth
Balance concurrency against buffering and synchronization overhead.

### PERF-14.8 — Exposed time
Only non-overlapped work contributes directly to the critical path.

### PERF-14.9 — Overlap efficiency
Measure how much nominally concurrent work is truly hidden.

---

# 15. CPU Affinity and NUMA

### PERF-15.1 — CPU affinity
Bind processes or threads to specific cores.

### PERF-15.2 — Thread migration
Why operating-system movement between cores can hurt locality.

### PERF-15.3 — NUMA locality
Keep CPU execution near the memory it accesses.

### PERF-15.4 — First-touch memory placement
How allocation/touch policy influences NUMA ownership.

### PERF-15.5 — Remote NUMA access
Latency and bandwidth penalties across sockets.

### PERF-15.6 — OpenMP placement
OMP_NUM_THREADS, OMP_PROC_BIND, OMP_PLACES, and thread distribution.

### PERF-15.7 — MPI process binding
Core, socket, NUMA-node, and no-binding strategies.

### PERF-15.8 — CPU helper/progress threads
Why library background threads also need sensible affinity.

### PERF-15.9 — Affinity overconstraint
How overly strict binding can reduce available resources or create contention.

---

# 16. GPU and Device Affinity

### PERF-16.1 — Rank-to-GPU mapping
Ensure each process controls the intended device.

### PERF-16.2 — CPU-to-GPU locality
Place host threads near the GPU's PCIe root or local NUMA node.

### PERF-16.3 — GPU-to-NIC locality
Match accelerators to nearby network interfaces.

### PERF-16.4 — Multi-GPU topology awareness
NVLink/NVSwitch, PCIe switches, sockets, and locality.

### PERF-16.5 — Device visibility
How CUDA_VISIBLE_DEVICES and equivalent mechanisms alter logical numbering.

### PERF-16.6 — Logical versus physical GPU IDs
Why remapped device numbering can confuse topology reasoning.

### PERF-16.7 — Balanced resource mapping
Avoid concentrating ranks, CPU helpers, or network traffic on one locality domain.

### PERF-16.8 — Affinity validation
Verify actual placement rather than trusting launcher intent.

---

# 17. Communication Performance Analysis

### PERF-17.1 — Communication time decomposition
Startup, transfer, software overhead, synchronization, and waiting.

### PERF-17.2 — Small-message behavior
Latency and protocol overhead dominate.

### PERF-17.3 — Large-message behavior
Bandwidth, injection rate, and contention dominate.

### PERF-17.4 — Point-to-point bandwidth
Measure actual link/path throughput.

### PERF-17.5 — Collective bandwidth
Interpret collective performance in relation to topology and algorithm.

### PERF-17.6 — Communication imbalance
Different ranks experience different paths or congestion.

### PERF-17.7 — Synchronization amplification
One slow rank can delay many others.

### PERF-17.8 — Exposed communication
Separate total communication activity from communication on the critical path.

### PERF-17.9 — Fast path versus fallback path
Recognize when GPU-direct or topology-local communication is not being used.

### PERF-17.10 — Microbenchmark versus application behavior
Why a fast benchmark does not guarantee the application communicates efficiently.

---

# 18. Load Balance and Rank Asymmetry

### PERF-18.1 — Computational load imbalance
Different workers receive different amounts of useful work.

### PERF-18.2 — Hardware asymmetry
Different NUMA, GPU, NIC, or topology paths can create unequal performance.

### PERF-18.3 — Runtime skew
Ranks begin or finish phases at different times.

### PERF-18.4 — Barrier amplification
Small upstream differences become visible as large waits at synchronization points.

### PERF-18.5 — Tail latency
Overall progress is often determined by the slowest rank.

### PERF-18.6 — Phase-by-phase comparison
Compare the same region across ranks rather than only total runtime.

### PERF-18.7 — Root cause versus symptom
Waiting ranks may not be the slow ranks causing the wait.

### PERF-18.8 — Correcting imbalance
Redistribute work, improve locality, reduce contention, or change synchronization structure.

---

# 19. Critical-Path Analysis

### PERF-19.1 — Directed acyclic graph view
Represent execution as tasks and dependencies.

### PERF-19.2 — Critical path
The longest dependency chain determining total runtime.

### PERF-19.3 — Parallel slack
Work that can be delayed without affecting completion time.

### PERF-19.4 — Optimize the critical path first
Speeding up non-critical work may not reduce total runtime.

### PERF-19.5 — Bottleneck migration
Once one stage improves, another can become dominant.

### PERF-19.6 — Hidden work versus exposed work
Concurrent activity matters only if it changes the completion path.

### PERF-19.7 — Cross-rank critical paths
Distributed applications can have dependencies spanning multiple ranks and devices.

### PERF-19.8 — Timeline-to-DAG reasoning
Use profiler traces to reconstruct the dependency structure.

---

# 20. Benchmarking Methodology

### PERF-20.1 — Synthetic benchmarks
Measure a specific hardware or software capability in isolation.

### PERF-20.2 — Application benchmarks
Measure realistic end-to-end behavior.

### PERF-20.3 — Microbenchmarks
Small focused tests for latency, bandwidth, kernel throughput, or synchronization.

### PERF-20.4 — Benchmark representativeness
A benchmark is only useful if it stresses the mechanism relevant to the real application.

### PERF-20.5 — Problem-size selection
Small tests may benchmark overhead instead of steady-state performance.

### PERF-20.6 — Saturation tests
Increase problem size or concurrency until a resource reaches its limit.

### PERF-20.7 — Scaling tests
Measure behavior as resource count changes.

### PERF-20.8 — Benchmark contamination
Filesystem activity, initialization, clock changes, competing jobs, and caching effects.

### PERF-20.9 — Benchmark validity
Check correctness, convergence, and stable execution before trusting performance.

---

# 21. Parameter Sweeps

### PERF-21.1 — Why sweep parameters
Many performance relationships are nonlinear and architecture-dependent.

### PERF-21.2 — One-factor-at-a-time sweeps
Simple but limited when parameters interact.

### PERF-21.3 — Grouped conceptual sweeps
Sweep parameters that control one mechanism together.

### PERF-21.4 — Cartesian explosion
Why testing every possible combination quickly becomes infeasible.

### PERF-21.5 — Coarse-to-fine search
Explore broadly first, then refine around promising regions.

### PERF-21.6 — Parameter interactions
Recognize when the best value depends on another setting.

### PERF-21.7 — Stable baselines during sweeps
Avoid changing unrelated variables.

### PERF-21.8 — Re-sweep after major changes
A previous optimum may become invalid when topology, problem size, or another bottleneck changes.

### PERF-21.9 — Transferable insight versus transferable value
Carry the mechanism across machines, not the exact parameter setting.

---

# 22. Experimental Design

### PERF-22.1 — Start with a hypothesis
Every experiment should answer a specific mechanism-level question.

### PERF-22.2 — Independent and dependent variables
Define what changes and what is measured.

### PERF-22.3 — Control variables
Hold unrelated configuration constant.

### PERF-22.4 — Confounding variables
Recognize environmental or coupled effects that can corrupt conclusions.

### PERF-22.5 — Falsifiable predictions
Write what evidence would support or reject the hypothesis.

### PERF-22.6 — Minimal experiment
Use the smallest test that can isolate the suspected mechanism.

### PERF-22.7 — Escalation path
Only move to expensive tracing or broader sweeps when simpler tests cannot distinguish hypotheses.

### PERF-22.8 — Stop conditions
Know when enough evidence has been collected.

### PERF-22.9 — Reproducibility
Store commands, environment, scripts, profiler settings, and result data.

---

# 23. Correlation, Causation, and Counter Interpretation

### PERF-23.1 — Counter correlation
A metric can rise with poor performance without causing it.

### PERF-23.2 — Denominator effects
Percent utilization can increase or decrease because total runtime changed.

### PERF-23.3 — Derived metrics
Understand how profiler percentages and rates are computed.

### PERF-23.4 — Counter multiplexing
Hardware may not measure all counters simultaneously.

### PERF-23.5 — Metric scope
Per-SM, per-device, per-process, per-rank, or whole-system values are not interchangeable.

### PERF-23.6 — Counter saturation
A resource can report high utilization while still being inefficient.

### PERF-23.7 — Use multiple independent signals
Confirm a hypothesis using timing, counters, topology, and scaling behavior.

### PERF-23.8 — Mechanism-first interpretation
Translate every important metric back into a hardware/software event.

---

# 24. HPL/HPL-MxP Performance Analysis

### PERF-24.1 — Separate benchmark phases
Panel factorization, broadcast, TRSM, GEMM, correction, and synchronization.

### PERF-24.2 — Total GFLOP/s versus phase timing
A single performance number hides where time is spent.

### PERF-24.3 — Panel bottleneck
Recognize dependency-heavy work that can starve GPUs.

### PERF-24.4 — GEMM efficiency
Determine whether the high-throughput phase approaches hardware capability.

### PERF-24.5 — Rank asymmetry
Compare timelines and phase durations across ranks.

### PERF-24.6 — Communication exposure
Determine whether broadcast/collective time overlaps effectively with trailing updates.

### PERF-24.7 — CPU-side bottlenecks
Host orchestration, OpenMP, memory placement, and progress threads.

### PERF-24.8 — Process-grid effects
Relate row/column communication patterns to topology.

### PERF-24.9 — Problem-size effects
Memory use, remainder blocks, kernel shapes, and scaling.

### PERF-24.10 — Flag interactions
Treat tuning options as controls over identifiable mechanisms rather than arbitrary switches.

---

# 25. Performance Portability

### PERF-25.1 — Architecture dependence
An optimization can help one GPU generation and hurt another.

### PERF-25.2 — Topology dependence
Single-node and multi-node optimal configurations can differ radically.

### PERF-25.3 — Vendor dependence
NVIDIA, AMD, and Intel expose different hardware and software characteristics.

### PERF-25.4 — Problem-size dependence
Optimal settings may change with matrix size, mesh size, batch size, or workload scale.

### PERF-25.5 — Software-stack dependence
Driver, compiler, MPI, NCCL/communication library, and runtime versions matter.

### PERF-25.6 — Portable mechanisms
Locality, reuse, overlap, load balance, reduced synchronization, and high arithmetic intensity generalize well.

### PERF-25.7 — Non-portable knobs
Exact thread counts, block sizes, process grids, and environment-variable values often require revalidation.

### PERF-25.8 — Retuning strategy
Use prior results to define hypotheses, not to skip measurement.

---

# 26. Optimization Workflow

### PERF-26.1 — Establish the baseline
Measure the unmodified workload carefully.

### PERF-26.2 — Build a system model
Understand hardware topology, software stack, and algorithm structure.

### PERF-26.3 — Profile broadly
Find where time is spent.

### PERF-26.4 — Classify the bottleneck
Compute, memory, latency, synchronization, communication, imbalance, or orchestration.

### PERF-26.5 — Form a hypothesis
State a mechanism and expected observable behavior.

### PERF-26.6 — Design the smallest discriminating experiment
Choose measurements that separate competing explanations.

### PERF-26.7 — Optimize
Change one conceptual mechanism.

### PERF-26.8 — Validate correctness
Ensure the result remains valid.

### PERF-26.9 — Re-measure end-to-end
Verify that the optimization improves total runtime, not just one local metric.

### PERF-26.10 — Re-profile
Check whether the bottleneck moved.

### PERF-26.11 — Record the result
Store commands, environment, data, interpretation, and next action.

### PERF-26.12 — Iterate
Performance engineering is a repeated loop, not a one-time tuning pass.

---

# 27. Reading Performance Results Like an HPC Engineer

### PERF-27.1 — Read a scaling plot
Identify ideal scaling, efficiency loss, and saturation points.

### PERF-27.2 — Read a Roofline plot
Interpret position relative to compute and bandwidth roofs.

### PERF-27.3 — Read a profiler timeline
Find gaps, synchronization, overlap, rank skew, and critical phases.

### PERF-27.4 — Read kernel metrics
Translate utilization and stalls into likely hardware behavior.

### PERF-27.5 — Read a bandwidth benchmark
Distinguish peak link rate from achieved application transfer rate.

### PERF-27.6 — Read a benchmark table
Check problem size, hardware count, precision, software versions, and statistical methodology.

### PERF-27.7 — Challenge a speedup claim
Ask whether the baseline, workload, measurement, and correctness criteria are fair.

### PERF-27.8 — Turn evidence into the next experiment
Every result should either support a mechanism or narrow the hypothesis space.

---

# Completion Goal

After completing this theme, the reader should be able to:

1. Define and measure **wall time, throughput, latency, utilization, efficiency, and scaling** correctly.
2. Distinguish **compute-, memory-, latency-, communication-, synchronization-, and imbalance-limited** behavior.
3. Use **Roofline, Amdahl's Law, and scaling models** as reasoning tools rather than just formulas.
4. Build reproducible benchmarks with controlled variables and statistically sensible measurements.
5. Profile CPU and GPU workloads at the appropriate level of detail.
6. Read GPU timelines and identify **kernel gaps, synchronization, communication exposure, transfer overlap, and rank skew**.
7. Interpret GPU metrics such as **occupancy, warp stalls, memory throughput, cache behavior, register pressure, and matrix-core utilization**.
8. Diagnose **NUMA, CPU affinity, GPU affinity, and GPU↔NIC locality** problems.
9. Separate **total communication time** from communication that is exposed on the critical path.
10. Recognize **load imbalance, tail effects, and synchronization amplification** in distributed applications.
11. Design **parameter sweeps and controlled experiments** around explicit hypotheses rather than blind trial and error.
12. Treat performance counters as evidence requiring interpretation, not as automatic diagnoses.
13. Analyze HPL/HPL-MxP using **phase timing, rank timelines, GEMM efficiency, panel behavior, communication overlap, and process-grid effects**.
14. Transfer **mechanism-level insights** across different machines while revalidating architecture-specific parameter values.
15. Follow a disciplined optimization loop: **baseline → model → profile → hypothesize → test → optimize → validate → re-profile**.
