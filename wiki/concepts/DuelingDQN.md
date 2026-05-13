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
- **输入**: 当前车辆 + 周围 6 辆车的历史轨迹 {x,y,v,a} 和交互信息 {Δx,Δy,Δv}
- **输出**: 换道决策 a ∈ {left, right, keep}
- **训练细节**:
  - 200 episodes × 5 random seeds
  - Epsilon-greedy 探索：ϵ 随训练逐渐减小，从探索过渡到利用
  - 目标网络更新: y_t = r_t + γ·Q_target(s_{t+1}, argmax_a Q_behave(s_{t+1}, a))
  - 损失函数: L_DQN = E[(Q_behave(s_t,a_t) - y_t)²]
- **决策准确率**: 激进 90%、普通 85%、保守 87%
- **F1-score**: 激进 0.80、普通 0.77、保守 0.75
- **不安全决策率**: 仅 **1.4%**，显著低于纯 DDQN 的 **4.8%**

**与纯 DDQN 的关键对比**:

| 方法 | 激进 F1 | 普通 F1 | 保守 F1 | 不安全决策率 |
|---|---|---|---|---|
| 纯 DDQN | 0.68 | 0.70 | 0.71 | 4.8% |
| **DuDQN + 博弈约束（本文）** | **0.80** | **0.77** | **0.75** | **1.4%** |

注意: 性能提升不仅来自 DuDQN 架构本身，更关键的是**动态博弈论安全约束**（I_Publish 碰撞指示函数作为硬约束）的引入。DuDQN 在安全约束定义的允许动作空间内求解最优策略。

## 关联连接

- [[PersonalizedAutonomousDriving]] — 使用 DuDQN 生成换道策略的个性化自动驾驶系统
- [[摘要-trajectory-data-driven-personalized-autonomous-driving-decision-system]] — 论文来源摘要
- [[syntheses/2026-Sun-Trajectory-Data-Driven-Personalized-Autonomous-Driving-Decision-System-for-Driving-Simulators]] — L3 精读笔记
