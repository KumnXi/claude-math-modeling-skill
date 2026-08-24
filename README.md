# Math Modeling Solver — Claude Code Skill

**A systematic 5-phase workflow to solve math modeling contest problems
(Huazhong Cup, CUMCM, MCM/ICM) — structured, reproducible, paper-ready.**

---

## Why this skill

AI-assisted math modeling usually fails for three reasons: the process drifts
without a fixed workflow, generated code is not reproducible, and the final
report is inconsistent in quality. This skill fixes all three by driving Claude
Code through a fixed five-phase pipeline — problem understanding, solution
architecture design, solver implementation, validation, and report generation.
Every run follows the same structure and produces runnable Python code plus a
paper-ready analysis report. You focus on the model; the skill keeps the
process honest and repeatable.

## Demo / effect comparison

> Coming soon — a before/after comparison of solving a typical Huazhong Cup
> problem with and without this skill (see `docs/comparison.md`, currently in
> preparation).

## Installation

Install from the marketplace (recommended):

```
/plugin marketplace add github:KumnXi/claude-math-modeling-skill
/plugin install math-modeling-solver@math-modeling-skills
```

Then restart Claude Code. The skill is loaded automatically.

## Manual installation

If you prefer not to use the marketplace:

1. Download [`skills/math-modeling-solver/SKILL.md`](https://github.com/KumnXi/claude-math-modeling-skill/blob/main/skills/math-modeling-solver/SKILL.md)
2. Place it in one of these locations:
   - **Project-level (this project only):** `.claude/skills/math-modeling-solver/SKILL.md`
   - **User-level (all projects):** `~/.claude/skills/math-modeling-solver/SKILL.md`
3. Restart Claude Code — the skill is auto-discovered.

## How it works

The skill runs a fixed five-phase workflow:

1. **Phase 1 — Understand the problem:** read the problem statement and data
   files, break into sub-problems, extract all constraints and model parameters.
2. **Phase 2 — Design the solution architecture:** scaffold a standard project
   directory (`problem_A/code|data|figures|report/`), choose the modeling
   approach, and lay out the solver pipeline.
3. **Phase 3 — Implement the solver:** generate runnable Python code with
   `pandas`/`numpy`/`scipy` only, covering greedy construction, 2-opt local
   search, time-dependent costs, and fleet constraints.
4. **Phase 4 — Validate and debug:** check every constraint, write targeted
   fixes for violations, and compare results against simple baselines.
5. **Phase 5 — Analyze results:** emit structured JSON, human-readable route
   summaries, statistics, visualizations, and a cross-problem comparison.

## Usage

Put your problem statement (PDF/Word) and data files (Excel/CSV) in the
current project directory, then trigger the skill in one of two ways:

- **Command trigger:** type `/math-modeling-solver`
- **Text trigger:** describe your task in natural language, e.g. "Start the
  math modeling workflow for this competition problem."

The skill will then guide you through providing the problem, data, and any
special requirements.

## Supported competitions

- **华中杯 (Huazhong Cup)** — national provincial-level contest, VRP / operations
  research / prediction style problems
- **CUMCM 国赛** — China Undergraduate Mathematical Contest in Modeling
- **MCM/ICM 美赛** — Mathematical Contest in Modeling / Interdisciplinary Contest in Modeling

## FAQ

1. **Command shows "Unknown command"?**
   Fall back to the text trigger, or verify the `SKILL.md` is correctly placed
   under `.claude/skills/` and restart Claude Code.

2. **Encoding errors when reading Excel files?**
   The skill handles common encoding issues automatically; if one persists, save
   the file as UTF-8 and retry.

3. **Context limit reached during a run?**
   Run `/compact` to compress the conversation, or split the workflow across
   phases (finish problem 1 before starting problem 2).

## License & credits

MIT License. Copyright (c) 2026 KumnXi. `LICENSE` will be included in the
first tagged release. Built for the math modeling community — contributions and
starring the repo are welcome.
