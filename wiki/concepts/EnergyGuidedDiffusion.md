---
title: "EnergyGuidedDiffusion"
type: concept
tags: [扩散模型, 生成模型, 能量函数, 轨迹生成, 自动驾驶]
sources: [https://arxiv.org/abs/2601.23266, 摘要-irl-dal]
last_updated: 2026-05-09
---

## 定义

能量引导扩散模型（Energy-Guided Diffusion）是一种将能量函数引入扩散模型采样过程的生成技术。通过定义反映目标约束（如安全性、平滑性、交通规则）的能量函数，在反向扩散过程中引导采样朝向低能量（高满足度）区域，从而生成满足特定约束的高质量样本。

## 关键信息

### IRL-DAL 中的具体实现

#### 能量函数设计
IRL-DAL 采用五项简单能量项的加权和：

| 能量项 | 数学定义 | 约束目标 |
|--------|---------|---------|
| E_lane | w_lane · (d_lane / s_lane)² | 车道保持 |
| E_lidar | w_lidar · max(0, (d_safe^plan - d_min)/s_lidar)² | 障碍物避让 |
| E_jerk | w_jerk · (1/(H-1)) · Σ ||a_i - a_{i-1}||² | 控制平滑性 |
| E_stability | w_stab · (1/H) · Σ(steer_i² + (speed_i - v_ref)²) | 稳定性 |
| E_expert | w_exp · (1/H) · Σ ||a_i - a_expert_i||² | 专家对齐（可选） |

#### 风险自适应权重
所有风险敏感权重通过危险水平 h_t 动态调整：
- h_t = clamp(1 - tanh(d_min,t / s_h), 0, 1)，s_h = 3.0
- w = w_base · (1 + h_t) 或 w = w_base · (1 + α_hazard · h_t)
- 越接近障碍物，车道保持和障碍物避让权重越高

#### 引导反向扩散
在 IRL-DAL 中，扩散模型作为按需安全监督器：
- 仅在高风险状态激活（d_min < 3.0m 或 |d_lane| > 120px）
- 每去噪步 k：Ã^0_k = Â^0_k - w_g · ∇E(Â^0_k, o_t) / (||∇E|| + ε)
- 引导强度 w_g = 0.1
- 条件向量 c_t ∈ R^64 编码危险水平、横向偏差、速度、转向、LiDAR 统计量

#### 动作混合与执行
- 临界状态（d_min < 1.5m）：完全使用扩散动作（w_b = 1.0）
- 一般状态：混合权重 h_blend = exp(-d_min/s1) + k_tanh·tanh(|d_lane|/s2)
- a_final = w_b·a_DAL + (1-w_b)·a_PPO，w_b 经 EMA 时序平滑

### 与标准扩散模型的区别

| 特性 | 标准扩散模型 | 能量引导扩散 |
|------|------------|-------------|
| 条件生成 | 需训练条件模型 | 通过能量函数即时引导 |
| 约束灵活性 | 固定训练时条件 | 可动态调整能量函数 |
| 安全性保证 | 依赖训练数据 | 显式能量约束 |
| 适用场景 | 通用生成 | 安全关键约束生成 |

## 关联连接

- [[IRLDAL]] — 应用能量引导扩散的自动驾驶框架
- [[DiffusionBasedPlanning]] — 基于扩散模型的运动规划
- [[摘要-irl-dal]] — 来源论文摘要
- [[2026-IRL-DAL]] — L3 精读笔记
