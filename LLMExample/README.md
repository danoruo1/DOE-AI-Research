# Image Fragment Classification Project

## What This Project Does

This project classifies tiny byte fragments from image files. It supports:

- **Multiclass predictions** (e.g., BMP, GIF, JPEG, PNG, TIF, WEBP, BPG, JXR, JPF)
- **Binary predictions** (Legacy vs Modern) where:
  - **Legacy** = BMP, GIF, JPEG, PNG, TIF
  - **Modern** = WEBP, BPG, JXR, JPF

The project can also use an AI teacher (a GPT-based helper) to guide the model during training via knowledge distillation.

## Core Ideas (How It Works)

### 1. Inputs as Bytes

- Training data consists of small file fragments saved as `.dat` (or equivalent)
- Fragments are treated as raw bytes and converted into tensors for the student model
- For AI queries, only a very small portion of each fragment is sent, either:
  - As Base64 (up to 256 bytes) for direct class guessing, or
  - As a compact list of the first 64 bytes (as integers) for Legacy/Modern probability

### 2. Labels and Class Maps

- A configurable classes list defines the label space
- Two maps exist conceptually:
  - `class → index` (for training targets)
  - `index → class` (for readable output and evaluation)

### 3. Student Model (You Train This)

- A standard classifier trained with supervised cross-entropy on your labeled fragments
- Reports loss, accuracy, and macro-F1 each epoch
- For evaluation, you'll also generate a confusion matrix and a classification report

### 4. AI Teacher (Optional Guidance)

The AI teacher is used in two ways:

1. **Direct class guess for bytes**: Given up to 256 raw bytes (Base64-encoded), it returns a predicted class name from your class list. A voting routine can query multiple random windows and use majority vote to boost robustness.

2. **Binary probability (Legacy vs Modern)**: Given the first 64 bytes as integers, it returns a JSON with probabilities that sum to 1 and is parsed to obtain P(Modern). This probability forms a soft target for knowledge distillation.

## Training Workflow

### A) Pure Supervised Training (Always Happens)

1. Load a batch of fragments and ground-truth labels
2. Run the student model to get logits
3. Compute supervised cross-entropy with the true labels
4. Backpropagate and update model parameters
5. Track metrics (accuracy and macro-F1)

### B) Optional AI Teacher Guidance (Binary Heads Only)

When enabled and the model's final layer has 2 logits (Legacy, Modern):

1. For a subset of samples in the batch (controlled by your sampling settings), request the AI teacher's P(Modern)
2. Convert that into a soft target distribution: `[P(Legacy), P(Modern)] = [1 − p, p]`
3. Compute a soft-label cross-entropy between the student's predictions and the teacher's soft targets
4. Blend losses:
   ```
   Final loss = (1 − α) × supervised_loss + α × distillation_loss
   ```
   where α is a tunable weight
5. Continue optimization as usual

This adds a "gentle nudge" from the teacher's belief without replacing your ground truth.

## Controlling Cost, Latency, and Determinism

- **Sampling probability**: Only query the teacher for some samples in each batch rather than all of them
- **Per-batch cap**: Hard-limit the maximum number of teacher calls per batch
- **Caching**: Teacher outputs are memoized using a short signature (e.g., first 64 bytes) so identical fragments don't trigger repeat queries
- **Deterministic prompts**: The teacher runs at temperature 0, aiming for stable, repeatable outputs given the same inputs

## Evaluation

- **Accuracy**: Overall correctness across samples
- **Macro-F1**: Balances performance across classes, helpful under class imbalance
- **Confusion matrix**: Reveals which classes are commonly confused; essential for diagnosing failure modes like "Legacy mislabeled as Modern"

For binary experiments, use `[Legacy, Modern]` as the label order. For multiclass experiments, evaluate over your full class list.

## Data Organization

- **Typical layout**: separate Training and Testing directories with subfolders per class
- If filenames are scrambled, maintain a `mapping.json` that links each fragment to its true label for dataset loading and analysis
- **Splits**: a conventional 75/25 training/testing split works, but you can further hold out a validation set

## Configuration Knobs (High-Level)

- `use_ai`: toggles teacher guidance on/off
- `ai_weight` (α): how strongly to follow the teacher's soft targets (common range: 0.1–0.5)
- `ai_sample_prob`: chance a given sample is sent to the teacher
- `ai_max_calls_per_batch`: cost guardrail; hard cap on teacher calls per batch
- `ai_fn`: optional custom teacher function (defaults to the provided Legacy/Modern probability teacher)
- `ai_cache`: in-memory cache object to reuse previous teacher outputs

## Assumptions and Limitations

- **Teacher guidance is binary-only** as implemented (Legacy vs Modern). For multiclass distillation, the teacher must return a full probability distribution over all classes
- **Byte budget**: AI queries only use tiny byte snippets (up to 256 bytes, or the first 64 bytes as integers). If your fragments are highly compressed or encrypted, these tiny windows may be less informative
- **Calibration**: If the teacher seems overconfident or miscalibrated, lower α, reduce query frequency, or introduce temperature scaling when forming soft targets
- **Privacy**: You are sending small portions of raw bytes to an external model. Ensure this is permissible for your data, especially if fragments could contain sensitive material

## Extending the System

- **Multiclass distillation**: Update the teacher to return class-probability vectors and extend the distillation accordingly
- **Pseudo-labeling**: Use confident teacher predictions to generate additional training targets on unlabeled data
- **Windowing strategies**: Improve robustness by aggregating teacher predictions across multiple offsets, sizes, or signatures
- **Calibration**: Add temperature or Dirichlet calibration to either teacher or student probabilities

## What You'll Need to Provide

- A list of target classes and the rule that defines Legacy vs Modern
- Labeled fragments and (optionally) a mapping file if names are scrambled
- Access to a GPT model for the teacher (including an API key) if you plan to use AI guidance
- Standard ML environment with PyTorch and scikit-learn for training and evaluation

## Summary

- The student learns from ground truth; optional teacher guidance provides soft targets that can improve generalization
- Cost and latency are managed through sampling, capping, and caching
- Evaluation focuses on macro-F1 and confusion matrices to catch class-specific weaknesses
- The binary teacher path is plug-and-play for Legacy vs Modern; multiclass distillation is a straightforward next step if needed