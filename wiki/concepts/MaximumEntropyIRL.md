---
title: "Maximum Entropy IRL"
type: concept
tags: [逆强化学习, 最大熵, 概率模型, 奖励学习]
sources: [https://arxiv.org/abs/2006.13704]
last_updated: 2026-05-09
---

## 定义

最大熵逆强化学习（Maximum Entropy Inverse Reinforcement Learning, MaxEnt IRL）是由Ziebart等人于2008年提出的IRL方法，通过最大熵原理解决奖励函数恢复中的歧义性问题。该方法假设专家行为并非完全最优，而是以概率方式倾向于高奖励轨迹。

## 关键信息

### 核心思想
在标准IRL中，多个奖励函数可能都能解释专家示范，导致歧义性。MaxEnt IRL通过引入最大熵原则，在所有满足约束的奖励分布中选择熵最大的分布，从而得到最"公平"的奖励分配。

### Boltzmann噪声理性模型
MaxEnt IRL假设轨迹的概率与其累积奖励呈指数关系：
$$P(\xi; \theta) \propto e^{\beta R(\xi; \theta)}$$

其中 $\beta$ 是描述示范者接近完美优化程度的超参数。当 $\beta \to \infty$ 时，示范者趋近于完美优化器。

### 奖励函数参数化
通常采用线性结构化的奖励函数：
$$R(\xi; \theta) = \theta^T f(\xi)$$

其中 $f(\xi)$ 是定义在轨迹上的特征向量，$\theta$ 是待学习的参数向量。

### 学习目标
通过最大化示范数据集的对数似然来优化参数：
$$\theta^* = \arg \max_\theta \frac{1}{M} \sum_{i=1}^M \log P(\xi_i | \theta)$$

关键在于计算配分函数（partition function）$Z_{\xi_i}$，即对轨迹空间中的积分或求和。

### 连续域扩展挑战
原始MaxEnt IRL定义在离散MDP框架中。在连续域应用中，配分函数的估计成为核心难点，催生了多种近似方法：
- **Laplace近似**（CIOC算法）：通过二阶Taylor展开近似局部奖励形状
- **优化近似**（Opt-IRL）：用最优轨迹代表期望特征计数
- **采样近似**（SMIRL）：通过高效采样生成代表性轨迹集来估计配分函数

## 关联连接

- [[SMIRL]] — 基于采样的高效最大熵IRL连续域算法
- [[InverseReinforcementLearning]] — 逆强化学习基础范式
- [[ClosedLoopIRL]] — 闭环逆强化学习变体
- [[RC_IRL]] — 排序条件逆强化学习
- [[摘要-efficient-sampling-based-maximum-entropy-irl]] — SMIRL论文来源
