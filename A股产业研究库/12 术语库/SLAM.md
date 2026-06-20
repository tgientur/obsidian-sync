---
created: 2026-06-20
tags: [术语/机器人]
---

# SLAM

**英文全称**：Simultaneous Localization and Mapping
**首次提出**：1986年 / 概率机器人学领域的奠基性概念
**所属产业**：机器人 > 自主导航 > 环境感知

## 定义
机器人在未知环境中同时完成两件事：确定自己在哪（定位），以及记录周围有什么（建图），且两者相互依赖——没有地图就没有定位，没有定位就无法建图。

## 核心作用
移动机器人自主移动的前置条件——没有 SLAM，机器人只能在已知路线运行（如固定轨迹 AGV），无法在陌生环境中自主探索和规划路径。

## 应用场景
- 扫地机器人（激光 SLAM / VSLAM）
- 自动驾驶的实时定位与地图更新
- 仓储物流 AGV/AMR 的动态环境适应
- 无人机自主导航和三维重建
- AR/VR 设备空间定位

## 关联概念
- 传感器选择:: [[激光SLAM]] / [[视觉SLAM]] (VSLAM)
- 配合模块:: [[运动规划]] / [[路径规划]]
- 经典算法:: [[ORB-SLAM]] / [[LOAM]]

## 重要参考资料
[1] Durrant-Whyte H, Bailey T. Simultaneous localization and mapping: part I. IEEE RAM, 2006.
[2] Cadena C, et al. Past, present, and future of simultaneous localization and mapping. IEEE TRO, 2016.
