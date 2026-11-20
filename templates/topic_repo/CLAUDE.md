# CLAUDE.md — {{REPO_NAME}}

Part of a personal ML learning path (hub: https://github.com/xafaroth/Intro_to_ML — full rules in its CLAUDE.md).

**Public repo:** never commit personal or employer info, machine hostnames/IPs/usernames, absolute paths
with usernames, or secrets. Check notebook outputs. A `CLAUDE.local.md`, if present, takes priority and is never committed.

## Roles
- **The learner writes the learning code** (models, losses, training loops, math). Claude Desktop tutors and reviews.
- **Claude Code** handles organization, environment, data scripts, plotting/logging helpers, tests, and
  explaining errors. It must **not** write or fix the concept being learned unless explicitly asked —
  give hints and point at the line instead. Don't refactor the learner's code or swap it for library calls.

## Layout
- `NN_stage_name/` — one folder per stage (`README.md`, `REVIEW.md`, code, `tests/`).
- `common/` — code reused across stages in this repo; code copied from other repos carries a header naming its source.
- `data/`, `outputs/NN_stage_name/` — gitignored.
- Run from the repo root: `python NN_stage_name/script.py`.

## Environment
- venv at `.venv/`; deps in `requirements.txt`.
- Windows laptop = CPU only (CPU PyTorch build). Linux box = RTX 3090 (CUDA build).
- Device via `torch.device("cuda" if torch.cuda.is_available() else "cpu")`; `pathlib` relative paths;
  GPU scripts take `--smoke`; long runs in `tmux` with periodic checkpoints.

## Git
- Identity (local): `xafaroth` / `172051262+xafaroth@users.noreply.github.com`.
- Commit messages prefixed with the stage: `[NN] message`. Commit/push only when asked.

## Review handoff
"Prep a review" → write `outputs/<stage>/review_bundle.md` (goals, code, run output, open questions) and give its full path.
