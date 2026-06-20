---
created: 2026-06-20
tags: [文献/大模型]
---

# Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer

**标题**：Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer
**作者**：Shazeer, Noam; Mirhoseini, Azalia; Maziarz, Krzysztof; Davis, Andy; Le, Quoc V.; Hinton, Geoffrey E.; Dean, Jeff
**机构**：Google Brain
**发表年份**：2017
**会议/期刊**：ICLR 2017
**DOI/链接**：https://arxiv.org/abs/1701.06538

## 摘要
提出稀疏门控混合专家层（Sparsely-Gated Mixture-of-Experts, MoE），将大规模全连接层拆分为多个"专家"子网络，通过可学习的门控机制为每个输入只激活少量专家。实现了参数规模增大数百倍但计算量仅线性增长的可能。在语言建模和机器翻译任务上验证了效果。

## 关键词
MoE, 混合专家, 稀疏激活, 大规模神经网络, 条件计算

## 所属产业
AI产业链

## 关联概念
- [[A股产业研究库/03 产业链图谱/AI产业链/大模型]]

## 研究价值
MoE架构是当前超大模型（如GPT-4、Mixtral、DeepSeek-V2等）降低推理成本的核心技术手段，该论文为MoE在现代Transformer中的实际落地提供了基础框架。

## 引用格式
Shazeer N, Mirhoseini A, Maziarz K, et al. Outrageously large neural networks: The sparsely-gated mixture-of-experts layer[C]//Proceedings of the 5th International Conference on Learning Representations (ICLR 2017). 2017.
