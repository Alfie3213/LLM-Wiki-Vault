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

### 核心架构

1. **FSM 专家模仿初始化**
   - 使用有限状态机（FSM）专家控制器提供稳定的策略初始化
   - 为后续 IRL 和 RL 训练奠定可靠起点

2. **IRL 判别器对齐**
   - 将环境反馈项与 IRL 判别器信号结合
   - 使策略学习目标与专家控制器的行为目标对齐

3. **条件扩散模型安全监督器**
   - 作为轨迹规划的核心安全模块
   - 确保生成路径满足车道保持、障碍物避让、平滑运动等安全约束
   - 基于能量引导的扩散模型生成高质量安全轨迹

4. **可学习自适应掩码（LAM）**
   - 根据车辆速度和周围危险动态调整视觉注意力
   - 实现自适应感知，聚焦关键区域

5. **PPO 微调**
   - 在 FSM 模仿和 IRL 对齐后，使用近端策略优化进行最终微调
   - 混合奖励函数结合环境反馈和 IRL 奖励

### 训练流程

1. **阶段一**: FSM 专家模仿学习 — 获得稳定初始化策略
2. **阶段二**: IRL + 扩散模型监督 — 对齐专家目标并保证安全
3. **阶段三**: PPO 微调 — 在 Webots 仿真器中两阶段课程训练优化

### 实验结果

- **平台**: Webots 仿真器
- **成功率**: 96%
- **碰撞率**: 0.05 次 / 千步
- **能力**: 车道保持、不安全条件处理达到专家水平

## 关联连接

- [[摘要-irl-dal]] — 论文摘要
- [[EnergyGuidedDiffusion]] — 能量引导扩散模型技术
- [[DiffusionBasedPlanning]] — 基于扩散模型的运动规划范式
- [[LearnableAdaptiveMask]] — 可学习自适应感知掩码
- [[InverseReinforcementLearning]] — 逆强化学习理论基础
- [[SeyedAhmadHosseiniMiangoleh]] — 第一作者
