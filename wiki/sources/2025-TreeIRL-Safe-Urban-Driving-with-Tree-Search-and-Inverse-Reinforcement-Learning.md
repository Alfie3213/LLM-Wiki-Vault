---
type: paper-note
level: L3
paper_title: "TreeIRL: Safe Urban Driving with Tree Search and Inverse Reinforcement Learning"
arXiv: "2509.13579"
date: 2025-10-25
authors: Momchil S. Tomov, Sang Uk Lee, Hansford Hendargo, Jinwook Huh, Teawon Han, Forbes Howington, Rafael da Silva, Gianmarco Bernasconi, Marc Heim, Samuel Findler, Xiaonan Ji, Alexander Boule, Michael Napoli, Kuo Chen, Jesse Miller, Boaz Floor, Yunqing Hu
fields:
  - 逆强化学习
  - 蒙特卡洛树搜索
  - 自动驾驶
  - 运动规划
  - 模拟到真实
  - 混合方法
paper_type: 会议/期刊投稿
reading_date: 2026-05-12
priority: P0
last_updated: 2026-05-13
---

# [L3 精读] TreeIRL: Safe Urban Driving with Tree Search and Inverse Reinforcement Learning

## 核心问题

**研究动机**: 自动驾驶中的运动规划面临一个根本性张力——基于规则的古典方法（IDM、MPC、A*、MCTS）能提供安全保证和灵活的场景适应能力，但常产生不舒适、不自然的行为，且参数难以用数据调优；基于机器学习的方法（模仿学习 IL、强化学习 RL）能随数据规模提升类人性，但在安全关键边缘案例中表现脆弱，且存在分布偏移问题。如何结合两者的优势，同时避免各自的缺陷？

**核心问题**: 如何将蒙特卡洛树搜索（MCTS）与逆强化学习（IRL）结合，构建一个既安全又舒适、既灵活又类人的自动驾驶规划器，并首次在真实城市道路上验证 MCTS-based 规划器？

**一句话概括**: TreeIRL 将 MCTS 重新定位为轨迹生成器（而非传统的单动作选择器），利用其决策时搜索能力聚焦于当前场景行为上合适的轨迹子空间，再通过在 1360 小时人类专家数据上训练的深度 IRL 评分器从中选择最类人的轨迹，在模拟和真实城市道路（拉斯维加斯，500+ 英里）上均达到 SOTA 性能。

## 核心贡献

1. **MCTS 作为轨迹生成器的重新定位**: 传统 MCTS 输出单动作，TreeIRL 让 MCTS 输出一组候选轨迹（k=100），显著降低了对 MCTS 的收敛要求，使其能在真实 AV 的计算预算内运行（~10ms）。
2. **首次 MCTS 规划器的真实道路验证**: 在拉斯维加斯市区 500+ 英里的真实自动驾驶中验证了 TreeIRL，此前所有 MCTS 在自动驾驶中的应用仅限于模拟。
3. **系统性的延迟优化策略**: 通过替换学习策略为 IDM 策略进行 rollout 和 padding，将延迟从 ~900ms 降至 ~10ms，同时保持或提升性能。
4. **全面的模拟+真实世界评估**: 在 nuPlan（7051 场景）、高保真全栈模拟器（Object Sim，600+ 场景）和真实道路（268.4 auto-miles）三个层级上评估，揭示了模拟器评估与真实性能之间的显著差距。
5. **混合架构的设计洞察**: MCTS 负责安全和行为模式选择，IRL 负责舒适性和类人性细化，两者互补形成"整体大于部分之和"的效果。

## 背景与问题定义

### 问题背景

自动驾驶运动规划方法可分为三类：
- **古典方法**: IDM（封闭形式 ACC）、MPC（连续优化）、A*/MCTS（离散搜索）。安全可解释，但行为生硬，参数难以数据驱动调优。
- **模仿学习（IL）**: 从人类示范中学习驾驶策略。PBP 等回归方法在预测任务上表现优异，但存在分布偏移问题，在安全关键场景中脆弱。
- **强化学习（RL）**: 通过闭环仿真试错学习策略。Gigaflow 通过自博弈（self-play）达到 SOTA，但严重依赖奖励函数设计和仿真环境真实性。

混合方法试图结合两者优势：
- DriveIRL-Safe: DriveIRL + 规则安全过滤器
- Lab2Car: ML 轨迹 → 时空约束 → MPC 满足
- SafetyNet: 将不可行 ML 轨迹投影到启发式车道跟随轨迹集

**MCTS 的独特价值**: MCTS 结合古典搜索与 ML 先验，能在决策时探索巨大的轨迹空间。但主要挑战是延迟——迭代次数随状态空间指数增长，且 ML 策略在 MCTS 循环内推理会进一步增加延迟。

### 形式化定义

**MDP 形式化**: M = ⟨S, A, T, R, γ, F⟩
- **状态空间 S**: s = (x_ego, ẋ_ego, ẍ_ego, x_lead, ẋ_lead, ẍ_lead, t, x_max, ẋ_max)
  - 包含自车和前车的纵向偏移、速度、加速度，时间偏移，最大偏移（目标或红绿灯），限速
- **动作空间 A**: 纵向加加速度（jerk），离散为 5 个值：{-2, -1, 0, 1, 2} m/s³
- **转移函数 T**: 确定性前向积分，自车使用运动学模型，前车使用 PBP 预测（top mode），时间步长 Δt = 0.5s
- **规划时域 H**: 8s，最大搜索深度 16
- **奖励函数 R**: 加权组合 11 项代价

### 挑战与难点

1. **延迟约束**: AV 规划器需在 10Hz 或更高频率运行，MCTS 传统配置无法满足实时要求
2. **覆盖 vs 精度**: 轨迹生成器需提供足够覆盖（recall），选择器需具备足够判别力（precision）
3. **Sim-to-Real 差距**: 模拟器中的微小优势可能在真实世界中被放大或缩小
4. **部署策略选择**: ML 规划器输出不满足运动学可行性，需要后处理（Lab2Car vs Smoother）

## 方法

### 核心思想

**分工原理**: MCTS 负责"安全与行为模式选择"（选择行为模式，如减速、加速、停车），IRL 负责"舒适与类人性细化"（在该模式附近选择最类人的具体轨迹）。

**关键洞察**: 通过将 MCTS 从"寻找最佳单动作"重新定位为"生成有前景的候选轨迹集"，可以大幅降低对 MCTS 的收敛要求，从而允许使用更简单的 MCTS 变体（无学习策略参与 rollout/padding），满足车载实时约束。

### 技术细节

#### 组件 1: MCTS 轨迹生成器

**MDP 组件**:
- **状态**: 1D 纵向控制状态（自车+前车+静态信息）
- **动作**: 离散 jerk 命令 5 档
- **转移**: 确定性运动学积分，加速度限制 [-7, 2] m/s²，不允许倒车
- **终止**: t ≥ H (8s)

**奖励函数（11 项）**:
| 项 | 公式 | 含义 | 权重 |
|---|---|---|---|
| jerk 代价 | w_jerk · ẍ_ego² | 舒适性 | 0.05 |
| 加速度代价 | w_accel · ẍ_ego² | 舒适性 | 0.2 |
| 限速偏差 | w_speed · \|ẋ_max - ẋ_ego\| | 跟踪限速 | 0.1 |
| 限速接近奖励 | -2w_speed · I[\|ẋ_max - ẋ_ego\| < 0.5] | 鼓励接近限速 | — |
| 碰撞惩罚（前车） | w_collision · I[x_ego ≥ x_lead] · (ẋ_lead - ẋ_ego)² | 避免追尾 | 10.0 |
| 碰撞惩罚（静态） | w_collision · I[x_ego ≥ x_max] · ẋ_ego² | 避免闯红灯/越界 | 10.0 |
|  clearance（前车） | w_clearance · I[0 < x_lead - x_ego < δ] · (x_lead - x_ego - δ)² | 保持车距 | 10.0 |
|  clearance（静态） | w_clearance · I[0 < x_max - x_ego < δ] · (x_max - x_ego - δ)² | 保持边界距离 | 10.0 |
| 停车位置（前车） | w_stop · I[ẋ_ego≈0 ∧ δ≤x_lead-x_ego<3m] · (ẋ_max - 2ẋ_ego) | 不停车过远 | 0.1 |
| 停车位置（静态） | w_stop · I[ẋ_ego≈0 ∧ 0≤x_max-x_ego<2m] · (ẋ_max - 2ẋ_ego) | 不停车过远 | 0.1 |

缩放因子 α = 1/30，折扣因子 γ = 0.99。

**MCTS 算法（Algorithm 1）**:
- **树策略**: PUCTS 公式
  - UCB(s,a) = Q(s,a)/Q_max + c_puct · P(s,a) · √(Σ_b N(s,b)+1) / (N(s,a)+1+ε)
  - c_puct = 1，Q_max = 1，ε ~ U(0, 0.001) 破平局噪声
- **先验 P**: 学习得到的 RL 策略 π_θ 或均匀分布 U_A
- **评估**: 两种方式
  - Bootstrap: 神经网络值估计 V̂_θ(s)
  - Monte Carlo: rollout 策略 π_rollout 的累积回报
- **Rollout 策略选项**: RL 策略 π_θ、IDM 策略 π_IDM、恒速策略 π_CS
- **Padding 策略**: 从 top-k 叶子出发，π_padding 生成完整轨迹

**关键延迟发现**（Table I）:
- π_θ 作为 rollout: ~912ms（不可接受）
- V̂_θ 作为评估: ~256ms（不可接受）
- π_IDM 作为 rollout + π_IDM 作为 padding: **~10ms**（选中配置）
- π_CS 作为 rollout + π_IDM 作为 padding: ~5ms

**选中配置**: n=400 迭代，k=100 轨迹，P ≡ U_A（均匀先验），π_rollout ≡ π_IDM，π_padding ≡ π_IDM。

#### 组件 2: IRL 评分器

**架构**: 场景-轨迹 Transformer E + 评分 Transformer S
- 输入: 场景上下文 c、场景嵌入 h、候选轨迹 τ_i
- 输出: 类人性评分 z_i（logit）

**训练**:
- **数据**: 1360 小时人类专家驾驶数据
- **方法**: 最大熵 IRL，形式化为分类问题
- **最优轨迹 τ***: 与人类专家轨迹 L2 距离（位置+速度）最近的候选轨迹，排除与其他智能体未来轨迹碰撞的轨迹
- **损失函数**: Focal Loss + 负对数似然
  - loss = -(1 - P(τ*))^γ · log P(τ*)
  - P(τ*) = exp(z*) / Σ_i exp(z_i)
- **优化器**: PPO（Proximal Policy Optimization）
- **训练细节**: L2 误差按时间指数衰减（近期航点权重更高），速度 L2 上加权 5x 以改善跟随行为和舒适性

#### 组件 3: 部署策略

两种后处理方案对比：
- **Lab2Car [19]**: 将轨迹转换为时空约束（"机动"），用工业级 MPC 求解。延迟高（P50 ~60ms, Max ~247ms）。
- **Smoother**: 轻量级 MPC，进行运动学校正和周期间连续性 bookkeeping。延迟低（P50 ~14ms, Max ~26ms）。

**关键发现**: MCTS 与 Lab2Car 不兼容！因为 MCTS 从测量位姿规划，而 Lab2Car 的 bookkeeping 机制在异步环境中对延迟敏感。MCTS 假设上一周期的 jerk 命令已执行，快速迭代后可能突然命令急刹，导致不适。因此 TreeIRL/MCTS 采用 Smoother，DriveIRL 采用 Lab2Car。

## 实验

### nuPlan 模拟评估（7051 场景，~40 小时数据）

**数据集**: 拉斯维加斯市区，包含 Strip (68%)、市中心 (17%)、机场 (7%) 等。场景类型：静止起步 (12%)、减速 (12%)、cut-in (3%)、挑战性 cut-in (2%)、挑战性 ACC (3%)、前车刹车 (2%)、静止等待 (18%)、正常驾驶 (48%)。

**基线**: IDM、MCTS (k=1)、PBP、DriveIRL、Gigaflow、DriveIRL-Safe

**结果**（Table II）:
- **安全性**: DriveIRL-Safe 和 TreeIRL 最佳（碰撞率 0.01），显著优于 IDM (0.02)、PBP (0.05)
- **舒适性**: TreeIRL (0.98) 接近最优，DriveIRL-Safe 较低 (0.93)
- **类人性 L2 误差**: DriveIRL (3.53m) 最佳，TreeIRL (3.75m) 接近
- **挑战性 cut-in 子集**（134 场景）: DriveIRL-Safe 和 TreeIRL 碰撞率约为 MCTS/DriveIRL 的一半

### 高保真模拟评估（Object Sim，600+ 场景，30s 闭环）

模拟整个 AV 栈（而非仅规划器），更接近真实部署。

**结果**（Table VI 上半）:
- TreeIRL 安全性与 MCTS 相当，舒适性优于 MCTS
- DriveIRL-Safe 安全性与 TreeIRL 接近，但舒适性更差
- 个别案例分析（Fig. 8）: DriveIRL 未能及时停车导致碰撞，DriveIRL-Safe 停车过早过急，TreeIRL 平衡最佳

### 真实道路评估（拉斯维加斯，Hyundai IONIQ 5）

**总里程**: TreeIRL 268.4 auto-miles，远超 MCTS (115.8)、DriveIRL-Safe (87.9)、DriveIRL (64.3)

**关键结果**（Table VI 下半）:
| 指标 | MCTS | DriveIRL | DriveIRL-Safe | TreeIRL |
|---|---|---|---|---|
| 安全接管（mi/event）↑ | 7.68 | 1.43 | 6.76 | **17.89** |
| ACC 失败（mi/event）↑ | 57.9 | 2.57 | 87.9 | **>268.4** |
| Cut-in 失败（mi/event）↑ | >115.8 | 5.36 | 43.95 | **>268.4** |
| 缓慢驾驶（mi/event）↑ | >115.8 | 5.85 | 2.84 | **134.2** |
| 不适（mi/event）↑ | 1.40 | 1.74 | 1.05 | **2.42** |
| 急刹（mi/event）↑ | 2.83 | 6.43 | 2.20 | **13.42** |

**震撼发现**: TreeIRL 在真实道路上的安全性能比 DriveIRL **好约 2 个数量级**！ACC 和 cut-in 场景零安全接管。

**Cut-in 案例分析**（Fig. 9）:
- MCTS: 减速有些颠簸
- DriveIRL: 减速不足，导致安全接管
- DriveIRL-Safe: 刹车过急
- TreeIRL: 最平滑的减速曲线

### 核心发现: Sim-to-Real 差距

模拟器中 TreeIRL 的优势相对较小（Table II），但在真实道路上被放大到 **1-2 个数量级**。原因：
1. **主观度量**: 安全接管基于感知安全，即使不会实际碰撞，安全驾驶员也可能接管
2. **接管连锁反应**: 接管时的突然刹车进一步影响舒适性
3. **模拟器局限**: 非反应性 log-playback、简化物理、仅评估规划器而非全栈

**方法论洞察**: 模拟器适合参数调优和排除明显劣等规划器，高保真全栈模拟器提供更真实的感知，但真实道路测试仍是全面评估的必要环节。

## 批判性分析

### 优点

1. **架构简洁优雅**: MCTS 生成候选集 + IRL 选择最佳，分工明确，模块间松耦合
2. **延迟工程极具价值**: 将 ~900ms 降至 ~10ms 的系统工程是 MCTS 首次上路的决定性因素
3. **真实世界验证**: 500+ 英里真实道路测试在规划领域极为罕见，结果可信度高
4. **多层级评估**: nuPlan → Object Sim → 真实道路的三层漏斗式评估方法论严谨
5. **开放性**: 明确承认局限并指出未来方向，学术态度诚实

### 缺点与局限

1. **1D 纵向控制**: 当前仅处理纵向控制（ACC + 车道跟随），横向控制委托给后处理。这在复杂城市环境中显然不足（无换道、无避障）。
2. **非反应性世界模型**: 使用 PBP 的 top-mode 预测作为非反应性世界模型，简化了交互但可能无法捕捉真实交通流的动态响应。
3. **IRL 评分器训练依赖**: IRL 评分器需要在 1360 小时数据上训练，数据成本高昂。且训练时 τ* 的选择基于 L2 距离，对多模态行为（同等合理的多种驾驶方式）处理不足。
4. **奖励函数手工设计**: MCTS 的奖励函数仍是手工设计的 11 项加权组合，调参工作量不小。
5. **计算资源限制**: 虽然在单线程 CPU 上达到 10ms，但未讨论多线程/并行化潜力，也未与 GPU 加速方案对比。

### 可改进方向

1. **学习奖励函数替代手工 R**: 论文末尾指出可用学习得到的奖励函数直接替代手工设计的 R(Eq. 2-11)，从而可能省去 IRL 评分器
2. **考虑预测不确定性**: 使用多模式预测而非仅 top-mode，在 MCTS 中平均奖励
3. **反应性世界模型**: 用 TrafficSim 或 Gigaflow 替代非反应性 PBP 预测
4. **扩展到 2D 规划**: 将状态/动作空间扩展到横向，实现换道和车道偏置。但这需要更多 MCTS 迭代或更强的引导策略
5. **RL+IL 混合训练 f_θ**: 用 RL 和 IL 的混合训练策略网络，可能进一步提升性能

## 反思

### 成功之处

- 清晰的问题定位和贡献陈述
- 系统的延迟优化工程展示了从研究到落地的完整路径
- 真实世界验证填补了 MCTS 在自动驾驶领域的空白
- 对 sim-to-real gap 的深刻观察和讨论具有方法论价值

### 遇到的问题

- **MCTS + Lab2Car 的不兼容**: 这是一个意料之外的工程问题，通过深入分析延迟和 bookkeeping 机制才得以解决
- **RL 策略的过度保守**: 由于低维状态空间无法感知侧向车辆，RL 策略倾向于保守行为，反而不如简单的 IDM 有效
- **模拟器指标的饱和**: nuPlan 中碰撞率普遍很低，难以区分规划器优劣，需要聚焦挑战性子集

### 改进方案

- 对 MCTS 使用 Smoother 而非 Lab2Car 是关键的工程决策
- 用 IDM 替代学习策略进行 rollout/padding 是延迟优化的核心
- 速度 L2 加权 5x 和近期航点加权是训练细节中的关键技巧

### 关键收获

1. **混合架构的设计哲学**: 不是简单拼接，而是让每个组件负责其最擅长的子问题（MCTS: 安全+模式选择；IRL: 舒适+细化）
2. **延迟是 MCTS 落地的第一性约束**: 再聪明的算法如果无法在实时约束内运行，就无法部署
3. **模拟器评估的局限**: 即使是大规模模拟（7000+ 场景），也可能严重低估真实性能差异

## 记忆抽取（L3 标准）

### Semantic Memories（知识升级，6 条）

#### 常驻记忆块

1. **TreeIRL-Architecture**
   - claim: TreeIRL 的核心架构是 MCTS 生成候选轨迹集（k=100）+ IRL 评分器选择最类人轨迹，MCTS 负责安全与行为模式，IRL 负责舒适与细化
   - evidence: 论文 Fig. 3, Section IV, 实验结果 Table II & VI
   - scope: 自动驾驶运动规划，混合方法
   - related_to: [[TreeIRL]], [[MonteCarloTreeSearch]], [[InverseReinforcementLearning]]
   - confidence: high
   - action: 在设计混合规划器时参考此分工模式
   - memory_tier: core

2. **MCTS-Latency-Engineering**
   - claim: MCTS 在 AV 上实时运行的关键是用简单启发式策略（IDM）替代学习策略进行 rollout 和 padding，可将延迟从 ~900ms 降至 ~10ms，同时保持性能
   - evidence: 论文 Table I, Section V-A
   - scope: MCTS 工程部署，实时系统
   - related_to: [[TreeIRL]], [[MonteCarloTreeSearch]]
   - confidence: high
   - action: 在部署 MCTS 到实时系统时优先评估启发式 rollout 策略
   - memory_tier: core

#### 检索型记忆

3. **TreeIRL-Reward-Function**
   - claim: TreeIRL 的 MCTS 奖励函数包含 11 项加权组合：jerk、加速度、限速跟踪、碰撞惩罚（前车+静态）、clearance（前车+静态）、停车位置（前车+静态），权重范围 0.05-10.0
   - evidence: 论文 Eq. 2-11, Section IV-A
   - scope: MCTS 奖励设计，纵向控制
   - related_to: [[TreeIRL]]
   - confidence: high
   - action: 参考此奖励函数结构进行纵向 MCTS 规划器的奖励设计
   - memory_tier: retrieval

4. **TreeIRL-Sim-to-Real-Gap**
   - claim: TreeIRL 在 nuPlan 模拟中只有小幅优势，但在真实道路上安全性能比 DriveIRL 好约 2 个数量级，凸显了在环测试对评估规划器的必要性
   - evidence: 论文 Table II vs Table VI, Section VI-B
   - scope: 模拟到真实差距，规划器评估方法论
   - related_to: [[TreeIRL]], [[SimToRealGap]]
   - confidence: high
   - action: 评估规划器时应设计模拟→高保真模拟→真实道路的三层漏斗，不依赖单一模拟指标
   - memory_tier: retrieval

5. **TreeIRL-Deployment-Strategy**
   - claim: MCTS 规划器对后处理延迟敏感，与 Lab2Car（重型 MPC）不兼容，应使用轻量级 Smoother；而 DriveIRL 等从测量位姿规划的方法可与 Lab2Car 配合
   - evidence: 论文 Section VI-A, Table IV-V
   - scope: AV 系统部署，规划-控制接口
   - related_to: [[TreeIRL]]
   - confidence: high
   - action: 部署搜索型规划器时优先测试轻量级后处理，验证延迟敏感性
   - memory_tier: retrieval

6. **TreeIRL-Training-Details**
   - claim: IRL 评分器训练使用 Focal Loss + NLL，PPO 优化器，L2 误差按时间指数衰减（近期权重更高），速度维度加权 5x
   - evidence: 论文 Section IV, Eq. loss 公式
   - scope: IRL 训练，损失函数设计
   - related_to: [[TreeIRL]], [[MaximumEntropyIRL]]
   - confidence: high
   - action: 训练轨迹选择器时考虑时间加权损失和速度加权
   - memory_tier: retrieval

### Episodic Memories（阅读洞察，2 条）

1. **MCTS-Trajectory-Generator-Insight**
   - episode: 最初以为 TreeIRL 是 AlphaGo 式的 MCTS（学习策略在循环内推理），但深入阅读后发现核心创新是将 MCTS 重新定位为轨迹生成器，并大幅简化 MCTS（无学习策略参与）以满足延迟约束
   - initial_understanding: MCTS + IRL = 学习策略引导搜索 + IRL 评分
   - confusion: 为何延迟能控制在 10ms？学习策略推理应该很慢
   - resolution_path: 发现实际使用的是均匀先验 + IDM rollout/padding，学习策略仅在离线训练 RL 网络 f_θ 时使用（且 f_θ 最终也未用于在线推理！）
   - verified: 是，Table I 明确显示各种配置的延迟
   - future_hint: 注意区分"训练时使用的策略"和"在线推理时使用的策略"

2. **Sim-to-Real-Magnitude-Surprise**
   - episode: 阅读到真实道路结果时被 2 个数量级的安全性能差异震撼
   - initial_understanding: 模拟器结果大致反映真实性能排序
   - confusion: nuPlan 中 TreeIRL 与 DriveIRL 差距很小，为何真实道路差距如此之大？
   - resolution_path: 安全接管是主观指标，包含感知安全和乘客体验维度；接管时的急刹进一步影响舒适性
   - verified: 是，论文 Section VI-B "Interim discussion" 详细解释
   - future_hint: 模拟器指标即使全面也可能低估真实差距，主观舒适度指标在真实世界中至关重要

### Procedural Memories（程序规则，2 条）

1. **MCTS-Real-Time-Deployment-Rule**
   - rule: 将 MCTS 部署到实时系统时，应优先评估启发式 rollout/padding 策略（如 IDM）替代学习策略，因为延迟收益（~100x）通常超过性能损失
   - applies_to: MCTS 在自动驾驶、机器人等实时系统中的部署
   - why_it_works: 学习策略推理开销大，而简单启发式策略在特定子问题（如纵向跟随）上已足够好
   - when_not_to_use: 当状态空间复杂到启发式无法提供合理行为时（如复杂交互场景）
   - confidence: high
   - action: 设计 MCTS 系统时，将延迟预算作为第一性约束，先评估简单策略，再考虑学习策略加速

2. **Hybrid-Planner-Evaluation-Rule**
   - rule: 评估混合规划器应采用"模拟筛选 + 高保真全栈模拟验证 + 真实道路测试"的三层漏斗，且不能压缩为单一标量分数
   - applies_to: 自动驾驶规划器评估，混合方法验证
   - why_it_works: 不同层级的评估捕获不同问题（模拟快速迭代、高保真捕获集成问题、真实道路捕获主观体验），多指标避免关键缺陷被掩盖
   - when_not_to_use: 早期概念验证阶段可仅使用模拟
   - confidence: high
   - action: 设计规划器评估方案时，预留真实道路测试资源，并设计安全、进度、舒适三类指标

## 知识升级

### 与已有知识的连接

**支持**:
- 与 [[DriveIRL]] 的关系: TreeIRL 直接继承 DriveIRL 的架构，仅替换轨迹生成器，验证了"生成器+选择器"范式的可扩展性
- 与 [[MaximumEntropyIRL]] 的关系: TreeIRL 使用 MaxEnt IRL 训练评分器，与 Driving with Style、SMIRL 等方法理论基础一致
- 与 [[SimToRealGap]] 的关系: 本文提供了 sim-to-real gap 在运动规划领域的具体量化证据（1-2 个数量级）

**扩展**:
- MCTS 不仅可用于离散动作选择，还可重新定位为连续轨迹生成器（这是对 MCTS 应用范式的扩展）
- 奖励函数可以非常详细地手工设计（11 项），且每一项都有明确的物理/行为含义

**冲突**:
- 之前理解 MCTS + ML 意味着学习策略在搜索循环内推理；本文证明对于实时系统，简单启发式策略在循环内可能优于学习策略

### 旧知识修订

| 旧知识 | 修订类型 | 新理解 |
| --- | --- | --- |
| MCTS 需要学习策略在搜索循环内引导才能达到实时性能 | contradict | 简单启发式策略（IDM）在特定子问题（纵向控制）上足以引导 MCTS，且延迟优势巨大 (~100x) |
| 模拟器性能排名大致反映真实道路排名 | update | 模拟器中的微小优势可能在真实道路上被放大 1-2 个数量级，排名可能不变但差距被严重低估 |
| ML 规划器输出后处理策略是通用的 | update | 搜索型规划器（MCTS）对后处理延迟敏感，需要与轻量级后处理（Smoother）配合，而非重型 MPC（Lab2Car） |

## 行动建议

### 立即行动
- 将 TreeIRL 的奖励函数结构和延迟优化策略纳入混合规划器设计参考
- 在评估规划器时，设计多指标（安全/进度/舒适）和多层评估（模拟/高保真/真实）

### 中期计划
- 探索将 MCTS 轨迹生成器扩展到 2D 规划（横向+纵向），研究状态空间爆炸和延迟控制方法
- 研究用学习奖励函数替代 TreeIRL 中手工设计的 MCTS 奖励函数 R

### 长期方向
- 构建结合 MCTS、RL、IL、IRL 的统一框架，探索不同组合的最优配置
- 深入研究 sim-to-real gap 在运动规划中的机理，开发更好的模拟评估指标

## 引用

```bibtex
@article{tomov2025treeirl,
  title={TreeIRL: Safe Urban Driving with Tree Search and Inverse Reinforcement Learning},
  author={Tomov, Momchil S. and Lee, Sang Uk and Hendargo, Hansford and Huh, Jinwook and Han, Teawon and Howington, Forbes and da Silva, Rafael and Bernasconi, Gianmarco and Heim, Marc and Findler, Samuel and Ji, Xiaonan and Boule, Alexander and Napoli, Michael and Chen, Kuo and Miller, Jesse and Floor, Boaz and Hu, Yunqing},
  journal={arXiv preprint arXiv:2509.13579},
  year={2025}
}
```

---

阅读完成时间: 2026-05-12
笔记字数: ~14KB
记忆条数: 10 条（6 Semantic + 2 Episodic + 2 Procedural）
知识升级: 3 条旧知识修订
验证率: 100%（2/2 个疑问已验证）
