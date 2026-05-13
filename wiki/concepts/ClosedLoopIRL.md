---
title: "Closed-Loop IRL"
type: concept
tags: [逆强化学习, 闭环控制, 机器人学, 最优控制, 微分动态规划]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 定义

闭环逆强化学习（Closed-Loop Inverse Reinforcement Learning）是一种 IRL 框架，使用闭环损失函数来捕捉专家示范的闭环特性。与常用的开环 IRL 不同，闭环 IRL 考虑了系统状态反馈对控制策略的影响，能够更好地学习闭环控制器中的隐式代价函数。

## 关键信息

### 开环 vs 闭环 IRL
- **开环 IRL**：仅比较示范轨迹与生成轨迹的开环轨迹差异，不考虑状态反馈。在闭环系统中，开环损失会引入偏差，因为相同的初始状态和参数在不同迭代中可能产生完全不同的轨迹。
- **闭环 IRL**：使用闭环损失函数，捕捉示范中隐含的闭环控制结构。损失函数匹配反馈策略而非整条轨迹，避免闭环数据带来的偏差。

### 闭环损失函数（Cao et al., 2024）

对于状态-动作对 $(x, u)$，定义闭环损失：

$$
L_{cl} := \sum_{k \in S} \|\epsilon\|_2^2
$$

其中：

$$
\epsilon := \hat{Q}_u + \hat{Q}_{ux}(x^{**} - x) + \hat{Q}_{uu}(u^{**} - u)
$$

这里：
- $\hat{Q}_u, \hat{Q}_{ux}, \hat{Q}_{uu}$ 是 DDP 后向传播得到的代价函数近似项
- $x^{**}, u^{**}$ 是专家示范的状态和动作
- $S$ 是数据点集合

**物理意义**：$\epsilon$ 衡量了在专家状态 $x$ 下，专家动作 $u$ 与 DDP 计算出的闭环反馈策略之间的差异。当 $\epsilon = 0$ 时，专家动作恰好等于 DDP 反馈策略在该状态的输出。

### 闭环梯度推导

对闭环损失求参数 $\theta$ 的梯度：

$$
\frac{\partial L_{cl}}{\partial \theta} = \sum_{k \in S} 2 \epsilon^\top \left( \frac{\partial \hat{Q}_u}{\partial \theta} + \frac{\partial \hat{Q}_{ux}}{\partial \theta}(x^{**}-x) + \frac{\partial \hat{Q}_{uu}}{\partial \theta}(u^{**}-u) \right)
$$

其中 $\frac{\partial \hat{Q}}{\partial \theta}$ 通过 **DDP 梯度求解器**（增广系统 DDP 递归）计算，时间复杂度为 $O(N)$，$N$ 为轨迹长度。

### Levenberg-Marquardt 更新

参数更新采用 LM 算法，解决小数据量时的病态问题：

$$
\theta_{new} = \theta + (J^\top J + \lambda I)^{-1} J^\top \epsilon
$$

其中 $J$ 是 $\epsilon$ 对 $\theta$ 的雅可比矩阵，$\lambda$ 是阻尼系数。

### 可恢复性条件（Recoverability）

闭环 IRL 的参数可恢复性取决于数据集的秩条件：

- **必要条件**：$\text{rank}(J) = m_\theta$（参数维度）
- **最小数据量**：$|S| \geq \lceil m_\theta / m_u \rceil$，其中 $m_u$ 是控制维度
- **实验验证**：仅 3 条轨迹（约 60 个数据点）即可恢复 18 维参数，而开环 IRL 需要 20 条以上轨迹

### 与逆最优控制的联系

闭环 IRL 的梯度求解器与约束逆最优控制（Constrained IOC）有密切联系：
- 在无约束情况下，DDP 梯度求解器等价于 Pontryagin 最大原理（PMP）的灵敏度分析
- 在有约束情况下，BarrierDDP 通过障碍函数处理约束，保持 $O(N)$ 复杂度
- 论文证明了 DDP 与 PDP（Pontryagin Differential Principle）在数学上的等价性

## 优势与局限

**优势**：
1. **数据效率高**：闭环损失仅需少量轨迹即可恢复参数（3 vs 20+ 条轨迹）
2. **避免闭环偏差**：匹配反馈策略而非开环轨迹，适合闭环示范数据
3. **计算高效**：DDP 梯度求解器 $O(N)$ 复杂度，适合高维系统
4. **理论保证**：参数可恢复性有明确的秩条件和最小数据量保证

**局限**：
1. **局部最优**：DDP 本身是局部优化方法，依赖初始猜测
2. **系统模型已知**：需要精确的动力学模型和梯度信息
3. **连续空间**：目前主要针对连续状态-动作空间的机器人系统
4. **实时性**：虽然梯度计算是 $O(N)$，但完整 IRL 迭代仍需一定计算时间

## 关联连接

- [[../syntheses/2024-A Differential Dynamic Programming Framework for Inverse Reinforcement Learning]] — 提出闭环 IRL 框架和 DDP 梯度求解器的论文（L3 精读笔记）
- [[摘要-ddp-framework-inverse-reinforcement-learning]] — 上述论文的核心摘要
- [[DifferentialDynamicProgramming]] — 内层轨迹优化和梯度计算的核心方法
- [[InverseOptimalControl]] — 闭环 IRL 与约束 IOC 的理论联系
- [[PontryaginsMaximumPrinciple]] — 与 DDP 梯度求解器等价的数学框架
- [[InverseReinforcementLearning]] — 逆强化学习总体范式
