---
title: "TreeIRL"
type: concept
tags: [自动驾驶, 运动规划, 蒙特卡洛树搜索, 逆强化学习, 混合方法]
sources: [raw/09-archive/TreeIRL - Safe Urban Driving with Tree Search and Inverse Reinforcement Learning.pdf]
last_updated: 2026-05-12
---

## 定义

TreeIRL 是一种将蒙特卡洛树搜索（MCTS）与逆强化学习（IRL）相结合的自动驾驶运动规划方法。由 Motional 研究团队提出，其核心创新是将 MCTS 重新定位为**轨迹生成器**（而非传统的单动作选择器），利用其决策时搜索能力聚焦于当前场景行为上合适的轨迹子空间，再通过在 1360 小时人类专家数据上训练的深度 IRL 评分器从中选择最类人的轨迹。

## 关键信息

### 核心架构

TreeIRL 采用模块化三阶段架构：

1. **编码/预测模块（PBP）**: 场景编码器 B 和解码器 D 计算场景嵌入 h 并对其他智能体进行轨迹预测 p
2. **轨迹生成模块（MCTS）**: 蒙特卡洛树搜索生成 k 条候选轨迹 {τ₁...τₖ}，替代 DriveIRL 中的启发式生成器
3. **轨迹选择模块（IRL Scorer）**: 场景-轨迹 Transformer E 和评分 Transformer S 为每条候选轨迹计算类人性评分 zᵢ

### MDP 形式化

TreeIRL 针对 1D 纵向控制（车道跟随 + ACC）定义了显式 MDP：

- **状态空间 S**: s = (x_ego, ẋ_ego, ẍ_ego, x_lead, ẋ_lead, ẍ_lead, t, x_max, ẋ_max)
  - 自车和前车的纵向偏移、速度、加速度，时间偏移，最大偏移（目标/红绿灯），限速
- **动作空间 A**: 纵向加加速度（jerk），离散为 5 个值 {-2, -1, 0, 1, 2} m/s³
- **转移函数 T**: 确定性运动学前向积分，Δt = 0.5s，加速度限制 [-7, 2] m/s²，不允许倒车
- **规划时域 H**: 8s，最大搜索深度 16
- **奖励函数 R**: 11 项加权组合（见下文）

### 奖励函数设计

MCTS 使用手工设计的 11 项加权奖励函数，α = 1/30，γ = 0.99：

| 项 | 含义 | 权重 |
|---|---|---|
| w_jerk · ẍ_ego² | jerk 代价（舒适性） | 0.05 |
| w_accel · ẍ_ego² | 加速度代价（舒适性） | 0.2 |
| w_speed · \|ẋ_max - ẋ_ego\| | 限速跟踪 | 0.1 |
| -2w_speed · I[\|ẋ_max - ẋ_ego\| < 0.5] | 接近限速奖励 | — |
| w_collision · I[x_ego ≥ x_lead] · (ẋ_lead - ẋ_ego)² | 避免追尾 | 10.0 |
| w_collision · I[x_ego ≥ x_max] · ẋ_ego² | 避免闯红灯/越界 | 10.0 |
| w_clearance · I[0 < x_lead - x_ego < δ] · (x_lead - x_ego - δ)² | 前车 clearance | 10.0 |
| w_clearance · I[0 < x_max - x_ego < δ] · (x_max - x_ego - δ)² | 静态 clearance | 10.0 |
| w_stop · I[ẋ_ego≈0 ∧ δ≤x_lead-x_ego<3m] · (ẋ_max - 2ẋ_ego) | 停车位置（前车） | 0.1 |
| w_stop · I[ẋ_ego≈0 ∧ 0≤x_max-x_ego<2m] · (ẋ_max - 2ẋ_ego) | 停车位置（静态） | 0.1 |

### MCTS 轨迹生成器

- **树策略**: PUCTS 公式
  - UCB(s,a) = Q(s,a)/Q_max + c_puct · P(s,a) · √(Σ_b N(s,b)+1) / (N(s,a)+1+ε)
  - c_puct = 1，Q_max = 1，ε ~ U(0, 0.001)
- **先验 P**: 学习得到的 RL 策略 π_θ 或均匀分布 U_A
- **评估函数**: 神经网络值估计 V̂_θ(s) 或蒙特卡洛 rollout 累积回报
- **Rollout 策略选项**: RL 策略 π_θ、IDM 策略 π_IDM、恒速策略 π_CS
- **Padding 策略**: 从 top-k 叶子节点出发，使用 padding 策略生成完整轨迹
- **轨迹提取**: DFS 遍历 top-k 最受欢迎叶子（按 N(s,a) 降序），再 padding 至完整轨迹

### 延迟优化工程

MCTS 上车的关键工程是**用 IDM 替代学习策略**进行 rollout 和 padding：

| 配置 | 延迟 |
|---|---|
| RL prior + RL rollout + RL padding | ~912ms |
| RL prior + RL value + RL padding | ~256ms |
| **Uniform prior + IDM rollout + IDM padding** | **~10ms** |
| Uniform prior + CS rollout + IDM padding | ~5ms |

**选中配置**: n=400 迭代，k=100 轨迹，P ≡ U_A，π_rollout ≡ π_IDM，π_padding ≡ π_IDM。在 Lenovo ThinkPad i9-10885H 单线程上达到 10Hz 要求。

**关键洞察**: RL 策略在低维状态空间（仅 ego + lead）下因"看不见"侧向车辆而过度保守；MCTS 虽用同样低维状态，但可通过 PBP 预测在搜索中预判交互，因此表现更好。

### IRL 评分器训练

- **训练数据**: 1360 小时人类专家驾驶数据
- **方法**: 最大熵 IRL，形式化为分类问题
- **最优轨迹 τ***: 与人类专家轨迹 L2 距离（位置+速度）最近的候选轨迹，排除与其他智能体未来轨迹碰撞的轨迹
- **损失函数**: Focal Loss 与负对数似然结合
  - loss = -(1 - P(τ*))^γ · log P(τ*)
  - P(τ*) = exp(z*) / Σ_i exp(z_i)
- **训练细节**: L2 误差按时间指数衰减（近期航点权重更高），速度维度加权 5x
- **优化器**: PPO（Proximal Policy Optimization）
- **RL 网络 f_θ**: 2 层 256 单元 MLP，用于训练时生成先验策略，但在线推理时不使用

### 部署策略

ML 规划器输出不满足运动学可行性，需要后处理。TreeIRL 的发现：

- **Lab2Car**: 将轨迹转换为时空约束，用工业级 MPC 求解。延迟高（P50 ~60ms, Max ~247ms）。
- **Smoother**: 轻量级 MPC，运动学校正 + 连续性 bookkeeping。延迟低（P50 ~14ms, Max ~26ms）。

**关键发现**: MCTS 与 Lab2Car **不兼容**！MCTS 从测量位姿规划，假设上一周期命令已执行；Lab2Car 的 bookkeeping 在异步环境中导致延迟敏感，可能引发突然急刹。因此 TreeIRL 采用 Smoother，DriveIRL 采用 Lab2Car。

### 实验结果

**nuPlan 模拟**（7051 场景）: 安全性与 DriveIRL-Safe 持平（碰撞率 0.01），舒适性 0.98，L2 误差 3.75m。在挑战性 cut-in 子集（134 场景）中碰撞率约为 MCTS/DriveIRL 的一半。

**真实道路**（拉斯维加斯，268.4 auto-miles）:
- 安全接管: **17.89 mi/event**（DriveIRL 仅 1.43）
- ACC 失败: **>268.4 mi/event** 零失败（DriveIRL 2.57）
- Cut-in 失败: **>268.4 mi/event** 零失败（DriveIRL 5.36）
- 安全性能比 DriveIRL 好约 **2 个数量级**

**关键优势**

| 维度 | 表现 |
|------|------|
| 安全性 | 真实道路安全接管率比 DriveIRL 好 ~2 个数量级 |
| 舒适性 | 纵向 jerk 和 accel 指标优于所有基线 |
| 类人性 | L2 误差接近 DriveIRL，显著优于 IDM/Gigaflow |
| 灵活性 | MCTS 搜索实现行为灵活性，无需预定义行为模板 |
| 实时性 | ~10ms 延迟满足 10Hz 车载运行要求 |

### 局限

- **1D 纵向控制**: 未处理横向控制（换道、避障），委托给后处理
- **非反应性世界模型**: 使用 PBP top-mode 预测，无法捕捉真实交通流动态响应
- **数据依赖**: IRL 评分器需要 1360 小时专家数据，训练成本高
- **奖励函数手工设计**: MCTS 的 11 项奖励仍需手工调参

### 模拟到真实（Sim-to-Real）

- nuPlan 模拟测试（7051 场景）: TreeIRL 相比其他方法仅有小幅优势
- 高保真全栈模拟（Object Sim）: 优势开始显现
- 真实城市道路测试（500+ 英里）: 优势放大至 **1-2 个数量级**
- **原因**: 安全接管是主观指标（感知安全），接管时的急刹进一步影响舒适性；模拟器的非反应性 log-playback 和简化物理无法完全复现真实交互
- **方法论启示**: 评估应采用 nuPlan（快速筛选）→ Object Sim（集成验证）→ 真实道路（最终评估）的三层漏斗

## 关联连接

- [[摘要-treeirl-safe-urban-driving]] — 论文来源摘要
- [[syntheses/2025-TreeIRL-Safe-Urban-Driving-with-Tree-Search-and-Inverse-Reinforcement-Learning]] — L3 精读笔记
- [[MonteCarloTreeSearch]] — TreeIRL 使用的搜索方法
- [[DriveIRL]] — TreeIRL 所基于的先验架构
- [[InverseReinforcementLearning]] — IRL 基础范式
- [[MaximumEntropyIRL]] — 训练使用的理论基础
- [[SimToRealGap]] — 论文讨论的关键问题
- [[MomchilSTomov]] — 论文第一作者
- [[Motional]] — 研究团队所属公司
