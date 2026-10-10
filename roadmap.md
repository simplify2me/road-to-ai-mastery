# Road to AI Mastery

> **Version:** 1.0 · **Start:** October 2026 · **Planning horizon:** 24 months · **Capacity:** ~4 focused hours/day, 5–6 days/week (~2,000–2,500 hours).  
> **Mission:** Become a systems-grounded AI engineer capable of deriving core ML methods, implementing models and runtimes from first principles, profiling their performance, and critically reproducing research.  
> **Source:** Extends the original *C Mastery Roadmap*. Its original phases (tooling, collections, allocator, shell, concurrency, HTTP, performance, Redis, storage, indexing, transactions, mini DB, observability, optional distributed systems) are preserved below, but sequenced by relevance to AI.

## 1. What success looks like

By the end, demonstrate—not merely claim—that you can:

1. Explain the mathematics of linear models, backpropagation, attention, optimization, and probabilistic inference, and derive their central equations.
2. Build and benchmark memory-safe C data structures, a custom allocator, concurrent/network services, and a compact storage engine.
3. Explain Python's object model, reference counting, GIL implications, NumPy memory layout, and the C/Python boundary; write and benchmark a C extension.
4. Implement a CPU tensor library, reverse-mode autodiff, training loops, a small transformer, and a basic LLM inference runtime.
5. Profile CPU cache behavior, numerical error, GPU memory traffic, kernel performance, and end-to-end inference throughput.
6. Reproduce a published ML result at a manageable scale with transparent methods, baselines, uncertainty, and limitations.
7. Publish readable repos, tests, benchmark protocols, experiment logs, and technical notes documenting tradeoffs.

**Meaning of “mastery”:** This is a roadmap to substantial practical fluency and research readiness, *not* a guarantee of frontier-research expertise in 24 months. The capstone can support a strong portfolio but does not replace evaluation of engineering quality, scope, and results.

## 2. Priority, constraints, and time budget

| Priority | Track | Depth | Why it matters |
|---|---|---|---|
| 1 | C + systems | Deep, hands-on | Memory, concurrency, performance, storage and runtime design |
| 2 | Data structures | Deep, scale-aware | Layout, asymptotic cost, locality and measurement |
| 3 | Mathematics | Daily from day one | Reason about models instead of memorizing APIs |
| 4 | Python | Targeted depth | Understand the runtime boundary used by ML tooling |
| 5 | AI/ML + PyTorch + GPU | Increasing depth | Build, train, profile and serve models |
| 6 | JS/Node | Maintenance only | Retain professional competence without taking study time |

### Daily rhythm (4 hours)

- **60 min math:** derivations + exercises; do not replace pencil work with videos.
- **120 min implementation:** C initially; progressively Python/ML/CUDA.
- **40 min tests/benchmark/debug:** record evidence, not impressions.
- **20 min retrieval + lab journal:** explain yesterday's concept from memory and record the next action.

**Weekly:** 5 full study days + one flexible 2–4 hour consolidation day + one rest day. For a 20-hour week, reduce implementation time before reducing math frequency. Every fourth week: reserve ~25% of time for review, refactoring, and unfinished tests. Hours are estimates, not deadlines.

### Rules that prevent the plan from stalling

- **One primary build at a time.** Math is the only continuously parallel track.
- **Time-box secondary systems projects.** The AI-specific build must begin by months 7–9 even if a mini DB is incomplete.
- **A benchmark needs context:** hardware, compiler flags, dataset, warmup, repetitions, percentile/variance, correctness check, and comparison baseline.
- **Billion-scale is a *test of reasoning*, not a universal requirement.** Use 1K → 1M → 10M → 100M → 1B only when memory, runtime, and semantics permit; never allocate beyond available RAM. Report complexity and bytes per item.
- **Read selectively.** Complete exercises and code that support current milestones; references are not books to finish cover-to-cover.
- **Recovery protocol:** after an interruption, read `NOW.md`, run the last green test, repeat one small exercise, and resume the next 30-minute task. No restarting the whole course.

## 3. Dependency map

```text
C + data layout ──> collections ──> allocator ──> concurrency/networking
       │                │                              │
       └────────────────┴────> CPU tensor kernels <────┘
                                      │
Math: linear algebra ─> matrix calculus ─> autodiff ─> neural nets
          │                 │                           │
          └─> probability ──┴─> statistics/optimization ─> transformers
Python object model ─> NumPy/PyTorch ─> training experiments ────┘
                                                        │
Numerical LA ─> GPU architecture/CUDA ─> kernels ─> LLM inference
                                                        │
                                         paper reproduction + capstone
```

## 4. The 24-month execution roadmap

**Phase gates matter more than dates.** The months below assume steady ~20–24 h/week; some phases overlap *only* for the daily math track and limited AI experiments.

| Window | Primary build / focus | Math focus | Gate / published artifact |
|---|---|---|---|
| Months 1–3 | C collections, tests, benchmarking, allocator basics | Applied linear algebra; visual intuition; start matrix calculus | Tested library + scale/performance report; implement matrix-vector multiply |
| Months 4–6 | Allocator; shell; thread pool; HTTP server (scoped) | Probability; gradients; numerical differentiation | Concurrent HTTP service with reproducible throughput/latency tests |
| Months 7–9 | Redis clone + *minimal* storage/index project; Python/C extension; begin CPU tensor library | Statistics, numerical linear algebra, optimization | C extension + CPU matmul benchmark; first tensor operations |
| Months 10–12 | CPU tensor library + reverse-mode autodiff; micrograd-style NN; PyTorch comparison | Matrix calculus in depth; ML fundamentals | Autodiff passes gradient checks; MLP trains on a real dataset |
| Months 13–15 | Transformer from scratch, tokenization, small-model training, nanoGPT-style implementation | Cross-entropy, KL, attention, statistical evaluation | Reproducible tiny language-model training run and ablations |
| Months 16–18 | GPU programming, CUDA or platform-equivalent kernels, profiling | Conditioning, floating-point stability, optimization | Naive/tiled matmul and attention benchmarks vs vendor libraries |
| Months 19–21 | LLM inference engine: tokenizer, weights, transformer forward pass, KV cache, batching, quantization | Sampling, numerical error, performance modeling | Runs a small open-weight Llama-like model; verified outputs and measured tokens/sec |
| Months 22–24 | Research reproduction; capstone hardening; contribution(s); technical writing | Selected papers + deeper theory gaps | Replication report, documented inference runtime, public portfolio |

**Hardware constraint:** macOS Apple Silicon is excellent for C/CPU work and PyTorch MPS, but **CUDA requires an NVIDIA GPU**. At months 16–18, obtain an NVIDIA machine/cloud instance for CUDA work; otherwise learn GPU concepts through Metal/MPS while explicitly marking CUDA milestones as pending. Do not claim CUDA results from Apple Silicon.

### Scope guardrails

- **Must finish:** collections, allocator essentials, concurrency/networking essentials, CPU tensor/autodiff, transformer, GPU experiments, inference capstone.
- **Time-box / reduce:** full shell job control, production Redis compatibility, comprehensive SQL, full transaction isolation, distributed systems.
- **Stretch only:** full mini DB with WAL/crash recovery, Raft/Kafka clone, sophisticated inference scheduler, FlashAttention reproduction.

## 5. First 12 weeks: executable plan

**Existing work counts.** If vector/deque/hashmap implementations already pass the gates, do not rebuild them. Audit and benchmark them, then move forward.

| Week | C/systems implementation | Math (60 min daily) | End-of-week evidence |
|---|---|---|---|
| 1 | Audit repo; CMake + cmocka; ASan/UBSan; baseline vector tests and benchmarks | Vectors, dot products, norms; 3Blue1Brown episodes 1–3; Applied LA opening chapters | Green CI/local test script; vector benchmark protocol |
| 2 | Vector growth, insert/remove edge cases, overflow handling; benchmark contiguous vs shifting access | Linear combinations, matrix-vector multiplication, geometric transformations | Written explanation of O(1) amortized append vs O(n) insertion |
| 3 | Deque/ring buffer: wraparound, invariants, memory layout, fuzz/property tests | Span, linear independence, bases | Randomized correctness suite and locality notes |
| 4 | Hashmap: chaining, resize, collisions, pointer ownership, profiling | Matrix multiplication, transpose, rank | Hash quality/load-factor sweep; month-1 review |
| 5 | Linked lists, stack, queue; choose array vs pointer representations | Orthogonality, projections, least squares | Benchmark linked vs contiguous traversal; derive least-squares geometry |
| 6 | Heap/priority queue, BST; complexity and invariant tests | QR intuition, solving linear systems | Heap property tests; solve least-squares problem in Python/NumPy |
| 7 | B+ tree/skip list minimal implementations; compare search and range scan | Eigenvectors/eigenvalues as transformations | Index correctness suite; worked eigen examples |
| 8 | Union-find/graph essentials; consolidate libcollections; document ownership | SVD, low-rank approximation; review | Versioned collections library; scale report with memory footprint |
| 9 | Allocator v1: arena, alignment, metadata, allocate/free | Differentials and chain rule refresher; Parr & Howard opening sections | Aligned allocations, zero/large-size tests, sanitizer run |
| 10 | Allocator v2: free lists, splitting, coalescing, fragmentation | Jacobians, gradients, shape conventions | Heap invariant checker; fragmentation experiment |
| 11 | Allocator v3: realloc/calloc, overflow and failure handling; compare malloc | Backpropagation through linear and nonlinear maps | Allocator test matrix; finite-difference gradient checks |
| 12 | Integrate allocator optionally into collections; profile; publish retrospective | Matrix calculus review; derive gradient of MSE regression | First milestone report, clean tagged release, month-3 plan |

### Weekly acceptance checklist

- [ ] I can explain the core concept without reading notes.
- [ ] I can derive the important complexity or mathematical result.
- [ ] I wrote correctness tests, including adversarial and boundary cases.
- [ ] I collected repeatable measurements (or documented why not applicable).
- [ ] I wrote a 200–500 word engineering note on the hardest issue.
- [ ] `NOW.md` states the next concrete action.

### Your first seven study sessions

1. Inventory existing vector/deque/hashmap code; identify which tests already exist; create `NOW.md`.
2. Run sanitizer-enabled build; fix one concrete bug or add a regression test.
3. Measure vector append and middle insertion at several sizes; record CPU and memory.
4. Derive amortized growth cost for your 1.5× vector strategy; explain `memmove` cost.
5. Audit deque indexing and wraparound with a reference-model randomized test.
6. Derive dot product and matrix-vector multiplication; implement both in C and compare with NumPy.
7. Write a one-page retrospective; select week-2's single biggest uncertainty.

## 6. Mathematics: reading order, exercises, and coding

**Do not study every listed text cover-to-cover.** The reading path is designed for usable mathematical fluency and increasingly rigorous explanations.

| Stage | Primary resource | How to use it | Required demonstration |
|---|---|---|---|
| M1–3 | Boyd & Vandenberghe, *Introduction to Applied Linear Algebra* | Main exercise text; focus on vectors, matrices, least squares, linear independence, QR | Implement projections, least squares, and matrix operations; explain shape and geometry |
| M1–3 | 3Blue1Brown, *Essence of Linear Algebra* | Watch before/alongside related chapters, never as exercise replacement | Draw and explain linear transforms, basis change, eigenvectors |
| M2–4 | Strang, *Linear Algebra and Its Applications* + MIT 18.06 | Use selectively for alternate intuition and problem sets | Explain column space/null space and rank from multiple viewpoints |
| M3–4 | Parr & Howard, *The Matrix Calculus You Need for Deep Learning* | Work through derivatives with explicit shape checks | Derive gradients for affine layer, MSE, sigmoid/tanh |
| M4+ | *The Matrix Cookbook* | Lookup reference only; verify formulas and conventions | Re-derive a looked-up identity independently |
| M4–6 | Blitzstein & Hwang, *Introduction to Probability* + Stat 110 | Main probability text; do exercises | Simulate conditional probability, Bayes, expectation, variance, LLN/CLT |
| M6–12 | Murphy, *Probabilistic Machine Learning: An Introduction* | Main ML-math reference alongside experiments | Derive logistic regression, likelihood, regularization, basic graphical-model ideas |
| M7–10 | Wasserman, *All of Statistics* | Selected inference, estimation, confidence intervals, tests | Evaluate experiments with uncertainty and leakage checks |
| M9–15 | MacKay, *Information Theory, Inference, and Learning Algorithms* | Selected entropy, coding, KL, inference | Derive cross-entropy and KL; measure language-model perplexity |
| M9–18 | Trefethen & Bau, *Numerical Linear Algebra* | Selected stability, conditioning, factorization | Demonstrate ill-conditioning and float32 error experimentally |
| M10–18 | Boyd & Vandenberghe, *Convex Optimization* | Select chapters 1–5 and 9; apply to ML | Explain convexity, constraints, duality basics, gradient/Newton methods |
| M12+ | Nocedal & Wright, *Numerical Optimization* | Reference for line search, quasi-Newton, stochastic optimization context | Compare SGD, momentum, Adam experimentally; distinguish Adam from classical methods |
| Later | Axler, *Linear Algebra Done Right* | Deepen linear maps, invariant subspaces, spectral reasoning | Prove selected theorems; explain diagonalization assumptions |
| Optional | Hubbard & Hubbard, *Vector Calculus, Linear Algebra, and Differential Forms* | Use for multivariate rigor when needed | Derive multivariable chain rule carefully |

**Important prerequisite correction:** Parr & Howard is short, but it presupposes basic single-variable differentiation and the chain rule. Spend a few sessions refreshing these before matrix calculus if necessary. Probability and calculus can run sequentially in the math hour; do not add an unbudgeted second math track.

### Mathematical checkpoint problems

- **Linear algebra:** Derive normal equations and explain why QR/SVD can be numerically preferable to explicitly computing \((A^TA)^{-1}\).
- **Matrix calculus:** For \(Y=XW+b\) and scalar loss \(L\), derive \(\partial L/\partial W=X^T(\partial L/\partial Y)\) with shape annotations.
- **Probability:** Derive Bayes' rule; explain likelihood vs posterior and conditional independence.
- **Statistics:** Explain train/validation/test splits, data leakage, uncertainty, and calibration.
- **Optimization:** Derive gradient descent; explain conditioning and why learning rates matter.
- **Information theory:** Derive cross-entropy, KL divergence, and perplexity relationships.
- **Numerics:** Show a reproducible float32 vs float64 error and distinguish roundoff from algorithmic instability.

## 7. Systems foundation: preserved and prioritized from the original C roadmap

Each project includes **what it teaches → minimum build → acceptance gate → AI connection**.

### S0. Tooling and debugging (weeks 1–2)

- CMake, compiler warnings (`-Wall -Wextra -Wpedantic`), cmocka, ASan/UBSan, debugger, CI; use `lldb` on macOS and `gdb`/`valgrind`/`perf`/`strace` in Linux where supported.
- Understand stack/heap, pointers, structs, function pointers, compilation/linking, alignment, object lifetime.
- **Gate:** reproduce and fix an out-of-bounds write, use-after-free, and leak in a test sandbox.

### S1. `libcollections` (weeks 1–8)

- **Linear:** vector, stack, queue, block deque, ring buffer.
- **Linked:** singly/doubly linked list.
- **Hash:** separate-chaining hashmap with resize and explicit key/value ownership.
- **Trees/index:** BST, B+ tree, skip list, binary heap/priority queue.
- **Graph:** graph representation, union-find.
- **Gate:** invariant tests, randomized comparison against a simple reference implementation, benchmarks, complexity table, allocation accounting.
- **Scale:** start with your existing 1.5× vector, block deque, and chaining hashmap. Record their existing large-scale performance as a baseline, not as proof of universal scalability.
- **AI bridge:** tensor storage layout, hash-based token tables, batching queues, cache locality.

### S2. `libmemory` allocator (weeks 9–12)

- Implement `my_malloc`, `my_free`, `my_realloc`, `my_calloc`; free list, alignment, split/coalesce, overflow handling, fragmentation metrics.
- **Gate:** deterministic and randomized alloc/free tests; verify no overlaps, correct alignment, valid reuse, and realloc preservation.
- **AI bridge:** tensor arenas, scratch buffers, KV-cache allocation.

### S3. `shell` (months 4–5; ~2 weeks)

- `fork/exec/wait`, signals, file descriptors, pipes, redirection, basic parsing; run Linux VM/container if needed.
- **Gate:** execute commands and pipelines, handle errors and child cleanup. Full interactive job control is optional.
- **AI bridge:** understand process isolation and training/inference launch mechanics.

### S4. `threadpool` (months 4–5; ~2 weeks)

- Work queue, worker lifecycle, mutexes, condition variables, shutdown, races, deadlock/starvation, backpressure.
- **Gate:** ThreadSanitizer where supported; stress tests with task loss/duplication checks.
- **AI bridge:** CPU parallel kernels, model-serving worker pools.

### S5. `http-server` (months 5–6; ~3 weeks)

- TCP sockets, bind/listen/accept, request parsing, single-threaded → thread-per-connection → pool; event-driven `epoll` on Linux or `kqueue` on macOS.
- **Gate:** serve concurrent requests with bounded memory, basic HTTP correctness, load-test latency p50/p95/p99 and throughput.
- **AI bridge:** inference serving, request queues, batching, backpressure.

### S6. Performance and observability (continuous)

- CPU caches, sequential/random access, branch effects, SIMD awareness, flame graphs, logging, metrics, tracing.
- **Gate:** one optimization writeup must show before/after *and* identify the bottleneck and tradeoff; use platform-appropriate profiling tools.
- **AI bridge:** matmul optimization and latency attribution.

### S7. `redis-clone` (months 7–8; ~3 weeks)

- Hash-backed `SET/GET/DEL/EXPIRE`, RESP subset, event loop, TTL; optional skip-list sorted sets, LRU eviction, snapshots/AOF.
- **Gate:** protocol tests, concurrent clients, TTL correctness, load-test report. Do not implement every Redis command.
- **AI bridge:** cache management, hot keys, serving infrastructure.

### S8–S11. Storage → indexing → transactions → `mini-db` (months 8–9; strict time box)

- **Storage:** fixed-size pages, serialization, page manager, buffer pool.
- **Index:** B+ tree search/insert/delete/range scan; split/merge tests.
- **Transactions:** WAL, crash recovery, shared/exclusive locking, ACID tradeoffs (stretch).
- **Mini DB:** simple `CREATE/INSERT/SELECT/UPDATE/DELETE`, `WHERE`, basic index; transaction syntax only if transaction engine is actually implemented.
- **Gate:** persist and reopen records; randomized index tests; crash-recovery tests only if WAL is implemented. Explicitly label unsupported isolation/SQL features.
- **AI bridge:** feature stores, vector index tradeoffs, model metadata, persistent KV caches.
- **Scope decision:** stop after a functional paged KV/B+ tree if behind schedule. A production-like SQL engine is not a prerequisite for neural networks.

### S12. Observability (continuous)

- Structured logs, throughput/latency/memory metrics, tracing and production debugging; integrate into all systems.

### S13. Distributed systems (optional after capstone)

- Replication, leader election, Raft, sharding, consistency/availability; mini Kafka, mini Raft, distributed cache are **future specializations**, not requirements for the two-year AI path.

## 8. Python: scoped depth (months 7–10)

- Python semantics: references vs values, mutability, object identity, classes, iterators, generators, exceptions, packaging, profiling.
- CPython model: `PyObject`, reference counting, cyclic GC, GIL and its version-specific caveats; why native NumPy/PyTorch operations can execute outside Python-level loops.
- NumPy: ndarray shape, dtype, strides, views vs copies, broadcasting, contiguity, BLAS dispatch.
- C extension: implement a vector or matrix operation with Python bindings (CPython API or a documented binding layer); benchmark vs Python loop and NumPy.
- **Gate:** unit tests, correct reference ownership, no leaks, benchmark and explanation of when native code wins/loses.
- **Stop line:** do **not** build a Python interpreter or study its full compiler/runtime internals.

## 9. AI-specific project ladder

### A1. CPU tensor library (months 9–11)

**Implement:** tensor metadata (shape, strides, dtype), allocation/ownership, indexing/views, broadcasting (incrementally), elementwise ops, reductions, matmul; start with contiguous float32 2D matrices before general N-D tensors.

**Tests:** shape/error cases, NumPy numerical comparisons with tolerances, aliasing/ownership tests, sanitizer runs.

**Benchmarks:** naive triple-loop → loop reordering → blocked/tiled matmul → optional SIMD/multithreading; compare with optimized BLAS on the same hardware. Track GFLOP/s, memory bandwidth and error.

**Gate:** repeatable correctness and performance report; explain why BLAS is faster, not just how much.

### A2. Reverse-mode autodiff + micrograd-style network (months 11–12)

**Implement:** computational graph, topological reverse traversal, local derivatives, gradient accumulation, broadcasting gradients, MSE, activations, SGD; extend from scalars to tensors.

**Gate:** finite-difference gradient checks on small problems; fit a toy regression and classify a small real dataset; compare gradients with PyTorch.

### A3. PyTorch from first principles (months 12–13)

**Implement:** dataset/dataloader, training/eval loops, autograd inspection, optimizer state, checkpointing, seeding, metrics, device transfer.

**Gate:** replicate A2 with PyTorch; report convergence, numerical differences, and profiler findings.

### A4. Transformer / nanoGPT-style training (months 13–15)

**Implement:** tokenization, embeddings, positional information, causal masking, scaled dot-product attention, multi-head attention, layer normalization, feedforward blocks, residuals, cross-entropy, AdamW, autoregressive sampling.

**Gate:** overfit one tiny batch as a correctness check, then train a tiny language model; track train/validation loss and perplexity, checkpoints, throughput, and reproducibility. Explain leakage, masking and parameter counts.

### A5. GPU kernels (months 16–18)

**Implement:** device buffers/transfers, naive matmul, tiled matmul, reductions/softmax; optional fused attention. On NVIDIA use CUDA; on Apple hardware use Metal for conceptual practice and reserve CUDA-specific benchmarks for NVIDIA.

**Gate:** correctness against reference, measured latency/throughput for several shapes, profiler evidence, compare with cuBLAS/vendor libraries, document why optimized vendor kernels may still win.

### A6. LLM inference runtime (months 19–21)

**Implement in stages:** (1) load weights of a specifically chosen small open-weight architecture; (2) tokenizer compatibility; (3) RMSNorm/rotary positions/attention/MLP; (4) autoregressive decoding; (5) KV cache; (6) sampling; (7) quantization; (8) basic batching and HTTP serving. Begin CPU-only and add GPU kernels selectively.

**Gate:** output parity or bounded numerical error against a trusted reference on known prompts; generate coherent text; report prefill vs decode latency, tokens/sec, peak memory, model size, context length, and concurrency. A Llama-like model is not identical to every Llama-family checkpoint: match its exact configuration and tokenizer.

### A7. Paper reproduction + final capstone (months 22–24)

- Choose a *tractable* paper (e.g., optimizer, attention variant, small architecture component) whose experiment can be run with available compute.
- Record hypothesis, datasets, baselines, implementation, random seeds, hardware, hyperparameters, evaluation, uncertainty and deviations from the paper.
- Publish a failure analysis if the reproduction does not match the claimed result.
- Harden inference runtime: tests, setup scripts, reproducible benchmark harness, architecture diagrams, clear limitations and a demo.
- **Stretch:** small upstream contribution to tinygrad, ggml, llm.c, vLLM, or another codebase you genuinely understand; contributions are opportunities, not guaranteed acceptance.

## 10. Research competence: habits and milestones

**Monthly paper routine from month 10:** read one relevant paper in three passes: (1) question and contribution; (2) equations and assumptions; (3) implementation/evaluation details. Write a one-page critique with a proposed falsification test.

**Research questions to learn to ask:** What is the baseline? Which variable changed? Is the result statistically and practically meaningful? Is compute matched? Are data and evaluation contaminated? Does the result generalize across seeds, datasets, and model sizes? Which tradeoffs were hidden?

**Research readiness gate:** independently reproduce at least one bounded result, identify its limitations, and propose a well-controlled follow-up. This is the starting point for research, not proof of independent frontier-level research.

## 11. Repository structure

```text
road-to-ai-mastery/
├── README.md
├── ROADMAP.md                 # this document
├── NOW.md                     # current state and next 30-minute action
├── progress/                  # weekly reviews, phase gates, study log
├── math/                      # worked derivations, notebooks, implementations
├── c/
│   ├── libcollections/
│   ├── libmemory/
│   ├── shell/
│   ├── threadpool/
│   ├── http-server/
│   ├── redis-clone/
│   ├── storage-engine/
│   └── mini-db/
├── python/
│   ├── runtime-notes/
│   └── c-extension/
├── ai/
│   ├── tensor-cpu/
│   ├── autodiff/
│   ├── pytorch-baselines/
│   ├── transformer/
│   ├── gpu-kernels/
│   └── inference-engine/
├── research/
│   ├── paper-notes/
│   └── reproduction/
├── benchmarks/
└── writing/
```

**Repo hygiene:** small commits with clear intent; README with build/run/test commands; license where appropriate; automated tests; benchmark raw results; reproducible environment specs; no copyrighted weights/datasets committed without permission.

## 12. Evidence portfolio: publish the hard parts

| When | Artifact | Specific claim it substantiates |
|---|---|---|
| M3 | `libcollections` + benchmarking note | Understands data layout, complexity, allocation and scaling |
| M6 | Thread pool + HTTP load-test report | Can design and measure concurrent systems |
| M9 | C extension + storage/index note | Can bridge runtimes and reason about persistence |
| M11 | Naive vs tiled CPU matmul vs BLAS | Can measure and explain CPU performance |
| M12 | Autodiff + gradient-check notebook | Understands backpropagation beyond framework calls |
| M15 | Tiny transformer training report | Can implement and evaluate training systems |
| M18 | GPU kernel vs vendor library report | Understands GPU memory hierarchy and optimization |
| M21 | LLM inference engine + benchmark suite | Can build and operate a model runtime |
| M24 | Reproduction report + capstone writeup | Can turn a paper into a testable implementation |

**Writing format:** question → initial hypothesis → implementation → test method → surprising result → diagnosis → revised implementation → tradeoffs → next experiment. A transparent result is more credible than an exaggerated speedup.

**Career note:** artifacts improve credibility, but hiring also depends on communication, collaboration, relevant experience, role fit, and interview performance. Don't assume a single capstone guarantees a job or outweighs all formal credentials.

## 13. Metrics dashboard and phase gates

Track weekly:

| Metric | How to record |
|---|---|
| Focused hours | Actual math / implementation / verification hours |
| Mathematical fluency | Problems solved without notes; derivations reproduced a week later |
| Code correctness | Tests passing; sanitizer findings; gradient-check errors |
| Performance | Baseline vs optimized, hardware, input size, repeated trials |
| Learning depth | One concept explained and one misconception corrected |
| Deliverable progress | Shipped artifact, blocker, next smallest task |
| Sustainable pace | Rest days and whether the schedule is realistic |

**Phase-gate questions:** Can I implement it without a tutorial? Can I explain why it works? Can I identify when it fails? Can I measure its cost? Can someone else reproduce my result?

## 14. Monthly review template

```markdown
# Month N Review

## Shipped
- 

## Mathematics I can derive from memory
- 

## Evidence: correctness and performance
- Hardware / software environment:
- Test coverage / failure cases:
- Benchmark and baseline:

## Hardest bug or conceptual misunderstanding
- 

## Scope decisions: finish, reduce, defer
- 

## Next month's one primary build
- 

## First task for tomorrow (<=30 minutes)
- 
```

## 15. If you fall behind: explicit tradeoffs

1. Keep daily math and correctness testing.
2. Preserve tensor/autodiff/transformer/inference milestones.
3. Reduce shell, Redis and mini-DB features before sacrificing AI time.
4. Prefer a complete CPU inference engine over an unfinished GPU engine.
5. Prefer one rigorous paper reproduction over five superficial ones.
6. Extend the timeline if needed; don't label an incomplete capability mastered.

## 16. Master checklist

### Foundations
- [ ] Reproducible C build, tests, sanitizers and debugger workflow
- [ ] Vector, deque, hashmap: audited and benchmarked
- [ ] Remaining collections: invariant-tested and documented
- [ ] Allocator: alignment, free list, coalescing, realloc, failure tests
- [ ] Processes/pipes/signals basics
- [ ] Thread pool and synchronization
- [ ] HTTP networking, concurrency and backpressure
- [ ] Profiling, logging, latency/throughput reporting
- [ ] Redis subset and minimal paged storage/B+ index
- [ ] WAL/mini SQL extension **(optional)**

### Mathematics
- [ ] Linear transformations, bases, rank, orthogonality, least squares
- [ ] Eigenvalues/SVD and conditioning
- [ ] Chain rule, Jacobians, backprop gradients
- [ ] Conditional probability, expectation, distributions
- [ ] Estimation, evaluation uncertainty and leakage
- [ ] Gradient-based optimization and regularization
- [ ] Entropy, cross-entropy, KL and perplexity
- [ ] Floating-point and numerical stability experiments
- [ ] Deeper Axler/convex optimization material **(optional)**

### Python and AI
- [ ] Python object model and native extension
- [ ] NumPy shape/stride/view/contiguity fluency
- [ ] CPU tensor library and BLAS benchmark
- [ ] Reverse-mode autodiff with gradient checks
- [ ] PyTorch training, checkpointing and profiling
- [ ] Tiny transformer with reproducible training
- [ ] GPU kernels and vendor-library comparisons
- [ ] Inference runtime with tokenizer, KV cache and sampling
- [ ] Quantization/batching with documented correctness/performance
- [ ] One paper reproduction and critical report
- [ ] Public, documented capstone with reproducible results

## 17. Next action

**Start with an audit, not a restart.** Create `NOW.md`; list the existing vector, deque and hashmap implementation/test/benchmark status; run all current tests under sanitizers; then begin week 1's linear algebra exercises. Your first milestone is a reliable foundation you can build on—not another fresh repository.

---

*This roadmap is a living engineering plan. Update it at monthly reviews based on evidence, not novelty or pressure to consume more resources.*
