---
created: 2026-06-20
tags: [文献/机器视觉]
---

# Deep Residual Learning for Image Recognition

**标题**：Deep Residual Learning for Image Recognition
**作者**：He, Kaiming; Zhang, Xiangyu; Ren, Shaoqing; Sun, Jian
**机构**：Microsoft Research
**发表年份**：2015
**会议/期刊**：CVPR 2016
**DOI/链接**：https://arxiv.org/abs/1512.03385

## 摘要
提出残差学习框架（Residual Learning），通过引入"跳跃连接（skip connection）"让层学习残差映射而非原始映射，解决了深度网络训练中的退化问题（degradation problem）。基于此构建了152层（后扩展至1000+层）的ResNet，在ImageNet上以3.57%的top-5错误率刷新纪录。该架构在ILSVRC 2015五个赛道全部夺冠。

## 关键词
ResNet, 残差学习, 深度卷积网络, 图像识别, 跳跃连接

## 所属产业
AI产业链

## 关联概念
- [[A股产业研究库/03 产业链图谱/AI产业链/机器视觉]]

## 研究价值
跳跃连接是深度学习历史上最重要的架构创新之一，也是Transformer中残差连接的直接前身。ResNet使训练极深网络成为可能，是CV领域最常引用的论文之一（引用量超20万次）。

## 引用格式
He K, Zhang X, Ren S, et al. Deep residual learning for image recognition[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR 2016). 2016: 770-778.
