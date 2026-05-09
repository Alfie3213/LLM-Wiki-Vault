---
title: "摘要-differentiable-integrated-motion-prediction-and-planning"
type: source
tags: [来源, 论文, 自动驾驶, 运动规划, 轨迹预测]
sources: [raw/02-papers/2207.10422v2.pdf]
last_updated: 2026-05-09
---

## 核心摘要

本文提出了一种名为 DIPP（Differentiable Integrated Prediction and Planning）的可微分集成预测与规划框架，用于解决自动驾驶系统中预测模块与规划模块分离、以及规划代价函数难以标定和调参的两大问题。该框架采用基于 Transformer 的神经网络同时预测周围交通参与者的未来轨迹，并将预测结果输入一个可微分的非线性运动规划优化器，为自车生成轨迹。整个框架的所有操作（包括代价函数权重）都是可微分的，使得规划误差能够反向传播到预测模块和代价函数，实现端到端联合训练。该框架在大规模真实驾驶数据集上进行训练，并通过开环和闭环两种方式验证，实验结果表明联合训练的预测-规划模块优于独立训练的模块。

## 关键信息

- **论文标题**: Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving
- **作者**: Zhiyu Huang, Haochen Liu, Jingda Wu, Chen Lv
- **arXiv**: 2207.10422v2
- **机构**: Nanyang Technological University, Singapore
- **页数**: 15
- **核心问题**: 自动驾驶中预测与规划模块分离、代价函数难以人工调参
- **方法**: Transformer 多智能体轨迹预测 + 可微分非线性优化器运动规划 + 可学习代价函数
- **关键洞察**: 通过端到端可微分架构，让预测模块感知下游规划任务，输出规划导向的预测结果

## 关联连接

- [[DIPP]] — 本文提出的可微分集成预测与规划框架
- [[2022-Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving]] — L3 精读笔记（完整技术细节与批判性分析）
- [[DifferentiableMotionPlanning]] — 可微分运动规划技术
- [[IntegratedPredictionAndPlanning]] — 预测与规划集成范式
- [[ZhiyuHuang]] — 第一作者
- [[HaochenLiu]] — 共同作者
- [[JingdaWu]] — 共同作者
- [[ChenLv]] — 通讯作者
