---
title: "Sim-to-Real Gap"
type: concept
tags: [自动驾驶, 模拟到真实, 强化学习, 仿真验证, 运动规划]
sources: [raw/09-archive/TreeIRL - Safe Urban Driving with Tree Search and Inverse Reinforcement Learning.pdf]
last_updated: 2026-05-12
---

## 定义

Sim-to-Real Gap（模拟到真实差距）是指自动驾驶规划器或控制策略在模拟器中的表现与在真实世界中的表现之间的差异。由于模拟器难以完全复现真实交通环境的复杂性、传感器噪声、其他道路使用者的不可预测行为等因素，模拟评估结果往往无法准确预测实际部署性能。

## 关键信息

### TreeIRL 揭示的 Sim-to-Real 现象

TreeIRL 论文通过三层评估体系揭示了显著的 sim-to-real gap：

| 评估层级 | 场景数/里程 | TreeIRL vs DriveIRL 安全性能差距 |
|---|---|---|
| nuPlan 模拟 | 7051 场景 | 小幅领先（碰撞率均为 0.01） |
| Object Sim 高保真模拟 | 717 场景，30s 闭环 | 优势开始显现 |
| 真实道路（拉斯维加斯） | 268.4 auto-miles | **~2 个数量级**（安全接管 17.89 vs 1.43 mi/event） |

**量化对比**:
- **nuPlan**: TreeIRL 碰撞率 0.01，DriveIRL 碰撞率 0.01 — 几乎无差别
- **真实道路**: TreeIRL 安全接管 17.89 mi/event，DriveIRL 仅 1.43 mi/event — 12.5 倍差距
- **ACC/Cut-in 零失败**: TreeIRL 在 268.4 英里中零 ACC 失败、零 cut-in 失败；DriveIRL 分别为 2.57 和 5.36 mi/event

**深层原因分析**:
1. **主观安全指标**: 安全接管不仅基于实际碰撞风险，还基于安全驾驶员的**感知安全**。即使不会实际碰撞，行为被感知为不安全也会触发接管
2. **接管连锁反应**: 接管时的突然急刹进一步影响舒适性指标，形成恶性循环
3. **模拟器简化**: nuPlan 使用 Python 简化模拟器、仅模拟规划器、非反应性 log-playback；Object Sim 虽模拟全栈但仍使用 log-playback
4. **物理差异**: 真实车辆动力学、传感器噪声、控制延迟无法在模拟中完全复现
5. **交互复杂性**: 真实人类驾驶员的反应比模拟预设更复杂、更多样

### 三层评估漏斗（TreeIRL 方法论）

TreeIRL 提出的系统性评估流程：

1. **快速模拟筛选**（nuPlan）: 大规模、低成本，用于参数调优和排除明显劣等规划器
2. **高保真集成验证**（Object Sim）: 模拟完整 AV 栈，捕获集成问题，更接近真实部署
3. **真实道路最终评估**（On-road）: 在目标运营区域进行实际测试，评估主观体验和边缘案例

**关键原则**: 模拟器评估是**必要但不充分**的；即使 7000+ 场景的大规模模拟也可能严重低估真实性能差异。

### 缓解策略

1. **在环测试**: 将候选规划器部署到真实车辆进行道路测试
2. **领域随机化**: 在模拟中引入多样化环境参数
3. **自博弈（Self-Play）**: 使用 Gigaflow 等方法让同一策略控制所有智能体，改善交互真实性
4. **多维度评估**: 不仅关注单一指标，而是综合评估安全、进度、舒适、类人等多维度
5. **混合评估流程**: 先在模拟中筛选候选方法，再对少量 promising 方法进行真实部署
6. **主观指标**: 在真实道路测试中纳入安全接管、舒适度等主观/半主观指标

### 对规划器评估的启示

- 模拟评估是**必要但不充分**的条件
- 早期在环测试应成为全面评估的关键组成部分
- 规划器比较应同时采用**整体性**（多指标）和**增量性**（先模拟后真实）策略

## 关联连接

- [[TreeIRL]] — 揭示显著 sim-to-real gap 的论文
- [[摘要-treeirl-safe-urban-driving]] — 论文来源摘要
- [[syntheses/2025-TreeIRL-Safe-Urban-Driving-with-Tree-Search-and-Inverse-Reinforcement-Learning]] — L3 精读笔记
- [[GeneralPurposePlanner]] — 另一种需要考虑 sim-to-real 的规划范式
- [[Motional]] — 在真实城市道路进行大规模测试的公司
