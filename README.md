# MDCNet: Manifold-aware Decoupled Consistency Network for Semi-Supervised Remote Sensing Change Detection

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Framework](https://img.shields.io/badge/Framework-PyTorch-orange.svg)](https://pytorch.org/)

Official project repository for **MDCNet**, a framework for semi-supervised remote sensing change detection.

**Authors**  
Qihao Liu, Ye Zhu, Jiaen Chen, Qingzhan Zhao, Yao Zheng, and Yuchen Zheng

Qihao Liu and Ye Zhu contributed equally. Corresponding authors are Yuchen Zheng and Yao Zheng.

The manuscript has been submitted to **IEEE Transactions on Geoscience and Remote Sensing (TGRS)**. This README describes the revised manuscript and its reported experiments.

## News

- **[2026-10-22]** Updated the method overview and experimental results to match the revised manuscript, including five benchmarks, repeated evaluations with 5% labels, and source-only zero-shot transfer.
- **[2026-08-12]** Released reproducibility materials for selected CDD baseline results.
- **[2026-02-04]** Created the repository and submitted the manuscript to IEEE TGRS.

## Abstract

Semi-supervised remote sensing change detection must distinguish target changes from background appearance differences using limited pixel-level annotations. This requires specifying the temporal information retained during feature perturbation and the relations used to organize background samples. We propose the Manifold-aware Decoupled Consistency Network (MDCNet), which separates prediction regularization under controlled feature variations from relation learning on clean background features. Difference-Preserving Feature Perturbation (DPFP) adds a shared offset to both temporal endpoints within each stream, varying their common component while exactly preserving their high-level difference before nonlinear projection. Labeled supervision and unlabeled consistency regularize prediction sensitivity to this variation. The Balanced Global Spherical Contrastive Constraint (BGSCC) represents background pairs on a product of two unit hyperspheres. Paired teacher prototypes use both dates to identify compatible background samples and guide positive retrieval and interaction weighting. The perturbed prediction and clean feature paths jointly optimize the shared encoder. Across five benchmarks, including season-varying scenes and cropland changes, MDCNet achieves the highest F1-score and IoU among the compared methods with 5%, 10%, and 20% labeled data. With 5% labels, it improves mean F1 by 8.79 and 10.87 percentage points over the strongest compared baselines on CDD and CLCD, respectively. MDCNet also achieves the highest F1-score and IoU in both source-only zero-shot transfer directions between LEVIR-CD and WHU-CD.

## Method Overview

MDCNet combines two complementary constraints that jointly optimize a shared student encoder.

### Difference-Preserving Feature Perturbation (DPFP)

Within each student stream, DPFP adds the same spatially broadcast offset to both high-level temporal features. For stream $v$ and acquisition date $t$, the construction satisfies

```math
\widetilde{\mathbf{H}}_{t,v}
=\mathbf{H}_{t,v}+\boldsymbol{\delta}_v,
\qquad t\in\{1,2\},
```

and therefore

```math
\widetilde{\mathbf{H}}_{2,v}-\widetilde{\mathbf{H}}_{1,v}
=\mathbf{H}_{2,v}-\mathbf{H}_{1,v}.
```

This preserves the observed high-level difference at the injection layer, before nonlinear projection. The decoder can still respond to the common offset. Ground-truth supervision, teacher supervision, and mutual prediction consistency train stability under this controlled variation.

### Balanced Global Spherical Contrastive Constraint (BGSCC)

BGSCC organizes background relations using paired normalized features on a product of two unit hyperspheres. Each prototype retains both temporal endpoints. The dictionary is maintained from valid labeled teacher background pairs, with subsequent prototype updates using exponential moving averages.

Matching both dates guides orientation-aligned positive retrieval, confidence-dependent candidate-interaction weighting, and mode-informed uniformity. These constraints operate on clean student features before DPFP. Here, clean means without feature perturbation, while input augmentation remains active.

### Training and Inference

Training uses an EMA teacher and a student with two prediction streams. The 100-epoch schedule begins with 30 epochs using labeled data, followed by 70 epochs using both labeled and unlabeled data. DPFP and BGSCC are introduced according to the activation and ramp schedules specified in the manuscript.

Inference uses the selected EMA teacher's clean prediction path. DPFP sampling and BGSCC auxiliary operations are inactive during inference.

## Datasets

The revised evaluation covers five benchmarks. All counts below refer to the processed $256\times256$ image pairs used in the reported experiments.

| Dataset | Main scene characteristics | Train | Validation | Test |
| --- | --- | ---: | ---: | ---: |
| LEVIR-CD | Building changes | 7,120 | 1,024 | 2,048 |
| WHU-CD | Building changes | 6,096 | 762 | 762 |
| CDD | Changes in season-varying scenes | 10,000 | 3,000 | 3,000 |
| S2Looking | Building changes in off-nadir rural imagery | 4,328 | 607 | 1,159 |
| CLCD | Cropland changes | 1,440 | 480 | 480 |

- LEVIR-CD uses the official partition, followed by non-overlapping cropping.
- WHU-CD uses the fixed partition reported in the manuscript.
- S2Looking combines newly built and demolished buildings into a single changed class. The reported results use the filtered subset following the CutMix-CD preprocessing protocol, which removes patches without change labels.
- CLCD is partitioned before cropping, with patches generated separately within each partition.

Please obtain the original data from the respective dataset providers and follow the manuscript's preprocessing and partitioning protocols.

## Results

### Standard Benchmarks

All metrics are percentages. At 5% labels, F1 is reported as mean ± sample standard deviation over six matched labeled subsets, and IoU is the corresponding mean. The training seed is held fixed across these six subsets. The 10% and 20% settings use a fixed split and training seed of 3278.

| Dataset | 5% F1 | 5% IoU | 10% F1 | 10% IoU | 20% F1 | 20% IoU |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| LEVIR-CD | 89.97 ± 0.11 | 81.76 | 90.60 | 82.81 | 90.90 | 83.32 |
| WHU-CD | 93.81 ± 0.28 | 88.34 | 93.98 | 88.64 | 93.91 | 88.52 |
| CDD | 90.56 ± 0.23 | 82.75 | 93.11 | 87.10 | 95.30 | 91.02 |
| S2Looking | 62.46 ± 0.31 | 45.41 | 64.17 | 47.24 | 64.88 | 48.02 |
| CLCD | 68.66 ± 0.28 | 52.28 | 71.08 | 55.14 | 74.31 | 59.13 |

MDCNet achieves the highest F1 and IoU among the compared methods in all 15 evaluated dataset-and-label-ratio settings. At 5% labels, the mean F1 gains are 8.79 percentage points over CutMix-CD on CDD and 10.87 points over Sup-only on CLCD. Sup-only is MDCNet's teacher-model result after the 30-epoch labeled-only stage.

### Source-Only Zero-Shot Transfer

The revised manuscript evaluates both **LEVIR-CD → WHU-CD** and **WHU-CD → LEVIR-CD** with 5%, 10%, and 20% source labels. Each setting uses three training seeds with a fixed source split. Source-trained checkpoints are applied directly to the target domain at a threshold of 0.5. Target-domain data are reserved for final evaluation and visualization and are not used for training or model selection.

MDCNet achieves the highest F1 and IoU among the compared methods in both directions at all three evaluated source-label ratios. See the manuscript for the complete transfer results and checkpoint-selection protocols.

## Code and Reproducibility Materials

Code and reproducibility materials are being organized and released progressively. The external materials listed below cover selected CDD baseline results. Training and evaluation instructions for released implementations should be taken from their accompanying README files.

### External Download

Large checkpoint and reproducibility packages are hosted on Baidu Netdisk.

- [Download the reproducibility materials](https://pan.baidu.com/s/1H5GahV45CxKuJNzENDAXGA?pwd=eq7d)
- Extraction code `eq7d`

The shared folder `MDCNet_data` contains CDD reproducibility packages for the following methods.

- CutMix-CD
- RCL
- RCR (SemiCD)
- C2F-SemiCD
- FPA
- UniMatch

Depending on the package, materials include evaluation code, dataset split files or split-generation notes, trained checkpoints, training logs, test metrics, file inventories, and reproducibility instructions. Please consult each package's README for its evaluation setting and contents.

### CDD Result Updates

An earlier manuscript table interchanged the F1 and IoU entries for C2F-SemiCD on CDD with 5% and 10% labels. The revised manuscript corrects these entries and reports repeated evaluations at 5% labels.

| Dataset | Method | Labeled ratio | Precision | Recall | F1 | IoU |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| CDD | C2F-SemiCD | 5% | 81.91 | 60.31 | 69.47 ± 0.26 | 53.22 |
| CDD | C2F-SemiCD | 10% | 87.06 | 61.78 | 72.27 | 56.58 |

The 5% row contains averages over six labeled subsets. Archived single-subset evaluations should be compared with their corresponding split, rather than directly with these means. Precision, recall, F1, and IoU are computed for each evaluation before averaging, so the mean F1 and IoU should not be reconstructed from averaged precision and recall.

## Citation

If you use this work, please cite the manuscript using the following entry until publication details are available.

```bibtex
@misc{liu2026mdcnet,
  title = {{MDCNet}: Manifold-aware Decoupled Consistency Network for Semi-Supervised Remote Sensing Change Detection},
  author = {Liu, Qihao and Zhu, Ye and Chen, Jiaen and Zhao, Qingzhan and Zheng, Yao and Zheng, Yuchen},
  year = {2026},
  note = {Manuscript submitted to IEEE Transactions on Geoscience and Remote Sensing},
  url = {https://github.com/Node-Hao/MDCNet}
}
```

## License

This project is released under the MIT License.
