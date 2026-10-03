# Reproducibility and code availability

This public repository currently provides the experiment outputs, Pareto-front data, methodology, result tables, and deployment figures used to document DATMO.

The original training/search source code is **not included** in the public repository at this time. Therefore, this repository should not be presented as a fully executable reproduction package.

## What is provided

- complete Phase II result table
- Pareto-front result table
- Phase I best configuration
- representative model comparison
- ONNX Runtime CPU benchmark summary
- methodology and search-space description
- generated research figures

## What is not provided

- model-training source code
- Optuna trial-generation code
- environment lockfile
- pretrained checkpoints
- raw PubMed 200K RCT dataset

## Reproduction notes

The reported experiments use a fixed sequence length of 128 tokens. Phase I uses Optuna TPE with seed 42 for 15 trials. Phase II uses Optuna NSGA-II with seed 42 for 30 architecture configurations. Selected models are exported to ONNX opset 17 and evaluated with ONNX Runtime on CPU.

If the source code is later released, add it under `src/` and update this file with:

1. Python version
2. dependency versions
3. dataset preparation instructions
4. exact train/validation/test split procedure
5. random seeds
6. hardware details
7. commands for Phase I
8. commands for Phase II
9. ONNX export command
10. benchmark commands
