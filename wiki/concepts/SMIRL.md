---
title: "SMIRL"
type: concept
tags: [逆强化学习, 最大熵, 采样方法, 自动驾驶, 轨迹规划]
sources: [https://arxiv.org/abs/2006.13704]
last_updated: 2026-05-09
l3_note: [[2020-Efficient-Sampling-Based-Maximum-Entropy-IRL]]
---

## 定义

SMIRL（Sampling-based Maximum Entropy Inverse Reinforcement Learning）是一种高效的基于采样的最大熵逆强化学习算法，专门设计用于在连续域中从人类驾驶示范中学习可解释的奖励函数。

## 关键信息

### 核心动机
传统IRL算法（如学徒学习、最大熵IRL、贝叶斯IRL）大多定义在离散MDP框架中，难以扩展到高维连续域的长时域应用。连续域IOC算法虽然解决了扩展性问题，但要求示范轨迹必须是最优或次优的，这在自然驾驶数据中难以满足。SMIRL旨在同时满足以下要求：
1. 在高维连续时空中良好扩展
2. 考虑车辆运动学约束
3. 兼容真实交通示范中的不确定性（非最优性）
4. 采用可解释且可泛化的特征

### 算法框架（形式化）
基于最大熵原理，轨迹概率服从 Boltzmann 噪声理性模型：
$$P(\xi; \theta) \propto e^{\beta R(\xi; \theta)}$$

采用线性结构化奖励函数 $R(\xi; \theta) = \theta^T f(\xi)$，通过最大化对数似然学习参数：
$$\theta^* = \arg \max_\theta \frac{1}{M} \sum_{i=1}^M \left( R(\xi_i; \theta) - \log Z_{\xi_i} \right)$$

其中配分函数 $Z_{\xi_i}$ 通过 $K$ 个样本近似：
$$Z_{\xi_i} \approx \sum_{m=1}^K e^{R(\tau_m^i; \theta)}$$

梯度更新为专家特征计数与期望特征计数之差：
$$\nabla_\theta L = \frac{\beta}{M} \sum_{i=1}^M \left( f(\xi_i) - \bar{f}(\xi_i) \right)$$

### 算法框架
SMIRL基于最大熵原理，假设轨迹的累积奖励越高，其出现概率呈指数增长（Boltzmann噪声理性模型）。目标是通过最大化示范数据集的对数似然来恢复奖励函数参数 $\theta$。

### 高效轨迹采样器（一次性设计）
采样器基于 [Gu et al., IROS 2015] 的实时轨迹规划框架，结合非保守防御性策略。关键优势是**一次性采样**：整个训练过程只需生成一次样本（约 1 分钟），后续仅进行梯度更新。

包含三个步骤：
1. **全局路径采样（离散弹性带）**：生成无碰撞路径，通过弹性节点的三种力（收缩力、排斥力、吸引力）和图搜索实现。$F_{threshold}$ 控制样本质量阈值
2. **路径平滑（纯追踪控制）**：将分段线性路径平滑为运动学可行路径
3. **速度采样**：先求时间最优速度曲线，再通过三阶多项式在邻域内采样。交互场景中为每种离散决策（如"让行"/"通过"）分别采样

### 样本重分布
为解决样本在特征空间中非均匀分布导致的概率评估偏差，SMIRL将特征空间均匀划分为离散箱子，并在每个箱子内重新采样，确保均匀覆盖。

**关键发现**：重分布显著改善概率度量（Log Likelihood、Win Count），但对确定性度量（MED）的改善不一致——交互场景中 w/o re-distribution 的 MED 反而更低。这说明概率模型优化与 MAP 预测优化目标可能不完全一致。

### 奖励函数特征
- **非交互特征**：速度偏差、纵向加速度、横向加速度、纵向急动度
- **交互特征**：未来距离（预测时域内最小空间距离）、未来交互距离（到冲突点的纵向距离差）

### 实验性能
在INTERACTION数据集上的对比结果（vs CIOC, Opt-IRL, GCL）：

| 指标 | SMIRL | CIOC | Opt-IRL | GCL |
|------|-------|------|---------|-----|
| NID MED (seen) | **0.21m** | 0.23m | 0.29m | 3.73m |
| ID MED (seen) | **0.066m** | 0.14m | 0.14m | 1.53m |
| NID MED (unseen) | **0.74m** | 0.90m | 0.89m | 46.70m |
| 收敛时间 (NID) | **6 min** | 60 min | 1800 min | 40 min |
| 收敛时间 (ID) | **5 min** | 90 min | 1260 min | 30 min |

- **预测精度**：在所有场景和指标上均最优
- **收敛速度**：相比 Opt-IRL 提升 300 倍，关键原因是一次性采样设计
- **泛化能力**：在未见环境中性能衰减远小于 GCL（GCL 在 NID unseen 中从 3.73m 飙升至 46.70m）
- **数据效率**：仅用 80/150 条轨迹即超越需要大量数据的 GCL

## 关联连接

- [[摘要-efficient-sampling-based-maximum-entropy-irl]] — 论文来源摘要
- [[InverseReinforcementLearning]] — 逆强化学习基础范式
- [[MaximumEntropyIRL]] — 最大熵IRL理论基础
- [[ZhengWu]] — 论文第一作者
- [[LitingSun]] — 论文共同第一作者
- [[WeiZhan]] — 论文作者
- [[MasayoshiTomizuka]] — 论文导师
