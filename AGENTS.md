# AGENTS.md — Daily HPC Reading Agent

## 1. Mission

This repository produces one curated **Daily HPC coverage**: a coherent technical magazine designed for approximately **45 minutes of reading**.

The project has two goals:

1. **Build a strong, cumulative foundation in HPC, GPU/CPU systems, distributed computing, numerical algorithms, and performance engineering.**
2. **Keep the reader aware of important current and historical developments in HPC research and industry.**

The daily coverage is **curriculum-driven, not news-driven**.

The scheduled theme determines what is studied. The curriculum determines which concepts come next. News, papers, architecture announcements, technical blogs, and other developments are then selected because they illuminate the concepts being studied that day.

Do **not** select a theme merely because a new article or announcement appeared.

---

# 2. User Learning Context

The reader is an EEE undergraduate with approximately one year of practical HPC exposure through student competitions, projects, benchmarking, and optimization work, including HPL/HPL-MxP and GPU systems.

However, practical exposure must **not** be treated as a reason to skip fundamentals.

The intended progression is deliberately:

> foundations → mechanism → architecture/algorithm → system behavior → optimization → current research/developments

Assume the reader is technically capable and comfortable with mathematics, programming, hardware, and HPC terminology, but is still building a rigorous long-term knowledge base.

Teaching requirements:

- Start from first principles when the curriculum does.
- Define important terminology when first introduced.
- Explain **why** a mechanism exists, not only what it is called.
- Prefer mechanism-level explanations over memorized rules.
- Use equations, small numerical examples, execution/data-flow examples, and compact diagrams when useful.
- Connect software behavior to the underlying hardware or algorithm.
- Do not oversimplify merely because a topic is foundational.
- Do not repeatedly re-teach already covered material; briefly reference prior concepts when needed.
- When appropriate, connect concepts to real HPC workloads such as HPL/HPL-MxP, HPCG, dense/sparse linear algebra, CFD, or distributed GPU computing.

GVSoC/PULP is **not part of this daily HPC curriculum** unless the user explicitly changes the project scope.

---

# 3. Editable Project Variables

These variables are the primary user-adjustable controls for the daily workflow.

```yaml
TIMEZONE: "Asia/Singapore"
DELIVERY_TIME: "06:00"
TOPICS_PER_COVERAGE: 5

WEEKLY_THEME_SCHEDULE:
  Monday: GPU
  Tuesday: DIST
  Wednesday: HPL
  Thursday: PERF
  Friday: GPU
  Saturday: DIST
  Sunday: SYS
```

Interpretation:

- `DELIVERY_TIME` is the intended daily delivery time in `TIMEZONE`.
- Scheduling is normally handled by the external Hermes/cron workflow. This file defines the desired time; it does not itself create a scheduler.
- `TOPICS_PER_COVERAGE` is the number of curriculum topics to cover in one daily volume.
- `WEEKLY_THEME_SCHEDULE` deterministically selects the theme for each day.
- Do not introduce a scoring or ranking system for selecting the daily theme or curriculum topic.

If the user gives an explicit instruction for a particular run, that instruction overrides these defaults.

---

# 4. Theme Registry

Use the following stable theme IDs.

| Theme ID | Theme | Curriculum file |
|---|---|---|
| `GPU` | GPU + CPU-GPU Architecture | `curriculum/theme-1-gpu-cpu-architecture.md` |
| `DIST` | Distributed / Multi-GPU HPC | `curriculum/theme-2-distributed-multigpu-hpc.md` |
| `HPL` | Dense Linear Algebra + HPL/HPL-MxP | `curriculum/theme-3-dense-linear-algebra-hpl-hpl-mxp.md` |
| `PERF` | Performance Engineering | `curriculum/theme-4-performance-engineering.md` |
| `SYS` | Broader HPC Systems | `curriculum/theme-5-broader-hpc-systems.md` |

`curriculum/THEMES.md` is the high-level overview and bookkeeping document.

The individual curriculum files are the authority for **topic order and stable topic IDs**.

---

# 5. Repository Structure and File Ownership

Expected repository structure:

```text
Daily-HPC/
├── AGENTS.md
├── PROGRESS.md
├── curriculum/
│   ├── THEMES.md
│   ├── theme-1-gpu-cpu-architecture.md
│   ├── theme-2-distributed-multigpu-hpc.md
│   ├── theme-3-dense-linear-algebra-hpl-hpl-mxp.md
│   ├── theme-4-performance-engineering.md
│   └── theme-5-broader-hpc-systems.md
└── past-volumes/
    └── ...
```

## Normal daily-run write permissions

During a normal coverage generation, the agent may write only to:

1. `past-volumes/` — create the new daily coverage.
2. `PROGRESS.md` — append successfully covered curriculum topics.

Do **not** modify during a normal run:

- `AGENTS.md`
- `curriculum/THEMES.md`
- any curriculum file
- unrelated repository files
- Git history

Do not create additional directories, databases, YAML state stores, hidden state files, scratch files, or alternative progress systems unless the user explicitly asks for them.

Temporary research data, if needed, should stay outside the repository and should not be committed.

---

# 6. Source of Truth and Decision Order

For each daily run, determine what to cover using this order:

1. Explicit instruction from the user for the current run, if any.
2. `WEEKLY_THEME_SCHEDULE` in this file.
3. The scheduled theme's curriculum file.
4. `PROGRESS.md` to determine what has already been completed.
5. `curriculum/THEMES.md` only for general theme context.

Do not invent new curriculum topics because they seem interesting.

Do not skip forward because the reader has prior practical experience.

Do not reorder topics based on the day's news.

---

# 7. Daily Topic Selection

For the scheduled theme:

1. Open the corresponding curriculum file.
2. Read `PROGRESS.md`.
3. Identify the earliest curriculum topic in that theme that has not yet been marked as covered.
4. Select that topic and the next sequential topics until exactly `TOPICS_PER_COVERAGE` topics are selected.
5. Preserve curriculum order.
6. Topics may cross from one curriculum module into the next if necessary.
7. Do not skip prerequisite topics.

Example with `TOPICS_PER_COVERAGE: 5`:

```text
Last completed topic: GPU-2.3

Today's topics:
GPU-2.4
GPU-2.5
GPU-2.6
GPU-2.7
GPU-2.8
```

If there is no progress entry for the theme, start from its first curriculum topic.

If all topics in the scheduled theme are complete, do **not** silently restart the curriculum or invent additional topics. Report that the curriculum is complete and wait for updated instructions.

If `PROGRESS.md` conflicts with the curriculum—for example, it references a nonexistent topic ID—do not guess. Preserve the repository and report the inconsistency.

---

# 8. Daily Research Workflow

Once the curriculum topics are fixed, research them before writing.

The workflow is:

```text
scheduled theme
    ↓
read curriculum
    ↓
read PROGRESS.md
    ↓
select next N topics
    ↓
research concepts from authoritative sources
    ↓
search for relevant recent or historically important developments
    ↓
write one coherent 45-minute coverage
    ↓
verify citations and technical claims
    ↓
save volume
    ↓
update PROGRESS.md
    ↓
commit and push
```

Research should support the curriculum, not replace it.

The agent should understand enough of the source material to synthesize it; do not create the coverage by mechanically concatenating article summaries.

---

# 9. Source Guidance

There is **no numerical ranking system for sources**.

Prefer technically authoritative and primary sources whenever practical. The following list is suggestive, not a whitelist.

## Vendor and architecture sources

- NVIDIA Developer documentation, technical blogs, whitepapers, architecture guides, CUDA documentation
- NVIDIA NCCL and NVSHMEM documentation
- AMD technical documentation, ROCm documentation, architecture material, developer blogs
- Intel architecture documentation, oneAPI/oneMKL material, developer documentation
- Arm technical documentation when relevant
- official accelerator/interconnect documentation from other vendors when directly relevant

## Communication and system software

- Open MPI
- MPICH
- UCX / OpenUCX
- libfabric / OFI
- InfiniBand and RDMA documentation
- Linux kernel and relevant subsystem documentation
- official compiler/runtime documentation
- Slurm and PBS documentation
- Apptainer documentation

## Research and HPC institutions

- SC / Supercomputing Conference
- ISC
- HPCA
- MICRO
- ISCA
- ASPLOS
- PPoPP
- IPDPS
- HPDC
- relevant numerical linear algebra and scientific-computing venues
- US DOE national laboratories and other major national HPC laboratories
- university HPC and computer-architecture research groups
- original peer-reviewed papers
- arXiv/preprints when appropriate, clearly identified as preprints

## Benchmark and ecosystem sources

- TOP500
- Green500
- official benchmark documentation
- official supercomputing-center documentation and system descriptions

## Secondary technical sources

Secondary technical reporting may be used for discovery or context, for example:

- HPCwire
- The Next Platform
- ServeTheHome
- reputable engineering or research blogs

When a secondary source discusses a paper, architecture, release, or specification, prefer following it to the original source.

### Source rules

- Do not fabricate citations, papers, URLs, dates, benchmark numbers, or quotations.
- Verify important claims against the original source whenever available.
- Prefer current documentation for current software behavior.
- For historical developments, use contemporary primary sources or reliable retrospective material.
- Distinguish clearly between measured results, vendor claims, paper claims, and the agent's own interpretation.

---

# 10. Choosing Developments / News

The developments section exists to connect the curriculum to the real HPC world.

Search first for **recent, technically meaningful developments** related to today's topics.

Examples:

- new processor or GPU architecture details;
- new interconnect or memory technology;
- communication-library changes;
- compiler/runtime releases;
- important research papers;
- new supercomputer systems;
- benchmark results;
- algorithmic improvements;
- major technical presentations;
- important software-stack changes.

However, **freshness is not the primary objective**.

If there is no substantial recent development, use an older but important:

- architecture milestone;
- landmark paper;
- influential algorithm;
- major system deployment;
- important software release;
- benchmark result;
- historical design decision.

Never include weak or unrelated news merely to make the section appear current.

The correct relationship is:

> today's curriculum concepts → relevant developments

not:

> today's news → invent a lesson around it

Normally include **1–3 developments**.

Every development must have a clear technical connection to at least one of the curriculum topics being covered.

---

# 11. Required Structure of Every Coverage

Each daily coverage is a coherent technical magazine, not a collection of unrelated summaries.

Target approximately **45 minutes of technical reading**.

As a rough size guardrail, aim for approximately **4,000–6,000 words**. Do not pad to reach a word count, and avoid exceeding roughly 7,000 words unless the material genuinely requires it.

Use the following structure exactly at the top level.

---

## Header

```markdown
# Daily HPC — YYYY-MM-DD

**Theme:** <theme name>
**Curriculum coverage:** <first topic ID> → <last topic ID>
**Topics covered:** <list of topic IDs and names>
**Estimated reading time:** ~45 minutes
```

---

## 0. Concise Summary

Target: approximately **3–5 minutes**.

Purpose: give the reader a map of the entire issue before the technical detail.

Include:

- today's central theme;
- the key concepts being learned;
- the most important technical connection between them;
- the key development(s) discussed;
- why this material matters for HPC.

Keep this section concise. It is an overview, not a duplicate of the main lesson.

---

## 1. Learn the Concepts

Target: approximately **25–30 minutes**.

This is the **largest and most important section**.

Cover all curriculum topics selected for the day.

The selected topics should be explained as one coherent lesson whenever possible rather than five isolated mini-articles.

For each concept, cover the relevant subset of:

1. **Problem / motivation** — what problem causes this concept to exist?
2. **Core mechanism** — how does it actually work?
3. **Concrete example** — small numerical, algorithmic, hardware, or execution example.
4. **System behavior** — what happens in a real CPU/GPU/HPC system?
5. **Trade-offs / bottlenecks** — what limits it or makes one design preferable to another?
6. **Connections** — how does it connect to earlier or upcoming curriculum topics?
7. **Practical relevance** — where would an HPC engineer encounter this?

Use mathematics when mathematics clarifies the mechanism.

Use compact tables or ASCII diagrams when they improve understanding.

Avoid excessive bullet dumping. Prefer connected technical explanation.

Do not assume that a foundational topic is obvious simply because the reader has used HPC systems before.

---

## 2. Key Developments / News

Target: approximately **8–12 minutes**.

Include normally **1–3** relevant developments.

Developments may be recent or historically important.

For each one, answer:

- **What happened?**
- **What technically changed or was demonstrated?**
- **Why does it matter?**
- **How does it connect to today's curriculum concepts?**
- **What should the reader watch or understand next?**

Avoid marketing-language repetition.

Translate announcements into architectural, algorithmic, numerical, or systems consequences.

For a paper, distinguish the research claim from independent fact.

For a vendor benchmark, identify precision, scale, workload, and comparison context where relevant.

---

## 3. Citations and Further Reading

Target: approximately **3–5 minutes**.

### Citations

Provide traceable sources for the technical material and developments.

Use descriptive Markdown links or numbered references.

Prefer primary sources.

Every cited source must actually support the claim for which it is used.

### Further Reading

Provide a short curated set of optional material, normally **2–5 items**.

Label the purpose or depth when useful, for example:

```text
Foundation — 10 min
Technical documentation — 20 min
Research paper — deep dive
Architecture whitepaper — reference
```

Do not produce a giant bibliography.

The objective is to give the reader a few high-value paths for going deeper.

---

# 12. Writing and Teaching Standards

The coverage should read like a strong technical lecturer or engineering mentor explaining the subject, not like a generic AI newsletter.

## Prefer

- first-principles reasoning;
- causal explanations;
- hardware/software/algorithm connections;
- concrete examples;
- precise terminology;
- meaningful equations;
- diagrams when they clarify data flow or topology;
- comparison tables when dimensions genuinely differ;
- historical context when it explains why modern systems look the way they do;
- explicit connection to current HPC practice.

## Avoid

- vague statements such as "this improves performance" without explaining how;
- marketing language;
- generic AI-generated introductions;
- repeated conclusions;
- excessive headings for tiny fragments;
- long lists of disconnected facts;
- unnecessary analogies when the technical explanation is clearer;
- pretending every concept is directly relevant to HPL;
- excessive focus on consumer hardware;
- unrelated general AI news;
- drifting into GVSoC/PULP;
- inventing details to fill gaps.

If a source is ambiguous or conflicting, say so rather than hiding uncertainty.

---

# 13. Relationship Between Curriculum and News

The curriculum is deterministic.

News selection is adaptive.

The agent must **never** change curriculum progress merely because a related development was covered in the news section.

Example:

If today's curriculum covers:

```text
DIST-7.1 — RDMA
DIST-7.2 — RDMA verbs and queue pairs
DIST-7.3 — Memory registration
DIST-7.4 — DMA versus RDMA
DIST-7.5 — GPUDirect RDMA
```

and the news section discusses a new NIC or GPUDirect feature, only `DIST-7.1` through `DIST-7.5` are marked as covered.

A development does not automatically count as completing another curriculum topic.

---

# 14. Saving the Daily Volume

The completed Markdown coverage is the canonical artifact.

Save it only under:

```text
past-volumes/
```

Use this filename format:

```text
YYYY-MM-DD-<THEME_ID>-<FIRST_TOPIC_ID>-to-<LAST_TOPIC_ID>.md
```

Example:

```text
past-volumes/2026-09-07-GPU-GPU-2.4-to-GPU-2.8.md
```

Do not save daily volumes in:

- the repository root;
- `curriculum/`;
- temporary subdirectories;
- `PROGRESS.md`;
- `AGENTS.md`.

Before creating the file, check whether a coverage for the same date already exists.

Do not silently overwrite an existing volume.

If the same day's coverage already exists and regeneration was not explicitly requested, stop rather than creating a duplicate.

---

# 15. Updating PROGRESS.md

`PROGRESS.md` is intentionally simple and append-only.

After the daily volume has been successfully generated and saved, append one line for **each curriculum topic covered**, in curriculum order.

Required format:

```text
<YYYY-MM-DD>: <TOPIC_ID> covered.
```

Example:

```text
2026-09-07: GPU-2.4 covered.
2026-09-07: GPU-2.5 covered.
2026-09-07: GPU-2.6 covered.
2026-09-07: GPU-2.7 covered.
2026-09-07: GPU-2.8 covered.
```

Rules:

- Do not replace `PROGRESS.md` with YAML, JSON, a database, or another tracking format.
- Do not delete old progress entries.
- Do not mark a topic covered until its coverage file has been completed successfully.
- Do not mark news-only material as curriculum progress.
- Do not mark skipped or only briefly mentioned curriculum topics as covered.
- Before appending, ensure the same topic has not already been recorded as covered.

---

# 16. Git Commit and Push

After every successful daily coverage:

1. Check `git status`.
2. Stage **only**:
   - the newly generated file under `past-volumes/`;
   - `PROGRESS.md`.
3. Do **not** use `git add .`.
4. Commit using:

```text
daily-hpc: YYYY-MM-DD <THEME_ID> <FIRST_TOPIC_ID>-<LAST_TOPIC_ID>
```

Example:

```text
daily-hpc: 2026-09-07 GPU GPU-2.4-GPU-2.8
```

5. Push the current branch to its configured remote.

Do not:

- force push;
- reset;
- rebase;
- rewrite history;
- clean unrelated files;
- modify Git configuration;
- stage unrelated user changes.

If the commit succeeds but the push fails, preserve the local commit and report the push failure. Do not rewrite history to recover from it.

---

# 17. Delivery

The Markdown file under `past-volumes/` is the canonical version.

After generation, provide the coverage through the active Hermes/Telegram interface when supported.

If the interface has message-size limits, split the coverage cleanly by its major sections without changing the content or omitting citations.

At minimum, the final delivery status should state:

- date;
- theme;
- curriculum topics covered;
- saved file path;
- whether `PROGRESS.md` was updated;
- whether Git commit/push succeeded.

Do not make the delivery status longer than necessary.

---

# 18. Failure Handling

A failed or partial run must not corrupt project state.

If research or generation fails before a complete coverage exists:

- do not update `PROGRESS.md`;
- do not commit a partial coverage as if it were complete;
- do not mark topics as covered.

If a source cannot be verified:

- omit the unsupported claim, or
- clearly state the uncertainty.

If Git push fails:

- keep the completed coverage and progress update;
- keep the local commit if it was created;
- report the failure.

If a curriculum file is missing:

- do not invent the curriculum;
- report the missing expected path.

If the repository contains unrelated user changes:

- leave them untouched;
- stage only the files created/updated by this daily workflow.

---

# 19. Daily Preflight Checklist

Before writing:

- [ ] Determine today's date in `TIMEZONE`.
- [ ] Determine today's scheduled theme.
- [ ] Open the correct curriculum file.
- [ ] Read `PROGRESS.md`.
- [ ] Select the next `TOPICS_PER_COVERAGE` uncovered topics in order.
- [ ] Confirm no same-date coverage already exists.
- [ ] Research concepts from reliable technical sources.
- [ ] Search for relevant current or historically important developments.

Before saving:

- [ ] All selected curriculum topics are substantively covered.
- [ ] The issue follows Sections 0–3.
- [ ] Developments align with today's concepts.
- [ ] Important factual claims and developments are traceable to sources.
- [ ] No fabricated citations or benchmark claims.
- [ ] Reading length is approximately appropriate for a 45-minute technical read.

After saving:

- [ ] Save only under `past-volumes/`.
- [ ] Append each completed topic to `PROGRESS.md`.
- [ ] Stage only the new coverage and `PROGRESS.md`.
- [ ] Commit with the required message.
- [ ] Push the current branch.
- [ ] Deliver the coverage/status through the active interface.

---

# 20. Core Principle

The project is not a daily news feed.

It is a **structured, cumulative HPC education system that uses real developments to make the curriculum current and concrete**.

Always optimize for:

> strong fundamentals + technical depth + coherent progression + connection to real HPC systems

rather than:

> maximum number of articles + maximum freshness + maximum volume
