---
layout: page
title: LALE - Lightweight-Transformer Architecture for Land-Cover Estimation
---

# LALE: Lightweight-Transformer Architecture for Land-Cover Estimation
**ICMV 2026** | [Ümit Mert Çağlar](https://scholar.google.com/citations?user=1bUVmLsAAAAJ&hl=en)<a class="orcid-link" href="https://orcid.org/0000-0002-0391-3847" target="_blank" rel="noopener" title="ORCID: 0000-0002-0391-3847"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" alt="ORCID iD"></a>, [Alptekin Temizel](https://scholar.google.com/citations?user=3grTeasAAAAJ&hl=en)<a class="orcid-link" href="https://orcid.org/0000-0001-6082-2573" target="_blank" rel="noopener" title="ORCID: 0000-0001-6082-2573"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" alt="ORCID iD"></a> | METU, Turkey

[![Paper](https://img.shields.io/badge/arXiv-2606.02092-b31b1b.svg)](https://arxiv.org/abs/2606.02092)
[![Code](https://img.shields.io/badge/Code-GitHub-181717.svg?logo=github)](https://github.com/caglarmert/LALE)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

> **TL;DR:** We introduce **LALE**, a hybrid convolution-transformer segmentation architecture that bifurcates its encoder by resolution, lightweight ConvMixer stages handle high-resolution local features, while transformer stages confine the quadratic cost of self-attention to deep, downsampled feature maps. Combined with an all-MLP multi-scale decoder and efficient RMSNorm/StarReLU operations, our smallest variant (1.6M parameters) reaches within 2.6 F1 points of the best baseline (UPerNet) on ARAS400k while using 4.5× fewer parameters, 7× less storage and 17× fewer GMACs.

![LALE architecture: convolution stem, convolution mixers, transformer blocks and multi-scale decoder](lale2.png)

---

## Key Contributions

* **Resolution-Bifurcated Hybrid Encoder:** A lightweight attention-convolution hybrid hierarchical architecture where ConvMixer blocks handle high-resolution local feature extraction (stages 1–2) and transformer blocks handle low-resolution global context (stages 3–4), confining self-attention's O(N²) cost to the smallest feature maps.
* **Lightweight Multi-Scale Decoder:** An all-MLP decoder that fuses multi-scale features via pointwise convolutions and bilinear upsampling, avoiding parameter-heavy upsampling heads.
* **Efficient Operation-Level Choices:** RMSNorm in place of LayerNorm, StarReLU in place of GELU, and 3×3 stride-2 convolutions in place of 7×7 kernels, together reducing compute and memory footprint without sacrificing accuracy.
* **Comprehensive Benchmark:** An extended reference benchmark on ARAS400k spanning CNN, transformer, and hybrid architectures, plus a generalization study on the LiTS liver/tumor segmentation benchmark, establishing LALE on a favorable accuracy-efficiency Pareto frontier.

---

## Architecture

LALE bifurcates its four-stage encoder by resolution: a convolutional stem (two 3×3 stride-2 convolutions with RMSNorm and StarReLU) reduces the input by a factor of four, followed by two ConvMixer stages for local feature extraction and two transformer stages (Multi-Head Self-Attention + ConvMLP) for global context modeling at the deepest, lowest-resolution feature maps. A lightweight multi-scale decoder projects each stage's features to a unified 128-channel dimension, upsamples and concatenates them, and predicts per-pixel class logits through a final 1×1 convolution.

---

## Results on ARAS400k

We benchmark LALE against CNN baselines (EfficientNet-backboned DeepLabV3, DeepLabV3+, FPN, LinkNet, PAN, UNet, UNet++, UPerNet, SegFormer) and dense-prediction transformers (EfficientFormer, DeiT3, MaxViT, FastViT) on the [ARAS400k](ARAS400k.html) segmentation benchmark.

| Architecture | F1 | IoU | Params (M) | GMACs |
|---|---|---|---|---|---|
| UPerNet | 77.31 | 65.42 | 11.6 | 13.62 |
| Unet | 77.23 | 65.28 | 6.3 | 3.05 |
| Segformer | 76.47 | 64.44 | 4.5 | 2.05 |
| DeiT3-Base | 76.10 | 63.97 | 117.1 | 39.89 |
| MaxViT-Tiny | 75.82 | 63.64 | 60.8 | 33.13 |
| **LALE-S2** | **75.88** | **63.67** | **2.6** | **0.78** |
| **LALE-S1** | **74.69** | **62.25** | **1.6** | **0.59** |

**Key Takeaways:**
1. **Strong Efficiency-Performance Trade-off:** LALE-S1 (1.6M params) trails the best CNN baseline (UPerNet) by only 2.6 F1 points while using 4.5× fewer parameters and 17× fewer GMACs.
2. **Transformers Cost More for Little Gain:** Dense-prediction transformer baselines (DeiT3-Base, MaxViT) require one to two orders of magnitude more compute than LALE for only marginal accuracy gains.
3. **Ablations Confirm the Design:** 3×3 stride-2 kernels combined with StarReLU/RMSNorm consistently outperform 7×7-kernel baselines at matched parameter counts, and ImageNet pre-training yields further gains at every scale, S2-K3-PT is the best-balanced configuration at 2.6M parameters and 0.78 GMACs.

## Generalization to Medical Imaging

To test whether the resolution-bifurcated design generalizes beyond remote sensing, we evaluated LALE on the Liver and Tumor Segmentation Benchmark (LiTS). LALE variants reach within ~1 F1 of the best baseline on liver segmentation, and remain competitive on the substantially harder tumor segmentation task (~5 F1 gap due to severe class imbalance and small lesion size), while retaining its small-parameter footprint, indicating the architecture transfers to medical imaging, though the optimal scale-accuracy point is task-dependent.

---

## Citation

If you find this work useful in your research, please consider citing:

```bibtex
@article{caglar2026lale,
  title={LALE: Lightweight-Transformer Architecture for Land-Cover Estimation},
  author={Caglar, Umit Mert and Temizel, Alptekin},
  journal={arXiv preprint arXiv:2606.02092},
  year={2026}
}
```
