---
created: 2026-06-20
tags: [术语/存储, 术语/AI算力]
---

# HBM

**英文全称**：High Bandwidth Memory（高带宽存储器）
**首次提出**：2013年（JEDEC标准HBM 1.0），2016年首次商业化（AMD Fiji GPU搭载HBM）
**所属产业**：AI产业链、半导体产业链

## 定义

一种基于3D堆叠DRAM Die和硅通孔（TSV）技术的高带宽存储器，通过宽位宽接口（1024 bits）和中介层与GPU/CPU集成，提供远超传统DDR/GDDR的带宽。

## 核心作用

解决AI训练/推理中"内存墙"瓶颈——GPU算力快速增长，传统GDDR显存带宽无法匹配，HBM通过堆叠+宽接口实现高带宽低功耗。

## 应用场景

- AI训练：NVIDIA H100/B200搭载HBM3/HBM3e
- AI推理：云端推理卡对HBM容量和带宽的差异化需求
- HPC：科学计算、气象模拟
- 高端FPGA/ASIC：需要大带宽的场景

## 关联概念

- 下游:: [[A股产业研究库/03 产业链图谱/AI产业链/GPU]]
- 制造技术:: [[A股产业研究库/12 术语库/先进封装]]（通过CoWoS与GPU集成）
- 上游:: [[A股产业研究库/03 产业链图谱/半导体产业链/半导体材料]]
- 竞争路线:: HMC（混合存储立方体，已边缘化）

## 各代对比

| 代际 | 标准 | 推出年 | 带宽/堆栈 | 代表产品 |
|------|------|--------|-----------|----------|
| HBM1 | JEDEC | 2013 | 128GB/s | AMD Fiji |
| HBM2 | JEDEC | 2016 | 256GB/s | V100 |
| HBM2e | 扩展 | 2019 | 460GB/s | A100 |
| HBM3 | JEDEC | 2022 | 819GB/s | H100 |
| HBM3e | 扩展 | 2024 | 1.2TB/s+ | B200/H200 |

## 上下游关系

DRAM晶圆制造 → TSV通孔 → 3D堆叠 → Micro-bump键合 → CoWoS封装 → 集成到GPU → AI服务器 → 数据中心

## 重要参考资料

[1] JEDEC. HBM3 Memory Standard[J]. 2022-01.
[2] Yole Group. Status of Memory Packaging 2024[R]. Yole Intelligence, 2024.
[3] SK Hynix. HBM3e Product Brief[R]. 2024.
