# Comparative Analysis: LLMs vs Reinforcement Learning for File Fragment Classification

## Overview

As cybersecurity threats continue to evolve, digital forensics requires increasingly sophisticated tools for file type identification from fragmented data. This repository presents a comprehensive comparison of two cutting-edge machine learning approaches for classifying file fragments:

- **Large Language Models (LLM)** - Leveraging GPT-4.0's pattern recognition capabilities
- **Reinforcement Learning (RL)** - Implementing policy gradient methods with MLP architectures

Our analysis focuses on binary classification of image file fragments into **Legacy** vs **Modern** categories, providing insights into which approach offers superior performance for digital forensics applications.

## Data Preprocessing

### Anonymization Strategy
To ensure robust model evaluation and prevent data leakage, we implemented a comprehensive preprocessing pipeline:

- **Filename Scrambling**: Original filenames were systematically scrambled to eliminate any implicit type information
- **Mapping Preservation**: A secure `mappings.json` file maintains the relationship between scrambled filenames and their original file types
- **Fragment Compression**: File fragments were processed using specialized compression techniques and stored as `.dat` files
- **Byte-to-Hex Conversion**: Raw binary data was converted to hexadecimal strings for LLM processing

This approach ensures that models learn from actual file content rather than filename patterns, providing more reliable real-world performance.

## Methodology

### Large Language Model Approach
- **Model**: GPT-4.0 with custom classification prompting
- **Architecture**: Transformer-based encoder optimized for pattern recognition
- **Input Processing**: Binary fragments converted to hexadecimal strings
- **Classification**: Binary output (1 = Legacy, 0 = Modern) with confidence scores
- **Pipeline**: Modified ML pipeline with custom GPT integration functions

### Reinforcement Learning Approach
- **Algorithm**: Policy Gradient (REINFORCE) with epsilon-greedy exploration
- **Architecture**: Multi-Layer Perceptron (MLP) policy network
- **State Representation**: Normalized byte sequences from file fragments
- **Action Space**: Binary classification decisions (Legacy/Modern)
- **Reward Function**: +1 for correct classification, 0 for incorrect
- **Exploration Schedule**: Decaying epsilon from 0.5 to 0.05 over training epochs

#### RL Implementation Details
- **Policy Network**: 512 → 256 → 128 → 2 (input → hidden layers → output classes)
- **Training**: REINFORCE algorithm with baseline variance reduction
- **Exploration**: Epsilon-greedy with scheduled decay for balanced exploration/exploitation
- **Optimization**: Adam optimizer with gradient clipping for stability

## Dataset Specification

### File Types Analyzed
- **Legacy Formats**: BMP (Bitmap), GIF (Graphics Interchange Format), JPEG (Joint Photographic Experts Group), TIF (Tagged Image File Format)
- **Modern Formats**: BPG (Better Portable Graphics), JPF (JPEG 2000), JXR (JPEG Extended Range), PNG (Portable Network Graphics), WEBP (Web Picture)

### Classification Labels
- **Legacy**: Image formats predominant through early 2000s
- **Modern**: Image formats developed from 2010s to present

## Evaluation Metrics

### Primary Metrics
- **F1 Score**: Harmonic mean of precision and recall, providing balanced performance assessment
- **Confusion Matrix**: Detailed breakdown of true positives, false positives, true negatives, and false negatives
- **Classification Accuracy**: Overall percentage of correctly classified fragments
- **Precision**: Ratio of true positives to total predicted positives
- **Recall**: Ratio of true positives to total actual positives

### Additional Analysis
- **Training Convergence**: Learning curves and reward progression for RL model
- **Inference Time**: Computational efficiency comparison between approaches
- **Robustness**: Performance on corrupted or partial fragments

## Results

### GPT-4.0 (Large Language Model)
- **F1 Score**: [Pending Results]
- **Accuracy**: [Pending Results]
- **Inference Time**: [Pending Results]
- **Confusion Matrix**: [Pending Results]

### Policy Gradient RL (Multi-Layer Perceptron)
- **F1 Score**: [Pending Results]
- **Final Training Reward**: [Pending Results]
- **Convergence Epochs**: [Pending Results]
- **Confusion Matrix**: [Pending Results]

### Comparative Analysis
[Detailed comparison of strengths, weaknesses, and use case recommendations will be added upon completion]

## Technical Implementation

### Dependencies
- PyTorch (RL implementation)
- OpenAI API (GPT-4.0 integration)
- NumPy, Pandas (data processing)
- Matplotlib, Seaborn (visualization)
- Scikit-learn (metrics evaluation)

### Reproducibility
- Fixed random seeds across all frameworks
- Deterministic training procedures
- Version-controlled hyperparameter configurations
- Comprehensive logging of experimental conditions

## Future Work

- Extension to multi-class classification (9 specific file types)
- Investigation of hybrid LLM-RL approaches
- Analysis of computational resource requirements
- Evaluation on additional file fragment datasets
- Real-world deployment considerations for forensic applications

## References

**Datasets:**
- [Original File Fragment Dataset](https://bmcresnotes.biomedcentral.com/articles/10.1186/s13104-019-4812-0) - BMC Research Notes
- [Modified Dataset (Kaggle)](https://www.kaggle.com/datasets/danielanoruo/file-fragment-classification/data?select=mappings.json) - Preprocessed version with anonymized filenames

**Technical References:**
- [BERT: Pre-training of Deep Bidirectional Transformers](https://arxiv.org/abs/1910.10781)
- [Deep Q-Network Tutorial](https://docs.pytorch.org/tutorials/intermediate/reinforcement_q_learning.html) - PyTorch Official Documentation
- [Policy Gradient Methods for Reinforcement Learning](https://proceedings.neurips.cc/paper/1999/file/464d828b85b0bed98e80ade0a5c43b0f-Paper.pdf)

**Additional Resources:**
- OpenAI GPT-4 Documentation
- PyTorch Reinforcement Learning Documentation