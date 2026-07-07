# hiv-run

Isolated test environment for [hiv-agent](https://github.com/jhuphila/hiv-agent). This repo simulates how a regular user would interact with hiv-agent — without access to evaluation tooling that lives in hiv-agent itself.

## Purpose

hiv-run exists to test hiv-agent and its use of the **sierrapy** skill under realistic conditions. The agent works here with only a user prompt (`PROMPT.txt`), input data (`data/`), and the sierrapy skill (`.cursor/skills/sierrapy/`). It does not see `rubric.md`, `eval-protocol.md`, or other grading artifacts from hiv-agent. If those were present, the agent could tailor its behavior to the rubric and fabricate skill usage, making the test invalid.

After a run completes, artifacts are staged back into hiv-agent for scoring (see `Makefile`).

## Research context

This repo is part of a broader benchmarking study of hiv-agent across LLM models. We measure:

- **Efficiency** — time, token usage, and cost per task
- **Fabrication** — whether the agent actually queried Sierra or invented results
- **Instruction adherence** — whether outputs follow the task spec and output skeleton

## Layout

| Path | Role |
|------|------|
| `PROMPT.txt` | User-facing task prompt pasted into the agent |
| `data/` | Input FASTA and other sequence files |
| `eval/` | Output skeleton and reference report (no rubric) |
| `results/` | Sierra JSON/CSV produced during the run |
| `.cursor/skills/sierrapy/` | Skill available to the agent in this sandbox |

## Workflow

1. Paste `PROMPT.txt` into a fresh Cursor session in this repo and run the agent.
2. Export the conversation transcript when finished.
3. From hiv-agent, run `make stage_run` to copy artifacts and generate the judge prompt (see `Makefile` for parameters).
