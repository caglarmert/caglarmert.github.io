---
layout: page
title: BELDE - Building a Large-scale Earth-observation Land-cover Dataset for Europe
---

<div align="center">
  <h1>BELDE: Building a Large-scale Earth-observation Land-cover Dataset for Europe</h1>
  
  <p>
    <strong>Ümit Mert Çağlar</strong> &nbsp;&nbsp;&nbsp;&nbsp; <strong>Alptekin Temizel</strong>
  </p>
  <p>
    <em>Graduate School of Informatics, Middle East Technical University, Ankara, Turkey</em>
  </p>
  
  <br>

  <!-- Quick Links -->
  <p>
    <a href="https://github.com/caglarmert/BELDE"><strong>[ 💻 Code ]</strong></a> &nbsp;&nbsp;
    <a href="https://huggingface.co/datasets/caglarmert/BELDE"><strong>[ 📊 BELDE Dataset ]</strong></a> &nbsp;&nbsp;
    <a href="https://huggingface.co/datasets/caglarmert/BELDE-K"><strong>[ 🇰🇷 BELDE-K ]</strong></a> &nbsp;&nbsp;
    <a href="https://huggingface.co/datasets/caglarmert/BELDE-CA-NV"><strong>[ 🇺🇸 BELDE-CA-NV ]</strong></a>
  </p>
</div>

<hr>

## Abstract
<p align="justify">
Earth observation imagery is fundamental for environmental monitoring, urban planning, disaster assessment, and climate analysis. While multi-spectral sensors are highly capable, true-color (RGB) imagery remains heavily utilized due to power, cost, and deployment constraints across many operational platforms. To address the limitations of existing datasets regarding geographic coverage and scale, this work introduces BELDE, a publicly available benchmark tailored for RGB-based remote sensing semantic segmentation. Constructed from Sentinel-2 images and ESA WorldCover maps, the dataset provides 1,088,385 curated image-mask pairs at a 10 m spatial resolution covering a diverse European footprint.
</p>

## Dataset Pipeline
<p align="justify">
The dataset is constructed via a fully automated pipeline that aligns true-color images with corresponding land-cover maps. Through rigorous, rule-based data curation and strict filtering criteria, over 1.94 million non-informative, corrupted, or single-class dominated patches were pruned. This ensures an information-dense training set optimized for efficient model distillation.
</p>

<figure align="center">
  <img src="belde2.jpg" alt="BELDE dataset creation and curation pipeline" width="100%">
  <figcaption><b>Figure 1:</b> Automated spatial querying and data curation pipeline for the BELDE dataset.</figcaption>
</figure>

## Key Contributions
* **Scale and Diversity:** The introduction of BELDE, comprising 1,088,385 geo-aligned image-mask pairs across Europe, established via an automated quality filtering pipeline.
* **Efficiency Under Constraint:** The demonstration of model distillation utility with lightweight architectures, showing that compact networks (e.g., LALE-S2 with 2.61M parameters) achieve competitive segmentation accuracy for resource-constrained edge deployments.
* **Generalization Benchmarks:** The release of out-of-domain evaluation datasets, BELDE-K (16,607 pairs in the Republic of Korea) and BELDE-CA-NV (88,155 pairs in California-Nevada, USA), to quantify geographic domain shift.
* **Architectural Evaluation:** A comprehensive benchmark evaluation of 17 semantic segmentation architectures spanning CNN, dense prediction transformer, and lightweight hybrid families.

## Cross-Region Generalization
<figure align="center">
  <img src="belde1.jpg" alt="Cross region generalization" width="85%">
  <figcaption><b>Figure 2:</b> Evaluation framework assessing out-of-distribution performance under geographic domain shift.</figcaption>
</figure>

<p align="justify">
To facilitate systematic evaluation of domain shift, models trained on the primary European BELDE dataset were tested on geographically distinct benchmarks without overlap. Performance evaluations on BELDE-K and BELDE-CA-NV indicate a consistent degradation under geographic domain shift across all architectural families. This highlights that RGB-only segmentation models rely heavily on region-specific spatial and textural cues, emphasizing the necessity of geographically diverse benchmarks for robust Earth observation systems.
</p>

<hr>

## Dataset Availability and Reproducibility
<p align="justify">
The datasets and software framework created and used throughout this work are publicly available under the Creative Commons Attribution 4.0 International license (<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>).
</p>

| Resource | Link |
| :--- | :--- |
| **BELDE** | [datasets/caglarmert/BELDE](https://huggingface.co/datasets/caglarmert/BELDE) |
| **BELDE-K** | [datasets/caglarmert/BELDE-K](https://huggingface.co/datasets/caglarmert/BELDE-K) |
| **BELDE-CA-NV** | [datasets/caglarmert/BELDE-CA-NV](https://huggingface.co/datasets/caglarmert/BELDE-CA-NV) |
| **Source Code** | [github.com/caglarmert/BELDE](https://github.com/caglarmert/BELDE) |
