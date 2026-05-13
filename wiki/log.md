## [2026-05-12] scholar-skill | L3 精读 Driving with Style 论文 (IROS 2019, arXiv:1905.00229)

- **变更**: 更新 [[syntheses/2019-Rosbach-Driving-With-Style-IRL-General-Purpose-Planning]]  frontmatter（修正 sources 路径为 archive、更新 last_updated、规范 venue/paper_title 字段）
- **升级**: 更新 [[GeneralPurposePlanner]]（修正 sources 路径、更新 last_updated）
- **升级**: 更新 [[MaximumEntropyIRL]]（修正 sources 路径、更新 last_updated）
- **验证**: 基于完整 PDF（8 页）重新核对 L3 笔记内容，确认知识准确性和完整性
- **更新**: 更新 [[log.md]]
- **冲突**: 无
- **备注**: 优先级 P1，该 L3 笔记此前已于 2026-05-09 创建（~13KB），本次为 frontmatter 规范化和内容验证

## [2026-05-12] scholar-skill | L3 精读 Vehicles 2026 论文 (MDPI)

- **变更**: 新增 [[syntheses/2026-Sun-Trajectory-Data-Driven-Personalized-Autonomous-Driving-Decision-System-for-Driving-Simulators]]（L3 精读笔记，~13KB）
- **升级**: 更新 [[PersonalizedAutonomousDriving]]（补充三车博弈模型、代价函数公式、IRL 权重表、DuDQN 训练细节、方法对比表、复杂场景测试、局限分析、核心方法论洞察）
- **升级**: 更新 [[DuelingDQN]]（补充训练细节、epsilon-greedy、目标网络更新、F1 对比表、博弈安全约束关键作用）
- **更新**: 更新 [[摘要-trajectory-data-driven-personalized-autonomous-driving-decision-system]]、[[index.md]]
- **记忆抽取**: 9 条（5 Semantic + 2 Episodic + 2 Procedural）
- **冲突**: 无
- **备注**: 优先级 P1，L3 精读标准，PDF 通过 PyPDF2 提取文本，24 页，武汉理工大学 + 河北高速集团，无人机航拍+博弈论+MaxEnt IRL+DuDQN

## [2026-05-12] scholar-skill | L3 精读 TreeIRL 论文 (arXiv:2509.13579)

- **变更**: 新增 [[syntheses/2025-TreeIRL-Safe-Urban-Driving-with-Tree-Search-and-Inverse-Reinforcement-Learning]]（L3 精读笔记，~14KB）
- **升级**: 更新 [[TreeIRL]]（补充完整 MDP 形式化、11 项奖励函数、延迟优化工程、部署策略、实验结果、局限分析）
- **升级**: 更新 [[MonteCarloTreeSearch]]（补充轨迹生成器重新定位、延迟优化范式转变）
- **升级**: 更新 [[SimToRealGap]]（补充三层评估漏斗、量化对比数据、深层原因分析）
- **升级**: 更新 [[摘要-treeirl-safe-urban-driving]]（补充 L3 笔记链接）
- **记忆抽取**: 10 条（6 Semantic + 2 Episodic + 2 Procedural）
- **冲突**: 无
- **备注**: 优先级 P0，L3 精读标准，PDF 通过 PyPDF2 提取文本，18 页，Motional 团队，首次 MCTS 真实道路验证，sim-to-real gap 1-2 个数量级

## [2026-05-12] sync | 配置 wiki/sources/ 按 last_updated 属性降序排序
- **变更**: 新增 [[wiki/sources/sortspec.md]]（Custom File Explorer Sorting 配置）
- **备注**: 按笔记 frontmatter `last_updated` 字段从新到旧排列，使用 `> a-z by-metadata: last_updated`

## [2026-05-12] ingest | 编译 Vehicles 2026, 8, 94 (个性化自动驾驶决策系统)

- **变更**: 新增 [[摘要-trajectory-data-driven-personalized-autonomous-driving-decision-system]]、[[PersonalizedAutonomousDriving]]、[[DuelingDQN]]、[[WenpengSun]]、[[YuZhang]]、[[NengchaoLyu]]; 更新 [[index.md]]
- **冲突**: 无
- **备注**: PDF 通过 PyPDF2 提取文本，24 页，武汉理工大学 + 河北高速集团，基于无人机航拍轨迹数据，融合 MaxEnt IRL 与 DuDQN，在高保真驾驶模拟器中闭环验证

## [2026-05-12] ingest | 编译 arXiv:2206.12348 (MPC-based Imitation Learning)

- **变更**: 新增 [[摘要-mpc-based-imitation-learning]]、[[MPCbasedImitationLearning]]、[[BehavioralCloningFromObservations]]、[[FlaviaSofiaAcerbo]]、[[JanSwevers]]、[[TinneTuytelaars]]、[[TongDuySon]]; 更新 [[index.md]]
- **冲突**: 无
- **备注**: arXiv API 摄入，无本地文件需归档，SL4AD workshop @ ICML 2022，将 MPC 作为可微分控制层嵌入模仿学习

## [2026-05-12] ingest | 编译 arXiv:2509.13579 (TreeIRL)

- **变更**: 新增 [[摘要-treeirl-safe-urban-driving]]、[[TreeIRL]]、[[MonteCarloTreeSearch]]、[[DriveIRL]]、[[SimToRealGap]]、[[MomchilSTomov]]、[[SangUkLee]]、[[HansfordHendargo]]、[[BoazFloor]]、[[YunqingHu]]、[[Motional]]; 更新 [[InverseReinforcementLearning]]、[[MaximumEntropyIRL]]、[[index.md]]
- **冲突**: 无
- **备注**: PDF 通过 PyPDF2 提取文本，18 页，Motional 团队提出，将 MCTS 与深度 IRL 结合，nuPlan + 真实城市道路验证

## [2026-05-09] ingest | 编译 arXiv:1905.00229 (Driving with Style)

- **变更**: 新增 [[摘要-driving-with-style-irl-general-purpose-planning]]、[[GeneralPurposePlanner]]、[[SaschaRosbach]]、[[VinitJames]]、[[SimonGrossjohann]]、[[SilviuHomoceanu]]、[[StefanRoth]]; 更新 [[InverseReinforcementLearning]]、[[MaximumEntropyIRL]]、[[index.md]]
- **冲突**: 无
- **备注**: arXiv URL 摄入，无本地文件需归档，IROS 2019，大众集团 + 达姆施塔特工业大学，8 页，将 MaxEnt IRL 与通用型规划器集成

## [2026-05-09] ingest | 编译 arXiv:2006.13704 (SMIRL)

- **变更**: 新增 [[摘要-efficient-sampling-based-maximum-entropy-irl]]、[[SMIRL]]、[[MaximumEntropyIRL]]、[[ZhengWu]]、[[LitingSun]]、[[WeiZhan]]、[[ChenyuYang]]、[[MasayoshiTomizuka]]; 更新 [[InverseReinforcementLearning]]、[[index.md]]
- **冲突**: 无
- **备注**: arXiv URL 摄入，无本地文件需归档，基于采样的最大熵逆强化学习，9页

# Wiki Behavior Log

行为流水线：以 Grep-friendly 格式记录 ingest/query 历史。

## Format
```
[YYYY-MM-DD HH:MM:SS] [ACTION] [DESCRIPTION]
```

Actions: `INGEST`, `QUERY`, `LINT`, `ARCHIVE`, `INIT`

---

## [2026-05-09] scholar-skill | L3 精读 Driving with Style 论文 (arXiv:1905.00229)

- **变更**: 新增 [[syntheses/2019-Rosbach-Driving-With-Style-IRL-General-Purpose-Planning]]（L3 精读笔记，~13KB）
- **升级**: 更新 [[GeneralPurposePlanner]]（补充完整技术架构: GPU 并行搜索算法、路径积分 IRL 公式、经验回放机制、投影度量三重作用、12 维特征设计、实验对比表格）
- **升级**: 更新 [[InverseReinforcementLearning]]（补充 L3 笔记链接、规划器内嵌 IRL 核心规则）
- **升级**: 更新 [[摘要-driving-with-style-irl-general-purpose-planning]]（补充 L3 笔记链接）
- **升级**: 更新 [[SaschaRosbach]]（补充 L3 笔记链接）
- **升级**: 更新 [[VinitJames]]（补充 L3 笔记链接）
- **升级**: 更新 [[SimonGrossjohann]]（补充 L3 笔记链接）
- **升级**: 更新 [[SilviuHomoceanu]]（补充 L3 笔记链接）
- **升级**: 更新 [[StefanRoth]]（补充 L3 笔记链接）
- **升级**: 更新 [[index.md]]（新增 L3 笔记索引）
- **记忆抽取**: 9 条（5 Semantic + 2 Episodic + 2 Procedural）
- **冲突**: 无
- **备注**: 优先级 P1，L3 精读标准，arXiv PDF 通过 requests 下载后 PyPDF2 提取文本，8 页

## [2026-05-09] scholar-skill | L3 精读 SMIRL 论文 (arXiv:2006.13704)

- **变更**: 新增 [[syntheses/2020-Efficient-Sampling-Based-Maximum-Entropy-IRL]]（L3 精读笔记，~12KB）
- **升级**: 更新 [[SMIRL]]（补充完整技术架构: Boltzmann 模型公式、配分函数近似、一次性采样设计、样本重分布关键发现、实验对比表格）
- **升级**: 更新 [[MaximumEntropyIRL]]（补充 CIOC/Opt-IRL/SMIRL 三种连续域扩展策略对比）
- **升级**: 更新 [[InverseReinforcementLearning]]（补充 L3 笔记链接、SMIRL 核心规则）
- **升级**: 更新 [[摘要-efficient-sampling-based-maximum-entropy-irl]]（补充 L3 笔记链接）
- **升级**: 更新 [[ZhengWu]]（补充 L3 笔记链接）
- **升级**: 更新 [[LitingSun]]（补充 L3 笔记链接）
- **升级**: 更新 [[WeiZhan]]（补充 L3 笔记链接）
- **升级**: 更新 [[MasayoshiTomizuka]]（补充 L3 笔记链接）
- **升级**: 更新 [[index.md]]（新增 L3 笔记索引）
- **记忆抽取**: 9 条（5 Semantic + 2 Episodic + 2 Procedural）
- **冲突**: 无
- **备注**: 优先级 P1，L3 精读标准，arXiv PDF 通过 requests 下载后 PyPDF2 提取文本，9 页

## [2026-05-09] scholar-skill | L3 精读 IRL-DAL 论文 (arXiv:2601.23266)

- **变更**: 新增 [[syntheses/2026-IRL-DAL]]（L3 精读笔记，~12KB）
- **升级**: 更新 [[IRLDAL]]（补充完整技术架构: LAM 公式、五能量项、SAEC 机制、消融数据、超参数表）
- **升级**: 更新 [[EnergyGuidedDiffusion]]（补充具体能量项公式、风险自适应权重、动作混合逻辑）
- **升级**: 更新 [[DiffusionBasedPlanning]]（补充"扩散模型作安全盾"定位、条件向量、引导公式）
- **升级**: 更新 [[LearnableAdaptiveMask]]（补充完整公式链、BC 端到端训练、消融验证）
- **升级**: 更新 [[SeyedAhmadHosseiniMiangoleh]]（补充机构信息、研究兴趣）
- **升级**: 更新 [[摘要-irl-dal]]（补充 L3 笔记链接）
- **升级**: 更新 [[index.md]]（新增 L3 笔记索引）
- **记忆抽取**: 14 条（6 Semantic + 2 Episodic + 3 Procedural + 3 知识升级）
- **冲突**: 无
- **备注**: 优先级 P1，L3 精读标准，arXiv PDF 通过 requests 下载后 PyPDF2 提取文本，10 页

## [2026-05-09] ingest | 编译 arXiv:2601.23266 (IRL-DAL)

- **变更**: 新增 [[摘要-irl-dal]]、[[IRLDAL]]、[[EnergyGuidedDiffusion]]、[[DiffusionBasedPlanning]]、[[LearnableAdaptiveMask]]、[[SeyedAhmadHosseiniMiangoleh]]、[[AminJalalAghdasian]]、[[FarzanehAbdollahi]]; 更新 [[InverseReinforcementLearning]]、[[index.md]]
- **冲突**: 无
- **备注**: arXiv URL 摄入，无本地文件需归档，基于能量引导扩散模型的 IRL 安全自适应轨迹规划

## [2026-05-09] scholar-skill | L3 精读 DIPP 论文 (arXiv:2207.10422v2)

- **变更**: 新增 [[syntheses/2022-Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving]]（L3 精读笔记，~15KB）
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

- **变更**: 新增 [[syntheses/2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]]（L3 精读笔记，~12KB）
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

- **输出**: [[InverseReinforcementLearning]]、[[RC_IRL]]、[[syntheses/2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]]、[[摘要-auto-tuning-framework-autonomous-vehicles]]

## [2026-05-07] ingest | 编译 arXiv:2407.19902 (DDP-based IRL)

- **变更**: 新增 [[摘要-ddp-framework-inverse-reinforcement-learning]]、[[KunCao]]、[[WanxinJin]]、[[KarlHJohansson]]、[[LihuaXie]]、[[XinhangXu]]、[[DifferentialDynamicProgramming]]、[[PontryaginsMaximumPrinciple]]、[[InverseOptimalControl]]、[[ClosedLoopIRL]]; 更新 [[InverseReinforcementLearning]]、[[index.md]]
- **冲突**: 无
- **备注**: arXiv URL 摄入，无本地文件需归档

## [2026-05-07] scholar-skill | L3 精读 arXiv:2407.19902 (DDP-based IRL)

- **变更**: 新增 [[syntheses/2024-A Differential Dynamic Programming Framework for Inverse Reinforcement Learning]]（L3 精读笔记，~14KB）
- **升级**: 更新 [[ClosedLoopIRL]]（补充闭环损失函数、梯度推导、LM 更新、可恢复性条件、优劣势分析）
- **升级**: 更新 [[InverseReinforcementLearning]]（补充 DDP-based IRL 链接）
- **升级**: 更新 [[DifferentialDynamicProgramming]]（补充逆问题梯度计算、BarrierDDP、与 PDP 等价性）
- **升级**: 更新 [[InverseOptimalControl]]（补充约束可恢复性条件、线性参数化假设）
- **升级**: 更新 [[摘要-ddp-framework-inverse-reinforcement-learning]]（补充 L3 笔记链接）
- **升级**: 更新 [[index.md]]（新增 L3 笔记索引）
- **记忆抽取**: 11 条（5 Semantic + 3 Episodic + 3 Procedural）
- **冲突**: 无
- **备注**: 优先级 P0，L3 精读标准，arXiv PDF 通过 requests 下载后 PyPDF2 提取文本，20 页
