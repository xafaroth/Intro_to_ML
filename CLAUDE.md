# CLAUDE.md — Personal ML Learning Hub

This is a **learning workspace**, not a product. The goal is to become an applied ML practitioner, going from
the training loop through CNNs, sequence models, transformers, RL and generative models to foundation models
and VLMs. Each stage builds the core mechanism **by hand** once, then uses the professional tools
(see `ROADMAP.md`).

## Public repos — keep them public-safe
All repos are **public**. Never commit personal or employer information, machine hostnames/IPs/usernames,
absolute paths containing usernames, or secrets. Check notebook outputs before committing.
If a `CLAUDE.local.md` exists, its rules take priority — and it must never be committed.

## Roles (read this first)

| Who | Role |
|---|---|
| **Learner** (the user) | Sets the direction. Writes the learning code: models, losses, training loops, math. |
| **Tutor: Claude (ML Expertise project on claude.ai)** | Tutor and reviewer. Owns the curriculum with the learner: plans `ROADMAP.md`, explains concepts, sets exercises, reviews code. Clones the public topic repos directly from GitHub. |
| **Claude Code** (you, here) | Assistant on request: autocompletes code and answers questions when asked, and does the repo/environment chores the learner asks for. |

### Claude Code does not set direction
- `ROADMAP.md` is written by the learner and the tutor. Do not change its content, stages, order,
  repo mapping or status unless the learner asks for that specific edit. Follow it; don't redesign it.
- Don't propose curriculum changes, next topics, extra stages or exercises unprompted. If asked a question,
  answer it; don't turn the answer into a plan.
- Don't start work the learner didn't ask for. No proactive scaffolding, refactoring or "while I'm here" changes.

### What Claude Code does when asked
- Autocomplete or finish code the learner has started, in their style, limited to what they asked for.
- Answer questions about ML concepts, PyTorch/NumPy APIs, errors and tracebacks.
- Run the learner's code and explain what's failing and where.
- Chores: create a topic repo or stage folder from the templates, manage venvs/installs, git commits and pushes,
  `.gitignore`, data download scripts, plotting/logging helpers, tests or gradient checks.
- Help get a stage review-ready (see "Review workflow"): results saved to `results/`, pushed.

### What Claude Code must NOT do (unless explicitly asked for in that message)
- Do not write or "fix" the learning target of a stage (e.g. the manual training loop,
  the transformer block, the DQN update, the DDPM noise schedule). Give a hint, point at the line, or ask a guiding question instead.
- Do not refactor the learner's code for style. Messy-but-theirs is the point.
- **No-library rule — "By hand" column only:** each stage in `ROADMAP.md` has a **By hand** part and a
  **With tools** part. In the By hand part, never replace from-scratch code with library calls
  (`sklearn`, `nn.MultiheadAttention`, `diffusers`, …). The With tools part is *meant* to use libraries —
  there, idiomatic library usage is the goal.
- Do not over-scaffold: create a topic repo / stage folder only when the learner starts it.

When unsure whether something is "the lesson" or "infrastructure", ask.

## Repository model: one hub + one GitHub repo per big topic

GitHub account: **https://github.com/xafaroth** (personal). Each big topic is a **separate, self-contained
git repo / GitHub project**. There are 7: `ml-foundations` (01–03), `cnns` (04), `sequence-models` (05),
`transformers` (06), `reinforcement-learning` (07–08), `generative-models` (09–10), `foundation-models` (11–13).
The mapping of stages → repos lives in `ROADMAP.md` (owned by the learner and the tutor) and is the single source of truth.

```
C:\PyProjects\Intro_to_ML\          # HUB repo → github.com/xafaroth/Intro_to_ML
├── CLAUDE.md                       # this file (hub-level rules)
├── ROADMAP.md                      # curriculum, stage→repo map, progress tracker
├── requirements.txt                # base deps reference
├── templates/
│   ├── topic_repo/                 # copied to start a new topic repo
│   └── stage/                      # copied to start a new stage inside a topic repo
└── projects/                       # gitignored by the hub — each child is its OWN repo
    └── <topic-repo>/               # e.g. generative-models → github.com/xafaroth/generative-models
        ├── CLAUDE.md               # short per-repo rules (works when cloned alone, e.g. on Linux)
        ├── README.md               # topic overview + stage index + results summary
        ├── requirements.txt        # this topic's deps
        ├── .gitignore
        ├── common/                 # code reused across stages in THIS repo (optional)
        ├── NN_stage_name/          # one folder per stage, numbered as in ROADMAP.md
        │   ├── README.md           # goals, concepts, own notes, results table
        │   ├── REVIEW.md           # tutor review log
        │   ├── *.py / *.ipynb
        │   ├── results/            # COMMITTED: key plots (PNG), metrics.json, small logs
        │   └── tests/
        ├── data/                   # gitignored
        └── outputs/                # gitignored: checkpoints, bulk outputs; outputs/NN_stage_name/...
```

### Rules
- **Never nest git history:** the hub ignores `projects/`; never add a topic repo to the hub (no submodules).
- **Topic repos are self-contained:** cloning one repo alone must be enough to run it. If a later topic
  needs the learner's code from an earlier one (e.g. their training loop from `ml-foundations` in `cnns`), **copy** it into
  that repo's `common/` with a header comment naming the source repo/stage. No cross-repo imports.
- **Starting a new topic:** copy `templates/topic_repo/` → `projects/<topic-repo>/`, fill placeholders,
  `git init -b main`, set the local identity (below), install the local pre-commit hook if `.local/hooks/` exists
  (copy its files into the new repo's `.git/hooks/`), initial commit. Then the learner creates the empty
  GitHub repo (no README/license) and Claude Code adds `origin` = `https://github.com/xafaroth/<topic-repo>.git`
  and pushes. Only edit `ROADMAP.md` (e.g. the repo link) if the learner asks.
- **Starting a new stage:** copy `templates/stage/` → `projects/<topic-repo>/NN_stage_name/`, add a line to
  the topic README's stage index. Don't touch `ROADMAP.md` status unless asked.
- **Git identity (per repo, never global, never the work email):**
  `git config user.name "xafaroth"` and
  `git config user.email "172051262+xafaroth@users.noreply.github.com"`.
- **Commits:** prefix with the stage, e.g. `[10] add cosine noise schedule`. Commit/push only when the learner asks.
- **Repo names:** kebab-case, as listed in `ROADMAP.md`.

### Code conventions (inside topic repos)
- `snake_case.py`; order experiments if useful: `01_numpy_scratch.py`, `02_pytorch_version.py`.
- Run scripts from the topic repo root: `python 10_diffusion/train.py`, so `data/` and `outputs/` resolve.
- **What gets committed vs. not:**
  - `NN_stage_name/results/` (committed): the key plots as PNG, `metrics.json`, small logs — what the tutor
    needs to review the stage. Keep it small (no checkpoints, no datasets, no large arrays).
  - `outputs/NN_stage_name/` (gitignored): checkpoints, intermediate plots, bulk logs, everything else.
- Every training script sets a seed and prints the config it ran with.

## Review workflow

No review bundles. The tutor clones the public topic repo from GitHub and reads it directly:
1. The learner finishes a stage step, saves key results into `NN_stage_name/results/`, commits, and pushes.
2. The learner asks the tutor (Claude, ML Expertise project on claude.ai) to "review stage NN".
3. The learner records the feedback in the stage's `REVIEW.md` (dated entry). When asked, Claude Code can
   turn review items into a checklist there — but the learner makes the code changes.

Before a push meant for review, check that `results/` is up to date, contains nothing private (see
"Public repos"), and that the stage README's results table matches `metrics.json`.

## Environment

- **OS:** Windows 11. Shell commands may be PowerShell or Git Bash; use paths that work in both.
- **Python:** 3.14 (`py -3.14`). One venv per topic repo at `<topic-repo>/.venv/`.
  If a package has no 3.14 wheels yet, flag it rather than silently downgrading; a 3.12 venv is the fallback.
- **Hardware — two machines:**
  - **Windows laptop (this machine):** CPU + integrated AMD Radeon 860M, 31 GB RAM, **no CUDA GPU**.
    Install the **CPU** build of PyTorch here. Used for stages 01–05 and for writing/debugging all code.
  - **Linux GPU box:** expected to be an **RTX 3090 (24 GB VRAM)** — not yet verified; confirm with
    `nvidia-smi` and update this line. Install the CUDA build of PyTorch there. Used for stage 06
    fine-tuning, deep RL tuning (08), and all of 09–13.
  - Colab/Kaggle remain a fallback if the Linux box is unavailable.
- **Cross-machine rules:**
  - Same code must run on both: pick the device with
    `torch.device("cuda" if torch.cuda.is_available() else "cpu")`; never hard-code `"cuda"`.
  - Use `pathlib.Path` and paths relative to the topic repo root — no Windows-style absolute paths.
  - Every GPU-bound script gets a `--smoke` flag (few steps, tiny batch) so it can be sanity-checked on
    the laptop CPU before launching on the 3090.
  - Code moves between machines via the topic repo on GitHub. `data/` and `outputs/` are not synced;
    results needed for review go into the stage's committed `results/` folder.
  - Long runs on Linux: launch inside `tmux`, log to `outputs/<stage>/`, checkpoint periodically so runs can resume.
  - Stage READMEs record which machine produced each result.
- **IDE:** PyCharm (`.idea/` is local config; gitignored).
