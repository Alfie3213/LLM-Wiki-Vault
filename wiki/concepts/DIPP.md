---
title: "DIPP"
type: concept
tags: [自动驾驶, 运动规划, 轨迹预测, 端到端学习, 可微分优化]
sources: [raw/02-papers/2207.10422v2.pdf, 摘要-differentiable-integrated-motion-prediction-and-planning]
last_updated: 2026-05-09
---

## 定义

DIPP（Differentiable Integrated Prediction and Planning）是一种用于自动驾驶的可微分集成预测与规划框架，核心思想是通过端到端可微分架构将周围交通参与者的轨迹预测与自车运动规划紧密结合，并使规划代价函数能够从真实驾驶数据中自动学习。

## 关键信息

### 核心架构

1. **多智能体轨迹预测模块**
   - **输入**: 最近 N=10 个智能体的历史状态（2D 位置、航向、速度、包围盒，H=20 步）+ 局部矢量化地图（最近 6 条车道 + 4 个人行横道）
   - **编码器**: LSTM 编码智能体历史（比 Transformer 更高效），MLP 编码地图元素
   - **交互建模**: 2 层自注意力 Transformer（智能体-智能体）+ 2 个交叉注意力 Transformer（智能体-车道/人行横道）+ 多模态注意力编码器
   - **解码器**: 共享 MLP 解码周围智能体位移，AV 控制动作（加速度、转向角）通过运动学模型转轨迹
   - **输出**: K=3 条联合未来轨迹及概率

2. **可微分运动规划器**
   - 采用可微分的 Gauss-Newton 最小二乘非线性优化器
   - 以预测模块输出的最高概率联合轨迹为输入
   - 使用可微分自行车模型作为运动学约束
   - 显式优化自车轨迹，满足安全性、舒适性、效率、交通规则等多目标
   - 优化问题形式化为加权平方残差和
   - 实现: Theseus 库嵌入 PyTorch 计算图

3. **可学习代价函数**
   - 9 个代价项: 速度效率、加速度、加加速度、转向角、转向速率、车道位置、车道方向、交通灯、安全距离
   - 安全和交通灯权重固定为大值（视为硬约束），其余权重通过 MLP 输出可学习
   - 权重不仅反映人类偏好，还平衡不同代价项的数值尺度

### 训练与验证

- **数据集**: Waymo Open Motion Dataset（10% 子集，10,156 场景，113,622 训练帧）
- **训练目标**: 4 项损失加权 — 预测损失(0.5) + 分数损失(1) + 模仿损失(1) + 规划代价(0.001)
- **训练细节**: Batch 32, Adam, lr=2e-4, 20 epochs, 预训练 5 epochs（无规划器），梯度裁剪 max norm=5
- **开环测试**: 规划轨迹直接与人类轨迹比较（位置误差 @1s/3s/5s）
- **闭环测试**: Log-replay 仿真，AV 执行规划首步后 replan（位置误差 @3s/5s/10s，进度、碰撞率）

### 实验结论

1. **联合训练 > 分离训练**: DIPP 在开环（规划误差 1.19m vs 1.25m）和闭环（进度 77.57m vs 76.28m）均优于分离的预测+规划
2. **IL 开环陷阱**: 模仿学习开环规划误差最小（0.89m）但闭环完全失效（40-42% 碰撞率），开环指标不能反映真实性能
3. **消融实验**: 可学习初始化对开环最关键（无初始化误差从 1.58m -> 2.67m），可学习预测对闭环最关键（无预测失败率从 5% -> 19%）
4. **代价函数学习**: 学习权重显著优于手工调参（规划误差 1.58m vs 1.83m，失败率 5% vs 13%）

### 主要贡献

1. 提出了全可微分的结构化学习框架，集成预测与规划模块
2. 实现了预测结果对下游规划任务的自适应性
3. 代价函数可从真实驾驶数据中自动学习
4. 联合训练优于预测与规划独立训练

## 关联连接

- [[摘要-differentiable-integrated-motion-pred
iction-and-planning]] — 论文摘要
- [[../syntheses/2022-Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving]] — L3 精读笔记
- [[DifferentiableMotionPlanning]] — 可微分运
动规划技术
- [[IntegratedPredictionAndPlanning]] — 预测与
规划集成范式
- [[ZhiyuHuang]] — 第一作者
- [[ChenLv]] — 通讯作者