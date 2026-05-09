---
title: "摘要-irl-dal"
type: source
tags: [来源, 论文, 自动驾驶, 逆强化学习, 扩散模型, 运动规划]
sources: [https://arxiv.org/abs/2601.23266]
last_updated: 2026-05-09
---

## 核心摘要

本文提出 IRL-DAL（Inverse Reinforcement Learning with Diffusion-based Adaptive Lookahead），一种基于能量引导扩散模型的自动驾驶安全自适应轨迹规划框架。该框架首先通过有限状态机（FSM）专家控制器进行模仿学习以提供稳定初始化，随后将环境反馈与 IRL 判别器信号结合以对齐专家目标，并通过 PPO 强化学习进行微调。核心创新在于引入条件扩散模型作为安全监督器规划安全路径，以及可学习自适应掩码（LAM）根据车速和周围危险动态调整视觉注意力。在 Webots 仿真器中使用两阶段课程训练，达到 96% 成功率，碰撞率降至每千步 0.05 次。

## 关键信息

- **论文标题**: IRL-DAL: Safe and Adaptive Trajectory Planning for Autonomous Driving via Energy-Guided Diffusion Models
- **作者**: Seyed Ahmad Hosseini Miangoleh, Amin Jalal Aghdasian, Farzaneh Abdollahi
- **arXiv**: 2601.23266
- **提交日期**: 2026-01-30
- **核心问题**: 自动驾驶中如何在保证安全的前提下实现自适应轨迹规划
- **方法**: FSM 模仿初始化 + IRL 判别器 + 条件扩散模型安全监督 + LAM 自适应感知 + PPO 微调
- **关键洞察**: 扩散模型作为安全监督器可提供路径级安全保证，LAM 根据动态环境调整感知焦点

## 关联连接

- [[IRLDAL]] — 本文提出的基于扩散模型的 IRL 自适应轨迹规划框架
- [[EnergyGuidedDiffusion]] — 能量引导扩散模型技术
- [[DiffusionBasedPlanning]] — 基于扩散模型的运动规划范式
- [[LearnableAdaptiveMask]] — 可学习自适应感知掩码
- [[InverseReinforcementLearning]] — 逆强化学习理论基础
- [[SeyedAhmadHosseiniMiangoleh]] — 第一作者
- [[AminJalalAghdasian]] — 共同作者
- [[FarzanehAbdollahi]] — 共同作者
