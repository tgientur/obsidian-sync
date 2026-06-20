---
created: 2026-06-20
tags: [术语/半导体]
---

# eFuse/eNVM

**英文全称**：Embedded Non-Volatile Memory
**首次提出**：2000s
**所属产业**：半导体产业链

## 定义
集成在SoC芯片内部的非易失存储器单元，包含eFuse（通过电迁移熔断实现一次编程）和eNVM（多次可编程，如MRAM/RRAM/PCM等新兴存储）。

## 核心作用
在芯片内部提供无需外部存储器的配置/校准/密钥存储能力，用于芯片ID、修调校准、安全密钥固化和启动代码存储。

## 应用场景
- 芯片ID和唯一标识（eFuse写入序列号）
- 电压/频率修调校准
- 安全根密钥存储（防止物理提取）
- MCU片上程序存储（eFlash/eMRAM）

## 关联概念
- 新兴方案:: [[A股产业研究库/12 术语库/存算一体]]
- 相关产品:: [[A股产业研究库/12 术语库/RISC-V]]

## 重要参考资料
[1] "eFuse Technology Overview." IBM Journal of Research and Development, 2006.
