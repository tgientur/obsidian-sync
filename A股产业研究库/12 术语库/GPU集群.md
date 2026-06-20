---
created: 2026-06-20
tags: [术语/AI]
---

# GPU集群

**英文全称**：GPU Cluster
**首次提出**：2010s
**所属产业**：AI产业链

## 定义
大量GPU通过高速网络（InfiniBand/NVLink/RoCE）互联组成的并行计算集群，用于大模型训练和规模化推理服务。

## 核心作用
为千亿/万亿参数模型的训练提供分布式算力底座，集群规模和互联带宽直接决定训练时间上限。

## 应用场景
- 大模型预训练（数千GPU并行数月的分布式训练）
- 大规模推理服务（高并发API的GPU负载均衡）
- 科学计算/HPC（分子模拟、气象预报）

## 关联概念
- 核心瓶颈:: [[A股产业研究库/12 术语库/光互联]]
- 核心瓶颈:: [[A股产业研究库/12 术语库/CPO]]
- 硬件组成:: [[A股产业研究库/12 术语库/AI加速卡]]
- 理论基础:: [[A股产业研究库/12 术语库/Scaling Law]]

## 重要参考资料
[1] Narayanan, D., et al. "Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM." SC 2021.
