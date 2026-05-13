---
title: "CooHOI"
type: concept
tags: [机器人学, 多智能体强化学习, 人机交互, 具身智能]
sources: [raw/09-archive/CooHOI - Learning Cooperative Human-Object Interaction with Manipulated Object Dynamics.pdf, 摘要-coohoi]
last_updated: 2026-05-12
---

## 定义

CooHOI（Cooperative Human-Object Interaction）是一种让多个人形机器人协作完成物体搬运任务的框架，核心是通过被操作物体的动力学状态实现智能体间的隐式通信与协调。

## 关键信息

### 核心思想

1. **两阶段学习范式**
   - **阶段一：单智能体技能学习** — 利用对抗运动先验（AMP）框架，通过模仿学习训练单个智能体与物体交互的能力
   - **阶段二：策略迁移** — 使用 CTDE（Centralized Training Decentralized Execution）多智能体 RL 算法，让智能体通过共享被操作物体的动力学变化实现协作

2. **隐式通信机制**
   - 当一个智能体与物体交互时，物体的动力学状态发生变化
   - 其他智能体通过观察这些变化来协调自身行为
   - 无需显式的通信协议或语言交互

3. **优势**
   - 无需多机器人协作的运动捕捉数据
   - 内在高效性（不依赖基于跟踪的方法）
   - 可扩展至更多参与者和更广泛的物体类型

### 与已有方法的区别

| 特性 | CooHOI | 传统方法 |
|------|--------|----------|
| 数据需求 | 仅需单机器人运动数据 | 需要多机器人协作运动捕捉数据 |
| 通信方式 | 隐式（通过物体动力学） | 通常需要显式通信 |
| 可扩展性 | 高（无缝扩展参与者） | 受限 |
| 方法类型 | 强化学习 + 模仿学习 | 多为基于跟踪的方法 |

## 关联连接

- [[摘要-coohoi]] — 论文摘要
- [[CTDE]] — 集中训练分散执行范式
- [[HumanObjectInteraction]] — 人机交互研究
- [[JiaweiGao]] — 第一作者
