---
title: "CTDE"
type: concept
tags: [多智能体强化学习, 机器学习, 分布式系统]
sources: [raw/02-papers/CooHOI.pdf]
last_updated: 2026-05-06
---

## 定义

CTDE（Centralized Training with Decentralized Execution，集中训练分散执行）是多智能体强化学习中的一种训练范式。在训练阶段，智能体可以访问全局信息（如其他智能体的观测、动作和状态）；在执行阶段，每个智能体仅基于自身局部观测做出决策。

## 关键信息

### 核心机制

- **集中训练（Centralized Training）**
  - 训练时使用全局状态信息
  - 可以学习联合动作价值函数
  - 缓解非平稳环境问题

- **分散执行（Decentralized Execution）**
  - 执行时仅使用局部观测
  - 每个智能体独立决策
  - 无需显式通信

### 典型算法

- MADDPG（Multi-Agent Deep Deterministic Policy Gradient）
- QMIX / VDN
- MAPPO（Multi-Agent PPO）

### 应用场景

- 多机器人协作
- 自动驾驶车队
- 分布式资源调度
- 协作游戏 AI

## 关联连接

- [[CooHOI]] — 使用 CTDE 实现多机器人协作人机交互
- [[HumanObjectInteraction]] — 人机交互研究
