---
title: "摘要-mpc-based-imitation-learning"
type: source
tags: [来源, 论文, 模仿学习, 模型预测控制, 自动驾驶, 车道保持]
sources: [https://arxiv.org/abs/2206.12348]
last_updated: 2026-05-12
---

## 核心摘要

本文提出一种将模型预测控制（MPC）与模仿学习（IL）无缝结合的自动驾驶控制器学习方法。核心思想是将 MPC 作为可微分控制层嵌入到分层模仿学习策略中，通过 MPC 的代价函数、模型或约束参数进行端到端的闭环模仿学习。

传统模仿学习难以保证闭环系统轨迹的安全性，而 MPC 虽然能处理带安全约束的非线性系统，但实现类人驾驶需要大量领域知识。本文方法通过 MPC 的可微分特性，使模仿学习能够直接优化 MPC 内部参数，从而在保留 MPC 安全保证的同时学习人类驾驶行为。

实验针对车道保持控制系统，使用固定基座驾驶模拟器上的人类示范数据，通过从观察中进行行为克隆（BCO）学习控制器。该方法在 SL4AD 研讨会（ICML 2022 协辦）上被接受。

## 关联连接

- [[MPCbasedImitationLearning]] — 本文提出的核心方法
- [[BehavioralCloningFromObservations]] — 本文使用的模仿学习技术
- [[FlaviaSofiaAcerbo]] — 论文第一作者
- [[JanSwevers]] — 论文作者
- [[TinneTuytelaars]] — 论文作者
- [[TongDuySon]] — 论文作者
