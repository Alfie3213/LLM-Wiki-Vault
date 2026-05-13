---
title: "摘要-auto-tuning-framework-autonomous-vehicles"
type: source
tags: [来源, 论文, 自动驾驶, 强化学习, 逆强化学习]
sources: [raw/02-papers/An Auto-tuning Framework for Autonomous Vehicles.pdf]
last_updated: 2026-05-07
---

## 核心摘要

本文提出了一种基于 Apollo 自动驾驶平台的数据驱动自动调参框架，用于解决 L4 级自动驾驶运动规划器（motion planner）的奖励/代价函数调参难题。传统人工调参需要大量仿真和路测时间，且随着场景复杂度提升，调参难度急剧增加。

核心贡献包括：
1. **RC-IRL 算法**：一种基于排序的条件逆强化学习（Rank-based Conditional Inverse Reinforcement Learning）算法，通过条件比较和基于排序的学习来估计奖励函数，相比传统 IRL 方法训练效率更高。
2. **离线训练策略**：在公开道路测试前安全地调整参数，避免在线训练的安全风险。
3. **自动数据收集与标注**：利用人类专家驾驶数据（约 1000+ 小时，过滤后 7.18 亿帧）和自动轨迹采样，大幅减少人工标注成本。
4. **Siamese 网络架构**：共享参数的价值网络用于比较人类驾驶轨迹与采样轨迹的价值差异。

实验验证覆盖 3400+ 仿真测试场景（含停车、转弯、变道、避让、超车等），并与 GAN 方法进行对比。结果表明 RC-IRL 在轨迹平滑性指标上显著优于 GAN（如 jerk 约束满足率 99.33% vs 86.80%）。截至 2018 年 7 月，该调参后的运动规划器已在公开道路完成 25000+ 英里测试。

## 关联连接

- [[Apollo]] — 百度开源的自动驾驶平台，本框架的基座
- [[HaoyangFan]] — 论文第一作者
- [[RC_IRL]] — 本文提出的核心算法
- [[InverseReinforcementLearning]] — RC-IRL 的理论基础
- [[../syntheses/2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]] — 本文的 L3 精读笔记（详细阅读）
