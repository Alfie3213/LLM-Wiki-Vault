---
title: "Inverse Optimal Control"
type: concept
tags: [最优控制, 逆问题, 控制理论, 机器学习, 约束优化]
sources: [https://arxiv.org/abs/2407.19902]
last_updated: 2026-05-07
---

## 定义

逆最优控制（Inverse Optimal Control, IOC）是从专家示范中恢复最优控制问题中代价函数的问题。与 IRL 密切相关，但通常更侧重于连续时间系统和控制理论框架。

## 关键信息

### 与 IRL 的关系
- **IOC**：通常假设系统动力学已知，专注于恢复代价函数
- **IRL**：更广泛的概念，可能同时恢复奖励函数、系统动力学和约束
- **联系**：闭环 IRL 框架在一定假设下可退化为约束逆最优控制问题

### 经典方法
- **残差最小化**: 寻找使最优性条件（KKT、PMP）违反程度最小的参数
- **优势**: 若代价函数结构特殊（如线性参数化）， optimality conditions 对参数线性，可解耦为二次规划问题
- **局限**: 传统方法未考虑阶段约束，或仅处理控制约束

### 一般约束 IOC（本文贡献）
本文建立了约束 IOC 的可恢复性条件：

**假设**:
1. 终止条件为  = Z^{**}$（最优轨迹等于示范轨迹）
2. 示范满足内点 min-max Bellman 方程
3. 阶段代价线性参数化: $\ell = 	heta^	op \phi(x,u)$
4. 终端代价、动力学、约束已知且与 $	heta$ 无关

**可恢复性条件**: 若 {lin,3} 
eq 0$ 且 $\lim_{\mu 	o 0} 	ext{Rank}(J_{lin,1:2}) = m_	heta + m_x$，则参数可解析恢复：

46420	heta^* = [\lim_{\mu 	o 0} -(J_{lin,1:2}^	op J_{lin,1:2})^{-1} J_{lin,1:2}^	op J_{lin,3}]_{1:m_	heta}46420

**关键洞察**: 秩条件仅依赖于收集的示范数据，与系统具体参数无关。

### 约束处理能力对比
| 方法 | 等式约束 | 不等式约束 | 一般非线性约束 |
|------|---------|-----------|--------------|
| 传统 IOC [29,30] | ✗ | ✗ | ✗ |
| Molloy et al. [31] | ✗ | ✓ (仅控制约束) | ✗ |
| **本文** | ✓ | ✓ | ✓ |

## 关联连接

- [[../syntheses/2024-A Differential Dynamic Programming Framework for Inverse Reinforcement Learning]] — 将闭环 IRL 与逆最优控制建立联系的论文（L3 精读笔记）
- [[摘要-ddp-framework-inverse-reinforcement-learning]] — 论文核心摘要
- [[InverseReinforcementLearning]] — 密切相关的方法论
- [[DifferentialDynamicProgramming]] — 本文求解逆最优控制问题的数值方法
- [[PontryaginsMaximumPrinciple]] — 逆最优控制的理论基础
- [[ClosedLoopIRL]] — 退化为约束 IOC 的框架
