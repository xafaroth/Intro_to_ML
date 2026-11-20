# ML Learning Roadmap

Goal: become an experienced **applied ML practitioner**, confident using known techniques as tools to solve problems.
Not a model developer. Time budget: under 4 hours/week, so roughly 3 to 4 weeks per stage, about a year in total.

**Principle: build the core mechanism by hand once, so I understand it and can debug it. Then use the professional tool.**
"By hand" parts follow the no-library rule; "with tools" parts are *supposed* to use libraries well.

Status: ⬜ not started · 🟨 in progress · ✅ done
Hub: https://github.com/xafaroth/Intro_to_ML
Each **topic** is its own GitHub repo under https://github.com/xafaroth, cloned locally into `projects/<repo>/`.
Each **stage** is a folder inside that repo: `projects/<repo>/NN_stage_name/`.
Every stage ends with a short debrief with the tutor (Claude, "ML Expertise" project on claude.ai): I explain the key ideas back.

## Topic repos
| Repo | Stages | GitHub | Status |
|---|---|---|---|
| `ml-foundations` | 01–03 | not created yet | ⬜ |
| `cnns` | 04 | not created yet | ⬜ |
| `sequence-models` | 05 | not created yet | ⬜ |
| `transformers` | 06 | not created yet | ⬜ |
| `reinforcement-learning` | 07–08 | not created yet | ⬜ |
| `generative-models` | 09–10 | not created yet | ⬜ |
| `foundation-models` | 11–13 | not created yet | ⬜ |

## Stages

### `ml-foundations`
| # | Stage | Core ideas | By hand | With tools | Done when | Status | Started | Done |
|---|---|---|---|---|---|---|---|---|
| 01 | `01_training_loop` | tensors, autograd, `nn.Module`, `DataLoader`, BCE with logits, L2 penalty vs weight decay, reproducibility | PyTorch logistic regression + manual training loop; small MLP | sklearn `LogisticRegression` as reference | coefficients match sklearn to a few decimals; MLP overfits one batch on purpose, then generalizes; train/val curves; same seed gives same numbers | ⬜ | | |
| 02 | `02_applied_tabular` | pipelines, `ColumnTransformer`, CV strategy, leakage, feature engineering, gradient boosting, imbalanced metrics, baselines | | sklearn pipelines, LightGBM, a Kaggle tabular competition (e.g. a Playground Series episode) | honest CV score that tracks the leaderboard; at least one leakage trap found and documented; beats a sane baseline | ⬜ | | |
| 03 | `03_dl_mechanics` | optimizers (SGD/momentum/Adam/AdamW), LR schedules, init, normalization, dropout, reading loss curves | deliberate breakage experiments on an MLP | | can diagnose a bad run from its curves and explain each fix | ⬜ | | |

### `cnns`
| # | Stage | Core ideas | By hand | With tools | Done when | Status | Started | Done |
|---|---|---|---|---|---|---|---|---|
| 04 | `04_cnns` | convolution, pooling, BatchNorm, residuals, augmentation, transfer learning, error analysis | small CNN on CIFAR-10 | pretrained backbone (torchvision/timm), fine-tuning on a Kaggle image dataset | fine-tuned model clearly beats scratch model; confusion matrix + inspected failure cases | ⬜ | | |

### `sequence-models`
| # | Stage | Core ideas | By hand | With tools | Done when | Status | Started | Done |
|---|---|---|---|---|---|---|---|---|
| 05 | `05_sequences_har` | windowing, subject-wise splits, 1D-CNN, LSTM/GRU, intro to attention | 1D-CNN and LSTM on a public IMU activity-recognition dataset (e.g. UCI HAR) | LightGBM on hand-crafted features as baseline | three-way comparison with subject-wise validation; can say where learned features win and where they don't | ⬜ | | |

### `transformers`
| # | Stage | Core ideas | By hand | With tools | Done when | Status | Started | Done |
|---|---|---|---|---|---|---|---|---|
| 06 | `06_transformers` | self-attention, multi-head, positional encoding, LayerNorm, residuals, tokenization, pretrained checkpoints | one transformer block, shape-checked and tested | Hugging Face `transformers` + `datasets`: fine-tune a pretrained model on a classification task | own block passes tests; fine-tuned model evaluated against a simple baseline | ⬜ | | |

### `reinforcement-learning`
| # | Stage | Core ideas | By hand | With tools | Done when | Status | Started | Done |
|---|---|---|---|---|---|---|---|---|
| 07 | `07_rl_foundations` | MDPs, Bellman equations, value iteration, Q-learning, exploration | tabular Q-learning on Gymnasium FrozenLake/Taxi | | converges; effect of exploration schedule shown | ⬜ | | |
| 08 | `08_deep_rl` | DQN, replay buffer, target network, policy gradients, PPO, variance reduction | DQN on CartPole | Stable-Baselines3 PPO on a harder control task | own DQN solves CartPole; PPO tuned and compared over several seeds | ⬜ | | |

### `generative-models`
| # | Stage | Core ideas | By hand | With tools | Done when | Status | Started | Done |
|---|---|---|---|---|---|---|---|---|
| 09 | `09_vae_and_gan` | latent variables, ELBO, reparameterization, adversarial training, mode collapse | VAE + small DCGAN on (Fashion-)MNIST | | samples + latent interpolation; GAN instability observed and explained | ⬜ | | |
| 10 | `10_diffusion` | forward/reverse process, DDPM, noise schedules, U-Net, DDIM, classifier-free guidance, latent diffusion | small DDPM on (Fashion-)MNIST | Hugging Face `diffusers`: pretrained pipelines, schedulers, conditioning | own DDPM produces recognizable samples; can explain each knob in a `diffusers` pipeline | ⬜ | | |

### `foundation-models`
| # | Stage | Core ideas | By hand | With tools | Done when | Status | Started | Done |
|---|---|---|---|---|---|---|---|---|
| 11 | `11_embeddings_and_zero_shot` | contrastive pretraining (CLIP objective), embeddings, zero-shot classification, retrieval | | open CLIP/SigLIP-family models, a vector index | zero-shot vs linear probe vs fine-tune compared on one dataset; working retrieval demo | ⬜ | | |
| 12 | `12_vlms` | vision encoder + LLM + projector, prompting, evaluation, failure modes | | open-weight VLM inference (quantized on the 3090) | evaluated on a public VQA/captioning set with a written failure analysis | ⬜ | | |
| 13 | `13_adaptation` | LoRA/QLoRA, instruction data, eval before/after, when to fine-tune vs prompt | | PEFT/LoRA fine-tune of an open VLM on the 3090 | measurable gain over the base model on held-out data, with a fine-tune-vs-prompt verdict | ⬜ | | |

> GPU note: stages 01–05 run on the Windows laptop (CPU). Stage 06 fine-tuning, deep RL tuning (08), and all of
> 09–13 go to the Linux RTX 3090 box (24 GB); prototype with `--smoke` on the laptop first. Colab/Kaggle as fallback.
> 24 GB is enough for small DDPMs, `diffusers` inference and LoRA fine-tuning of small (≤7B, quantized) VLMs.

> Data note: public datasets only. No employer data in any repo.

## Log
<!-- Short dated entries: what you finished, what clicked, what's still fuzzy. -->
