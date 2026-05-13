---
title: "摘要-coohoi"
type: source
tags: [来源, 论文, 机器人学, 人机交互, 多智能体]
sources: [raw/09-archive/CooHOI - Learning Cooperative Human-Object Interaction with Manipulated Object Dynamics.pdf]
last_updated: 2026-05-12
---

## 核心摘要

CooHOI（Cooperative Human-Object Interaction）是 NeurIPS 2024 Spotlight 论文，提出了一种让多个人形机器人协作搬运物体的框架。核心创新在于两阶段学习范式：先通过模仿学习让单个机器人学会与物体交互，再通过 CTDE（Centralized Training Decentralized Execution）多智能体强化学习实现协作策略迁移。该方法无需多机器人协作的运动捕捉数据，具有内在高效性，可无缝扩展至更多参与者和更广泛的物体类型。

## 关键信息

- **论文标题**: CooHOI: Learning Cooperative Human-Object Interaction with Manipulated Object Dynamics
- **作者**: Jiawei Gao, Ziqin Wang, Zeqi Xiao, Jingbo Wang, Tai Wang, Jinkun Cao, Xiaolin Hu, Si Liu, Jifeng Dai, Jiangmiao Pang
- **arXiv**: 2406.14558
- **会议**: NeurIPS 2024 Spotlight
- **核心问题**: 多个人形机器人如何协作完成大型/重型物体搬运任务
- **方法**: 单智能体技能学习（对抗运动先验 AMP + 模仿学习）→ 多智能体策略迁移（CTDE）
- **关键洞察**: 通过共享被操作物体的动力学状态实现隐式通信与协调

## 关联连接

- [[CooHOI]] — 本文提出的协作人机交互框架
- [[CTDE]] — 集中训练分散执行的多智能体强化学习范式
- [[JiaweiGao]] — 第一作者
- [[HumanObjectInteraction]] — 人机交互研究领域
