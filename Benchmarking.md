---
layout: page
title: Benchmarking the Alignment of Data-Quality Metrics
---

# Benchmarking the Alignment of Data-Quality Metrics, Human Judgment and Land-Cover Segmentation Performance for Earth Observation
**ECCV 2026 (Workshop)** | [Ümit Mert Çağlar](https://scholar.google.com/citations?user=1bUVmLsAAAAJ&hl=en)<a class="orcid-link" href="https://orcid.org/0000-0002-0391-3847" target="_blank" rel="noopener" title="ORCID: 0000-0002-0391-3847"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" alt="ORCID iD"></a>, [Alptekin Temizel](https://scholar.google.com/citations?user=3grTeasAAAAJ&hl=en)<a class="orcid-link" href="https://orcid.org/0000-0001-6082-2573" target="_blank" rel="noopener" title="ORCID: 0000-0001-6082-2573"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" alt="ORCID iD"></a> | METU, Turkey

[![Paper](https://img.shields.io/badge/arXiv-2606.25128-b31b1b.svg)](https://arxiv.org/abs/2606.25128)
[![Dataset](https://img.shields.io/badge/Datasets-ARAS400k_%7C_BELDE-blue.svg)](https://huggingface.co/caglarmert)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

> **TL;DR:** Automated quality metrics (FID, KID, IS, LPIPS, SSIM) are the default tool for filtering synthetic Earth observation data, but we show they can be fooled: semantics-preserving perturbations like rotation drastically alter metric scores while leaving human recognition unaffected, and synthetic samples that score poorly on these metrics can look just as realistic to humans and still improve downstream segmentation performance when blended with real data. Automatic evaluation of synthetic EO data cannot rely on latent metrics alone, it needs joint consideration of downstream utility and human perception.

![Image transformations and perturbations](bench1.jpg)

---

## Key Contributions

* **Pitfalls of Automated Data Pruning:** Standard latent-space metrics exhibit high volatility under spatial augmentations, proving them unreliable for automated data pruning or synthetic quality assessment.
* **Human Perception Study:** A four-stage study with 95 participants (88 retained after quality-control filtering) and 5,768 total responses, quantifying recognition accuracy, visual preference, and cognitive load to provide a human baseline for generative data quality.
* **Three-Way Alignment Analysis:** Major disconnections among automated quality metrics, human visual preference, and downstream model performance, highlighting the misalignment between fidelity, preference, and utility.
* **Downstream Utility Benchmark:** Synthetic datasets with poor fidelity scores act as highly effective additions when blended with real data, yielding performance gains that standard quality metrics fail to predict.

---

## Datasets Evaluated

All patches are 256×256 pixels at 10 m spatial resolution with seven shared land-cover classes.

| Dataset | Type | Generator | Region | Patches |
|---|---|---|---|---|
| ARAS-train / ARAS-test | Real | - | Türkiye | 80,192 / 10,024 |
| BELDE | Real | - | Europe | 1,088,385 |
| BELDE-K | Real | - | Rep. of Korea | 16,607 |
| BELDE-CA-NV | Real | - | California-Nevada | 88,155 |
| ARAS-CSD / BELDE-CSD | Conditional | Stable Diffusion | Türkiye | 80,192 each |
| ARAS-CUGAN | Conditional | U-Net GAN | Türkiye | 80,192 |
| ARAS-SGAN3 / ARAS-SGAN3-D | Unconditional | StyleGAN3 | Türkiye | 300,000 / 100,000 |

---

## Human Perception and Evaluation Pipeline
<figure align="center">
  <img src="bench2.jpg" alt="Human Perception Study" width="85%">
  <figcaption><b>Figure:</b> Human perception study screens for (a) data augmentation alignment, (b) utility for downstream tasks, (c) conditional generation preference and (d) data realism scores.</figcaption>
</figure>

To evaluate the alignment between automated quality metrics, human perception, and downstream data utility, the study is structured into four sequential phases:

1. **Augmentation Alignment:** Participants judge whether a baseline image and its transformed variant depict the same scene. Human accuracy stays above 90% for rotation, perspective, flip, and resized-crop, dropping to 79.2% under noise and 56.6% under combined perturbation, even though these same transforms can swing FID from 2.09 to over 100.
2. **Utility for Downstream Tasks:** Participants judge whether an Earth observation image matches its land-cover segmentation map. Real and synthetic pairs are verified with comparable accuracy (72.9% vs. 72.4%), showing synthetic masks are as interpretable as real ones.
3. **Conditional Generation Preference:** Given a real image and its segmentation mask, participants pick the most consistent synthetic output. Stable Diffusion trained on ARAS400k (ARAS-CSD) is preferred most (39%), followed by BELDE-CSD (33%) and ARAS-CUGAN (28%).
4. **Data Realism Scores:** Participants rate individual images on a 5-point Likert scale. The synthetic ARAS-SGAN3 dataset (μ = 3.43) is rated *more* realistic than two of the four real datasets, BELDE (μ = 3.32) and BELDE-CA-NV (μ = 3.09), despite having a substantially worse FID.

## Downstream Utility vs. Metrics and Perception

Augmenting the real ARAS400k baseline with CUGAN-generated data degrades FID from 2.09 to 21.27, yet paradoxically *improves* downstream segmentation F1 from 76.5% to 77.6%. Likewise, participants rated BELDE-K as more realistic than BELDE-CA-NV (3.42 vs. 3.09 HPQS), yet segmentation models score substantially higher F1 on BELDE-CA-NV (66.2%) than BELDE-K (58.3%). Neither automatic metrics nor human perception reliably predicts downstream utility on their own.

## Guidelines for Data-Centric Synthetic Curation

Based on these findings, we propose the following practical guidelines for dataset curation and sample pruning:

* **Avoid metric-only dataset pruning:** Isolated quality metrics heavily penalize spatial features that deep segmentation models utilize.
* **Account for orientation bias in filtering:** Feature extractors used in data quality metrics are orientation-sensitive, leading to false pruning triggers under standard geometric augmentations.
* **Prioritize task-aware utility over fidelity:** Curation pipelines must complement generic quality metrics with direct downstream task evaluation.

---

## Dataset Availability and Reproducibility
<figure align="center">
  <img src="bench3.jpg" alt="Image generation comparison of different models" width="85%">
  <figcaption><b>Figure:</b> Image generation comparison of conditional Stable Diffusion for BELDE-trained (BELDE-CSD), ARAS-trained (ARAS-CSD) and conditional U-Net GAN (ARAS-CUGAN) with the real and conditioning images (segmentation).</figcaption>
</figure>

The datasets used throughout this work are publicly available under the Creative Commons Attribution 4.0 International license ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)).

| Resource | Link |
| :--- | :--- |
| **BELDE** | [datasets/caglarmert/BELDE](https://huggingface.co/datasets/caglarmert/BELDE) |
| **BELDE-K** | [datasets/caglarmert/BELDE-K](https://huggingface.co/datasets/caglarmert/BELDE-K) |
| **BELDE-CA-NV** | [datasets/caglarmert/BELDE-CA-NV](https://huggingface.co/datasets/caglarmert/BELDE-CA-NV) |
| **ARAS400k** | [zenodo/records/18890661](https://zenodo.org/records/18890661) |
| **ARAS-CSD** | [datasets/caglarmert/ARAS-CSD-2](https://huggingface.co/datasets/caglarmert/ARAS-CSD-2) |
| **BELDE-CSD** | [datasets/caglarmert/ARAS-CSD-1E7](https://huggingface.co/datasets/caglarmert/ARAS-CSD-1E7) |
| **ARAS-CUGAN** | [datasets/caglarmert/ARAS-C-UNetGAN](https://huggingface.co/datasets/caglarmert/ARAS-C-UNetGAN) |

---

## Citation

If you find this work useful in your research, please consider citing:

```bibtex
@article{caglar2026benchmarking,
  title={Benchmarking the Alignment of Data-Quality Metrics, Human Judgment and Land-Cover Segmentation Performance for Earth Observation},
  author={Caglar, Umit Mert and Temizel, Alptekin},
  journal={arXiv preprint arXiv:2606.25128},
  year={2026}
}
```
