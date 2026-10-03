# Methodology

## 1. Problem formulation

DATMO treats Transformer deployment as a multi-objective optimization problem. For an architecture θ, the Phase II objective is:

minimize

`[-MCC(θ), Latency(θ), PeakMemory(θ), FLOPs(θ)]`

The negative sign converts MCC maximization into a minimization objective for the multi-objective optimizer.

## 2. Phase I — training configuration optimization

The standard BERT-base architecture is kept fixed while four training variables are optimized:

- learning rate: `[1e-5, 5e-5]`, log-uniform
- weight decay: `[1e-6, 1e-2]`, log-uniform
- batch size: `{8, 16, 32}`
- freeze ratio: `[0, 0.5]`

The freeze ratio `R` determines the number of frozen lower encoder layers as `floor(RL)`, where `L` is the number of encoder layers.

Optuna TPE with seed 42 performs 15 trials. Each trial uses AdamW and two training epochs. The configuration with the highest validation MCC is fixed for Phase II.

## 3. Phase II — architecture-aware multi-objective search

The architecture is represented by:

`θ = (N_L, N_H, m_FFN)`

where:

- `N_L` = number of Transformer encoder layers
- `N_H` = attention heads per layer
- `m_FFN` = feed-forward-network multiplier
- hidden dimension `d = 768` is fixed

The search space is:

- layers: 4–12
- heads: `{4, 6, 8, 12}`
- FFN multiplier: `{1, 2, 3, 4}`

This gives `9 × 4 × 4 = 144` possible architectures. NSGA-II with seed 42 evaluates 30 configurations.

Each candidate is trained for two epochs with the Phase I configuration. Pretrained weights are retained where dimensions are compatible; incompatible parameters are reinitialized.

## 4. Evaluation

### Predictive performance

Validation MCC is the primary predictive metric because it provides a balanced multiclass evaluation and accounts for all confusion-matrix components.

### Inference latency

For architecture search, latency is measured at batch size 1 with sequence length 128. After 20 warm-up passes, 100 timed forward passes are performed. Mean, P50, P95, and P99 latency are recorded.

### Memory and FLOPs

Peak allocated GPU memory is measured at batch size 1. FLOPs are estimated using `fvcore`'s `FlopCountAnalysis`.

## 5. ONNX deployment validation

Selected models are exported to ONNX using opset 17 and benchmarked with ONNX Runtime on CPU. The evaluation includes single-request latency, batch-size scaling, and concurrency stress testing.

Batch sizes: `{1, 2, 4, 8, 16, 32}`.

Concurrency levels: `{1, 2, 4, 8, 16}` CPU threads.

For batch and concurrency experiments, 10 warm-up and 50 measured runs are used.
