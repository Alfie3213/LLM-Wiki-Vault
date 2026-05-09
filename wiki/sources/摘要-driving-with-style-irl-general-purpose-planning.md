---
title: "摘要-driving-with-style-irl-general-purpose-planning"
type: source
tags: [来源, 论文, 逆强化学习, 自动驾驶, 运动规划, 大众]
sources: [https://arxiv.org/abs/1905.00229]
last_updated: 2026-05-09
---

## 核心摘要

本文提出将最大熵逆强化学习（MaxEnt IRL）与通用型规划器（General-Purpose Planner）集成，用于自动驾驶中奖励函数的自动调参。传统分层规划架构将行为规划与局部运动规划解耦，而通用型规划器通过单一奖励函数联合优化行为与运动规划。本文的核心贡献是将路径积分最大熵 IRL 嵌入到基于 GPU 并行采样的图搜索规划器中，利用规划器自身的图表示来近似配分函数，避免了传统状态访问频率计算在高维连续空间中的不可行性。

实验在大众集团原型车上进行，使用 12 维奖励特征（包括运动特征和基础设施特征），从人类驾驶示范中学习奖励权重。结果表明，学习得到的奖励函数在期望价值差（EVD）和期望距离（ED）两个指标上均超越了运动规划专家的手动调参结果：EVD 降低 63%-67%，ED 降低 44%-54%。即使从随机初始化开始训练，也能达到超越专家调参的驾驶风格模仿效果。

## 关联连接

- [[GeneralPurposePlanner]] — 本文采用的通用型规划范式
- [[2019-Rosbach-Driving-With-Style-IRL-General-Purpose-Planning]] — 本文 L3 精读笔记
- [[InverseReinforcementLearning]] — 逆强化学习基础范式
- [[MaximumEntropyIRL]] — 最大熵逆强化学习理论基础
- [[SMIRL]] — 同样采用采样近似配分函数的连续域 IRL 方法
- [[SaschaRosbach]] — 论文第一作者
- [[VinitJames]] — 论文作者
- [[SimonGrossjohann]] — 论文作者
- [[SilviuHomoceanu]] — 论文作者
- [[StefanRoth]] — 论文作者，达姆施塔特工业大学教授
