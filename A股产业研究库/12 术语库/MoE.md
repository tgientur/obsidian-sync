---
created: 2026-06-20
tags: [术语/AI]
---

# MoE

**英文全称**：Mixture of Experts
**首次提出**：2017 (Shazeer), 2024大规模应用(Mixtral/DeepSeek)
**所属产业**：AI产业链

## 定义
将模型拆分为多个专家子网络，每个输入只激活部分专家，由门控网络动态选择路由路径。在推理时计算量远低于同等参数量的稠密模型。

## 核心作用
在不成比例增加计算成本的条件下大幅扩展模型参数量，是当前千亿级以上大模型的主流架构选择。

## 应用场景
- 千亿/万亿参数大语言模型（Mixtral 8x7B、DeepSeek-V2、GPT-4）
- 多任务学习（不同专家擅长不同任务类型）
- 大规模推荐系统（每个用户群分配不同专家）

## 关联概念
- 模型架构:: [[A股产业研究库/03 产业链图谱/AI产业链/大模型]]
- 相关技术:: [[A股产业研究库/12 术语库/稀疏计算]]

## 重要参考资料
[1] Shazeer, N., et al. "Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer." ICLR 2017.
[2] Jiang, A. Q., et al. "Mixtral of Experts." arXiv 2024.
