---
title: "DriveIRL"
type: concept
tags: [自动驾驶, 运动规划, 逆强化学习, 轨迹预测, Motional]
sources: [raw/02-papers/2509.13579v4.pdf]
last_updated: 2026-05-12
---

## 定义

DriveIRL 是 Motional 研究团队提出的模块化深度神经网络规划架构，将自动驾驶运动规划分解为三个子模块：场景编码/预测、轨迹生成和 IRL 轨迹选择。TreeIRL 在 DriveIRL 架构基础上，将启发式轨迹生成器替换为蒙特卡洛树搜索（MCTS）生成器。

## 关键信息

### 三阶段架构

1. **编码/预测模块**: 场景编码器 B 和解码器 D 计算场景嵌入 h 并预测其他智能体轨迹 p（使用 PBP 方法）
2. **轨迹生成模块**: 启发式生成器计算加加速度最优轨迹，到达人工选定的纵向目标偏移点集
3. **轨迹选择模块**: IRL 评分器（场景-轨迹 Transformer E + 评分 Transformer S）为每条轨迹计算类人性评分

### 变体

- **DriveIRL**: 基础版本，使用启发式轨迹生成器
- **DriveIRL-Safe**: 在启发式生成器后增加安全过滤器，排除可能与未来轨迹碰撞的候选轨迹

### 与 TreeIRL 的关系

| 维度 | DriveIRL | TreeIRL |
|------|----------|---------|
| 生成器 | 启发式加加速度最优 | MCTS 搜索 |
| 候选轨迹数 k | 143 | 100 |
| 行为多样性 | 有限（固定目标点） | 高（搜索探索） |
| 安全性 | DriveIRL-Safe 版本较好 | 原生安全 |
| 灵活性 | 较低 | 高 |

TreeIRL 保留了 DriveIRL 的 IRL 评分器和 PBP 预测模块，仅替换生成器，实现了模块化的架构升级。

## 关联连接

- [[TreeIRL]] — 在 DriveIRL 架构上将生成器替换为 MCTS 的升级方法
- [[摘要-treeirl-safe-urban-driving]] — TreeIRL 论文来源摘要
- [[Motional]] — 研究团队所属公司
- [[MomchilSTomov]] — 论文第一作者
- [[InverseReinforcementLearning]] — IRL 基础范式
