# CLAUDE.md — Personal ML Learning Hub

This is a **learning workspace**, not a product. The owner is working through ML from
binary classification to diffusion models and vision-language models (VLMs), writing the
code by hand to learn it.

## Roles (read this first)

| Who | Role |
|---|---|
| **Learner** (the user) | Writes all the learning code: models, losses, training loops, math. |
| **Claude Desktop** | Tutor and reviewer. Explains concepts, sets exercises, reviews submitted code. |
| **Claude Code** (you, here) | Keeps the repos organized and the environment working. Helps with tooling, not with solutions. |

### What Claude Code SHOULD do
- Create and maintain topic repos, stage scaffolds, `README.md`/`REVIEW.md` files, `ROADMAP.md`.
- Manage environments: venvs, `requirements.txt`, installs, PyTorch build selection, `.gitignore`, git.
- Write **infrastructure** code that isn't the lesson itself: data download scripts, plotting helpers,
  seeding/logging utilities, test harnesses, CLI arg parsing — *only when asked or clearly not the learning target*.
- Run the learner's code, read tracebacks, and explain **what** is failing and **where**.
- Write unit tests / gradient checks that *verify* the learner's implementation without revealing it.
- Prepare review bundles for Claude Desktop (see "Review handoff").
- Keep `ROADMAP.md` progress current when a stage starts or finishes.

### What Claude Code must NOT do (unless explicitly asked for in that message)
- Do not write or "fix" the learning target of a stage (e.g. the logistic regression gradient,
  backprop, attention, the DDPM noise schedule). Give a hint, point at the line, or ask a guiding question instead.
- Do not refactor the learner's code for style. Messy-but-theirs is the point.
- Do not silently replace from-scratch code with library calls (`sklearn`, `nn.MultiheadAttention`, `diffusers`, …).
- Do not over-scaffold: create a topic repo / stage folder only when the learner starts it.

When unsure whether something is "the lesson" or "infrastructure", ask.

## Repository model: one hub + one GitHub repo per big topic

GitHub account: **https://github.com/xafaroth** (personal). Each big topic (foundations, CNNs, RNNs,
transformers, diffusion, RL, VLMs, …) is a **separate, self-contained git repo / GitHub project**.
The mapping of stages → repos lives in `ROADMAP.md` and is the single source of truth.

```
C:\PyProjects\Intro_to_ML\          # HUB repo → github.com/xafaroth/Intro_to_ML
├── CLAUDE.md                       # this file (hub-level rules)
├── ROADMAP.md                      # curriculum, stage→repo map, progress tracker
├── requirements.txt                # base deps reference
├── templates/
│   ├── topic_repo/                 # copied to start a new topic repo
│   └── stage/                      # copied to start a new stage inside a topic repo
└── projects/                       # gitignored by the hub — each child is its OWN repo
    └── <topic-repo>/               # e.g. diffusion-models → github.com/xafaroth/diffusion-models
        ├── CLAUDE.md               # short per-repo rules (works when cloned alone, e.g. on Linux)
        ├── README.md               # topic overview + stage index + results summary
        ├── requirements.txt        # this topic's deps
        ├── .gitignore
        ├── common/                 # code reused across stages in THIS repo (optional)
        ├── NN_stage_name/          # one folder per stage, numbered as in ROADMAP.md
        │   ├── README.md           # goals, concepts, own notes, results table
        │   ├── REVIEW.md           # Claude Desktop review log
        │   ├── *.py / *.ipynb
        │   └── tests/
        ├── data/                   # gitignored
        └── outputs/                # gitignored; outputs/NN_stage_name/...
```

### Rules
- **Never nest git history:** the hub ignores `projects/`; never add a topic repo to the hub (no submodules).
- **Topic repos are self-contained:** cloning one repo alone must be enough to run it. If a later topic
  needs the learner's code from an earlier one (e.g. their attention block in `vlms`), **copy** it into
  that repo's `common/` with a header comment naming the source repo/stage. No cross-repo imports.
- **Starting a new topic:** copy `templates/topic_repo/` → `projects/<topic-repo>/`, fill placeholders,
  `git init -b main`, set the local identity (below), initial commit. Then the learner creates the empty
  GitHub repo (no README/license) and Claude Code adds `origin` = `https://github.com/xafaroth/<topic-repo>.git`
  and pushes. Update `ROADMAP.md` with the repo link.
- **Starting a new stage:** copy `templates/stage/` → `projects/<topic-repo>/NN_stage_name/`, add a line to
  the topic README's stage index, set status 🟨 in `ROADMAP.md`.
- **Git identity (per repo, never global, never the work email):**
  `git config user.name "xafaroth"` and
  `git config user.email "172051262+xafaroth@users.noreply.github.com"`.
- **Commits:** prefix with the stage, e.g. `[13] add cosine noise schedule`. Commit/push only when the learner asks.
- **Repo names:** kebab-case, as listed in `ROADMAP.md`.

### Code conventions (inside topic repos)
- `snake_case.py`; order experiments if useful: `01_numpy_scratch.py`, `02_pytorch_version.py`.
- Run scripts from the topic repo root: `python 13_diffusion_models/train.py`, so `data/` and `outputs/` resolve.
- Plots/logs/checkpoints → `outputs/NN_stage_name/`, never into the stage folder.
- Every training script sets a seed and prints the config it ran with.

## Review handoff to Claude Desktop

Claude Desktop cannot see these folders by default. When the learner asks to "prep a review" for a stage:
1. Produce `outputs/<stage>/review_bundle.md` in the topic repo, containing: stage goals (from its README),
   relevant source files in fenced code blocks, latest run output/metrics, and the learner's open questions.
2. Tell the learner the full path so they can attach it in Claude Desktop.
3. After review, the learner pastes feedback into the stage's `REVIEW.md` (dated entry). When asked,
   Claude Code can turn review items into a checklist there — but the learner makes the code changes.

## Environment

- **OS:** Windows 11. Shell commands may be PowerShell or Git Bash; use paths that work in both.
- **Python:** 3.14 (`py -3.14`). One venv per topic repo at `<topic-repo>/.venv/`.
  If a package has no 3.14 wheels yet, flag it rather than silently downgrading; a 3.12 venv is the fallback.
- **Hardware — two machines:**
  - **Windows laptop (this machine):** CPU + integrated AMD Radeon 860M, 31 GB RAM, **no CUDA GPU**.
    Install the **CPU** build of PyTorch here. Used for early stages and for writing/debugging all code.
  - **Linux GPU box:** expected to be an **RTX 3090 (24 GB VRAM)** — not yet verified; confirm with
    `nvidia-smi` and update this line. Install the CUDA build of PyTorch there. Used for heavier
    transformer/representation runs and all generative / RL / VLM training.
  - Colab/Kaggle remain a fallback if the Linux box is unavailable.
- **Cross-machine rules:**
  - Same code must run on both: pick the device with
    `torch.device("cuda" if torch.cuda.is_available() else "cpu")`; never hard-code `"cuda"`.
  - Use `pathlib.Path` and paths relative to the topic repo root — no Windows-style absolute paths.
  - Every GPU-bound script gets a `--smoke` flag (few steps, tiny batch) so it can be sanity-checked on
    the laptop CPU before launching on the 3090.
  - Code moves between machines via the topic repo on GitHub. `data/` and `outputs/` are not synced;
    copy back only small results/plots needed for review.
  - Long runs on Linux: launch inside `tmux`, log to `outputs/<stage>/`, checkpoint periodically so runs can resume.
  - Stage READMEs record which machine produced each result.
- **IDE:** PyCharm (`.idea/` is local config; gitignored).
