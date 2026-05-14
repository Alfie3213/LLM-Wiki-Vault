---
title: 摘要-cost-function-estimation-irl-minimal-observations
type: source
tags:
  - 来源
  - 原始文件
  - 逆强化学习
  - 最大熵
  - 代价函数估计
sources:
  - raw/09-archive/Cost Function Estimation Using Inverse Reinforcement Learning with Minimal Observations.pdf
last_updated: 2026-05-13
---

## 核心摘要

本文提出一种基于最大熵准则的迭代逆强化学习算法，用于在连续空间中推断最优代价函数。与现有方法相比，该算法的核心创新在于：（1）可单独调节每个观测对配分函数的有效性，无需大量样本即可实现更快学习；（2）通过求解最优控制问题（而非随机采样）生成样本轨迹，获得更具信息量的样本。实验在多个模拟环境中与两种 SOTA 算法对比验证了方法优势。

## 关键信息

- **标题**: Cost Function Estimation Using Inverse Reinforcement Learning with Minimal Observations
- **作者**: Sarmad Mehrdad, Avadesh Meduri, Ludovic Righetti
- **arXiv**: 2505.08619 [cs.LG, cs.RO]
- **提交日期**: 2025-05-13
- **PDF大小**: ~3.9 MB
- **方法**: 迭代 MaxEnt IRL，连续空间代价函数估计

## 方法要点

### 核心思想
基于最大熵准则，迭代寻找权重改进步长，并提出一种确定适当步长的方法，确保学习到的代价函数特征与演示轨迹特征保持相似。

### 关键创新
1. **最小观测适配**: 可单独调节每个观测对配分函数的有效性，降低对大量样本的依赖
2. **最优控制采样**: 通过求解最优控制问题生成样本轨迹，相比随机采样产生更具信息量的轨迹
3. **迭代步长策略**: 提出方法确定权重更新的适当步长，保证特征相似性

### 实验对比
与两种 SOTA 算法在多个模拟环境中对比，验证了在样本效率和收敛速度上的优势。

## 关联连接

- [[MaximumEntropyIRL]] — 最大熵逆强化学习基础方法
- [[InverseReinforcementLearning]] — 逆强化学习基础范式
- [[InverseOptimalControl]] — 从专家示范中恢复最优控制代价函数
- [[OptimalControl]] — 最优控制理论
- [[SarmadMehrdad]] — 第一作者
- [[AvadeshMeduri]] — 共同作者
- [[LudovicRighetti]] — 共同作者
