---
created: 2026-06-20
tags: [文献/大模型]
---

# FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness

**标题**：FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness
**作者**：Dao, Tri; Fu, Daniel Y.; Ermon, Stefano; Rudra, Atri; Ré, Christopher
**机构**：Stanford University; Together Computer; NYU
**发表年份**：2022
**会议/期刊**：NeurIPS 2022
**DOI/链接**：https://arxiv.org/abs/2205.14135

## 摘要
分析了标准注意力计算在GPU上运行时的瓶颈——慢在HBM（高带宽内存）的读写而非计算本身。提出FlashAttention算法：将注意力计算分块（tiling），在SRAM上完成中间结果计算，避免来回读写HBM。最终实现2-4倍的训练加速，且内存消耗从O(n²)降至线性。

## 关键词
FlashAttention, GPU优化, 注意力机制, IO感知算法, 大模型训练

## 所属产业
AI产业链

## 关联概念
- [[A股产业研究库/03 产业链图谱/AI产业链/大模型]]
- [[A股产业研究库/03 产业链图谱/AI产业链/GPU]]

## 研究价值
解决了长序列Transformer训练中显存瓶颈的关键问题，是当前几乎所有主流大模型训练框架（如PyTorch原生、vLLM等）中attention层的底层实现基础。

## 引用格式
Dao T, Fu D Y, Ermon S, et al. FlashAttention: Fast and memory-efficient exact attention with IO-awareness[C]//Advances in Neural Information Processing Systems 35 (NeurIPS 2022). 2022: 16344-16359.
