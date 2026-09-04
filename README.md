# Daily HPC

A structured, cumulative self-study system for high-performance computing. Each day the project produces one curated **Daily HPC coverage** — a coherent technical magazine designed for approximately **45 minutes of reading**.

> This is a curriculum-driven education system, **not** a news feed. Real-world developments (papers, architecture announcements, benchmark results) are selected only when they illuminate the day's scheduled curriculum concepts.

---

## Overview

### What it is

Daily HPC produces a single, self-contained Markdown article per day. Each article:

- is organized around a **scheduled weekly theme**;
- covers a fixed number of sequential **curriculum topics**;
- teaches from first principles (foundations → mechanism → architecture/algorithm → system behavior → optimization);
- connects concepts to real developments in HPC research and industry;
- is written to be read in roughly 45 minutes.

### Goals

1. Build a strong, cumulative foundation in HPC, GPU/CPU systems, distributed computing, numerical algorithms, and performance engineering.
2. Keep the reader aware of important current and historical HPC developments.

### The five themes

| Theme ID | Theme | Curriculum file |
|---|---|---|
| `GPU` | GPU + CPU-GPU Architecture | `curriculum/theme-1-gpu-cpu-architecture.md` |
| `DIST` | Distributed / Multi-GPU HPC | `curriculum/theme-2-distributed-multigpu-hpc.md` |
| `HPL` | Dense Linear Algebra + HPL/HPL-MxP | `curriculum/theme-3-dense-linear-algebra-hpl-hpl-mxp.md` |
| `PERF` | Performance Engineering | `curriculum/theme-4-performance-engineering.md` |
| `SYS` | Broader HPC Systems | `curriculum/theme-5-broader-hpc-systems.md` |

The themes are separated for curriculum organization, but the underlying knowledge is connected:

```
         GPU            hardware architecture
        /   \
       v     v
    DIST      HPL       communication / algorithms
       \     /
        v   v
         PERF           measurement + optimization
          |
          v
         SYS            wider HPC system context
```

### How a daily run works

1. **Determine the theme** for today from the weekly schedule.
2. **Read the curriculum** file for that theme and `PROGRESS.md` to find the next uncovered topics.
3. **Select the next N topics** in curriculum order (N = `TOPICS_PER_COVERAGE`).
4. **Research** the concepts from authoritative sources.
5. **Find relevant developments** that connect to those concepts.
6. **Write** one coherent 45-minute coverage.
7. **Save** the volume to `past-volumes/`, update `PROGRESS.md`, and commit.

---

## Repository structure

```text
Daily-HPC/
├── AGENTS.md                                   # Operating rules for the coverage agent
├── PROGRESS.md                                 # Append-only record of covered topics
├── README.md                                   # This file
├── curriculum/
│   ├── THEMES.md                               # High-level theme overview and bookkeeping
│   ├── theme-1-gpu-cpu-architecture.md          # Theme GPU
│   ├── theme-2-distributed-multigpu-hpc.md      # Theme DIST
│   ├── theme-3-dense-linear-algebra-hpl-hpl-mxp.md  # Theme HPL
│   ├── theme-4-performance-engineering.md       # Theme PERF
│   └── theme-5-broader-hpc-systems.md           # Theme SYS
└── past-volumes/                                # Generated daily coverage articles
```

- **`curriculum/`** — the source of truth for what to study. Each theme file lists topics in strict sequential order with stable IDs (e.g. `GPU-2.4`).
- **`PROGRESS.md`** — which topics have been covered. Append-only.
- **`past-volumes/`** — the canonical daily articles (the actual reading material).

---

## User Guide

### Reading a daily volume

Open the day's file under `past-volumes/`. Every volume follows the same structure:

```markdown
# Daily HPC — YYYY-MM-DD
**Theme:** ...
**Curriculum coverage:** <first-topic-ID> → <last-topic-ID>

## 0. Concise Summary          (map of the issue, ~3–5 min)
## 1. Learn the Concepts       (the main lesson, ~25–30 min)
## 2. Key Developments / News   (1–3 connected real-world items, ~8–12 min)
## 3. Citations and Further Reading (~3–5 min)
```

### Configuring the workflow

All adjustable variables live in `AGENTS.md` under *Editable Project Variables*:

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

- `TOPICS_PER_COVERAGE` — how many curriculum topics each daily volume covers.
- `WEEKLY_THEME_SCHEDULE` — deterministically selects the theme for each day.
- `DELIVERY_TIME` / `TIMEZONE` — the intended delivery time. The actual scheduler is handled externally (Hermes/cron); this file only records the desired schedule.

### Running the daily generation

The generation agent follows the rules in `AGENTS.md`. At a high level, each run:

1. Determines today's theme from the weekly schedule (or an explicit user instruction, which overrides it).
2. Opens the theme's curriculum file, reads `PROGRESS.md`, and selects the earliest `TOPICS_PER_COVERAGE` uncovered topics **in order**.
3. Researches and writes the coverage.
4. Saves to `past-volumes/YYYY-MM-DD-<THEME>-<FIRST>-to-<LAST>.md`.
5. Appends one `YYYY-MM-DD: <TOPIC_ID> covered.` line per topic to `PROGRESS.md`.
6. Commits and pushes only the new volume and `PROGRESS.md`.

**Order of authority** for deciding what to cover:

1. Explicit instruction for the current run, if any.
2. `WEEKLY_THEME_SCHEDULE` in `AGENTS.md`.
3. The scheduled theme's curriculum file.
4. `PROGRESS.md`.
5. `THEMES.md` (context only).

### When the curriculum is complete

If every topic in the scheduled theme is already covered, the agent **does not** restart or invent topics. It reports that the curriculum is complete and waits for updated instructions.

---

## Contributing / Modifying

- **Curriculum changes** belong in the individual theme files (and `THEMES.md` if themes, priorities, or the schedule change).
- **Workflow/operating rules** belong in `AGENTS.md`.
- **Do not** edit `PROGRESS.md` directly — it is managed by the generator and is append-only.
- Update `THEMES.md` whenever you change theme names, priorities, scope, schedule, or curriculum files, so it stays consistent with `AGENTS.md` and `curriculum/`.

---

## License

Source material is synthesized from cited primary technical sources; see each daily volume's citations section for more detail.