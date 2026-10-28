# CPTS 437 Final Project: Efficient Learning for Sparse-Reward PandaReach

## Project question

How do learned dynamics models and curiosity-driven exploration models improve learning efficiency on a sparse-reward robotic reaching task?

We will start with `PandaReach-v3` in [panda-gym](https://github.com/qgallouedec/panda-gym), which simulates a Franka Panda arm. We will also attempt more challenging tasks in panda-gym time allowing. The environment generates experience during training; there is no fixed dataset. Our main measure is goal success rate versus real environment steps. We will also report wall-clock training time and variation across random seeds.

## Planned methods

| Method | Purpose | Starting point |
| --- | --- | --- |
| SAC | Common model-free baseline for continuous actions | Haarnoja et al., [Soft Actor-Critic](https://arxiv.org/abs/1801.01290) |
| SAC + RND | Add an intrinsic reward for unfamiliar observations | Burda et al., [Random Network Distillation](https://arxiv.org/abs/1810.12894) |
| SAC + learned dynamics | Use short predicted rollouts to supplement real experience | Janner et al., [Model-Based Policy Optimization](https://arxiv.org/abs/1906.08253) |
| SAC + HER | Provide a strong goal-conditioned sparse-reward comparison | Andrychowicz et al., [Hindsight Experience Replay](https://arxiv.org/abs/1707.01495) |

The model-based branch is MBPO-inspired, not a claim that we will reproduce the entire paper.

## Repository layout

```text
.
├── README.md
├── requirements.txt          # Add and pin packages after a tested setup
├── .gitignore
├── src/                      # Shared environment, training, and evaluation code
│   └── methods/              # Method-specific implementations
├── configs/                  # Run settings and fixed comparison budgets
├── notebooks/                # Colab launcher to call python files
└── results/                  # Small final tables and figures for the report
```

## Train with Google Colab

Google Colab is our default training environment. The algorithms and evaluation logic live in Python files in `src/`; the notebook is a small launcher for setup, training commands, and plots. Do not maintain a second copy of algorithms inside a notebook.

### Start a Colab session

1. Go to [Google Colab](https://colab.research.google.com/), choose **File > Open notebook > GitHub**, find this repository, and open the launcher notebook after it has been added. You can also use **File > Upload notebook** for a local `.ipynb` file.
2. Start with a standard CPU runtime. Try a GPU later only if a timed pilot shows that it improves throughput.
3. In the launcher notebook, clone the repository and install its tested dependencies. Replace `OWNER` with the GitHub owner:

```python
!git clone https://github.com/OWNER/CPTS437-Final-Project.git
%cd CPTS437-Final-Project
!python -m pip install -r requirements.txt
```

## Team responsibilities

| Role | Responsibility |
| --- | --- |
| Baseline and setup | Integrate the existing setup code; make SAC train and save reliably |
| Curiosity | Implement RND and verify the intrinsic reward before full runs |
| Dynamics | Train and validate a next-state model; add short model-generated rollouts |
| Evaluation and integration | Build shared evaluation, organize seeds, summarize results and figures |

Each member should review another member's changes. Agree on the observation format, run configuration, and logged fields before the method branches diverge.

## Git workflow

Work in a feature branch (for example, `feature/baseline` or `feature/rnd`), then open a pull request for review. Commit source code, configuration, and small report figures. Keep generated experience, checkpoints, full logs, local environments, and secrets out of Git.


