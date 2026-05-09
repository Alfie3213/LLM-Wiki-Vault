# Wiki Index

全局内容字典：记录所有 wiki 页面及其一句话索引。

---

## 🏗️ Concepts (抽象层)

- [[DIPP]] — 可微分集成预测与规划框架，通过端到端可微分架构联合优化自动驾驶中的轨迹预测与运动规划
- [[DifferentiableMotionPlanning]] — 将经典运动规划优化器以可微分形式嵌入神经网络的技术范式
- [[IntegratedPredictionAndPlanning]] — 打破预测与规划模块壁垒、实现联合优化的自动驾驶系统架构范式
- [[CooHOI]] — 多智能体协作人机交互框架，通过物体动力学实现隐式通信
- [[CTDE]] — 集中训练分散执行的多智能体强化学习范式
- [[InverseReinforcementLearning]] — 从专家示范中恢复奖励函数的逆强化学习范式
- [[RC_IRL]] — 基于排序的条件逆强化学习算法，用于自动驾驶运动规划器自动调参
- [[DifferentialDynamicProgramming]] — 用于轨迹优化的迭代数值方法，通过二阶近似求解非线性最优控制问题
- [[PontryaginsMaximumPrinciple]] — 最优控制理论核心定理，给出最优控制的一阶必要条件
- [[InverseOptimalControl]] — 从专家示范中恢复最优控制代价函数的问题
- [[IRLDAL]] — 结合扩散模型与 IRL 的自动驾驶安全自适应轨迹规划框架，通过能量引导扩散模型实现路径级安全保证
- [[EnergyGuidedDiffusion]] — 将能量函数引入扩散模型采样过程的约束生成技术
- [[DiffusionBasedPlanning]] — 利用扩散生成模型进行轨迹或路径规划的范式
- [[LearnableAdaptiveMask]] — 根据车速和周围危险动态调整视觉注意力的自适应感知机制
- [[ClosedLoopIRL]] — 使用闭环损失函数捕捉专家示范闭环特性的逆强化学习框架
- [[SMIRL]] — 基于采样的最大熵逆强化学习算法，在连续域中高效学习可解释的人类驾驶奖励函数
- [[MaximumEntropyIRL]] — 通过最大熵原理解决奖励函数歧义性的逆强化学习基础方法

## 👥 Entities (实体层)

- [[ZhiyuHuang]] — DIPP 论文第一作者，南洋理工大学自动驾驶与运动规划研究者
- [[HaochenLiu]] — DIPP 共同作者，南洋理工大学自动驾驶研究者
- [[JingdaWu]] — DIPP 共同作者，南洋理工大学机器人学研究者
- [[ChenLv]] — 南洋理工大学教授，IEEE Senior Member，DIPP 通讯作者
- [[Apollo]] — 百度开源的 L4 级自动驾驶平台
- [[HaoyangFan]] — 自动驾驶与机器学习研究者，RC-IRL 自动调参框架论文第一作者
- [[JiaweiGao]] — CooHOI 论文第一作者，机器人学与强化学习研究者
- [[KunCao]] — 机器人学与控制理论研究者，DDP-based IRL 框架论文第一作者
- [[WanxinJin]] — 机器人学与控制理论研究学者
- [[KarlHJohansson]] — KTH 教授，控制理论与网络系统领域著名学者
- [[LihuaXie]] — NTU 教授，IEEE Fellow，控制理论与传感器网络领域学者
- [[SeyedAhmadHosseiniMiangoleh]] — IRL-DAL论文第一作者，研究逆强化学习与扩散模型在自动
驾驶中的应用
- [[AminJalalAghdasian]] — IRL-DAL 共同作者
- [[FarzanehAbdollahi]] — IRL-DAL 共同作者
- [[XinhangXu]] — 机器人学与控制理论研究学者
- [[ZhengWu]] — SMIRL 论文共同第一作者，加州大学伯克利分校机器人学研究者
- [[LitingSun]] — SMIRL 论文共同第一作者兼通讯作者，伯克利自动驾驶与逆强化学习研究者
- [[WeiZhan]] — 伯克利研究者，INTERACTION 数据集核心贡献者
- [[ChenyuYang]] — 上海交通大学研究者，访问伯克利期间参与 SMIRL 研究
- [[MasayoshiTomizuka]] — 加州大学伯克利分校教授，机器人学与控制理论领域著名学者

## 🔍 Sources (来源层)

- [[摘要-differentiable-integrated-motion-prediction-and-planning]] — Differentiable Integrated Motion Prediction and Planning: 可微分
集成预测与规划框架，使代价函数可从数据中自动
学习 (arXiv:2207.10422v2)
- [[sources/2022-Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving]] — Differentiable Integrated Motion Prediction and Planning: 可微分集成预测与规划框架，Transformer 预测 + Gauss-Newton 可微分优化器 + 可学习代价函数（L3 精读笔记）
- [[2018-Fan-Auto-tuning-Framework-Autonomous-Vehicles]] — An Auto-tuning Framework for Autonomous Vehicles: 基于 Apollo 平台和 RC-IRL 算
法的自动驾驶运动规划器自动调参框架（L3 精读笔
记）
- [[摘要-auto-tuning-framework-autonomous-vehicles]] — 上述论文的核心摘要
- [[摘要-ddp-framework-inverse-reinforcement-learning]] — A Differential Dynamic Programming Framework for Inverse Reinforcement Learning: 基于 DDP 的 IRL 框架，提出闭环 IRL 与逆最优控制理论联系
- [[sources/2026-IRL-DAL]] — IRL-DAL: Safe and Adaptive Trajectory Planning for Autonomous Driving via Energy-Guided Diffusion Models:
 基于能量引导扩散模型的 IRL 安全自适应轨迹规划框架，SAEC 解耦安全盾 + LAM 极简注意力 +
 能量引导扩散（L3 精读笔记）
- [[摘要-irl-dal]] — IRL-DAL: Safe and Adaptive Trajectory Planning for Autonomous Driving via Energy-Guided Diffusion Models: 基于能量引导扩散模型的 IRL 安全自适应轨迹规划框架 (a
rXiv:2601.23266)
- [[摘要-coohoi]] — CooHOI: Learning Cooperative Human-Object Interaction with Manipulated Object Dynamics (NeurIPS 2024 Spotlight)
- [[摘要-efficient-sampling-based-maximum-entropy-irl]] — Efficient Sampling-Based Maximum Entropy IRL: 基于采样的最大熵逆强化学习算法，在连续域中从人类驾驶数据学习奖励函数 (arXiv:2006.13704)

## 💎 Syntheses (综合层)

*No synthesis pages yet.*

---

> ℹ️ This index is automatically updated by the
  and  skills.
> Last updated: 2026-05-09
