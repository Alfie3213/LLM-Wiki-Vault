---
title: "ZhiyuHuang"
type: entity
tags: [研究者, 自动驾驶, 运动规划, 轨迹预测]
sources: [raw/02-papers/2207.10422v2.pdf, 摘要-differentiable-integrated-motion-prediction-and-planning]
last_updated: 2026-05-09
---

## 定义

Zhiyu Huang 是自动驾驶与机器人学领域研究者，南洋理工大学（Nanyang Technological University）机械与宇航工程学院博士/研究人员。研究方向聚焦于自动驾驶中的运动规划、轨迹预测以及预测与规划的集成方法。

## 关键信息

### 代表工作

- **DIPP** — Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving (arXiv:2207.10422, IEEE Transactions)
  - 第一作者
  - 提出可微分集成预测与规划框架
  - 核心: Transformer 多智能体预测 + Gauss-Newton 可微分优化器 + 可学习代价函数

- **DTPP** — Differentiable joint conditional prediction and cost evaluation for tree policy planning for autonomous driving (ICRA 2023)
  - 与 Chen Lv 等合作
  - 将 DIPP 扩展到树策略规划，引入条件预测和代价评估

- **GameFormer** — Game-theoretic modeling and learning of transformer-based interactive prediction and planning for autonomous driving (ICCV 2023)
  - 与 Haochen Liu, Chen Lv 等合作
  - 基于博弈论的 Transformer 交互式预测与规划

### 研究脉络

DIPP (2022) -> DTPP (2023) -> GameFormer (2023):
- **DIPP**: 解决"预测-规划分离 + 代价函数难调参"问题，通过可微分优化器实现端到端联合训练
- **DTPP**: 扩展 DIPP 到树策略规划，处理多步决策中的不确定性
- **GameFormer**: 引入博弈论建模多智能体交互，从单智能体优化扩展到多智能体博弈

### 所属机构

- 南洋理工大学（Nanyang Technological University, Singapore）
- 机械与宇航工程学院（School of Mechanical and Aerospace Engineering）

## 关联连接

- [[摘要-differentiable-integrated-motion-prediction-and-planning]] — DIPP 论文摘要
- [[DIPP]] — 提出的集成预测与规划框架
- [[ChenLv]] — 长期合作导师
- [[HaochenLiu]] — DIPP 共同作者
- [[JingdaWu]] — DIPP 共同作者
