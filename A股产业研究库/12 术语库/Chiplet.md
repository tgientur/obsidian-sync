---
created: 2026-06-20
tags: [术语/半导体]
---

# Chiplet

**英文全称**：Chiplet
**首次提出**：2010s
**所属产业**：半导体产业链

## 定义
将大型单片SoC拆分为多个较小的小芯片（die），通过先进封装（2.5D/3D）和标准化互连接口（UCIe/BoW）集成为完整系统的设计范式。

## 核心作用
突破单片芯片的良率和面积限制，允许不同制程节点（如7nm逻辑+28nm模拟）的die异构集成，降低设计成本和生产风险。

## 应用场景
- 高性能CPU/GPU（AMD Zen架构CCD+IOD分离）
- 服务器SoC合封（算力die+IO die+HBM堆叠）
- 定制化AI芯片（客户选配不同规格的die组合）

## 关联概念
- 封装技术:: [[A股产业研究库/12 术语库/先进封装]]
- 互连标准:: [[A股产业研究库/12 术语库/TSV]]

## 重要参考资料
[1] "Universal Chiplet Interconnect Express (UCIe) Specification." UCIe Consortium, 2022.
