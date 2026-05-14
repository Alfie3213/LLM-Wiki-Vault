---
title: 摘要-automatic-parameter-tuning-self-driving-vehicles
type: source
tags:
  - 来源
  - 原始文件
  - 自动驾驶
  - 参数调优
  - 模型预测控制
  - 逆强化学习
sources:
  - raw/09-archive/Automatic Parameter Tuning of Self-Driving Vehicles.pdf
last_updated: 2026-05-14
---

## 核心摘要

本文提出一种基于专家示范的自动驾驶车辆轨迹规划器和控制器参数自动调整方法。现代自动驾驶系统的轨迹规划和控制组件包含大量需要针对不同驾驶场景和车辆类型进行调整的参数，手动调参耗时且困难。

核心方法：定义一个代价函数来捕捉控制器闭环运行与记录期望驾驶行为之间的偏差，然后使用三种局部优化技术进行参数优化：
1. **梯度下降 (GD)**：数值计算代价函数梯度，沿反方向更新参数
2. **无迹卡尔曼滤波 (UKF)**：将参数增广到系统状态中，同时估计状态和参数
3. **最大似然估计 (ML)**：使用代价函数作为似然函数，迭代优化参数

案例研究：在 BMW 测试场（Aschheim）的真实车道保持场景中，使用线性时变约束 MPC 进行横向轨迹规划，基于人类驾驶员的示范数据调参。三种方法均能显著改善手动调参的初始参数，即使在噪声数据下也能有效工作。ML 方法在横向偏差和整体代价上表现最优。

## 关键信息

- **作者**: Hung-Ju Wu, Vladislav Nenchev, Christian Rathgeber (BMW Group, Munich)
- **会议**: 2024 IEEE Conference on Control Technology and Applications (CCTA)
- **arXiv**: 2406.17757 [eess.SY]
- **页数**: 6 页
- **核心应用**: 横向轨迹规划器的 MPC 代价函数权重自动调优
- **调参目标**: w_d (横向偏差), w_θ (航向角), w_κ (曲率), w_κ̇ (曲率变化率), w_u (控制输入)

## 关联连接

- [[HungJuWu]] — 第一作者
- [[VladislavNenchev]] — 第二作者
- [[ChristianRathgeber]] — 第三作者
- [[BMWGroup]] — 研究机构
- [[MaximumEntropyIRL]] — 相关工作：逆强化学习方法也可用于从示范中学习代价函数
- [[InverseReinforcementLearning]] — 相关工作：IRL 与本文方法的比较
- [[MPCbasedImitationLearning]] — 相关工作：MPC 与模仿学习的结合
- [[AutomaticParameterTuning]] — 本文核心概念
