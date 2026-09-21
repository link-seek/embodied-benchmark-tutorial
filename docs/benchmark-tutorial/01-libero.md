# 01 LIBERO：终身机器人学习的知识迁移榜单

> 模板来源：`.opencode/skills/benchmark-survey/`（七段结构复用 `templates/chapter.md`，引用格式复用 `templates/citation.md`）。表述口径与 `00-overview.md` 一致。

## 版本头

- 论文：Liu et al., NeurIPS 2023 Datasets and Benchmarks Track，arXiv:2306.03310，<https://arxiv.org/abs/2306.03310>（版本细节以 arXiv 页面为准）
- 本地仓库：`benchmarks/LIBERO@8f1084e`
- 整理日期：2026-09-20
- 口径说明：定义以论文为准，数字以仓库 commit 为准

## 定位

- LIBERO 是面向终身机器人学习（LLDM）的仿真榜单，核心是度量知识迁移与灾难性遗忘，回答“学新任务时旧知识保住多少、新任务学得多快”这类问题，不测真机迁移排名（来源：论文第 1 节引言定位句与摘要）。
- 研究动机是区分陈述性知识（物体、空间关系）与程序性知识（动作、行为）的迁移，五个研究主题为知识类型迁移、策略架构设计、终身学习算法设计、任务排序鲁棒性、预训练效应（来源：论文第 3 节 T1–T5）。
- 一句话定位表述参考教程站 LIBERO 章节（<https://benchmark.linkseek.net.cn/libero.html>），事实口径以论文与仓库为准。

## 任务规模

- 130 个语言条件操作任务，分 4 组套件：LIBERO-SPATIAL、LIBERO-OBJECT、LIBERO-GOAL 各 10 个任务，LIBERO-100 含 100 个任务（来源：论文第 4.2 节与图 1；仓库 `benchmarks/LIBERO/README.md` 榜单介绍节）。
- 三个 10 任务套件做解耦对照：SPATIAL 只变空间关系、OBJECT 只变物体种类、GOAL 只变任务目标；LIBERO-100 测混合知识迁移（来源：论文第 4.2 节）。
- LIBERO-100 拆分为 LIBERO-90（预训练数据源）与 LIBERO-LONG（10 个长时程任务，下游终身学习评估用）（来源：论文第 4.2 节；仓库套件注册见 `benchmarks/LIBERO/libero/libero/benchmark/__init__.py`）。
- 每个任务配 50 条高质量人工遥操作演示（3D 动作控制器采集），支持行为克隆式终身模仿学习（来源：论文第 4.4 节）。
- 任务由 Ego4D 行为模板经 PDDL 生成管线产生，场景、初始分布与目标谓词（开、关、在上、在内等）全部程序化定义（来源：论文第 4.1 节与图 2；任务定义文件见仓库 `benchmarks/LIBERO/libero/libero/bddl_files/`）。

## 引擎与本体

- 仿真引擎为 robosuite 之上的 MuJoCo 物理，生成管线明确构建于 Robosuite 之上（来源：论文第 4.1 节；依赖版本见仓库 `benchmarks/LIBERO/requirements.txt` 中的 `robosuite==1.4.0`）。
- 操作本体为 robosuite 桌面操作臂（总览表记为 MuJoCo + robosuite / Franka Panda，细节以仓库 `benchmarks/LIBERO/libero/libero/envs/` 配置为准）。
- 奖励为稀疏目标谓词（完成得 1），论文把问题形式化为稀疏奖励有限时域 MDP（来源：论文第 2.1 节；仓库 `benchmarks/LIBERO/README.md` 任务节同样声明当前只支持稀疏奖励，主攻终身模仿学习）。
- 渲染相机含 `agentview` 与 `robot0_eye_in_hand` 双视角（来源：仓库 `benchmarks/LIBERO/libero/libero/envs/env_wrapper.py`）。

## 观测动作

- 观测模态：多视角 RGB 图像、语言指令、关节与夹爪本体感知，论文要求策略融合视觉、时序与语言三路信息（来源：论文第 3 节 T2 与第 4.4 节；语言指令解析见仓库 `benchmarks/LIBERO/libero/libero/envs/bddl_utils.py`）。
- 三种官方策略架构：RESNET-RNN、RESNET-T、VIT-T，语言用预训练 BERT 嵌入编码（FiLM 融合或独立词元），输出头为高斯混合模型（来源：论文第 4.4 节）。
- 动作空间为连续末端执行器动作，采样自 GMM 输出分布后执行（来源：论文第 4.4 节；仓库 README 任务示例以 7 维动作步进环境）。

## 指标与基线

- 主指标全部按稀疏成功率计算：前向迁移 FWT（新任务学得多快越高越好）、负向遗忘 NBT（旧任务掉多少越低越好）、成功率曲线面积 AUC（兼顾两者），定义见论文第 5.1 节与图 3，实现见仓库 `benchmarks/LIBERO/libero/lifelong/metric.py`。
- 配套分析含混淆矩阵式的跨任务遗忘对照与注意力显著图（来源：论文附录 E.4；对照口径与总览表“稀疏成功率 + 混淆矩阵 + 前向迁移”一致）。
- 基线算法：经验回放 ER、弹性权重巩固 EWC、PackNet，以及顺序微调 SEQL（下界）与多任务 MTL（上界）（来源：论文第 4.3 节）。
- 代表性基线分数（2023-06，出处：论文 Table 2，策略固定为 RESNET-T，三种子平均）：LIBERO-SPATIAL 上 SEQL 的 FWT 为 0.72±0.01，ER 为 0.65±0.03，PackNet 为 0.55±0.01；LIBERO-LONG 上 ER 的 AUC 为 0.32±0.01，显著高于 SEQL 的 0.15±0.00。
- 核心结论：顺序微调的前向迁移反而最好，说明所测终身学习算法均在一定程度上损害前向迁移；PackNet 防遗忘最强但在 LIBERO-LONG 上前向迁移不足；朴素监督预训练可能损害下游终身学习（来源：论文第 5.2 节各研究问答与图 5）。
- 未公开单一榜首分数（2026-09-20 整理，出处：论文 Table 1 / Table 2 按套件与算法组合报告，无统一总榜，不转述二手排名）。

## 复现路径

- 复现入口以仓库文档为准，不写部署步骤：
  - `benchmarks/LIBERO/README.md`（任务、训练、评估三节与数据集说明）
  - `benchmarks/LIBERO/libero/libero/benchmark/__init__.py`（套件注册与任务—语言映射）
  - `benchmarks/LIBERO/libero/lifelong/main.py`（训练入口，BENCHMARK / POLICY / ALGO 三选，配置见 `libero/configs/`）
  - `benchmarks/LIBERO/libero/lifelong/evaluate.py`（独立评估入口，训练时默认随训评估）
  - `benchmarks/LIBERO/libero/lifelong/metric.py`（FWT / NBT / AUC 实现）
  - `benchmarks/LIBERO/benchmark_scripts/download_libero_datasets.py`（演示数据获取，另有 HuggingFace 镜像，见 README 数据集节）
  - `benchmarks/LIBERO/notebooks/`（上手示例）与官方文档 <https://lifelong-robot-learning.github.io/LIBERO/>

## 局限与对我们的意义

- 局限（来源：论文第 7 节结论与局限段、第 5.2 节）：纯仿真无真机验证；语言嵌入换用 BERT / CLIP / GPT-2 乃至任务编号嵌入均无显著差异，语义信息未被有效利用；算法表现随任务排序明显波动；朴素离线监督预训练可能损害下游终身学习；稀疏奖励下主要覆盖模仿学习，强化学习路径未深挖。
- 对我们的意义：适合终身与持续学习研究、遗忘矩阵与前向迁移度量、策略架构对照；不适合真机迁移排名（该需求看 03 SimplerEnv）、不适合大规模真机数据训练（该需求看总览表 04 与 08 分章）。
