# MDCNet: Manifold-aware Decoupled Consistency for Semi-Supervised Change Detection

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Framework](https://img.shields.io/badge/PyTorch-1.10%2B-orange)](https://pytorch.org/)

This repository contains the official implementation of the paper:

**MDCNet: Manifold-aware Decoupled Consistency for Semi-Supervised Change Detection**

The manuscript has been submitted to **IEEE Transactions on Geoscience and Remote Sensing (TGRS)** and is currently under revision for resubmission.

> **Authors:** Qihao Liu, Ye Zhu, Jiaen Chen, Qingzhan Zhao, Yao Zheng, Yuchen Zheng

## 📢 News

- **[2026-02-04]** The repository was created.
- **[2026-02-04]** The paper was submitted to IEEE TGRS.
- **[2026-08-10]** The manuscript received a reject-and-resubmit decision from IEEE TGRS.
- **[2026-08-12]** Reproducibility materials for verifying the CDD results were released.

## 📝 Abstract

Semi-supervised change detection (SSCD) mitigates annotation costs, yet prevailing paradigms remain constrained by their reliance on enforcing holistic feature consistency within unconstrained Euclidean embeddings. However, this paradigm is inherently fragile for remote sensing imagery, as unconstrained Euclidean metrics are susceptible to **magnitude-induced semantic bias** caused by seasonal spectral shifts, while holistic consistency permits the learning of **co-adapted superficial patterns** in low-data regimes, collectively leading to unstable decision boundaries.

To address these challenges, we propose the **Manifold-aware Decoupled Consistency Network (MDCNet)**, which integrates **Orthogonal Feature Perturbation (OFP)** and the **Balanced Global Spherical Contrastive Constraint (BGSCC)**.

- **OFP** partitions feature channels into mutually exclusive subspaces and enforces consistency across decoupled views, thereby reducing feature co-adaptation to superficial patterns.
- **BGSCC** projects features onto a unit hypersphere to suppress magnitude-induced variations and aligns representations with learnable global prototypes, yielding a geometrically stable feature manifold.

Extensive experiments on four benchmark datasets, including **LEVIR-CD**, **WHU-CD**, **CDD**, and **S2Looking**, demonstrate that MDCNet achieves competitive or superior performance compared with existing semi-supervised change detection methods. The proposed framework also shows strong generalization capability in cross-domain transfer scenarios, indicating its ability to learn more intrinsic and domain-invariant representations.

## 📂 Datasets

We conduct experiments on the following benchmark change detection datasets:

- **LEVIR-CD**
- **WHU-CD**
- **CDD**
- **S2Looking**

Please follow the official dataset providers for downloading and preparing these datasets. Dataset split files used for reproducibility are provided when applicable.

## 📦 Reproducibility Materials

Due to GitHub file size limitations, large checkpoint and reproducibility packages are hosted externally.

**Baidu Netdisk:**  
Link: https://pan.baidu.com/s/1H5GahV45CxKuJNzENDAXGA?pwd=eq7d  
Extraction code: `eq7d`

The shared folder `MDCNet_data` contains reproducibility packages for verifying the CDD experimental results. The packages include, where available:

- evaluation code;
- dataset split files or split-generation notes;
- trained checkpoints;
- training logs;
- test metrics;
- file inventories;
- reproducibility README files.

The currently provided CDD reproducibility packages cover the following methods:

- CutMixCD
- RCL
- RCR
- C2F-SemiCD
- FPA
- UniMatch

These materials are provided to facilitate independent verification of the quantitative results reported in the manuscript.

## 🔎 CDD Table Correction Note

In the previous manuscript version, the F1 and IoU values of **C2F-SemiCD** on the **CDD** dataset under the **5%** and **10%** labeled settings were inadvertently swapped during table formatting. This was a table-entry error rather than an experimental or evaluation-code error.

The corrected values are:

| Dataset | Method | Labeled Ratio | Precision | Recall | F1 | IoU |
|---|---|---:|---:|---:|---:|---:|
| CDD | C2F-SemiCD | 5% | 81.64 | 59.99 | 69.16 | 52.85 |
| CDD | C2F-SemiCD | 10% | 87.06 | 61.78 | 72.27 | 56.58 |

The correction can be directly verified using:

```math
F1 = \frac{2 \times Precision \times Recall}{Precision + Recall}
and
IoU = \frac{F1}{2 - F1}
where Precision, Recall, F1, and IoU are expressed in decimal form.
🚀 Code Release
The code and reproducibility materials are being progressively organized and released. The current repository focuses on supporting transparent verification of the reported results, especially the CDD results discussed in the manuscript revision.
📌 Citation
If you find this work helpful for your research, please consider citing:
@article{MDCNet2026,
  title={MDCNet: Manifold-aware Decoupled Consistency for Semi-Supervised Change Detection},
  author={Liu, Qihao and Zhu, Ye and Chen, Jiaen and Zhao, Qingzhan and Zheng, Yao and Zheng, Yuchen},
  journal={IEEE Transactions on Geoscience and Remote Sensing, under revision},
  year={2026}
}
📄 License
This project is released under the MIT License.
