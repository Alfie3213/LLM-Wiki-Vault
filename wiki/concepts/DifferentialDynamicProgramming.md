---
title: "Differential Dynamic Programming (DDP)"
type: concept
tags: [最优控制, 动态规划, 轨迹优化, 数值方法, 梯度计算]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 定义

微分动态规划（Differential Dynamic Programming, DDP）是一种用于轨迹优化的迭代数值方法，通过局部二阶近似动态规划来高效求解非线性最优控制问题。本文进一步将其应用于逆强化学习的外层梯度计算。

## 关键信息

### 核心思想
DDP 基于 Bellman 最优性方程，通过在当前轨迹附近进行二阶展开，迭代优化控制序列。具有线性计算复杂度（关于 horizon）和局部二次收敛性。

### 约束 DDP 求解器
本文提出广义内点 DDP 算法，处理等式和不等式约束：
- **内点 min-max Bellman 方程**:  = \min_u \max_{\lambda \geq 0, \gamma} Q := \ell + V^+ + \lambda^	op g + \gamma^	op h$
- **反馈控制律**: $\delta u = k + K \delta x$，同时对偶变量也有反馈形式
- **Backward recursions**: 计算控制增益
- **Forward recursions**: 更新轨迹

**替代方案**: Active-set DDP — 识别活跃不等式约束，合并为等式约束处理。

### DDP for 逆问题梯度计算（核心创新）
传统上 DDP 仅用于求解前向 OC 问题。本文的关键发现：DDP 递归中的中间矩阵 $\hat{Q}(\cdot)$ 恰好是外层逆问题梯度计算所需的项。

**增广系统构造**: 将学习参数 $	heta$ 视为额外状态：
46420y_{k+1} = ar{f}(y_k, u_k) = egin{bmatrix} 	heta \ f(x_k, u_k; 	heta) \end{bmatrix}46420

**核心定理**: 在增广系统上执行一步 backward-forward DDP 递归，即可同时获得最优轨迹和 /d	heta$。计算复杂度 (N)$。

**与 PDP 的等价性**: DDP-based 梯度计算与 PMP-based (PDP) 梯度计算数学等价。DDP 中的 cost-to-go $ 和 $ 分别对应 PDP 中的 Hamiltonian $ 和动力学约束对偶变量 $\lambda$。

### BarrierDDP（实用变体）
将约束通过障碍函数融入阶段代价，从而使用无约束 DDP 求解器处理约束问题：
46420ar{\ell}(y_k, u_k) - \mu 1^	op \log(-ar{g}) + rac{1}{2\mu}\|ar{h}\|_2^246420

优势：不增加问题规模，同时继承 DDP 在高维问题上的计算优势。

### 在 IRL 中的完整流程
1. **Trajectory Solver (Algorithm 1)**: DDP 求解约束 OC 问题得到 (	heta_t)$
2. **Gradient Solver (Algorithm 2)**: 在增广系统上执行 DDP 递归得到 /d	heta$
3. **外层更新**: 结合损失函数梯度更新参数

## 关联连接

- [[../syntheses/2024-A Differential Dynamic Programming Framework for Inverse Reinforcement Learning]] — 将 DDP 用于 IRL 外层逆问题梯度计算的论文（L3 精读笔记）
- [[摘要-ddp-framework-inverse-reinforcement-learning]] — 论文核心摘要
- [[InverseReinforcementLearning]] — DDP 在 IRL 中的应用场景
- [[PontryaginsMaximumPrinciple]] — 与 DDP 建立等价性的经典方法
- [[InverseOptimalControl]] — DDP 在逆最优控制中的应用
- [[ClosedLoopIRL]] — 使用 DDP 作为数值求解器的闭环 IRL 框架
