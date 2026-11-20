# CLAUDE.md — {{REPO_NAME}}

Part of a personal ML learning path (hub: https://github.com/xafaroth/Intro_to_ML — full rules in its CLAUDE.md).

**Public repo:** never commit personal or employer info, machine hostnames/IPs/usernames, absolute paths
with usernames, or secrets. Check notebook outputs. A `CLAUDE.local.md`, if present, takes priority and is never committed.

## Roles
- **The learner writes the learning code** (models, losses, training loops, math). The tutor — Claude
  (ML Expertise project on claude.ai) — teaches and reviews by cloning this repo from GitHub.
- **Claude Code** works on request only: autocompletes code and answers questions when asked, plus chores (environment, data scripts, tests, git).
  It does not set direction or change the roadmap. It must **not** write or fix the concept being learned unless explicitly asked —
  give hints and point at the line instead. Don't refactor the learner's code.
- **No-library rule applies to the "By hand" part of a stage only.** The "With tools" part is meant to use libraries.

## Layout
- `NN_stage_name/` — one folder per stage (`README.md`, `REVIEW.md`, code, `results/`, `tests/`).
- `NN_stage_name/results/` — **committed**: key plots (PNG), `metrics.json`, small logs for review.
- `common/` — code reused across stages in this repo; code copied from other repos carries a header naming its source.
- `data/`, `outputs/NN_stage_name/` — gitignored (checkpoints, bulk outputs).
- Run from the repo root: `python NN_stage_name/script.py`.

## Environment
- Laptop: shared venv at the hub root (`../../.venv/`); Linux box: this repo's own `.venv/`. Deps in `requirements.txt`.
- Windows laptop = CPU only (CPU PyTorch build). Linux box = RTX 3090 (CUDA build).
- Device via `torch.device("cuda" if torch.cuda.is_available() else "cpu")`; `pathlib` relative paths;
  GPU scripts take `--smoke`; long runs in `tmux` with periodic checkpoints.

## Git
- Identity (local): `xafaroth` / `172051262+xafaroth@users.noreply.github.com`.
- Commit messages prefixed with the stage: `[NN] message`. Commit/push only when asked.

## Review workflow
Learner saves key results to `NN_stage_name/results/`, commits and pushes, then asks the tutor to "review stage NN".
Feedback is logged in the stage's `REVIEW.md`. No review bundles.
