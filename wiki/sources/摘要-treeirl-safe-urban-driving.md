---
title: "摘要-treeirl-safe-urban-driving"
type: source
tags: [来源, 论文, 逆强化学习, 蒙特卡洛树搜索, 自动驾驶, 运动规划, Motional]
sources: [raw/09-archive/TreeIRL - Safe Urban Driving with Tree Search and Inverse Reinforcement Learning.pdf]
last_updated: 2026-05-12
---

## 核心摘要

本文提出 TreeIRL，一种将蒙特卡洛树搜索（MCTS）与逆强化学习（IRL）相结合的新型自动驾驶规划器。核心思想是利用 MCTS 生成一组有前景的安全候选轨迹，再通过深度 IRL 评分函数从中选择最类人的轨迹。

TreeIRL 采用模块化架构：上游感知模块提供场景上下文，PBP（Path-Based Prediction）进行多智能体运动预测，MCTS 替代 DriveIRL 中的启发式轨迹生成器，输出候选轨迹集，最后由基于场景-轨迹 Transformer 的 IRL 评分器选择最优轨迹。IRL 评分器在 1360 小时的人类专家驾驶数据上通过最大熵 IRL 训练，使用分类框架和 Focal Loss 优化。

在 nuPlan 基准测试（7051 个场景）和真实城市道路测试中，TreeIRL 在安全性、舒适性、类人性和灵活性方面均达到或超越现有方法。特别地，在真实城市道路测试中，TreeIRL 相比模拟器中的小幅优势转化为 1-2 个数量级的实际性能提升，凸显了在环测试（on-road evaluation）对于评估规划器的重要性。

## 关联连接

- [[TreeIRL]] — 本文提出的核心方法
- [[syntheses/2025-TreeIRL-Safe-Urban-Driving-with-Tree-Search-and-Inverse-Reinforcement-Learning]] — L3 精读笔记
- [[MonteCarloTreeSearch]] — 蒙特卡洛树搜索方法
- [[DriveIRL]] — TreeIRL 所基于的先验方法
- [[InverseReinforcementLearning]] — 逆强化学习基础范式
- [[MaximumEntropyIRL]] — 最大熵逆强化学习理论基础
- [[SimToRealGap]] — 模拟到真实的差距问题
- [[MomchilSTomov]] — 论文第一作者
- [[SangUkLee]] — 论文作者
- [[HansfordHendargo]] — 论文作者
- [[BoazFloor]] — 论文作者
- [[YunqingHu]] — 论文作者
- [[Motional]] — 论文作者所属公司
