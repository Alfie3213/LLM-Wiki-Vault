---
title: "摘要-trajectory-data-driven-personalized-autonomous-driving-decision-system"
type: source
tags: [来源, 论文, 个性化自动驾驶, 无人机航拍, 逆强化学习, 驾驶模拟器, 驾驶风格]
sources: [raw/02-papers/vehicles-08-00094-v2.pdf]
last_updated: 2026-05-12
---

## 核心摘要

本文提出一种基于无人机航拍轨迹数据的个性化自动驾驶决策系统，用于高保真驾驶模拟器中的算法验证。系统利用真实无人机航拍数据构建自然交通流，解决传统模拟平台中背景车辆行为缺乏类人特性和多样性的问题。

研究团队基于 4997 条真实车辆轨迹建立相对特征指标体系，使用改进的 K-means++ 方法对驾驶风格进行聚类，得到激进型、普通型和保守型三种驾驶风格的先验特征。在此基础上，提出融合动态博弈论与最大熵逆强化学习（MaxEnt IRL）的个性化决策框架，从无人机航拍示范数据中学习安全、舒适、效率之间的偏好权重，并结合 DuDQN（Dueling DQN）生成类人换道策略。

在规划与控制层面，系统采用五次多项式轨迹规划、横向 LQR 控制和纵向级联 PID 控制，在高保真驾驶模拟平台上实现策略的实时闭环执行。实验结果表明，生成的换道策略与真实专家决策的吻合度超过 85%（激进型 90%、普通型 85%、保守型 87%），横向跟踪误差低于 0.1 米，纵向速度跟踪误差稳定在 ±0.3 m/s 以内。

## 关联连接

- [[PersonalizedAutonomousDriving]] — 本文提出的个性化自动驾驶决策系统
- [[syntheses/2026-Sun-Trajectory-Data-Driven-Personalized-Autonomous-Driving-Decision-System-for-Driving-Simulators]] — L3 精读笔记
- [[DuelingDQN]] — 本文使用的深度强化学习算法
- [[MaximumEntropyIRL]] — 本文用于学习偏好权重的理论基础
- [[WenpengSun]] — 论文第一作者
- [[YuZhang]] — 论文作者
- [[NengchaoLyu]] — 论文通讯作者
