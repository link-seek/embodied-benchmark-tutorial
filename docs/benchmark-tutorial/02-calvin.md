# 02 CALVIN：语言条件长时序操作榜单

> 模板来源：`.opencode/skills/benchmark-survey/`（七段结构复用 `templates/chapter.md`，引用格式复用 `templates/citation.md`）。表述口径与 `00-overview.md` 一致。

## 版本头

- 论文：Mees et al., IEEE RA-L 2022，arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实），<https://arxiv.org/abs/2112.03227>（版本细节以 arXiv 页面与期刊卷期为准）
- 本地仓库：`benchmarks/calvin@fa03f01`
- 整理日期：2026-09-20
- 口径说明：定义以论文为准，数字以仓库 commit 为准；凡引 arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）均已标记

## 定位

- CALVIN 是语言条件长时序桌面操作榜单，测智能体按连续自然语言指令完成 5 步链式任务的能力，以及跨 environments A / B / C / D 的泛化，不测大规模真机排名（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 1 节与第 3 节）。
- 核心设定是单策略、多任务、纯语言指定：评测时只给语言指令，不给目标图像，机器人每次从中性位姿出发（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.3 节评测协议；仓库 `benchmarks/calvin/README.md` 挑战节）。
- 一句话定位表述参考教程站总览与 CALVIN 对应章节，事实口径以论文与仓库为准（对照源见总览 `00-overview.md` 复现入口指引节）。

## 任务规模

- 34 类原子操作任务（开抽屉、推滑门、旋转 / 推动三色方块、按开关 / 按钮、堆叠与拆塔等），成功判据为初末状态变化（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）图 6 任务清单；仓库 `benchmarks/calvin/calvin_models/calvin_agent/evaluation/multistep_sequences.py` 的任务字典，旋转 6、推动 6、滑门 2、抽屉 2、抓取 9、放置 2、灯光 4、推入抽屉 1、堆叠 / 拆塔 2，共 34 项）。
- 长时序评测用 1000 条 5 步指令链，去环、去冗余、去相似并过滤不可行序列（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.3 节；链生成实现见仓库 `multistep_sequences.py`，评测常量见 `evaluate_policy.py` 的 `NUM_SEQUENCES = 1000`）。
- 数据为约 24 小时 VR 遥操作自由 play 数据（每环境约 6 小时），约 240 万交互步，可重标定出约 4000 万个短时程窗口；众包语言标注 400 余条模板化指令，仅约 1% 数据配语言（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.2 节）。
- 语言嵌入默认用 MiniLM（384 维），仓库同时提供重标注脚本以更换语言模型（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.2 节；仓库 `benchmarks/calvin/README.md` 重标注节）。
- 四个环境纹理与静态物件位置各不相同，支撑单环境、多环境与零样本（ABC 训、D 测）三档难度（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.1 节与第 3.3 节）。

## 引擎与本体

- 物理与渲染引擎为 PyBullet，支持图形处理器并行采集与 EGL 离屏渲染（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.1 节；仓库 `benchmarks/calvin/README.md` 问答节 EGL 说明）。
- 操作本体为 7 自由度 Franka Panda 并联夹爪操作台，配滑动门、抽屉、按钮、开关与三色方块（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.1 节）。
- 数据集划分（D / ABC / ABCD / debug）与下载方式见仓库 `benchmarks/calvin/dataset/`（下载脚本与校验说明见该目录 `README.md` 与 `download_data.sh`）。

## 观测动作

- 观测模态（来源：仓库 `benchmarks/calvin/README.md` 感官观测节；arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）图 2 与图 3）：
  - 静态相机 RGB（200×200×3）与深度（200×200）
  - 夹爪相机 RGB（84×84×3）与深度（84×84）
  - 视觉触觉图像（仓库记为 120×160×6 通道实现，论文图示记 120×160×2）
  - 本体感知：末端位置 3、末端欧拉角 3、夹爪宽度 1、关节角 7、夹爪动作 1
- 动作空间三选一，30Hz 闭环连续控制（来源同上）：绝对笛卡尔位姿（位置 3 + 欧拉角 3 + 夹爪 1）、相对笛卡尔位移（同维度）、关节空间（关节 7 + 夹爪 1）。
- 论文建议按任务粗细搭配观测与动作：静态相机配绝对动作适合跨台面大范围移动，夹爪相机配相对动作适合堆叠抓取等精细操作（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.1 节）。

## 指标与基线

- MTLC（单步多任务语言控制）：34 个任务各 10 次 rollout 的平均成功率，测试指令为训练未见过的新表述（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.3 节；仓库 `benchmarks/calvin/README.md` 挑战节）。
- LH-MTLC（长时序链式评估）：1000 条 5 步链，报告 SR_1 至 SR_5（至少连续完成 k 步的序列占比）与平均成功链长，任一步失败即终止该链（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 3.3 节；计数实现见仓库 `calvin_agent/evaluation/utils.py` 的 `count_success` 与链长统计）。
- 基线为多上下文模仿学习 MCIL（序列到序列条件变分自编码器 + 目标条件策略，1% 语言标注即可训练语言条件策略）（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 4 节）。
- MCIL 基线分数（2021-12，出处：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）图 8，静态相机 RGB 输入）：单环境 D→D 的 MTLC 为 53.9%；同设置 LH-MTLC 的 SR_1 为 48.9%、SR_2 为 12.9%、SR_3 为 2.6%、SR_4 为 0.5%、SR_5 为 0.08%；零样本 ABC→D 的 MTLC 为 38.6%。
- 仓库 README 另设 SOTA 榜单节，收录超越 MCIL 的开源模型并指向官方排行榜（来源：仓库 `benchmarks/calvin/README.md` 的 SOTA 与排行榜节，榜单页 <http://calvin.cs.uni-freiburg.de/>）；本篇不转述榜单页实时数字，分数以论文图 8 为准。

## 复现路径

- 复现入口以仓库文档为准，不写部署步骤：
  - `benchmarks/calvin/README.md`（快速开始、训练基线、感官与动作、挑战赛三节）
  - `benchmarks/calvin/calvin_models/calvin_agent/evaluation/evaluate_policy.py`（LH-MTLC 评测入口，含 `CustomModel` 与 `CustomLangEmbeddings` 扩展接口）
  - `benchmarks/calvin/calvin_models/calvin_agent/evaluation/multistep_sequences.py`（1000 条链生成与任务字典）
  - `benchmarks/calvin/calvin_models/calvin_agent/evaluation/evaluate_policy_singlestep.py`（MTLC 单步评测入口）
  - `benchmarks/calvin/dataset/`（数据划分、下载与语言嵌入说明）
  - `benchmarks/calvin/slurm_scripts/`（集群训练指引）与 `benchmarks/calvin/RL_with_CALVIN.ipynb`（稀疏奖励强化学习示例）
  - 论文 arXiv 页 <https://arxiv.org/abs/2112.03227>（版本头已列，此处复列为复现入口）与仓库首页 <https://github.com/mees/calvin>

## 局限与对我们的意义

- 局限（来源：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）第 5 节实验结论与第 6 节结论段；仓库 README 问答节）：MCIL 长时序能力弱（5 步链成功率仅 0.08%）；红蓝方块等视觉 grounding 易混淆；多环境与零样本设置下分数显著下滑；EGL 与中央处理器渲染纹理存在细微差异，预训练模型迁移需注意；物体为单色基元，复杂度低于真实家务。
- 对我们的意义：适合语言长时序分解、跨环境泛化与观测动作搭配对照；长链评测方法（SR_1 至 SR_5）可直接借用到我们自己的多步任务报告；不适合大规模真机排名与开放家务全集（该需求看 03 与 06 分章）。
