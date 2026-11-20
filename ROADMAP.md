# ML Learning Roadmap

Status: ⬜ not started · 🟨 in progress · ✅ done
Each **topic** is its own GitHub repo under https://github.com/xafaroth, cloned locally into `projects/<repo>/`.
Each **stage** is a folder inside that repo: `projects/<repo>/NN_stage_name/`.

## Topic repos
| Repo | Stages | GitHub | Status |
|---|---|---|---|
| `ml-foundations` | 01–04 | not created yet | ⬜ |
| `pytorch-fundamentals` | 05 | not created yet | ⬜ |
| `cnns` | 06 | not created yet | ⬜ |
| `rnns` | 07 | not created yet | ⬜ |
| `transformers` | 08–10 | not created yet | ⬜ |
| `representation-learning` | 11 | not created yet | ⬜ |
| `generative-models` | 12 | not created yet | ⬜ |
| `diffusion-models` | 13–14 | not created yet | ⬜ |
| `reinforcement-learning` | 15–17 | not created yet | ⬜ |
| `vlms` | 18–19 | not created yet | ⬜ |

## Stages

### `ml-foundations` — NumPy, from scratch
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 01 | `01_binary_classification` | sigmoid, BCE loss, gradient descent, decision boundary, accuracy/precision/recall | logistic regression in NumPy | ⬜ | | |
| 02 | `02_multiclass_and_regularization` | softmax, cross-entropy, L1/L2, train/val/test splits, overfitting | softmax regression in NumPy | ⬜ | | |
| 03 | `03_classical_ml` | kNN, decision trees, SVM, ensembles, cross-validation, feature scaling | a decision tree; then compare with scikit-learn | ⬜ | | |
| 04 | `04_neural_nets_from_scratch` | MLP, backprop, activations, init, gradient checking | 2-layer MLP + backprop in NumPy | ⬜ | | |

### `pytorch-fundamentals`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 05 | `05_pytorch_fundamentals` | tensors, autograd, `nn.Module`, `DataLoader`, optimizers, training loop | port stage 04 MLP; your own reusable train loop | ⬜ | | |

### `cnns`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 06 | `06_cnns` | convolution, pooling, BatchNorm, dropout, augmentation, ResNet | small CNN + ResNet block on CIFAR-10 | ⬜ | | |

### `rnns`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 07 | `07_sequence_models` | embeddings, RNN/LSTM/GRU, BPTT, teacher forcing | char-level RNN language model | ⬜ | | |

### `transformers`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 08 | `08_attention_and_transformers` | self-attention, multi-head, positional encoding, LayerNorm, residuals | transformer block from scratch | ⬜ | | |
| 09 | `09_language_models` | tokenization (BPE), GPT-style decoder, sampling, scaling basics | mini-GPT on a small text corpus | ⬜ | | |
| 10 | `10_vision_transformers` | patch embeddings, ViT, comparison with CNNs | ViT on CIFAR-10 | ⬜ | | |

### `representation-learning`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 11 | `11_representation_learning` | autoencoders, contrastive learning (SimCLR), CLIP objective | contrastive image-text model on a tiny dataset | ⬜ | | |

### `generative-models`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 12 | `12_vaes_and_gans` | latent variables, ELBO, reparameterization, adversarial training | VAE + DCGAN on MNIST | ⬜ | | |

### `diffusion-models`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 13 | `13_diffusion_models` | forward/reverse process, DDPM, noise schedules, U-Net, DDIM, classifier-free guidance | DDPM on MNIST / CIFAR-10 | ⬜ | | |
| 14 | `14_latent_and_conditional_diffusion` | latent diffusion, text conditioning, cross-attention | conditional diffusion in a VAE latent space | ⬜ | | |

### `reinforcement-learning`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 15 | `15_rl_foundations` | MDPs, Bellman equations, value/policy iteration, Q-learning, exploration | tabular Q-learning on Gymnasium FrozenLake/Taxi | ⬜ | | |
| 16 | `16_deep_rl` | DQN, replay buffer, target networks, REINFORCE, actor-critic | DQN on CartPole; REINFORCE | ⬜ | | |
| 17 | `17_ppo_and_rlhf` | PPO, GAE, reward models, RLHF / DPO for language models | PPO on a control task; DPO on a small LM | ⬜ | | |

### `vlms`
| # | Stage | Core ideas | Build it yourself | Status | Started | Done |
|---|---|---|---|---|---|---|
| 18 | `18_vision_language_models` | vision encoder + LLM, projection layers, LLaVA-style training, captioning/VQA | tiny VLM: frozen CLIP encoder + small LM + projector | ⬜ | | |
| 19 | `19_finetuning_and_adaptation` | transfer learning, LoRA, instruction tuning, evaluation | LoRA fine-tune a pretrained VLM | ⬜ | | |

> GPU note: early topics run on the Windows laptop (CPU). Larger runs in `transformers` / `representation-learning`
> and all training in `generative-models`, `diffusion-models`, `reinforcement-learning` (deep RL) and `vlms` go to the
> Linux RTX 3090 box (24 GB); prototype with `--smoke` on the laptop first. Colab/Kaggle as fallback.
> 24 GB is enough for DDPM/latent diffusion on small datasets and LoRA fine-tuning of small (≤7B, quantized) VLMs.

## Log
<!-- Short dated entries: what you finished, what clicked, what's still fuzzy. -->
