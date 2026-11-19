---
layout: page
title: work
permalink: /work/
description:
nav: true
nav_order: 3
---

Two systems I built at Neuralgap. Details are limited to what I can share.

## Automated binding analysis for a drug discovery platform

Docking and structure-prediction models such as DiffDock, AutoDock Vina and Boltz return many poses for a single target, ranked by a score that is often wrong. Deciding which pose is worth taking forward is manual work: open each result, look at the bonds it forms, check for clashes, and write the findings up.

I built the model behind this analysis, the interpretability agents around it, and the pipeline that runs them. For every pose returned, the system characterises the interaction, counts the contacts, flags steric clashes, and produces a written report, so a chemist compares candidates instead of inspecting them one by one. It ships as part of a platform used by cheminformaticians and university research labs.

**My part:** the model, the interpretability agents, and the pipeline.

## Cardiac video analysis, 140× faster

A cardiac video model ran as a pipeline that kept clinicians waiting minutes for a result. I rebuilt the model's inference as a TensorRT-optimised 3D CNN-Transformer in FP16, deployed on a T4. Inference ran about 140× faster than the legacy model, with accuracy essentially unchanged after the FP16 conversion.

**My part:** the inference path and deployment. The clinical decision support platform around it ran on multimodal PyTorch models behind Flask APIs, which I also built.
