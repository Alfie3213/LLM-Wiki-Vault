---
title: "Inverse Reinforcement Learning"
type: concept
tags: [强化学习, 模仿学习, 奖励学习, 逆强化学习]
sources: [raw/02-papers/An Auto-tuning Framework for Autonomous Vehicles.pdf]
last_updated: 2026-05-07
---

## 定义

逆强化学习（Inverse Reinforcement Learning, IRL）是强化学习的逆问题：给定专家示范轨迹，推断出专家所优化的奖励函数（reward function），而非直接学习策略。IRL 假设专家行为近似最优，其核心是寻找使专家表现优于其他策略的奖励函数。

## 关键信息

### 基本问题设定
在标准 MDP（马尔可夫决策过程）框架下，IRL 的目标是找到奖励函数  \in \mathcal{R}$，使得专家策略 $\pi_E$ 的期望累积奖励高于任何其他策略：

46420\max_{r \in \mathcal{R}} (\mathbb{E}[V^{\pi_E}(s_0)] - \max_{\pi \in \mathcal{P}} \mathbb{E}[V^{\pi}(s_0)])46420

其中 $\hat{\pi}_r$ 是在奖励 $ 下通过强化学习估计的最优策略。

### 主要挑战（在自动驾驶场景中）
1. **计算开销大**：传统 IRL 每轮奖励更新都需要通过 RL 或采样生成策略，计算成本极高
2. **在线训练安全风险**：许多学习方法需要大量在线训练时间来收集真实驾驶反馈，危及道路安全
3. **数据难以复现**：专家驾驶数据容易收集，但在仿真中复现（自车与周围环境交互）极其困难
4. **约束整合困难**：运动规划器必须同时满足车辆动力学要求和交通法规，系统性地将这些约束融入强化学习并不直观
5. **背景偏移问题**：不同场景（高速/本地/拥堵）的行为指标差异显著，联合训练时最优方向可能偏移

### 经典方法
- **学徒学习（Apprenticeship Learning）**：Abbeel & Ng (2004) 提出通过特征期望匹配来学习奖励函数
- **最大间隔法（Maximum Margin）**：Ratliff 等将 IRL 扩展为广义最大间隔优化问题
- **最大熵 IRL（MaxEnt IRL）**：Ziebart 等通过最大熵原理解决奖励函数歧义性问题

### 自动驾驶中的应用
在自动驾驶运动规划中，IRL 被用于从人类专家驾驶数据中学习奖励/代价函数，以自动调参运动规划器，替代传统耗时的人工调参方式。

### RC-IRL 的创新
针对传统 IRL 在自动驾驶中的挑战，RC-IRL 提出了三个关键改进：
1. **条件比较**：逐状态比较价值函数，消除背景偏移
2. **排序学习**：避免内层 RL 循环，提高计算效率
3. **Siamese 网络**：共享参数架构，高效比较人类与采样轨迹

## 关联连接

- [[RC_IRL]] — 针对自动驾驶场景优化的 IRL 变体，解决传统 IRL 计算昂贵和安全性问题
- [[2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]] — 将 IRL 应用于 Apollo 自动驾驶平台运动规划器调参的论文（L3 精读笔记）
- [[摘要-auto-tuning-framework-autonomous-vehicles]] — 论文核心摘要
- [[CTDE]] — 集中训练分散执行，多智能体学习中的条件化思想
