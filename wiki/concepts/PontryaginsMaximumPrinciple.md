---
title: "Pontryagin's Maximum Principle (PMP)"
type: concept
tags: [最优控制, 变分法, 控制理论, 数学优化]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 定义

庞特里亚金最大值原理（Pontryagin's Maximum Principle, PMP）是最优控制理论中的核心定理，由 Lev Pontryagin 及其同事于 1950 年代提出。该原理给出了最优控制问题的一阶必要条件，是变分法在最优控制中的推广。

## 关键信息

### 核心内容
PMP 指出，对于最优控制问题，存在协态变量（costate），使得最优控制在每个时刻都使哈密顿函数（Hamiltonian）达到最大值（或最小值，取决于约定）。

### 在 IRL 中的应用
PMP 被广泛用于 IRL 的梯度计算：
- **前向问题**：利用 PMP 求解给定代价函数下的最优轨迹
- **梯度计算**：通过协态方程计算代价函数参数对轨迹的梯度

### 与 DDP 的关系
本文证明了基于 DDP 的 IRL 框架与基于 PMP 的现有方法在数学上的等价性：
- DDP 和 PMP 都提供了求解最优控制问题的途径
- DDP 通过二阶展开提供数值解法
- PMP 通过一阶必要条件提供解析框架
- 在 IRL 的逆问题中，两者可以推导出相同的梯度表达式

## 关联连接

- [[摘要-ddp-framework-inverse-reinforcement-learning]] — 建立 DDP 与 PMP 等价性的论文
- [[DifferentialDynamicProgramming]] — 与 PMP 等价的方法
- [[InverseReinforcementLearning]] — PMP 在 IRL 中的应用场景
- [[InverseOptimalControl]] — PMP 在逆最优控制中的应用
