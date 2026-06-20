---
created: 2026-06-20
tags: [术语/AI]
---

# Scaling Law

**英文全称**：Scaling Laws for Neural Language Models
**首次提出**：2020 (Kaplan et al)
**所属产业**：AI产业链

## 定义
模型性能（交叉熵损失）与模型参数量、训练数据量和计算量之间呈幂律关系——三者同比例增加时性能可预测地提升，且模型越大数据效率越高。

## 核心作用
为"堆算力、堆参数、堆数据"的AI发展路线提供理论依据，是千亿/万亿模型竞赛背后的底层驱动力。

## 应用场景
- 大模型训练预算规划（给定算力-数据-参数的最优分配）
- 模型架构决策（稠密 vs MoE 的Scaling效率对比）
- 推理成本预测（小模型+蒸馏 vs 大模型+压缩的性价比分析）

## 关联概念
- 理论基础:: [[A股产业研究库/03 产业链图谱/AI产业链/大模型]]
- 约束因素:: [[A股产业研究库/12 术语库/GPU集群]]

## 重要参考资料
[1] Kaplan, J., et al. "Scaling Laws for Neural Language Models." arXiv 2020.
[2] Hoffmann, J., et al. "Training Compute-Optimal Large Language Models" (Chinchilla Law). NeurIPS 2022.
