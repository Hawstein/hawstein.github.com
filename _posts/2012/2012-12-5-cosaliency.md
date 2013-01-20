---
layout: post
category: Research
title: Co-saliency Notes
---

## Saliency Detection: A Spectral Residual Approach

该论文的方法非常简单，与物体的特征，类别，及其他先验知识无关。通过分析输入图像光
谱的log值，提取图像的光谱残留，并提出了一种方法快速地建出该图像在空间域的显著性
map。

## A Co-saliency Model of Image Pairs


## Co-matting

半自动的显著性检测，第一帧中由矩形框圈出前景(火)。我们有如下基本假设：
矩形框中前景占据绝大部分的内容。定义这部分内容为“显著”的，同时，将矩形框以外的
内容定义为“不显著”的(实际可以只考虑矩形框外的一条窄带)。
