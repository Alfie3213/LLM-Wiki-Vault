---
title: "IntegratedPredictionAndPlanning"
type: concept
tags: [自动驾驶, 运动规划, 轨迹预测, 模块化融合]
sources: [raw/09-archive/Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving.pdf, 摘要-differentiable-integrated-motion-prediction-and-planning]
last_updated: 2026-05-12
---

## 定义

预测与规划集成（Integrated Prediction and Planning）是自动驾驶中的一种系统架构范式，旨在打破传统 pipeline 中预测模块与规划模块相互独立的壁垒，使两者能够相互感知、联合优化，从而提升复杂交互场景下的决策质量。

## 关键信息

### 三种典型架构范式

1. **传统串行预测-规划（Sequential）**
   - 预测模块独立运行，输出周围智能体的未来轨迹
   - 规划模块接收固定预测结果进行自车轨迹优化
   - 缺点：预测结果不感知下游规划需求，可能在关键交互场景下产生不一致

2. **端到端预测-规划（End-to-End）**
   - 使用单一神经网络直接输出自车规划轨迹和周围智能体预测轨迹
   - 隐式处理智能体间交互
   - 缺点：缺乏可解释性和安全性保证

3. **集成预测-规划（Integrated）**
   - 保持预测和规划的模块化结构
   - 通过可微分接口或联合优化使两模块相互感知
   - 预测模块输出规划导向的结果
   - 规划模块的反馈可影响预测行为

### 核心挑战

- **交互建模**: 自动驾驶车辆的行为会影响周围车辆的反应，形成动态交互
- **代价函数设计**: 规划模块需要在安全、效率、舒适等多目标间权衡
- **分布偏移**: 开环训练与闭环部署之间存在行为分布差异
- **开环指标误导**: 模仿学习在开环测试中规划误
差最小，但闭环测试中完全失效（40%+ 碰撞率）

### 联合训练证据（DIPP）

| 测试模式 | DIPP（联合训练） | Sep. Plan+Pred（分离训练） | 优势来源 |
|---------|----------------|------------------------|---------|
| 开环规划误差 @3s | 1.19m | 1.25m | 预测减少有害规划的错误 |
| 开环 ADE | 0.74m | 0.77m | 规划导向的预测结果 |
| 闭环进度 | 77.57m | 76.28m | 预测自适应交互场景 |
| 闭环碰撞率 | 5% | 7% | 更准确的交互预测 |

**关键洞察**: 规划误差反向传播到预测模块，使预测产生"规划导向"的结果（如预判 cut-in、减少不必要停车），从而在不显著降低原始预测精度的情况下提升规划质量。

### 代表性方法

- **DIPP**: 通过可微分优化器实现预测与规划的端到端联合训练
- **GameFormer**: 基于博弈论的 Transformer 交互式预测与规划
- **DTPP**: 可微分联合条件预测与代价评估的树策略规划

## 关联连接

- [[DIPP]] — 基于可微分优化的集成预测与规划框架
- [[DifferentiableMotionPlanning]] — 实现集成的关键技术
- [[摘要-differentiable-integrated-motion-prediction-and-planning]] — 来源论文摘要
