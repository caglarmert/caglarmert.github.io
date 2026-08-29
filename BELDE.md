---
layout: page
title: BELDE - Building a Large-scale Earth-observation Land-cover Dataset for Europe
---

# BELDE: Building a Large-scale Earth-observation Land-cover Dataset for Europe
**ECCV 2026** | [Ümit Mert Çağlar](https://scholar.google.com/citations?user=1bUVmLsAAAAJ&hl=en)<a class="orcid-link" href="https://orcid.org/0000-0002-0391-3847" target="_blank" rel="noopener" title="ORCID: 0000-0002-0391-3847"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" alt="ORCID iD"></a>, [Alptekin Temizel](https://scholar.google.com/citations?user=3grTeasAAAAJ&hl=en)<a class="orcid-link" href="https://orcid.org/0000-0001-6082-2573" target="_blank" rel="noopener" title="ORCID: 0000-0001-6082-2573"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" alt="ORCID iD"></a> | METU, Turkey

[![Paper](https://img.shields.io/badge/arXiv-2606.20909-b31b1b.svg)](https://arxiv.org/abs/2606.20909)
[![Code](https://img.shields.io/badge/Code-GitHub-181717.svg?logo=github)](https://github.com/caglarmert/BELDE)
[![Dataset](https://img.shields.io/badge/Dataset-BELDE-blue.svg)](https://huggingface.co/datasets/caglarmert/BELDE)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

> **TL;DR:** We introduce **BELDE**, a continental-scale RGB Earth-observation benchmark built from Sentinel-2 true-color imagery and ESA WorldCover maps, containing 1,088,385 curated 256×256 image-mask pairs across Europe at 10 m resolution. We evaluate 17 segmentation architectures and show that compact hybrid transformers (LALE-S2, 2.61M parameters) retain over 94% of the performance of a 100M-parameter model, while two out-of-domain extensions, BELDE-K (Korea) and BELDE-CA-NV (California-Nevada), reveal significant performance degradation under geographic domain shift.

![BELDE dataset creation and curation pipeline](belde2.jpg)

---

## Key Contributions

* **Large-Scale Curated Dataset:** BELDE, 1,088,385 geo-aligned RGB image-mask pairs across Europe at 10 m resolution, produced by an automated pipeline that prunes over 1.94 million non-informative, corrupted, or single-class dominated patches.
* **Efficient Model Distillation:** Demonstration that lightweight hybrid architectures (e.g., LALE-S2, 2.61M parameters) retain over 94% of the performance of 100M-parameter models, enabling deployment on bandwidth- and payload-constrained platforms.
* **Cross-Region Generalization Benchmarks:** Two out-of-domain extensions, BELDE-K (16,607 pairs, Republic of Korea) and BELDE-CA-NV (88,155 pairs, California-Nevada, USA), built with the same pipeline to quantify geographic domain shift.
* **Architectural Evaluation:** A comprehensive benchmark of 17 semantic segmentation architectures spanning CNN, transformer, and lightweight hybrid families, plus a data-scaling study showing that diffusion-based synthetic augmentation improves segmentation performance beyond full real-data capacity.

---

## The BELDE Dataset

BELDE is constructed from Sentinel-2 true-color imagery (TCI) and ESA WorldCover 2021 land-cover maps (77.9% regional label accuracy over Europe), covering 13°W–50°E and 33°N–60°N. The original 11 WorldCover classes are harmonized into 7 (tree, shrub, grass, crop, built-up, barren, water), and patches with missing data or more than 90% water coverage are discarded, pruning 1,941,727 patches in total.

| Dataset | Patches | Classes | Resolution | Patch Size | Multi-spectral |
|---|---|---|---|---|---|
| BigEarthNet | 549,488 | 19 | 10m | 120×120 | No |
| ARAS (Real) | 100,240 | 7 | 10m | 256×256 | No |
| EuroSAT | 27,000 | 10 | 10m | 64×64 | Yes |
| FLAIR | 77,762 | 19 | 0.2m | 512×512 | Yes |
| LoveDA | 5,987 | 7 | 0.3m | 1024×1024 | No |
| DeepGlobe | 1,146 | 7 | 0.5m | 2448×2448 | Yes |
| LandCover.ai | 10,674 | 4 | 0.5m | 512×512 | No |
| YieldSAT | 113,555 | 12 | 20m | Varied | Yes |
| **BELDE (ours)** | **1,088,385** | **7** | **10m** | **256×256** | **No** |
| **BELDE-K (ours)** | **16,607** | **7** | **10m** | **256×256** | **No** |
| **BELDE-CA-NV (ours)** | **88,155** | **7** | **10m** | **256×256** | **No** |

---

## Semantic Segmentation Results

We evaluated 17 architectures spanning CNN (DeepLabV3, DeepLabV3+, FPN, LinkNet, PSPNet, UNet, UNet++), transformer (SegFormer, DeiT3, MaxViT) and lightweight hybrid (EfficientFormer, FastViT, LALE) families, ranging from 1.5M to 117M parameters, all trained under an identical AdamW + Dice-loss protocol.

| Architecture | F1 | IoU | Params |
|---|---|---|---|
| DeepLabV3 | 80.2 | 69.2 | 33.06M |
| DeepLabV3+ | 81.2 | 70.4 | 29.49M |
| FPN | 81.1 | 70.4 | 30.17M |
| LinkNet | 81.0 | 70.2 | 28.75M |
| PSPNet | 77.9 | 66.2 | 28.44M |
| SegFormer | 81.1 | 70.5 | 28.89M |
| UNet | 81.8 | 71.3 | 31.22M |
| UNet++ | 81.7 | 71.2 | 31.91M |
| DeiT3 | 81.5 | 71.0 | 117.08M |
| EfficientFormer-L1 | 81.6 | 71.1 | 29.83M |
| EfficientFormer-L3 | 82.5 | 72.3 | 49.13M |
| **EfficientFormer-L7** | **83.0** | **72.8** | 100.32M |
| FastViT-mci0 | 81.6 | 71.1 | 29.07M |
| FastViT-sa12 | 82.2 | 71.8 | 29.24M |
| MaxViT | 82.9 | 72.8 | 60.80M |
| LALE-S3 | 78.4 | 66.8 | 3.66M |
| **LALE-S2** | 78.2 | 66.5 | **2.61M** |

**Key Takeaways:**
1. **Transformers Lead, Lightweight Models Compete:** EfficientFormer-L7 (100.3M params) and MaxViT (60.8M params) achieve the highest F1-scores (83.0% and 82.9%), while LALE-S2 reaches 78.2% F1 with only 2.61M parameters, retaining 94.2% of peak performance at 38.4× fewer parameters.
2. **In-Domain Performance Nears a Label-Noise Ceiling:** The narrow 1.5% IoU spread between CNN and transformer baselines suggests in-domain accuracy is bounded by the ~77.9% accuracy of the underlying WorldCover pseudo-labels.
3. **Shrub is the Hardest Class:** Spectrally homogeneous classes (tree, water) segment well across all models, while the rare and visually ambiguous shrub class remains the most challenging under RGB-only observation.

---

## Cross-Region Generalization

<figure align="center">
  <img src="belde1.jpg" alt="Cross region generalization" width="85%">
  <figcaption><b>Figure:</b> Experimental setup training on BELDE (Europe) and evaluating zero-shot on BELDE-K (Korea) and BELDE-CA-NV (California-Nevada).</figcaption>
</figure>

Models trained exclusively on BELDE (Europe) were evaluated zero-shot on BELDE-K and BELDE-CA-NV. All architectures show consistent performance degradation under domain shift, MaxViT and EfficientFormer-L7 lead on both extensions (58.2%/58.3% F1 on BELDE-K, 66.4%/66.2% F1 on BELDE-CA-NV), while LALE models trade some accuracy for stability and efficiency. This confirms that RGB-only segmentation models rely heavily on region-specific spatial and textural cues, motivating geographically diverse benchmarks like BELDE.

| Architecture | BELDE-K F1 | BELDE-CA-NV F1 |
|---|---|---|
| UNet | 56.6 | 65.1 |
| UNet++ | 56.5 | 65.1 |
| SegFormer | 55.4 | 65.0 |
| MaxViT | **58.2** | **66.4** |
| EfficientFormer-L7 | **58.3** | 66.2 |
| LALE-S3 | 52.1 | 60.9 |
| LALE-S2 | 51.3 | 60.3 |

For reference, in-domain (European) F1 scores range from 77.9-83.0%, so even the best zero-shot transfer models lose roughly 20-25 F1 points when moved to a different continent.

We further evaluated data scaling and diffusion-based synthetic augmentation on a BELDE subset: real-data scaling alone reaches 74.66% F1 at full capacity (80,000 pairs), and adding synthetic samples from a latent diffusion model conditioned on segmentation masks improves this further to 75.68% F1 (+1.02%), showing that generative augmentation provides utility beyond real-data availability.

**Compute:** 211 training runs totaling 1,077 GPU-hours on NVIDIA H100s — 544 hours for architecture search/conceptualization, 206 hours for the California-Nevada extension, 122 hours for the Korea extension, and the remainder split across the CNN, transformer, and lightweight architecture benchmarks. All results are reported as mean ± standard deviation over 3 repeated runs.

---

## Dataset Availability and Reproducibility

The datasets and software framework created and used throughout this work are publicly available under the Creative Commons Attribution 4.0 International license ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)).

| Resource | Link |
| :--- | :--- |
| **BELDE** | [datasets/caglarmert/BELDE](https://huggingface.co/datasets/caglarmert/BELDE) |
| **BELDE-K** | [datasets/caglarmert/BELDE-K](https://huggingface.co/datasets/caglarmert/BELDE-K) |
| **BELDE-CA-NV** | [datasets/caglarmert/BELDE-CA-NV](https://huggingface.co/datasets/caglarmert/BELDE-CA-NV) |
| **Source Code** | [github.com/caglarmert/BELDE](https://github.com/caglarmert/BELDE) |

---

## Citation

If you find this dataset or codebase useful in your research, please consider citing:

```bibtex
@article{caglar2026belde,
  title={BELDE: Building a Large-scale Earth-observation Land-cover Dataset for Europe},
  author={Caglar, Umit Mert and Temizel, Alptekin},
  journal={arXiv preprint arXiv:2606.20909},
  year={2026}
}
```
