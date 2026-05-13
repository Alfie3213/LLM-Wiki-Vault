---
title: "RC-IRL"
type: concept
tags: [逆强化学习, 自动驾驶, 奖励学习, 排序学习, 孪生网络, 条件比较]
sources: [raw/02-papers/An Auto-tuning Framework for Autonomous Vehicles.pdf]
last_updated: 2026-05-07
---

## 定义

RC-IRL（Rank-based Conditional Inverse Reinforcement Learning，基于排序的条件逆强化学习）是一种用于自动驾驶运动规划器奖励函数自动调参的逆强化学习算法。它通过条件比较和基于排序的学习策略，从人类专家驾驶示范中恢复隐式的奖励函数。

## 关键信息

### 核心思想
RC-IRL 包含两个关键部分：
1. **条件比较（Conditional Comparison）**：将价值函数的期望重写为初始状态分布上的积分，通过逐状态比较人类轨迹与采样轨迹的价值差异来学习奖励函数，显著降低背景方差
2. **基于排序的学习（Rank-based Learning）**：不直接估计奖励的绝对值，而是利用排序关系（人类轨迹优于随机采样轨迹）构建损失函数，避免传统 IRL 每轮迭代都需要 RL 求解最优策略的计算开销

### 数学形式

传统 IRL 目标：
$\max_{r \in \mathcal{R}} (\mathbb{E}[V^{\pi_E}(s_0)] - \max_{\pi \in \Pi} \mathbb{E}[V^{\pi}(s_0)])$

RC-IRL 条件比较形式：
$\mathbb{E}_{s_0 \sim \mathcal{D}}[V^{\pi_E}(s_0) - V^{\pi_r}(s_0)]$

RC-IRL 排序学习形式：
$\mathbb{E}_{s_0 \sim \mathcal{D}} \sum_{i=1,...,N} L(V^{\pi_E}(s_0) - V^{\pi_i}(s_0))$

### 网络架构
采用 **Siamese 网络（孪生网络）** 架构：
- 人类驾驶轨迹与随机采样轨迹共享同一组价值网络参数
- 损失函数：Leaky-RELU，$lpha = 0.05$
- 价值网络结构：21 输入 → 15 隐藏 → 18 奖励编码 → 15 输出
- 17 个固定时间点 ,...,t_{17}$，共享参数 $	heta$ 保持一致性
- 损失函数评估两者价值网络输出的差异

### 背景偏移问题 (Background Shifting)
在异构场景下（高速/本地/拥堵），不同场景的行为指标差异显著。联合训练时，最优奖励函数方向会发生偏移（如 Figure 3 所示），导致在任一单独场景中都不是最优。条件比较相当于 pairwise 比较，消除了背景差异。

### 特征设计
输入特征共 21 维（Table I）：
- 横向坐标 $、导数 $、二阶导数 $、曲率
- 纵向：station、time、velocity、speed limit、acceleration、jerk
- 障碍物交互：collision dist.、follow obs.、overtake obs.、stop obs.、virtual obs.、nudge obs.

### 训练数据
- 约 1000+ 小时人类专家驾驶数据
- 过滤后 7.18 亿帧有效数据（过滤无障碍物或无速度变化的帧）
- 生成 28 亿条随机采样轨迹查询
- 约 2 个 epoch 的随机梯度下降即可收敛
- 优化器：ADAM 或 RMSProp（对简单奖励函数影响不大）

### 实验表现
在 3400+ 仿真场景中，RC-IRL（基于 Siamese 网络）相比 GAN 方法：
- 碰撞-free 率：100% vs 100%
- 速度限制满足率：100% vs 99.49%
- 纵向加速度约束满足率：99.33% vs 86.80%
- 纵向 jerk 约束满足率：97.77% vs 71.20%
- 截至 2018 年 7 月，基于 RC-IRL 调参的运动规划器已在公开道路完成 25000+ 英里测试

### 优势
1. 计算效率高：避免了传统 IRL 的内层 RL 循环
2. 场景适应性强：条件比较消除了背景偏移
3. 安全性：离线训练策略避免了在线学习的道路风险
4. 可扩展性：适合大规模训练和长尾 corner cases

### 局限性
1. 特征工程依赖：21 维特征需要人工设计
2. 随机采样质量：随机轨迹可能无法覆盖最优策略空间
3. 横向性能仍有提升空间：横向 jerk 约束满足率 82.77%
4. 缺乏理论收敛保证

## 关联连接

- [[../syntheses/2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]] — 首次提出 RC-IRL 的论文（L3 精读笔记）
- [[摘要-auto-tuning-framework-autonomous-vehicles]] — 论文核心摘要
- [[InverseReinforcementLearning]] — RC-IRL 的理论基础
- [[Apollo]] — RC-IRL 应用的自动驾驶平台
- [[CTDE]] — 集中训练分散执行，与条件化思想相关
