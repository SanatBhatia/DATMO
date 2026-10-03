# Results

## Phase I

The selected training configuration was:

- Learning rate: `2.0041e-5`
- Weight decay: `1.4619e-5`
- Batch size: `8`
- Freeze ratio: `0.1832`

## Pareto front

The evaluated Pareto-optimal configurations span MCC values from approximately **0.7711 to 0.8262**, mean latency from **3.0551 to 8.8276 ms**, GFLOPs from **2.4209 to 9.9776**, and model sizes from **165.33 to 390.62 MB**.

| Trial | Layers | Heads | FFN | MCC | Mean latency (ms) | GFLOPs | Size (MB) |
|---|---:|---:|---:|---:|---:|---:|---:|
| T20 | 11 | 12 | 4 | 0.8262 | 8.8276 | 9.9776 | 390.62 |
| T10 | 10 | 8 | 4 | 0.8229 | 8.0270 | 9.0706 | 363.58 |
| T12 | 8 | 4 | 4 | 0.8137 | 6.4996 | 7.2567 | 309.50 |
| T9 | 6 | 6 | 4 | 0.8137 | 4.9496 | 5.4428 | 255.43 |
| T4 | 7 | 12 | 1 | 0.7915 | 4.5804 | 3.1789 | 187.90 |
| T19 | 6 | 6 | 3 | 0.7890 | 4.3367 | 4.5368 | 228.41 |
| T13 | 6 | 8 | 3 | 0.7847 | 4.3252 | 4.5368 | 228.41 |
| T15 | 5 | 12 | 3 | 0.7837 | 3.6949 | 3.7809 | 205.88 |
| T21 | 4 | 4 | 2 | 0.7807 | 3.0551 | 2.4209 | 165.33 |
| T24 | 6 | 4 | 1 | 0.7711 | 4.2393 | 2.7249 | 174.38 |

## Standard BERT comparison

Standard BERT: MCC `0.8288`, 10.8845 GFLOPs, 9.6450 ms mean latency, and 417.66 MB model size.

T20 reaches MCC `0.8262`, while reducing GFLOPs by 8.3%, mean latency by 8.5%, and model size by 6.5% relative to Standard BERT.

T21 reaches 2.4209 GFLOPs and 3.0551 ms mean latency, corresponding to reductions of 77.8% and 68.3%, respectively, while retaining MCC `0.7807`.

## ONNX Runtime CPU results

| Model | Mean (ms) | P50 (ms) | P95 (ms) | P99 (ms) | Throughput (samples/s) |
|---|---:|---:|---:|---:|---:|
| Standard BERT | 165.49 | 160.28 | 183.61 | 249.11 | 6.04 |
| T20 | 144.87 | 144.66 | 148.26 | 156.65 | 6.90 |
| T9 | 78.90 | 76.65 | 86.02 | 123.36 | 12.67 |
| T4 | 52.48 | 52.32 | 54.66 | 56.66 | 19.05 |
| T21 | 34.41 | 34.09 | 36.01 | 44.49 | 29.07 |

T21 reduces mean CPU latency by approximately 79.2% and increases throughput by approximately 4.81× relative to Standard BERT in this benchmark.
