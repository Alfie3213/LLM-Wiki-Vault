---
title: "Monte Carlo Tree Search"
type: concept
tags: [搜索算法, 决策算法, 蒙特卡洛方法, 自动驾驶, 游戏AI]
sources: [raw/09-archive/TreeIRL - Safe Urban Driving with Tree Search and Inverse Reinforcement Learning.pdf]
last_updated: 2026-05-12
---

## 定义

蒙特卡洛树搜索（Monte Carlo Tree Search, MCTS）是一类基于采样的决策时规划算法，通过反复模拟（rollout）来估计状态-动作值，并在搜索树上逐步构建最优策略。经典应用包括围棋 AI（AlphaGo）、象棋（AlphaZero）和 Atari 游戏。

在自动驾驶领域，TreeIRL 首次将 MCTS 成功应用于真实城市道路的运动规划，实现了安全性、舒适性和类人驾驶风格的统一。

## 关键信息

### 经典四阶段流程

1. **选择（Selection）**: 从根节点出发，使用树策略（如 UCT/PUCT）选择子节点，直到到达未充分探索的节点
2. **扩展（Expansion）**: 将新节点加入搜索树
3. **评估（Evaluation）**: 使用值函数估计或蒙特卡洛 rollout 评估叶子节点价值
4. **回溯（Backup）**: 将评估结果沿搜索路径反向传播，更新节点访问计数 N 和动作价值 Q

### TreeIRL 中的 MCTS 变体

TreeIRL 对 MCTS 的核心创新是**将 MCTS 重新定位为轨迹生成器**（而非传统的单动作选择器）。n 次迭代后，执行 DFS 遍历按 N(s,a) 降序访问的 top-k 叶子节点，再用 padding 策略生成完整轨迹。

- **树策略**: PUCTS 公式（结合神经网络先验 P 的 UCT 扩展）
  - UCB(s,a) = Q(s,a)/Q_max + c_puct · P(s,a) · √(Σ_b N(s,b)+1) / (N(s,a)+1+ε)
- **评估方式**: 神经网络值估计 V̂_θ(s) 或 rollout 模拟回报
- **轨迹提取**: DFS 遍历 top-k 最受欢迎叶子节点，结合 padding 策略生成完整轨迹
- **状态空间**: 一维纵向（位置、速度、加速度）
- **动作空间**: 加加速度（jerk），离散 5 档
- **延迟优化**: 使用均匀先验 + IDM rollout/padding 将延迟从 ~900ms 降至 ~10ms

### MCTS 延迟优化的范式转变

传统思路认为 MCTS 需要学习策略在搜索循环内引导以减少迭代次数。TreeIRL 证明：
- 学习策略在循环内推理开销巨大（~900ms）
- 简单启发式策略（IDM）在特定子问题（纵向控制）上足以引导搜索
- **延迟收益（~100x）远超性能损失**，这是 MCTS 首次在真实 AV 上部署的决定性因素

### 在自动驾驶中的优势

- **安全性**: 通过搜索树显式评估多条候选轨迹，避免单点决策风险
- **多样性**: 搜索过程自然探索多种行为模式（跟车、减速、停车等）
- **决策时规划**: 在决策时刻探索轨迹空间，能根据当前场景动态调整行为模式
- **可解释性**: 搜索树结构提供决策透明性

### 局限

- **计算开销**: 相比单轨迹优化方法，MCTS 需要更多计算资源
- **状态空间维度**: 高维状态空间（如包含横向控制）会导致搜索空间指数增长
- **模拟依赖**: 需要准确的环境模型进行状态转移模拟
- **延迟敏感性**: 对后处理延迟敏感，需要与轻量级后处理（Smoother）配合

## 关联连接

- [[TreeIRL]] — 将 MCTS 重新定位为轨迹生成器并与 IRL 结合的自动驾驶规划方法
- [[摘要-treeirl-safe-urban-driving]] — TreeIRL 论文来源
- [[syntheses/2025-TreeIRL-Safe-Urban-Driving-with-Tree-Search-and-Inverse-Reinforcement-Learning]] — L3 精读笔记
- [[InverseReinforcementLearning]] — 与 MCTS 结合的评分学习方法
- [[DriveIRL]] — 使用启发式生成器而非 MCTS 的先验方法
- [[GeneralPurposePlanner]] — 另一种基于搜索的规划范式
