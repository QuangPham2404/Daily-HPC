# Theme 3 — Dense Linear Algebra + HPL / HPL-MxP

**Theme ID:** `HPL`  
**Priority:** P1  
**Purpose:** Build a first-principles understanding of dense numerical linear algebra and the algorithms behind HPL and HPL-MxP, from matrix operations and floating-point arithmetic to distributed LU factorization, mixed precision, iterative refinement, and algorithmic performance trade-offs.

## Scope

This curriculum focuses on:

- core matrix and vector operations used in HPC;
- BLAS levels and dense linear algebra kernels;
- floating-point arithmetic and precision;
- LU factorization and Gaussian elimination;
- pivoting and numerical stability;
- blocked matrix algorithms;
- GEMM, TRSM, GEMV, and panel factorization;
- arithmetic intensity and data reuse at the algorithm level;
- distributed dense matrices and block-cyclic layouts;
- process grids and HPL algorithm structure;
- mixed-precision linear algebra;
- iterative refinement;
- HPL-MxP algorithmic structure and low-precision acceleration;
- mathematical and algorithmic trade-offs in optimizing HPL/HPL-MxP.

Detailed GPU hardware architecture belongs primarily to **Theme 1 — GPU + CPU-GPU Architecture**.

Detailed MPI/NCCL/NVSHMEM and distributed communication mechanisms belong primarily to **Theme 2 — Distributed / Multi-GPU HPC**.

Detailed profiling methodology and system-level performance tuning belong primarily to **Theme 4 — Performance Engineering**.

## Progression Rule

Follow the curriculum in order unless there is a strong reason to revisit an earlier topic.

A daily volume should normally combine **2–4 adjacent, tightly related topics** into one coherent lesson. Dense mathematical topics may occupy a full volume by themselves.

---

# 1. Linear Algebra Foundations

### HPL-1.1 — Scalars, vectors, and matrices
Dimensions, indexing, notation, and how numerical data is represented.

### HPL-1.2 — Matrix addition and scalar multiplication
Element-wise operations and their computational cost.

### HPL-1.3 — Dot products
The fundamental multiply-accumulate operation behind many dense kernels.

### HPL-1.4 — Matrix-vector multiplication
How rows, columns, and dot products form GEMV-like computation.

### HPL-1.5 — Matrix-matrix multiplication
How matrix multiplication decomposes into many dot products and why it dominates dense HPC.

### HPL-1.6 — Transpose
Why row/column orientation and transposition matter for storage and computation.

### HPL-1.7 — Identity, diagonal, triangular, and permutation matrices
Special matrix structures used throughout factorization algorithms.

### HPL-1.8 — Systems of linear equations
The meaning of solving \(Ax=b\) and why direct solvers factor the matrix first.

---

# 2. Computational Cost and Data Movement

### HPL-2.1 — Operation counts
Count additions, multiplications, and fused operations in matrix kernels.

### HPL-2.2 — FLOPs and FLOP/s
Distinguish algorithmic work from measured execution rate.

### HPL-2.3 — Why matrix multiplication is \(O(n^3)\)
Derive the cubic operation count.

### HPL-2.4 — Why matrix storage is \(O(n^2)\)
Relate problem size to memory capacity.

### HPL-2.5 — Compute work versus data movement
Why algorithms with the same FLOP count can have very different performance.

### HPL-2.6 — Arithmetic intensity
Operations performed per byte moved.

### HPL-2.7 — Data reuse
Why reusing matrix blocks from fast memory is central to high-performance dense linear algebra.

### HPL-2.8 — Surface-to-volume intuition
Why larger blocks often improve compute-to-communication ratio.

---

# 3. Floating-Point Arithmetic

### HPL-3.1 — Real numbers versus floating-point numbers
Why computers represent only a finite subset of real values.

### HPL-3.2 — Floating-point format structure
Sign, exponent, significand/mantissa, and normalized representation.

### HPL-3.3 — Common HPC precisions
FP64, FP32, TF32-like formats, BF16, FP16, FP8-class formats, and their trade-offs.

### HPL-3.4 — Machine epsilon
The spacing between representable floating-point numbers near 1.

### HPL-3.5 — Rounding
Round-to-nearest and the origin of rounding error.

### HPL-3.6 — Overflow and underflow
Finite dynamic range and what happens outside it.

### HPL-3.7 — Non-associativity
Why \((a+b)+c\) may differ from \(a+(b+c)\) in floating-point arithmetic.

### HPL-3.8 — Error accumulation
How many small floating-point errors propagate through long computations.

### HPL-3.9 — Fused multiply-add
Why FMA can improve both throughput and numerical behavior.

### HPL-3.10 — Precision versus speed
Why lower precision can dramatically increase throughput but reduce numerical robustness.

---

# 4. BLAS Foundations

### HPL-4.1 — What BLAS is
The Basic Linear Algebra Subprograms as standard computational building blocks.

### HPL-4.2 — BLAS Level 1
Vector-vector operations and their memory-bound nature.

### HPL-4.3 — BLAS Level 2
Matrix-vector operations and limited data reuse.

### HPL-4.4 — BLAS Level 3
Matrix-matrix operations and high arithmetic intensity.

### HPL-4.5 — AXPY
The canonical Level-1 operation \(y \leftarrow \alpha x + y\).

### HPL-4.6 — GEMV
General matrix-vector multiplication.

### HPL-4.7 — GEMM
General matrix-matrix multiplication.

### HPL-4.8 — TRSM
Triangular solve with multiple right-hand sides.

### HPL-4.9 — Why Level-3 BLAS is fast
Blocking, reuse, and high compute intensity.

### HPL-4.10 — Vendor BLAS libraries
cuBLAS, rocBLAS, oneMKL, BLIS, OpenBLAS, and why tuned kernels matter.

---

# 5. Gaussian Elimination

### HPL-5.1 — The elimination idea
Transforming a linear system into triangular form.

### HPL-5.2 — Elimination step by step
Use one pivot row to eliminate entries below the pivot.

### HPL-5.3 — Forward elimination
Building an upper-triangular system.

### HPL-5.4 — Back substitution
Solving the final upper-triangular system.

### HPL-5.5 — Elimination matrices
Representing each elimination step as a matrix operation.

### HPL-5.6 — Computational complexity
Why dense Gaussian elimination requires approximately \(\frac{2}{3}n^3\) FLOPs.

### HPL-5.7 — Data dependencies
Which parts of the matrix depend on the current pivot and which parts can proceed independently.

---

# 6. LU Factorization

### HPL-6.1 — What LU factorization is
Factor \(A\) into lower- and upper-triangular matrices.

### HPL-6.2 — Relationship between Gaussian elimination and LU
How elimination multipliers form \(L\) and transformed rows form \(U\).

### HPL-6.3 — Solving \(Ax=b\) using LU
Forward solve followed by backward solve.

### HPL-6.4 — In-place LU storage
How \(L\) and \(U\) can share the original matrix storage.

### HPL-6.5 — Outer-product view of LU
Express trailing updates as rank-1 matrix updates.

### HPL-6.6 — Right-looking LU
Factor a panel, then update the entire trailing matrix.

### HPL-6.7 — Left-looking LU
Accumulate previous updates before factoring the current block.

### HPL-6.8 — Crout-style variants
Alternative decomposition ordering and data-access implications.

### HPL-6.9 — LU dependency graph
Identify panel work, triangular solves, and trailing updates.

---

# 7. Pivoting and Numerical Stability

### HPL-7.1 — Why pivoting is needed
Small pivots can amplify rounding error.

### HPL-7.2 — Partial pivoting
Select the largest-magnitude pivot from the current column.

### HPL-7.3 — Row permutations
Permutation matrices and how pivoting changes matrix row order.

### HPL-7.4 — Complete pivoting
Searching rows and columns and why it is more expensive.

### HPL-7.5 — Growth factor
How intermediate values can become much larger than the original matrix entries.

### HPL-7.6 — Stability versus performance
Why numerically safer algorithms may introduce extra communication and synchronization.

### HPL-7.7 — Pivot search as a parallel bottleneck
Why finding and exchanging pivots can create dependencies in distributed LU.

### HPL-7.8 — Tournament and communication-avoiding pivoting
Alternative strategies for reducing synchronization and communication.

---

# 8. Blocked LU Factorization

### HPL-8.1 — Why block the matrix
Convert low-intensity scalar/vector work into higher-intensity matrix operations.

### HPL-8.2 — Matrix partitioning
Panel, diagonal block, row block, column block, and trailing matrix.

### HPL-8.3 — Panel factorization
Factor the next block-column and determine pivots.

### HPL-8.4 — Triangular solve
Use the factored panel to solve for neighboring matrix blocks.

### HPL-8.5 — Trailing matrix update
Update the remaining matrix using GEMM.

### HPL-8.6 — Block size
Why block size controls a trade-off between panel overhead, GEMM efficiency, locality, and parallelism.

### HPL-8.7 — Panel width versus GEMM work
How larger blocks reduce panel frequency but alter cache/memory and concurrency behavior.

### HPL-8.8 — Critical path
Why the next panel must become ready before factorization can advance.

### HPL-8.9 — Look-ahead
Start the next panel while the current trailing update is still running.

### HPL-8.10 — Recursive panel factorization
Use nested blocking to improve cache locality and parallelism inside panels.

---

# 9. GEMM as the Performance Engine

### HPL-9.1 — Why GEMM dominates dense LU FLOPs
Most arithmetic is performed in trailing matrix updates.

### HPL-9.2 — Tiled matrix multiplication
Break matrices into blocks to improve locality.

### HPL-9.3 — Register tiling
Reuse matrix fragments inside registers.

### HPL-9.4 — Cache/shared-memory tiling
Stage reusable blocks in faster memory.

### HPL-9.5 — GEMM loop ordering
How loop permutations alter reuse and memory traffic.

### HPL-9.6 — Matrix packing
Why high-performance BLAS libraries reorganize data before computation.

### HPL-9.7 — Microkernels
Small architecture-specific inner kernels at the heart of optimized GEMM.

### HPL-9.8 — Tensor/matrix-core GEMM
How specialized matrix hardware changes the ideal kernel structure.

### HPL-9.9 — GEMM shape effects
Why square, tall-skinny, and short-wide matrix multiplications have different efficiency.

### HPL-9.10 — Peak versus sustained GEMM performance
Why real kernels do not achieve theoretical peak continuously.

---

# 10. Distributed Dense Matrices

### HPL-10.1 — Why distribute a matrix
Memory capacity and parallel execution across multiple devices/nodes.

### HPL-10.2 — One-dimensional distributions
Row-wise and column-wise partitioning.

### HPL-10.3 — Two-dimensional process grids
Arrange processes as \(P \times Q\).

### HPL-10.4 — Block distribution
Assign contiguous blocks to processes.

### HPL-10.5 — Cyclic distribution
Cycle ownership to balance work.

### HPL-10.6 — Block-cyclic distribution
Combine locality with load balance.

### HPL-10.7 — Mapping global matrix indices to ranks
Determine which process owns a given matrix block.

### HPL-10.8 — Local matrix storage
How each rank stores only its owned blocks.

### HPL-10.9 — Distribution granularity
How block size affects load balance, local efficiency, and communication frequency.

---

# 11. Parallel LU Factorization

### HPL-11.1 — Distributed panel ownership
Which rank owns and factors each panel.

### HPL-11.2 — Pivot exchange across ranks
How pivot information and rows are communicated.

### HPL-11.3 — Panel broadcast
Why other ranks need the factored panel.

### HPL-11.4 — Distributed TRSM
How ranks compute the row/column blocks needed for the trailing update.

### HPL-11.5 — Distributed trailing update
Each rank updates its local matrix blocks independently once dependencies arrive.

### HPL-11.6 — Communication/computation overlap
Overlap panel communication with GEMM work.

### HPL-11.7 — Process-grid effects
Why \(P \times Q\) changes message counts, local matrix shapes, and communication paths.

### HPL-11.8 — Load balance
Why matrix dimensions, block sizes, and process-grid shapes affect how evenly work is distributed.

### HPL-11.9 — Parallel critical path
Identify the work that fundamentally limits forward progress.

---

# 12. HPL Fundamentals

### HPL-12.1 — What HPL measures
Dense double-precision LU performance on a distributed system.

### HPL-12.2 — The HPL benchmark problem
Generate and solve a dense linear system.

### HPL-12.3 — Problem size \(N\)
How matrix dimension determines memory use and total FLOPs.

### HPL-12.4 — Block size \(NB\)
How HPL's algorithmic block size affects panel work and BLAS efficiency.

### HPL-12.5 — Process grid \(P \times Q\)
How HPL maps MPI ranks into a two-dimensional grid.

### HPL-12.6 — Process mapping order
Row-major versus column-major rank mapping.

### HPL-12.7 — Panel factorization variants
Different approaches to factoring distributed panels.

### HPL-12.8 — Broadcast algorithms
Why HPL provides multiple broadcast strategies.

### HPL-12.9 — Look-ahead depth
How HPL overlaps panels and trailing matrix updates.

### HPL-12.10 — Residual check
How HPL verifies that the computed solution is numerically acceptable.

### HPL-12.11 — HPL FLOP count
Understand the benchmark's expected operation count and GFLOP/s calculation.

---

# 13. HPL Parameter Reasoning

### HPL-13.1 — Choosing \(N\)
Balance memory utilization, runtime, and matrix shape.

### HPL-13.2 — Memory footprint of HPL
Estimate matrix storage and per-rank memory requirements.

### HPL-13.3 — Choosing \(NB\)
Trade off panel efficiency, GEMM performance, synchronization, and memory behavior.

### HPL-13.4 — \(N \bmod NB\)
Why remainder blocks can change work balance and kernel shapes.

### HPL-13.5 — Choosing \(P\) and \(Q\)
Trade off communication distance, local matrix dimensions, and topology.

### HPL-13.6 — Grid aspect ratio
Why square-ish grids are often—but not always—good.

### HPL-13.7 — Rank ordering
Why logical process-grid order can interact with physical hardware topology.

### HPL-13.8 — Panel/broadcast interaction
Why the best factorization strategy depends on the communication environment.

### HPL-13.9 — HPL tuning as a coupled problem
Why individual parameters cannot always be optimized independently.

---

# 14. Mixed-Precision Numerical Computing

### HPL-14.1 — Why use mixed precision
Perform expensive computation cheaply while recovering high-precision accuracy.

### HPL-14.2 — Storage precision versus compute precision
Matrices may be stored, multiplied, accumulated, and corrected at different precisions.

### HPL-14.3 — Low-precision GEMM
Why matrix hardware provides much higher throughput at reduced precision.

### HPL-14.4 — Accumulation precision
Why multiplication precision and accumulation precision are separate design choices.

### HPL-14.5 — Quantization and rounding error
How lower precision changes representable values and error.

### HPL-14.6 — Dynamic range
Why exponent range matters separately from mantissa precision.

### HPL-14.7 — Scaling
Rescale values to use low-precision formats more safely.

### HPL-14.8 — Mixed-precision factorization
Perform the bulk factorization in a lower precision.

### HPL-14.9 — Accuracy recovery
Use higher-precision residuals/corrections to reach the desired solution quality.

---

# 15. Iterative Refinement

### HPL-15.1 — Basic iterative refinement
Solve approximately, compute the residual, correct the solution, and repeat.

### HPL-15.2 — Residual computation
Evaluate \(r=b-Ax\) accurately enough to detect remaining error.

### HPL-15.3 — Correction solve
Solve for an error correction using the existing factorization.

### HPL-15.4 — Convergence
Why refinement succeeds for some matrices and precision combinations but not others.

### HPL-15.5 — Condition number
A measure of how sensitive a linear system is to perturbations.

### HPL-15.6 — Conditioning versus numerical stability
Distinguish a difficult mathematical problem from an unstable algorithm.

### HPL-15.7 — Precision hierarchy in refinement
Factorization precision, working precision, residual precision, and solution precision.

### HPL-15.8 — Performance trade-off
Why a much faster low-precision factorization can outweigh the cost of a few refinement steps.

---

# 16. HPL-MxP Fundamentals

### HPL-16.1 — Why HPL-MxP exists
Use mixed precision and modern matrix hardware to measure accelerated dense linear algebra performance.

### HPL-16.2 — HPL versus HPL-MxP
What remains similar and what changes algorithmically.

### HPL-16.3 — Low-precision factorization path
Where reduced-precision compute enters the algorithm.

### HPL-16.4 — High-precision correction path
How accuracy is recovered after low-precision computation.

### HPL-16.5 — Matrix-engine utilization
Why HPL-MxP is designed around high-throughput tensor/matrix hardware.

### HPL-16.6 — Panel work versus matrix-core work
Why some parts of the algorithm benefit less from low-precision acceleration.

### HPL-16.7 — Communication under faster compute
Why making GEMM much faster can expose communication and panel bottlenecks more strongly.

### HPL-16.8 — HPL-MxP as a heterogeneous algorithm
CPU work, GPU work, communication, low-precision kernels, and high-precision correction interact.

---

# 17. Low-Precision Kernel Design

### HPL-17.1 — Matrix multiplication with low-precision inputs
How reduced-precision operands increase throughput.

### HPL-17.2 — Mixed-precision accumulation
Accumulate products in a higher precision than the inputs.

### HPL-17.3 — Tile shapes
Why matrix hardware operates on specific fragment/tile dimensions.

### HPL-17.4 — Data layout
Row-major, column-major, packed, interleaved, and hardware-friendly layouts.

### HPL-17.5 — Conversion overhead
Precision conversion can become nontrivial when the main compute is extremely fast.

### HPL-17.6 — Scaling and normalization kernels
Prepare data for safe low-precision execution.

### HPL-17.7 — Batched and fused operations
Reduce launch and data-movement overhead by combining small operations.

### HPL-17.8 — Kernel shape efficiency
Why dimensions aligned with tile sizes and block sizes can produce much better throughput.

---

# 18. Communication-Avoiding and Algorithmic Optimization

### HPL-18.1 — Why communication is more expensive than arithmetic
Modern systems can execute FLOPs faster than they can move data.

### HPL-18.2 — Communication lower bounds
The idea that algorithms have fundamental data-movement requirements.

### HPL-18.3 — Communication-avoiding LU
Reorganize factorization to reduce messages or transferred data.

### HPL-18.4 — Tall-skinny QR and communication avoidance
A related example showing how algebraic reformulation reduces communication.

### HPL-18.5 — Replication versus communication
Use additional memory to reduce communication.

### HPL-18.6 — 2.5D and 3D matrix algorithms
Use replicated data and additional process dimensions to reduce communication volume.

### HPL-18.7 — Algorithmic blocking for hierarchy
Choose different block sizes at different levels of CPU/GPU/network hierarchy.

### HPL-18.8 — Reducing synchronization
Why fewer global dependencies can improve scalability.

### HPL-18.9 — Computation/communication trade-offs
Sometimes doing extra arithmetic is worthwhile if it reduces communication.

---

# 19. Matrix Shapes and Work Decomposition

### HPL-19.1 — Square versus rectangular kernels
How matrix shape affects available parallelism and kernel efficiency.

### HPL-19.2 — Tall-skinny matrices
Why panel-like matrix shapes are harder to optimize than large square GEMMs.

### HPL-19.3 — Small GEMM inefficiency
Why tiny matrix multiplications often underutilize GPUs.

### HPL-19.4 — Edge/remainder blocks
What happens when matrix dimensions are not divisible by algorithmic tile sizes.

### HPL-19.5 — Load imbalance from remainders
How uneven block ownership can create rank asymmetry.

### HPL-19.6 — Padding
Trade extra computation/memory for cleaner kernel shapes.

### HPL-19.7 — Chunking
Split large work into pieces to improve overlap or pipeline depth.

### HPL-19.8 — Granularity
Fine-grained versus coarse-grained work decomposition.

---

# 20. Critical-Path Analysis of HPL/HPL-MxP

### HPL-20.1 — DAG view of factorization
Represent the algorithm as tasks and dependencies.

### HPL-20.2 — Panel as serial/weakly parallel work
Understand why the panel can limit scalability.

### HPL-20.3 — Trailing update as massively parallel work
Why GEMM is easier to scale efficiently.

### HPL-20.4 — Look-ahead dependency
Exactly what must finish before the next panel can proceed.

### HPL-20.5 — Communication on the critical path
When panel broadcast or pivot communication directly delays progress.

### HPL-20.6 — GPU starvation
How very fast trailing kernels can wait for slow panel/communication stages.

### HPL-20.7 — Rank skew
How asymmetric task completion propagates through later synchronization points.

### HPL-20.8 — Bottleneck migration
Why optimizing one phase can reveal a new dominant phase.

---

# 21. Mathematical Correctness and Validation

### HPL-21.1 — Residuals
Measure how well the computed solution satisfies the original equations.

### HPL-21.2 — Forward error
Difference between the computed and exact solution.

### HPL-21.3 — Backward error
How much the original problem would need to change for the computed solution to be exact.

### HPL-21.4 — Norms
Vector and matrix norms used to quantify errors.

### HPL-21.5 — Condition number
Relate perturbations in data to changes in the solution.

### HPL-21.6 — Stability of LU with partial pivoting
Why the standard algorithm is usually reliable but not mathematically infallible.

### HPL-21.7 — Mixed-precision failure modes
When low precision or refinement can fail to recover acceptable accuracy.

### HPL-21.8 — Performance without correctness is meaningless
Why benchmark optimization must preserve validation criteria.

---

# 22. From Algorithm to Hardware

### HPL-22.1 — Map LU phases to hardware
Panel, TRSM, GEMM, pivoting, communication, and refinement on CPUs/GPUs.

### HPL-22.2 — Compute-bound phases
Identify operations that can approach matrix-engine throughput.

### HPL-22.3 — Memory-bound phases
Identify operations dominated by memory traffic rather than arithmetic.

### HPL-22.4 — Latency-sensitive phases
Identify small, dependency-heavy kernels where startup and synchronization dominate.

### HPL-22.5 — Communication-bound phases
Identify work limited by network or GPU interconnect behavior.

### HPL-22.6 — CPU-sensitive phases
Understand why host-side orchestration and panel work can matter even in a GPU benchmark.

### HPL-22.7 — Precision-sensitive phases
Identify where low precision is acceptable and where higher precision is required.

### HPL-22.8 — Build an algorithm-to-hardware bottleneck map
Associate every major HPL/HPL-MxP phase with its likely limiting resource.

---

# 23. HPL/HPL-MxP Optimization Reasoning

### HPL-23.1 — Separate algorithmic and implementation parameters
Understand which settings change mathematical decomposition and which change execution details.

### HPL-23.2 — Parameter interactions
Why \(N\), block size, process grid, topology, communication, and kernel behavior interact.

### HPL-23.3 — Change one conceptual mechanism at a time
Design sweeps around hypotheses rather than arbitrary flag combinations.

### HPL-23.4 — Use transferable insights
Carry forward mechanism-level understanding without copying an old machine's optimal parameters.

### HPL-23.5 — Detect phase shifts
Recognize when a previously minor stage becomes dominant after another stage is optimized.

### HPL-23.6 — Relate runtime to algorithmic work
Ask whether a slowdown comes from more work, slower kernels, more communication, or poorer overlap.

### HPL-23.7 — Interpret non-monotonic behavior
Why larger \(N\), larger blocks, or more GPUs do not always improve performance.

### HPL-23.8 — Optimization as constrained co-design
Treat algorithm, precision, decomposition, communication, and hardware topology as one coupled system.

---

# 24. Reading Dense Linear Algebra Research Like an HPC Engineer

### HPL-24.1 — Read a dense linear algebra paper
Identify problem, algorithm, complexity, precision, hardware, baseline, and claimed speedup.

### HPL-24.2 — Separate algorithmic novelty from implementation novelty
Determine whether a result comes from mathematics, scheduling, kernel engineering, communication, or hardware.

### HPL-24.3 — Interpret FLOP/s claims
Check precision, operation definition, problem size, and theoretical peak.

### HPL-24.4 — Interpret speedup correctly
Ask what baseline is used and whether total solution time or only a kernel is measured.

### HPL-24.5 — Evaluate scaling plots
Distinguish strong scaling, weak scaling, and raw problem-size growth.

### HPL-24.6 — Evaluate numerical accuracy
Check residuals, condition numbers, convergence, and accepted tolerance.

### HPL-24.7 — Reproduce the core argument
Translate the paper into a simple dependency/data-movement model.

### HPL-24.8 — Form a testable optimization hypothesis
Turn a research insight into an experiment that could be tested on a real HPL/HPL-MxP system.

---

# Completion Goal

After completing this theme, the reader should be able to:

1. Understand the core mathematics of **vectors, matrices, matrix multiplication, linear systems, Gaussian elimination, and LU factorization**.
2. Explain why dense linear algebra performance is dominated by **data reuse, blocking, and BLAS Level-3 operations**.
3. Understand the roles of **GEMM, GEMV, TRSM, panel factorization, and trailing matrix updates**.
4. Explain **partial pivoting, conditioning, numerical stability, residuals, and floating-point error** at a practical HPC level.
5. Derive the approximate operation count and memory scaling of dense LU.
6. Explain why blocked LU converts much of the work into high-throughput **GEMM**.
7. Understand **block-cyclic matrix distribution**, process grids, and the distributed dependency structure of LU.
8. Explain the main HPL parameters \(N\), \(NB\), \(P\), \(Q\), process mapping, panel strategy, and look-ahead.
9. Understand why **mixed precision** can dramatically accelerate dense linear algebra and how **iterative refinement** recovers accuracy.
10. Explain the conceptual differences between **HPL and HPL-MxP**.
11. Understand how low-precision matrix kernels, conversion/scaling, panel work, communication, and refinement interact.
12. Reason about **critical paths, panel bottlenecks, communication hiding, GPU starvation, rank skew, and bottleneck migration**.
13. Connect mathematical structure to hardware limits: **compute, memory bandwidth, latency, communication, precision, and CPU orchestration**.
14. Read an HPL/HPL-MxP paper, result, or optimization study and identify whether the improvement is primarily **mathematical, algorithmic, communication-related, kernel-level, or architectural**.
15. Form **hypothesis-driven optimization experiments** instead of tuning parameters blindly.
