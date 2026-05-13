---
title: "IRLDAL"
type: concept
tags: [自动驾驶, 逆强化学习, 扩散模型, 运动规划, 安全导航]
sources: [https://arxiv.org/abs/2601.23266, 摘要-irl-dal]
last_updated: 2026-05-09
---

## 定义

IRL-DAL（Inverse Reinforcement Learning with Diffusion-based Adaptive Lookahead）是一种结合逆强化学习与能量引导扩散模型的自动驾驶安全自适应轨迹规划框架。该框架通过条件扩散模型作为安全监督器生成安全路径，并通过可学习自适应掩码（LAM）动态调整感知注意力，实现专家级的安全导航能力。

## 关键信息

### 完整技术架构

#### 1. 感知模块 — LAM
- 轻量级自适应掩码，仅 **2 个可学习参数**（α_speed, α_lidar）
- 输入：归一化速度 v_norm 和危险水平 h_t = clamp((d_safe - d_min,t)/d_safe, 0, 1)
- 调制因子：f_speed = 1 + α_speed·v_norm，f_hazard = 1 + α_lidar·h_t
- 垂直梯度掩码：M_t(y) = w_base,upper + (w_lower - w_base,upper)·y/(H-1)
- 4 通道输入（RGB + mask），与 CNN 骨干端到端训练

#### 2. FSM 专家数据与感知回放
- FSM 四模式专家：Lane Following / Obstacle Avoidance / Driving Straight / Returning
- 按 FSM 状态分区存储经验缓冲区 D_s，小批量平衡采样
- 解决罕见边缘案例（窄缝通过、车道漂移恢复）欠采样问题

#### 3. 多阶段训练课程
- **Phase 1 — BC 预训练（20k steps）**：策略与扩散规划器同步启动，L_BC = E[||π_θ(s_t) - a*_t||²] + λ_L2||θ_policy||²
- **Phase 2 — PPO 微调（30k steps）**：混合奖励 r_t = 0.7·r_env + 0.3·r_irl
- GAIL 判别器提供密集模仿奖励：r_irl = -log(1 - D_ψ(s_t, a_t) + ε)
- BC 自适应继续（混合阶段每 500 steps）

#### 4. 扩散安全监督器（DAL）
- **按需激活**：仅当 d_min,t < 3.0m 或 |d_lane,t| > 120px 时触发
- 能量引导反向扩散：Ã^0_k = Â^0_k - w_g·∇E/(||∇E|| + ε)，w_g = 0.1
- 五项能量：E = E_lane + E_lidar + E_jerk + E_stability + E_expert
- 风险自适应权重：w = w_base·(1 + h_t)，h_t = clamp(1 - tanh(d_min/s_h), 0, 1)
- 动作混合：a_final = w_b·a_DAL + (1-w_b)·a_PPO，混合权重通过 EMA 时序平滑

#### 5. SAEC 安全感知经验修正
- 扩散动作执行防止即时碰撞（运行时生存）
- 但回放缓冲区存储的是 **FSM 专家恢复动作** 而非扩散动作（安全专家标签）
- 避免策略学习扩散模型的保守偏差，同时积累边缘案例训练数据

### 感知骨干架构
- 视觉：3 层 CNN（kernel 5/3/3）→ z_vision
- LiDAR：3 层 MLP → z_lidar ∈ R^32
- 融合：concat + MLP → s_t ∈ R^512

### 实验结论

| 指标 | 基线 PPO | +FSM Replay | +Diffusion | 完整 IRL-DAL |
|------|---------|-------------|------------|-------------|
| 平均奖励 | 85.2 | 120.4 (+41%) | 155.1 (+29%) | **180.7** (+16%) |
| 碰撞/千步 | 0.63 | 0.30 (-52%) | 0.15 (-50%) | **0.05** (-67%) |
| 成功率 | 78.1% | 88.4% | 92.0% | **96.3%** |
| 动作相似度 | 65.3% | 75.1% | 80.2% | **85.7%** |

- 平台：Webots 仿真器，10 Hz，64×64 RGB + 180 束 LiDAR
- 总训练：50k steps（20k BC + 30k PPO）

### 关键洞察
- SAEC 的解耦设计是核心创新：执行层扩散盾保证即时安全，训练层 FSM 标签维持策略学习效率
- LAM 极简设计（2 参数）带来显著安全增益，证明"物理先验 + 少量可学习参数"的有效性
- 能量引导扩散通过五项简单能量+风险自适应权重实现工程简洁性

## 关联连接

- [[摘要-irl-dal]] — 论文摘要
- [[../syntheses/2026-IRL-DAL]] — L3 精读笔记（完整技术细节与批判性分析）
- [[EnergyGuidedDiffusion]] — 能量引导扩散模型技术
- [[DiffusionBasedPlanning]] — 基于扩散模型的运动规划范式
- [[LearnableAdaptiveMask]] — 可学习自适应感知掩码
- [[InverseReinforcementLearning]] — 逆强化学习理论基础
- [[SeyedAhmadHosseiniMiangoleh]] — 第一作者
