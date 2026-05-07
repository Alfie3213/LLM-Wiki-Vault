---
title: "Differential Dynamic Programming (DDP)"
type: concept
tags: [最优控制, 动态规划, 轨迹优化, 数值方法]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 定义

微分动态规划（Differential Dynamic Programming, DDP）是一种用于轨迹优化的迭代数值方法，通过局部二阶近似动态规划来高效求解非线性最优控制问题。

## 关键信息

### 核心思想
DDP 是一种基于动态规划的轨迹优化方法，通过在当前轨迹附近进行二阶展开，迭代优化控制序列。与直接方法（如直接转录法）相比，DDP 通常具有更快的收敛速度和更好的数值稳定性。

### 在 IRL 中的应用
传统上，DDP 被用于 IRL 的内层前向问题（即给定奖励函数，求解最优轨迹）。而本文的创新在于将 DDP 用于外层逆问题：
- **外层逆问题**：从示范数据中恢复代价函数参数、系统动力学和约束条件
- **高效梯度计算**：利用 DDP 的高效性计算逆问题中所需的梯度
- **约束处理**：支持等式和不等式约束

### 优势
1. **计算效率高**：相比直接方法，DDP 通常收敛更快
2. **数值稳定性好**：二阶展开提供了更好的局部近似
3. **约束支持**：可以处理等式和不等式约束

## 关联连接

- [[摘要-ddp-framework-inverse-reinforcement-learning]] — 将 DDP 用于 IRL 外层逆问题的论文
- [[InverseReinforcementLearning]] — DDP 在 IRL 中的应用场景
- [[PontryaginsMaximumPrinciple]] — 与 DDP 相关的最优控制理论
- [[InverseOptimalControl]] — DDP 在逆最优控制中的应用
