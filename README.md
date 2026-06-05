# STDPatch

> STDPatch: A Three-Stream Framework for Long-Term Time Series Forecasting via SG-Filter-Based Decomposition and Patch Reconstruction

## Introduction

STDPatch is a novel framework for Long-Term Time Series Forecasting (LTSF) that combines:

* SG-Filter-based Seasonal-Trend Decomposition
* Fine-grained Trend Decomposition
* Adaptive Patch Reconstruction
* Three-Stream Neural Architecture

Unlike traditional decomposition approaches based on Moving Average or EMA smoothing, STDPatch employs a Savitzky-Golay (SG) filter to preserve trend shape while reducing seasonal leakage. The framework further decomposes trend signals into ascending and descending sub-trends and introduces an adaptive patch reconstruction strategy to improve semantic continuity.

## Framework Overview

The overall architecture consists of four major components:

### 1. SG-Filter Seasonal-Trend Decomposition

The original sequence is decomposed into:

* Trend Component
* Seasonal Component

Advantages:

* Better preservation of local extrema
* Reduced seasonal residuals in trend signals
* Shape-preserving smoothing

### 2. Trend Decomposition Module

The extracted trend is further separated into:

* Upward Trend Segment
* Downward Trend Segment

This enables the model to capture:

* Acceleration patterns
* Deceleration patterns
* High-order temporal dynamics

### 3. Patch Reconstruction Module

Traditional fixed-length patching often destroys semantic continuity.

Our adaptive reconstruction module:

* Computes cosine similarity between neighboring patches
* Generates learnable hard masks
* Aggregates structurally similar patches
* Suppresses irrelevant long-range correlations

### 4. Three-Stream Architecture

Different neural architectures are assigned to different temporal characteristics:

| Component          | Network     |
| ------------------ | ----------- |
| Seasonal Component | CNN         |
| Global Trend       | MLP         |
| Sub-Trend Dynamics | Transformer |

This architecture explicitly aligns model inductive biases with the statistical properties of decomposed signals.

---

## Model Architecture

```text
Input Series
      │
      ▼
SG Filter Decomposition
      │
 ┌────┴────┐
 ▼         ▼
Trend   Seasonal
 │
 ▼
Trend Decomposition
 │
 ├─────────────┐
 ▼             ▼
Up Trend   Down Trend
 │             │
 └─────Transformer─────┘

Seasonal ───► Patch Reconstruction ─► CNN

Trend ─────────────────────────────► MLP

        ▼
Feature Fusion
        ▼
 Prediction
```

---

## Features

* Shape-preserving SG-filter decomposition
* Fine-grained trend modeling
* Adaptive patch reconstruction
* Three-stream hybrid architecture
* Strong robustness against non-stationary time series
* State-of-the-art forecasting performance

---

## Experimental Datasets

Experiments are conducted on seven benchmark datasets:

* ETTh1
* ETTh2
* ETTm1
* ETTm2
* Weather
* Electricity
* Traffic

Evaluation metrics:

* Mean Squared Error (MSE)
* Mean Absolute Error (MAE)

---

## Main Results

STDPatch achieves state-of-the-art performance on most benchmark datasets.

Highlights:

* Best MSE performance on the majority of forecasting horizons.
* Consistent improvements over:

  * PatchTST
  * DLinear
  * MICN
  * TimesNet
  * MSGNet
  * iTransformer
  * TimeMixer
  * xPatch
  * TimeKAN

The framework demonstrates strong generalization ability under both standard and hyperparameter-search settings.

---

## Installation

```bash
git clone https://github.com/ll18528335432-commits/STDPatch.git

cd STDPatch

```

## Training

Example:

```bash
python run.py \
  --model STDPatch \
  --dataset ETTh1 \
  --seq_len 96 \
  --pred_len 96
```

---

## Testing

```bash
python run.py \
  --is_training 0 \
  --model STDPatch \
  --dataset ETTh1
```

---


## Citation

If you find this work useful, please cite:

```bibtex
@article{li2025stdpatch,
  title={STDPatch: A Three-Stream Framework for Long-Term Time Series Forecasting via SG-Filter-Based Decomposition and Patch Reconstruction},
  author={Li, Lanlan and Liu, Di and Miao, Shenfa and Zahir, Ahmed and Mu, Yongkang and Deng, Hualong and Jin, Xin and Jiang, Qian and Wang, Puming and Jang, Hua and Yao, Shaowen},
  journal={Elsevier},
  year={2025}
}
```

---

## License

This project is released under the MIT License.

---

## Acknowledgements

This work is developed by researchers from:

* Yunnan University
* Engineering Research Center of Cyberspace
* Norwegian University of Science and Technology (NTNU)

We thank the authors of PatchTST, TimeMixer, xPatch, TimeKAN, TimesNet, DLinear, MICN, and iTransformer for their open-source contributions.

