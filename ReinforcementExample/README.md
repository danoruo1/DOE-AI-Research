# Project Objective & Reproducibility

This notebook classifies **file fragments** into image file types using **reinforcement learning**. Fragments are modeled as **1D byte sequences** (not images).

**Why RL here?** We frame each fragment as a **context**; the agent selects a **label action** and receives a **reward** (+1 correct, 0 or −1 otherwise). This contextual bandit/RL setup encourages exploration and robust decision policies under distribution shift.

## Plan

### Dataset
**`FragmentBinaryDataset`** loads `.dat` fragments as 1D byte tensors (0–255 → 0–1), with optional slicing/augmentation.

### Environment (Gym-style)
- **State**: normalized byte vector (and optional metadata features)
- **Action**: discrete file type (e.g., {BMP, BPG, GIF, JPEG, JPF, JXR, PNG, TIF, WEBP} or your reduced set)
- **Reward**: +1 if predicted type matches ground truth, else 0 (or −1 for stronger signal)
- **Episode**: one step per fragment (contextual bandit) or a short sequence of sub-slices per fragment (multi-step)

### Agent / Policy
- **Policy network**: 1D CNN (or small Transformer/TCN) → logits over classes
- **Exploration**: ε-greedy over logits or softmax with **temperature/entropy bonus**
- **Algorithms**: start with **policy gradient (REINFORCE)** or **cross-entropy as a bandit surrogate** (treat reward as advantage); optionally upgrade to **A2C** for the multi-step variant

### Training
- Optimize policy with reward-weighted log-prob (REINFORCE) or standard cross-entropy with exploration schedule (bandit surrogate)
- Track moving average reward and learning curves

### Evaluation
- Accuracy, macro **F1**, **confusion matrix**, and **per-class reward**
- Optional **GPT comparator**: aggregate sub-slice votes and compare to policy outputs

## RL Formulation (concise)

- **State** s: byte tensor x ∈ [0,1]^L (+ optional stats like entropy, n-gram counts)
- **Action** a: predicted class ∈ {1..K}
- **Reward** r: 1 if a=y else 0 (or −1)
- **Objective**: maximize E[∑r] (contextual bandit → single-step)
- **Policy loss (REINFORCE)**: −log π_θ(a|s) · (r − b) with baseline b (e.g., running mean)

## Reproducibility

- **Seeds** for Python, NumPy, PyTorch (CPU/CUDA/MPS)
- **Determinism**: enable cudnn deterministic; disable nondeterministic ops when possible
- **Config snapshot**: save hyperparams, seed, git commit hash, and class mapping
- **Artifacts**: save best policy weights, confusion matrix PNG, and evaluation CSV

## Practical Notes

- **Sub-slice voting**: sample multiple fixed-length slices per fragment; combine logits (mean/median) to reduce variance
- **Class imbalance**: use reward shaping or per-class weighting; report macro F1
- **Exploration schedule**: decay ε or temperature over epochs; keep small floor (e.g., ε=0.05)
- **Throughput**: throttle any rendering/plots during training; use `plt.pause(0.001)` and finalize with `plt.show()` once

## Seeds

We set seeds for **Python**, **NumPy**, and **PyTorch** and log them along with device (CPU/CUDA/MPS) to ensure runs are comparable.