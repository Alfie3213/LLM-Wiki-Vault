---
title: "摘要-ddp-framework-inverse-reinforcement-learning"
type: source
tags: [来源, 论文, 逆强化学习, 微分动态规划, 最优控制, 机器人学]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 核心摘要

本文提出了一种基于微分动态规划（DDP）的逆强化学习（IRL）框架，用于从专家示范中恢复代价函数参数、系统动力学和约束条件。与现有工作不同，现有方法仅将 DDP 用于带不等式约束的内层前向问题，而本框架将 DDP 用于外层逆问题中所需梯度的高效计算，同时处理等式和不等式约束。

核心贡献包括：
1. **DDP-based IRL 框架**：利用 DDP 高效计算逆问题中的梯度，支持等式和不等式约束
2. **与 PMP 方法的等价性**：证明了所提方法与基于庞特里亚金最大值原理（PMP）的现有方法在数学上的等价性
3. **闭环 IRL 框架**：提出了闭环损失函数来捕捉示范的闭环特性，相比常用的开环损失函数表现更优
4. **理论保证**：在一定假设和秩条件下，证明了学习参数可以从示范数据中恢复
5. **实验验证**：在 4 个数值机器人示例和 1 个真实四旋翼系统上进行了广泛验证

## 关联连接

- [[KunCao]] — 论文第一作者
- [[WanxinJin]] — 论文作者之一
- [[KarlHJohansson]] — 论文作者之一
- [[LihuaXie]] — 论文作者之一
- [[XinhangXu]] — 论文作者之一
- [[InverseReinforcementLearning]] — 本文的理论基础
- [[DifferentialDynamicProgramming]] — 本文方法的核心技术
- [[PontryaginsMaximumPrinciple]] — 与本文方法建立等价性的经典方法
- [[InverseOptimalControl]] — 本文闭环 IRL 框架的特例
- [[ClosedLoopIRL]] — 本文提出的闭环 IRL 框架
