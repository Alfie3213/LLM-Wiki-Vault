---
title: "Dueling DQN"
type: concept
tags: [深度强化学习, 价值分解, Q学习, 自动驾驶]
sources: [raw/02-papers/vehicles-08-00094-v2.pdf]
last_updated: 2026-05-12
---

## 定义

Dueling DQN（DuDQN）是深度 Q 网络（DQN）的一种改进架构，由 Wang 等人于 2016 年提出。其核心创新是将 Q 函数分解为状态价值函数 V(s) 和动作优势函数 A(s, a) 两部分，从而更有效地学习哪些状态是有价值的，而不必关心每个动作的具体影响。

## 关键信息

### 核心思想

标准 DQN 直接估计 Q(s, a)，而 Dueling DQN 将 Q 函数分解为:

$$Q(s, a) = V(s) + A(s, a) - \frac{1}{|A|}\sum_{a'}A(s, a')$$

其中:
- **V(s)**: 状态价值函数，表示处于状态 s 的固有优势
- **A(s, a)**: 动作优势函数，表示在状态 s 下选择动作 a 相对于其他动作的优劣

### 网络架构

Dueling DQN 在标准 DQN 的卷积层之后增加两个独立的分支:
1. **价值流**: 输出标量 V(s)
2. **优势流**: 输出向量 A(s, a)（每个动作一个值）

最后通过聚合层组合为 Q(s, a)。

### 优势

- **样本效率**: 在某些状态下所有动作价值相近时，V(s) 可以直接学习而不必为每个动作单独估计
- **策略评估**: 更清晰地识别有价值的状态，有助于策略评估和改进
- **泛化能力**: 对动作选择的微小变化具有更好的泛化性

### 在个性化自动驾驶中的应用

在 Personalized Autonomous Driving 论文中，DuDQN 被用于生成类人换道策略:
- **输入**: 周围车辆信息、自车状态、驾驶风格偏好权重
- **输出**: 换道决策（换道/保持）
- **训练**: 使用从真实航拍数据中提取的专家决策作为监督信号
- **结果**: 换道策略准确率超过 85%，且能区分激进/普通/保守三种风格

## 关联连接

- [[PersonalizedAutonomousDriving]] — 使用 DuDQN 生成换道策略的个性化自动驾驶系统
- [[摘要-trajectory-data-driven-personalized-autonomous-driving-decision-system]] — 论文来源摘要
