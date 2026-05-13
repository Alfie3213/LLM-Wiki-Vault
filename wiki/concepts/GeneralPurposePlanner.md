---
title: "General Purpose Planner"
type: concept
tags: [自动驾驶, 运动规划, 行为规划, 通用规划, 图搜索, 模型预测控制]
sources: [raw/09-archive/Driving with Style - Inverse Reinforcement Learning in General-Purpose Planning for Automated Driving.pdf]
last_updated: 2026-05-12
l3_note: [[2019-Rosbach-Driving-With-Style-IRL-General-Purpose-Planning]]
---

## 定义

通用型规划器（General-Purpose Planner）是一种将行为规划（Behavior Planning）与局部运动规划（Local Motion Planning）整合到单一框架中的自动驾驶规划范式。通过高分辨率动作采样和单一奖励函数优化，它能够隐式生成多种驾驶行为（如车道保持、变道、避让、紧急制动），而无需预定义行为模板。

## 关键信息

### 与传统分层规划的对比

传统分层架构将规划解耦为三个层级：
1. **战略层**: 路径规划（导航信息）
2. **战术/行为层**: 行为规划（变道、跟车、紧急制动等预定义动作）
3. **操作层**: 局部运动规划（参考轨迹生成）

**分层规划的局限**:
- 行为规划缺乏对运动约束的充分知识，可能导致动作不可行（高估能力）或被错误丢弃（低估能力）
- 在复杂和不可预见的驾驶场景中，行为规划器难以匹配预定义的可用模板

### 通用型规划器的核心思想

通过**高分辨率连续动作采样**和**基于 GPU 的并行搜索**，在一个规划周期内生成大量候选策略（平均约 4000 条），这些策略隐式包含多种行为模式。最终通过单一奖励函数选择最优策略。

**关键优势**:
- 无需预定义行为模板，行为通过奖励函数隐式涌现
- 行为规划器可在线调整奖励函数，无需分层架构的复杂协调
- 所有候选策略均满足车辆运动学约束

### 算法流程

基于模型预测控制（MPC）的周期触发：
1. **状态初始化**: 由环境模型或前一策略确定当前状态 $s_0$
2. **动作采样**: 对每个状态 $s \in S_t$，基于可行车辆动力学采样连续动作集 $A_s$（纵向速度曲线最高五阶，横向转角曲线最高三阶）
3. **状态转移**: 在环境模型 $M$ 中执行动作，观察结果状态 $s'$、转移 $T$ 和特征 $f(s,a)$
4. **标签构建**: 为转移分配类别标签（如碰撞标签）
5. **价值累积**: $V(s') = V(s) + \gamma^t R(s,a)$
6. **剪枝**: 基于价值 $V(s)$、标签和可达集属性终止冗余状态，限制指数增长并保证行为多样性
7. **策略选择**: 基于策略价值和模型约束选择最终驾驶策略 $\pi_S$

**关键参数**:
- 每个规划周期平均生成约 **4000 条策略**
- 算法本质类似并行广度优先搜索 + 前向价值迭代
- GPU 并行计算支撑大规模动作采样

### 与 IRL 的集成

通用型规划器的图表示天然适合 IRL：
- 规划器探索的大量策略集可用于近似配分函数（类似马尔可夫链蒙特卡洛）
- 人类驾驶示范可通过投影度量映射到规划器的状态-动作空间
- 奖励函数权重直接决定驾驶风格，无需分解学习任务

**路径积分 MaxEnt IRL 的关键公式**:
- 对数似然: $L(\theta) = \sum_{\pi_D \in \Pi_D} \ln p(\pi_D | \theta)$
- 策略概率: $p(\pi | \theta) = \frac{1}{Z} \exp(\theta^T f^\pi)$
- 配分函数近似: $Z = \sum_{\pi \in \Pi} \exp(\theta^T f^\pi)$（直接使用规划器策略集）
- 梯度: $\nabla L(\theta) = \sum_{\pi \in \Pi} p(\pi | \theta) f^\pi - \hat{f}^{\Pi_D}$

**经验回放机制**:
- 每个 MPC 周期的特征路径积分向量 $f^\Pi$ 存入回放缓冲区
- 训练时随机抽取小批量经验进行多次训练
- 更新奖励后，规划器配置同步更新，人类特征相似策略获得更高价值

**投影度量**:
$$d(\zeta, \pi) = \int_t^{t'} \gamma^t ||\zeta_t - \pi_t|| dt$$

包含纵向/横向欧氏距离 + 偏航角平方差 + 折扣因子。三重作用：约束验证器、风格评估器、数据增强器。

## 关联连接

- [[摘要-driving-with-style-irl-general-purpose-planning]] — 将 MaxEnt IRL 与通用型规划器集成的论文来源
- [[../syntheses/2019-Rosbach-Driving-With-Style-IRL-General-Purpose-Planning]] — 论文 L3 精读笔记（~13KB）
- [[InverseReinforcementLearning]] — 逆强化学习基础范式
- [[MaximumEntropyIRL]] — 最大熵逆强化学习理论基础
- [[SMIRL]] — 另一种结合运动规划先验的采样型 IRL 方法
- [[SaschaRosbach]] — 论文第一作者
- [[StefanRoth]] — 论文合作者
