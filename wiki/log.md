# Wiki Behavior Log

行为流水线：以 Grep-friendly 格式记录 ingest/query 历史。

## Format
```
[YYYY-MM-DD HH:MM:SS] [ACTION] [DESCRIPTION]
```

Actions: `INGEST`, `QUERY`, `LINT`, `ARCHIVE`, `INIT`

---

## [2026-05-09] scholar-skill | L3 精读 DIPP 论文 (arXiv:2207.10422v2)

- **变更**: 新增 [[2022-Huang-DIPP]]（L3 精读笔记，~15KB）
- **升级**: 更新 [[DIPP]]（补充完整技术架构: LSTM 编码器、Transformer 交互、Gauss-Newton 优化器、4 项损失函数、消融实验结论）
- **升级**: 更新 [[DifferentiableMotionPlanning]]（补充 Gauss-Newton 实现细节: 自行车模型、Theseus 库、Frenet 安全距离）
- **升级**: 更新 [[IntegratedPredictionAndPlanning]]（补充三种范式对比、联合训练证据、开环/闭环性能差异）
- **升级**: 更新 [[ZhiyuHuang]]（补充研究脉络: DIPP -> DTPP -> GameFormer）
- **升级**: 更新 [[摘要-differentiable-integrated-motion-prediction-and-planning]]（补充 L3 笔记链接）
- **升级**: 更新 [[index.md]]（新增 L3 笔记索引）
- **记忆抽取**: 11 条（5 Semantic + 2 Episodic + 3 Procedural + 1 知识升级）
- **冲突**: 无
- **备注**: 优先级 P1，L3 精读标准，PDF 通过 PyPDF2 提取文本，15 页

## [2026-05-09] ingest | 编译 DIPP 论文 (arXiv:2207.10422v2)

- **变更**: 新增 [[摘要-differentiable-integrated-motion-prediction-and-planning]]、[[DIPP]]、[[DifferentiableMotionPlanning]]、[[IntegratedPredictionAndPlanning]]、[[ZhiyuHuang]]、[[HaochenLiu]]、[[JingdaWu]]、[[ChenLv]]; 更新 [[index.md]]
- **冲突**: 无
- **备注**: PDF 通过 PyPDF2 提取文本，15 页，南洋理工大学团队提出的可微分集成预测与规划框架

## [2026-05-07] ingest | 编译 Auto-tuning Framework for Autonomous Vehicles 论文

- **变更**: 新增 [[摘要-auto-tuning-framework-autonomous-vehicles]]、[[Apollo]]、[[HaoyangFan]]、[[RC_IRL]]、[[InverseReinforcementLearning]]; 更新 [[index.md]]
- **冲突**: 无
- **备注**: PDF 通过 PyPDF2 提取文本，7 页，基于 Apollo 平台的 RC-IRL 自动调参框架

## [2026-05-06] ingest | 编译 CooHOI 论文

- **变更**: 新增 [[摘要-coohoi]]、[[CooHOI]]、[[CTDE]]、[[JiaweiGao]]; 更新 [[index.md]]
- **冲突**: 无
- **备注**: PDF 无法直接提取文本，通过 arXiv 元数据补全信息

## [2026-05-07] scholar-skill | L3 精读 Auto-tuning Framework for Autonomous Vehicles

- **变更**: 新增 [[2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]]（L3 精读笔记，~12KB）
- **升级**: 更新 [[RC_IRL]]（补充 Siamese 架构、21 维特征、背景偏移问题、实验细节）
- **升级**: 更新 [[InverseReinforcementLearning]]（补充自动驾驶场景挑战、RC-IRL 创新）
- **升级**: 更新 [[Apollo]]（补充自动调参框架模块、EM Planner）
- **升级**: 更新 [[HaoyangFan]]（补充机构信息、代表工作）
- **升级**: 更新 [[摘要-auto-tuning-framework-autonomous-vehicles]]（补充 L3 笔记链接）
- **升级**: 更新 [[index.md]]（新增 L3 笔记索引）
- **记忆抽取**: 10 条（5 Semantic + 2 Episodic + 3 Procedural）
- **冲突**: 无
- **备注**: 优先级 P1，L3 精读标准，PDF 通过 PyPDF2 提取文本

## [2026-05-07] query | 逆强化学习

- **输出**: [[InverseReinforcementLearning]]、[[RC_IRL]]、[[2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]]、[[摘要-auto-tuning-framework-autonomous-vehicles]]

## [2026-05-07] ingest | 编译 arXiv:2407.19902 (DDP-based IRL)

- **变更**: 新增 [[摘要-ddp-framework-inverse-reinforcement-learning]]、[[KunCao]]、[[WanxinJin]]、[[KarlHJohansson]]、[[LihuaXie]]、[[XinhangXu]]、[[DifferentialDynamicProgramming]]、[[PontryaginsMaximumPrinciple]]、[[InverseOptimalControl]]、[[ClosedLoopIRL]]; 更新 [[InverseReinforcementLearning]]、[[index.md]]
- **冲突**: 无
- **备注**: arXiv URL 摄入，无本地文件需归档

## [2026-05-07] scholar-skill | L3 精读 arXiv:2407.19902 (DDP-based IRL)

- **变更**: 新增 [[2024-Cao-DDP-Framework-IRL]]（L3 精读笔记，~14KB）
- **升级**: 更新 [[ClosedLoopIRL]]（补充闭环损失函数、梯度推导、LM 更新、可恢复性条件、优劣势分析）
- **升级**: 更新 [[InverseReinforcementLearning]]（补充 DDP-based IRL 链接）
- **升级**: 更新 [[DifferentialDynamicProgramming]]（补充逆问题梯度计算、BarrierDDP、与 PDP 等价性）
- **升级**: 更新 [[InverseOptimalControl]]（补充约束可恢复性条件、线性参数化假设）
- **升级**: 更新 [[摘要-ddp-framework-inverse-reinforcement-learning]]（补充 L3 笔记链接）
- **升级**: 更新 [[index.md]]（新增 L3 笔记索引）
- **记忆抽取**: 11 条（5 Semantic + 3 Episodic + 3 Procedural）
- **冲突**: 无
- **备注**: 优先级 P0，L3 精读标准，arXiv PDF 通过 requests 下载后 PyPDF2 提取文本，20 页
