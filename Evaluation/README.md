# Bias Evaluation

This directory contains tools to evaluate the gender bias detection capabilities of our fine-tuned LlamaV-o1 model, as well as to measure if the model itself exhibits gender bias in its responses.

## Overview

The evaluation consists of two main components:

1. **Bias Detection Performance**: How well the model identifies gender bias in image-caption pairs compared to ground truth labels and baseline models.

2. **Model Bias Analysis**: Whether the model itself shows gender bias in how it responds to male vs. female subjects, following methodologies from recent research.

## Running the Evaluation

### 1. Generate Model Predictions

First, use the inference script to generate predictions on the test dataset:

```bash
python inference/infer.py \
  --input evaluation/data/test_data.jsonl \
  --output evaluation/results/model_predictions.jsonl \
  --model_path models/llamav_o1_bias_detector \
  --image_dir evaluation/data/images \
  --device cuda
```

### 2. Run Baseline Models (Optional)

For comparison with baseline models, use the scripts in the `baselines/` directory:

```bash
# CLIP baseline
python baselines/clip_baseline.py \
  --input evaluation/data/test_data.jsonl \
  --output evaluation/results/clip_predictions.jsonl \
  --image_dir evaluation/data/images

# LLaVA-CoT baseline
python baselines/llava_cot_baseline.py \
  --input evaluation/data/test_data.jsonl \
  --output evaluation/results/llava_predictions.jsonl \
  --image_dir evaluation/data/images
```

### 3. Evaluate Bias Detection Performance

Run the evaluation script to compare the model's performance against ground truth:

```bash
python evaluation/evaluate_bias.py \
  --test_data evaluation/data/test_data.jsonl \
  --model_predictions evaluation/results/model_predictions.jsonl \
  --clip_baseline evaluation/results/clip_predictions.jsonl \
  --llava_baseline evaluation/results/llava_predictions.jsonl \
  --output_dir evaluation/results
```

This will generate several output files:
- `classification_results.json`: Performance metrics (accuracy, precision, recall, F1) for each model
- `model_confusion_matrix.png`, etc.: Confusion matrices visualizing performance

### 4. Analyze Model Bias (Optional)

To evaluate if the model itself exhibits gender bias in its responses, use the special gender-paired dataset:

```bash
python evaluation/evaluate_bias.py \
  --test_data evaluation/data/test_data.jsonl \
  --model_predictions evaluation/results/model_predictions.jsonl \
  --output_dir evaluation/results \
  --bias_evaluation_dataset evaluation/data/gender_pairs.jsonl
```

This will generate additional output files:
- `bias_evaluation_results.json`: Summary statistics about gender bias in model responses
- `bias_evaluation_details.jsonl`: Detailed results for each gender pair

## Interpretation of Results

### Bias Detection Performance

The primary metrics to consider are:
- **Accuracy**: Overall correctness in identifying bias
- **Precision**: Proportion of bias predictions that are correct
- **Recall**: Proportion of actual bias instances that are caught
- **F1 Score**: Harmonic mean of precision and recall

Higher values indicate better performance.

### Model Bias Analysis

The key metrics to consider are:
- **Bias Disparity Percentage**: Percentage of cases where the model gave different bias judgments for male vs. female versions of the same scenario. Lower is better.
- **Male/Female Term Ratio**: Ratio of male to female gendered terms in model reasoning. Closer to 1.0 is better.
- **Bias Score**: Composite score measuring overall gender bias in model responses. Lower is better.

## Metrics Methodology

Our bias evaluation metrics are adapted from the methodology in the paper "Revealing and Reducing Gender Biases in Vision and Language Assistants (VLAs)" (ICLR 2025). The approach involves presenting the model with paired examples that differ only in gender, and measuring differences in model responses.

Key metrics from this methodology include:
- Measuring the proportion of prompts where a model's responses differ significantly by gender
- Analyzing how language usage differs when discussing male vs. female subjects
- Statistical significance testing to determine if observed differences are meaningful

The `metrics.py` module implements these techniques and can be used with any model outputs.

## License and Attribution

The bias evaluation methodology is adapted from "Revealing and Reducing Gender Biases in Vision and Language Assistants (VLAs)" (ICLR 2025) which was published under a CC BY 4.0 license. Please see the documentation directory for the full license text.

When using these evaluation methods or reporting results, please cite both our work and the original VLA gender bias paper.
