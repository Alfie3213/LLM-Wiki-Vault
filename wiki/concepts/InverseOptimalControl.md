---
title: "Inverse Optimal Control"
type: concept
tags: [最优控制, 逆问题, 控制理论, 机器学习]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 定义

逆最优控制（Inverse Optimal Control, IOC）是从专家示范中恢复最优控制问题中代价函数的问题。与 IRL 密切相关，但通常更侧重于连续时间系统和控制理论框架。

## 关键信息

### 与 IRL 的关系
逆最优控制与逆强化学习（IRL）在目标上相似，但有以下区别：
- **IOC**：通常假设系统动力学已知，专注于恢复代价函数
- **IRL**：更广泛的概念，可能同时恢复奖励函数和系统动力学
- **联系**：本文证明了闭环 IRL 框架在一定假设下可以退化为约束逆最优控制问题

### 本文中的角色
本文提出的闭环 IRL 框架在一定假设下退化为约束逆最优控制问题：
- 在特定假设和秩条件下，学习参数可以从示范数据中恢复
- 为 IRL 提供了控制理论层面的理论保证

### 理论保证
本文证明了：
- 在一定假设和秩条件下，闭环 IRL 的学习参数可以从示范数据中唯一恢复
- 这一结果将 IRL 与经典的逆最优控制理论联系起来

## 关联连接

- [[摘要-ddp-framework-inverse-reinforcement-learning]] — 将闭环 IRL 与逆最优控制建立联系的论文
- [[InverseReinforcementLearning]] — 密切相关的方法论
- [[DifferentialDynamicProgramming]] — 本文求解逆最优控制问题的数值方法
- [[PontryaginsMaximumPrinciple]] — 逆最优控制的理论基础
