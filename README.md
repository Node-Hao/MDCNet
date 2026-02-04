# MDCNet: Manifold-aware Decoupled Consistency for Weakly Supervised Change Detection

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Framework](https://img.shields.io/badge/PyTorch-1.10%2B-orange)](https://pytorch.org/)

This repository contains the official implementation of the paper: **"MDCNet: Manifold-aware Decoupled Consistency for Weakly Supervised Change Detection"**, which has been submitted to **IEEE Transactions on Geoscience and Remote Sensing (TGRS)**.

> **Authors:** Qihao Liu, Ye Zhu, Jiaen Chen, Qingzhan Zhao, Yao Zheng, Yuchen Zheng

## 📢 News
- **[2026-02-04]** The repository is created.
- **[2026-02-04]** The paper is submitted to IEEE TGRS.
- **[Coming Soon]** Code and pretrained weights will be released upon acceptance.

## 📝 Abstract
Semi-supervised change detection (SSCD) mitigates annotation costs, yet prevailing paradigms remain constrained by their reliance on enforcing holistic feature consistency within unconstrained Euclidean embeddings. However, this paradigm is inherently fragile for remote sensing imagery, as unconstrained Euclidean metrics are susceptible to **magnitude-induced semantic bias** caused by seasonal spectral shifts, while holistic consistency permits the learning of **co-adapted, superficial patterns** in low-data regimes, collectively leading to unstable decision boundaries. 

To address these challenges, we propose the **Manifold-aware Decoupled Consistency Network (MDCNet)**, which integrates **Orthogonal Feature Perturbation (OFP)** and the **Balanced Global Spherical Contrastive Constraint (BGSCC)**. 
- Specifically, **OFP** partitions feature channels into mutually exclusive subspaces to force consistency across decoupled views, thereby preventing feature co-adaptation to superficial patterns. 
- Simultaneously, **BGSCC** projects features onto a unit hypersphere to eliminate seasonal magnitude variations and aligns representations with learnable prototypes, ensuring a geometrically stable manifold. 

Extensive experiments on four benchmark datasets (**LEVIR-CD, WHU-CD, CDD, S2Looking**) demonstrate that MDCNet significantly outperforms state-of-the-art semi-supervised methods. Notably, the framework exhibits superior generalization capabilities in cross-domain transfer scenarios, validating its capability to extract intrinsic, domain-invariant representations that persist across significant distribution shifts.

## 🚀 Framework
*(Suggestion: Place your `framework.pdf` or `framework.png` here to make the repo attractive)*

![MDCNet Framework](figs/framework.png)

## 📂 Datasets
We use the following datasets in our experiments:
- **LEVIR-CD**
- **WHU-CD**
- **CDD (Change Detection Dataset)**
- **S2Looking**

##  citation
If you find this work helpful for your research, please consider citing:

```bibtex
@article{MDCNet2026,
  title={MDCNet: Manifold-aware Decoupled Consistency for Weakly Supervised Change Detection},
  author={Liu, Qihao and Zhu, Ye and Chen, Jiaen and Zhao, Qingzhan and Zheng, Yao and Zheng, Yuchen},
  journal={Under Review at IEEE Transactions on Geoscience and Remote Sensing},
  year={2026}
}
