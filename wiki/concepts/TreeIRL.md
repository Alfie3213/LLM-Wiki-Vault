---
title: "TreeIRL"
type: concept
tags: [自动驾驶, 运动规划, 蒙特卡洛树搜索, 逆强化学习, 混合方法]
sources: [raw/02-papers/2509.13579v4.pdf]
last_updated: 2026-05-12
---

## 定义

TreeIRL 是一种将蒙特卡洛树搜索（MCTS）与逆强化学习（IRL）相结合的自动驾驶运动规划方法。由 Motional 研究团队提出，其核心思想是利用 MCTS 生成大量安全且多样化的候选轨迹，再通过深度 IRL 评分函数从中选择最符合人类驾驶风格的轨迹。

## 关键信息

### 核心架构

TreeIRL 采用模块化三阶段架构：

1. **编码/预测模块（PBP）**: 场景编码器 B 和解码器 D 计算场景嵌入 h 并对其他智能体进行轨迹预测 p
2. **轨迹生成模块（MCTS）**: 蒙特卡洛树搜索生成 k 条候选轨迹 {τ₁...τₖ}，替代 DriveIRL 中的启发式生成器
3. **轨迹选择模块（IRL Scorer）**: 场景-轨迹 Transformer E 和评分 Transformer S 为每条候选轨迹计算类人性评分 zᵢ

### MCTS 轨迹生成器

- **树策略**: 采用 PUCTS 公式进行节点选择，结合神经网络先验 P 引导搜索
- **评估函数**: 使用神经网络值估计 ˆV_θ(s) 或蒙特卡洛 rollout 估计
- **Rollout 策略选项**: 学习得到的 RL 策略 π_θ、IDM 策略、或恒速策略
- **Padding 策略**: 从 top-k 叶子节点出发，使用 padding 策略生成完整轨迹
- **状态空间**: 一维纵向控制（纵向位置、速度、加速度）
- **动作空间**: 加加速度（jerk）

### IRL 评分器训练

- **训练数据**: 1360 小时人类专家驾驶数据
- **方法**: 最大熵 IRL，形式化为分类问题
- **损失函数**: Focal Loss 与负对数似然损失结合
  - loss = −(1 − P(τ*))^γ log P(τ*)
  - 其中 τ* 为与人类专家轨迹 L2 距离最近的候选轨迹
- **优化器**: PPO（Proximal Policy Optimization）

### 关键优势

| 维度 | 表现 |
|------|------|
| 安全性 | 碰撞率与 DriveIRL-Safe 持平，显著优于纯 MCTS 和 IDM |
| 舒适性 | 纵向加速度和加加速度指标优于所有基线 |
| 类人性 | L2 误差显著低于经典方法和纯学习方案 |
| 灵活性 | 通过 MCTS 搜索实现行为灵活性，无需预定义行为模板 |
| 可扩展性 | 随着数据量增加，IRL 评分器持续改进 |

### 模拟到真实（Sim-to-Real）

- nuPlan 模拟测试（7051 场景）: TreeIRL 小幅领先
- 真实城市道路测试: TreeIRL 优势放大至 **1-2 个数量级**
- 结论: 模拟器评估需谨慎，在环测试是全面评估规划器的必要环节

## 关联连接

- [[摘要-treeirl-safe-urban-driving]] — 论文来源摘要
- [[MonteCarloTreeSearch]] — TreeIRL 使用的搜索方法
- [[DriveIRL]] — TreeIRL 所基于的先验架构
- [[InverseReinforcementLearning]] — IRL 基础范式
- [[MaximumEntropyIRL]] — 训练使用的理论基础
- [[SimToRealGap]] — 论文讨论的关键问题
- [[MomchilSTomov]] — 论文第一作者
- [[Motional]] — 研究团队所属公司
