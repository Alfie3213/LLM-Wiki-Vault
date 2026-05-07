---
title: "Closed-Loop IRL"
type: concept
tags: [逆强化学习, 闭环控制, 机器人学, 最优控制]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 定义

闭环逆强化学习（Closed-Loop Inverse Reinforcement Learning）是一种 IRL 框架，使用闭环损失函数来捕捉专家示范的闭环特性。与常用的开环 IRL 不同，闭环 IRL 考虑了系统状态反馈对控制策略的影响，能够更好地学习闭环控制器中的隐式代价函数。

## 关键信息

### 开环 vs 闭环 IRL
- **开环 IRL**：仅比较示范轨迹与生成轨迹的开环轨迹差异，不考虑状态反馈
- **闭环 IRL**：使用闭环损失函数，捕捉示范中隐含的闭环控制结构

### 本文贡献
本文提出了基于 DDP 的闭环 IRL 框架：
1. **闭环损失函数**：能够捕捉专家示范的闭环特性
2. **性能优势**：被证明优于常用的开环损失函数
3. **理论保证**：在一定假设和秩条件下，学习参数可以从示范数据中恢复
4. **与 IOC 的联系**：闭环 IRL 框架退化为约束逆最优控制问题

### 实验验证
在 4 个数值机器人示例和 1 个真实四旋翼系统上验证了闭环 IRL 的有效性。

## 关联连接

- [[摘要-ddp-framework-inverse-reinforcement-learning]] — 首次提出闭环 IRL 框架的论文
- [[InverseReinforcementLearning]] — 闭环 IRL 的理论基础
- [[InverseOptimalControl]] — 闭环 IRL 的特例
- [[DifferentialDynamicProgramming]] — 闭环 IRL 的数值求解方法
