---
title: "摘要-efficient-sampling-based-maximum-entropy-irl"
type: source
tags: [来源, 论文, 逆强化学习, 自动驾驶]
sources: [https://arxiv.org/abs/2006.13704]
last_updated: 2026-05-09
---

## 核心摘要

本文提出了一种高效的基于采样的最大熵逆强化学习算法（SMIRL），用于在连续域中从人类驾驶数据中学习奖励函数。与现有IRL算法不同，SMIRL通过引入高效的连续域轨迹采样器，能够直接在连续空间中学习奖励函数，同时考虑人类驾驶示范中的不确定性。该算法在INTERACTION数据集中的非交互和交互驾驶场景上进行了验证，实验结果表明其在预测精度、收敛速度和泛化能力上均优于CIOC、Opt-IRL和GCL等基线方法。

SMIRL的核心技术贡献包括：（1）将非保守防御性运动规划算法与采样方法结合，生成可行且代表性的长时域轨迹样本；（2）在特征空间中通过欧氏距离进行样本重分布，消除概率评估中的偏差；（3）采用线性结构化奖励函数，特征包括速度、纵向/横向加速度、纵向急动度以及交互场景中的未来距离和未来交互距离。

实验结果显示，SMIRL在非交互场景中的MED为0.21m，交互场景中仅为0.066m；收敛时间仅需5-6分钟，而CIOC需要60-90分钟，Opt-IRL更是长达1260-1800分钟。

## 关联连接

- [[SMIRL]] — 本文提出的采样型最大熵逆强化学习算法
- [[../syntheses/2020-Efficient-Sampling-Based-Maximum-Entropy-IRL]] — 本文 L3 精读笔记
- [[InverseReinforcementLearning]] — 逆强化学习范式
- [[MaximumEntropyIRL]] — 最大熵逆强化学习基础方法
- [[ZhengWu]] — 论文第一作者
- [[LitingSun]] — 论文共同第一作者
- [[WeiZhan]] — 论文作者，INTERACTION数据集贡献者
- [[ChenyuYang]] — 论文作者
- [[MasayoshiTomizuka]] — 论文通讯作者导师
