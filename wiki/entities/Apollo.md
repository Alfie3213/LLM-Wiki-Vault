---
title: "Apollo"
type: entity
tags: [自动驾驶, 开源平台, 百度, L4级自动驾驶]
sources: [raw/09-archive/An Auto-tuning Framework for Autonomous Vehicles.pdf]
last_updated: 2026-05-12
---

## 定义

Apollo（阿波罗）是百度开源的自动驾驶平台，提供从感知、预测、规划到控制的完整 L4 级自动驾驶软件栈。

## 关键信息

- **开发方**：百度（Baidu）
- **开源地址**：https://github.com/ApolloAuto/apollo
- **在本论文中的角色**：作为自动调参框架的基座平台，运动规划器（motion planner）的奖励/代价函数通过本文提出的 RC-IRL 算法进行自动调参
- **路测记录**：截至 2018 年 7 月，基于该平台调参后的运动规划器已完成 25000+ 英里的公开道路测试
- **核心模块**：
  - 在线模块：实时轨迹优化，生成最优轨迹
  - 离线模块：奖励/代价函数参数调参，通过 RC-IRL 从人类驾驶数据中学习
  - 共享模块：原始特征生成器、轨迹采样器
- **运动规划器**：采用 EM Planner（同一作者 2018），通过优化奖励/代价函数生成轨迹

## 关联连接

- [[../syntheses/2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]] — 基于 Apollo 平台提出的自动调参框架（L3 精读笔记）
- [[摘要-auto-tuning-framework-autonomous-vehicles]] — 论文核心摘要
- [[RC_IRL]] — 用于调参 Apollo 运动规划器的核心算法
- [[HaoyangFan]] — 论文第一作者，Apollo 团队核心成员
