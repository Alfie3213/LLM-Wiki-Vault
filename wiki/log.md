# Wiki Behavior Log

行为流水线：以 Grep-friendly 格式记录 ingest/query 历史。

## Format
```
[YYYY-MM-DD HH:MM:SS] [ACTION] [DESCRIPTION]
```

Actions: `INGEST`, `QUERY`, `LINT`, `ARCHIVE`, `INIT`

---

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
