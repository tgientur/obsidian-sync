---
created: 2026-06-20
tags: [文献/大模型]
---

# Training Compute-Optimal Large Language Models

**标题**：Training Compute-Optimal Large Language Models
**作者**：Hoffmann, Jordan; Borgeaud, Sebastian; Mensch, Arthur; Buchatskaya, Elena; Cai, Trevor; Rutherford, Eliza; 等（DeepMind）
**机构**：DeepMind
**发表年份**：2022
**会议/期刊**：NeurIPS 2022
**DOI/链接**：https://arxiv.org/abs/2203.15556

## 摘要
系统性地研究了在固定计算预算下如何最优分配模型参数量和训练数据量。发现此前的大模型普遍"欠训练"——更大的模型需要成比例更多的数据才能达到最优效果。据此训练了Chinchilla（70B参数，1.4T tokens），在多项基准上超越当时更大的模型（如Gopher 280B）。

## 关键词
Chinchilla, 缩放定律, 计算最优, 训练数据量, 大语言模型

## 所属产业
AI产业链

## 关联概念
- [[A股产业研究库/03 产业链图谱/AI产业链/大模型]]

## 研究价值
修正了"纯增大参数就能提升性能"的直觉，建立了模型参数量和训练数据量之间的定量缩放关系，对后续大模型训练的资源配置策略具有直接指导意义。

## 引用格式
Hoffmann J, Borgeaud S, Mensch A, et al. Training compute-optimal large language models[C]//Advances in Neural Information Processing Systems 35 (NeurIPS 2022). 2022: 30016-30030.
