# 00 总览：9 个榜单横向对照与选型

> 模板来源：`.opencode/skills/benchmark-survey/`（表头复用 `templates/overview-table.md`，引用格式复用 `templates/citation.md`）。维度不增删：一行一榜单，共 9 行。

## 版本头

- 整理日期：2026-09-20
- 论文版本范围（本地 `benchmarks/papers/` 共 12 篇 md，定义以论文为准）：
  - Liu et al., arXiv 预印本 2023，arXiv:2306.03310，<https://arxiv.org/abs/2306.03310>（LIBERO）
  - Mees et al., IEEE RA-L 2022，arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实），<https://arxiv.org/abs/2112.03227>（CALVIN）
  - Li et al., arXiv 预印本 2024，arXiv:2405.05941，<https://arxiv.org/abs/2405.05941>（SimplerEnv）
  - Nasiriany et al., RSS 2024，arXiv:2406.02523 ⚠️待点开确认（RoboCasa 原版，原因：与仓库引用段对应关系未核实），<https://arxiv.org/abs/2406.02523>（RoboCasa 原版）
  - Nasiriany et al., ICLR 2026，arXiv:2603.04356（2026-03-04，标题作者与官网 PDF 一致，已核实采用），官网固定版本 <https://robocasa.ai/assets/robocasa365_iclr26.pdf>（RoboCasa365）
  - Gu et al., ICLR 2023，arXiv:2302.04659 ⚠️待点开确认（ManiSkill2，原因：版本号未核实），<https://arxiv.org/abs/2302.04659>（ManiSkill2）
  - Tao et al., RSS 2025，arXiv:2410.00425，<https://arxiv.org/abs/2410.00425>（ManiSkill3）
  - Li et al., arXiv 预印本 2024，arXiv:2403.09227，<https://arxiv.org/abs/2403.09227>（BEHAVIOR-1K）
  - Mu et al., CVPR 2025 Highlight，arXiv:2504.13059，<https://arxiv.org/abs/2504.13059>（RoboTwin 1.0，对照用）
  - Chen et al., ICML 2026（仓库标注），arXiv:2506.18088，<https://arxiv.org/abs/2506.18088>（RoboTwin 2.0）
  - Chen et al., arXiv 预印本 2026，arXiv:2607.04434，<https://arxiv.org/abs/2607.04434>（RoboDojo）
  - Choi et al., arXiv 预印本 2026，arXiv:2603.13966，<https://arxiv.org/abs/2603.13966>（VLA-Harness / vla-eval）
- 本地仓库 commit：见各分章（`benchmarks/LIBERO`、`benchmarks/calvin`、`benchmarks/SimplerEnv`、`benchmarks/robocasa`、`benchmarks/ManiSkill`、`benchmarks/BEHAVIOR-1K`、`benchmarks/RoboTwin`、`benchmarks/RoboDojo`、`benchmarks/vla-evaluation-harness`）。数字以仓库 commit 为准，定义以论文为准。
- 论文总数与总览关系：`benchmarks/papers-overview.md` 共 12 行（含 RoboCasa365、ManiSkill3、RoboTwin 1.0 三个同仓多文），本表按 9 个榜单归并，ManiSkill 行合并 ManiSkill2 与 ManiSkill3，RoboCasa365 行以 365 为主并注明原版 100 任务对照，RoboTwin 行以 2.0 为主并注明 1.0 对照。

## 横向对照表

| 榜单 | 定位 | 规模（任务数/演示数） | 引擎 | 指标 | SOTA + 日期 | 复现成本 |
| ---- | ---- | -------------------- | ---- | ---- | ----------- | -------- |
| LIBERO | 终身机器人学习的任务注册与遗忘度量，测知识迁移与灾难性遗忘，不测真机迁移 | 130 个语言条件任务，分 4 组套件；演示数以仓库数据配置为准 | MuJoCo + robosuite / Franka Panda | 稀疏成功率 + 混淆矩阵 + 前向迁移（含负向遗忘与成功率曲线面积，定义见论文评估节） | 未公开单一榜首分数（2026-09-20 整理，出处：论文 Table 1 / Table 2 基线对比，ER 与 PackNet 等方法按套件报告） | 低：纯仿真，复现入口见 `benchmarks/LIBERO/README.md` 与分章 01 |
| CALVIN | 语言条件长时序操作协议，测 5 步链式执行与跨环境泛化，不测大规模真机排名 | 约 34 类原子操作，5 步序列约 1000 条链，4 个环境 A/B/C/D；训练与评测划分以论文与仓库配置为准 | PyBullet + PlayTable 操作台 / 7 自由度机械臂 | LH-MTLC 链式评估：SR_1 至 SR_5 + 平均成功链长（定义见论文评估节） | 53.9%（2021-12，出处：论文评估表 MCIL 基线 D→D 的 MTLC 单步）；arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实） | 低：纯仿真，复现入口见 `benchmarks/calvin/README.md` 与分章 02 |
| SimplerEnv | 真实策略的 real-to-sim 排名复现，测仿真评估是否保序，不测新策略训练榜首 | Google Robot 与 WidowX 两类本体约 10 个任务族，配对外观与位姿变体；论文报告约 1500 轮配对评估 | SAPIEN + ManiSkill2 真机映射分支 / Google Robot 与 WidowX | 成功率 + Pearson 相关 + MMRV（排错的最大真实代价，定义见论文评估节） | 未公开单一成功率榜首（2026-09-20 整理，出处：论文 Fig.1 强相关结论，仿真与真实配对评估约 1500 轮；各策略分值见分章 03，不转述二手排名） | 中：纯仿真但需视觉匹配资源，复现入口见 `benchmarks/SimplerEnv/README.md` 与分章 03 |
| RoboCasa365 | 厨房原子到组合的泛化阶梯，测组合泛化与数据规模效应，不测开放家务全集 | 365 个任务，覆盖约 60 类厨房活动；约 2500 个厨房场景；约 612 小时人工演示 + 约 1615 小时合成演示（定义见 365 论文第 1 节；原版 100 任务对照见分章） | MuJoCo + robosuite / 移动操作平台 | 分见与未见组合成功率 + 终身学习阶段成功率（定义见 365 论文评估节） | 平均 22.5%（Atomic-Seen 44.1% / Composite-Seen 9.0% / Composite-Unseen 11.7%）（2026-03，出处：365 论文正文系统评估节）；仿真加真实联合训练 79.8%，纯真实 61.8%（出处同）；原版分数引用 arXiv:2406.02523 ⚠️待点开确认（RoboCasa 原版，原因：与仓库引用段对应关系未核实） | 中高：大规模数据与多场景渲染，复现入口见 `benchmarks/robocasa/README.md` 与分章 04 |
| ManiSkill | 图形处理器并行的大规模操作基准，测并行吞吐与跨本体泛化，不测家务语义规划 | ManiSkill2 约 20 个任务族；ManiSkill3 约 12 类域、20 余种机器人本体，数百万帧演示（含规划、强化学习与遥操作来源，定义见 ManiSkill3 论文第 1 节） | SAPIEN 3 + PhysX 并行物理 + Vulkan 并行渲染 / 多形态本体 | success_once / success_at_end / 回报值（定义见论文评估节） | 91.6%（2024-10，出处：ManiSkill3 论文正文真实评估，22 次成功共 24 次试验平均）；ManiSkill2 分数引用 arXiv:2302.04659 ⚠️待点开确认（ManiSkill2，原因：版本号未核实），细项见分章 05 | 中：需图形处理器并行，复现入口见 `benchmarks/ManiSkill/README.md` 与分章 05 |
| BEHAVIOR-1K | 完整家务活动的符号化定义，测长时程规划与状态变化处理，不测单步抓取榜首 | 1000 个日常活动，50 个场景，9000 余个物体；挑战子集约 100 任务与约 2 万条轨迹（定义见论文第 1 节） | Isaac Sim + OmniGibson / 移动双臂平台 | BDDL 谓词满足率 + 效率分（定义见论文评估节） | 未公开单一榜首分数（2026-09-20 整理，出处：论文实验节，长时程与复杂操作仍是挑战，基线分散未收敛，细项见分章 06） | 高：真实感渲染与大场景资源需求大，复现入口见 `benchmarks/BEHAVIOR-1K/README.md` 与分章 06 |
| RoboTwin 2.0 | 双臂数字孪生的评测与数据工厂，测双臂协作与视觉指令泛化，不测单臂桌面榜首 | 50 个评测任务，731 个物体共 147 类，5 种双臂构型，10 万余条预采集轨迹（定义见 2.0 论文摘要与第 1 图；1.0 对照见分章） | SAPIEN 3.0 / Aloha、ARX、Franka 等双臂平台 | 任务成功率 + 视觉与指令泛化剖面（含杂物、光照、背景、台面高度与语言五轴域随机，定义见论文评估节） | 71.3%（2025-06，出处：论文 Fig.1 自研方法）；代码生成成功率提升 10.9 个百分点（出处：论文摘要与正文）；纯合成零样本相对提升 228%，加 10 条真实演示相对提升 367%（出处同，细项见分章 07） | 中：纯仿真但双臂与域随机配置多，复现入口见 `benchmarks/RoboTwin/README.md` 与分章 07 |
| RoboDojo | 按能力维度剖分的统一评测，测记忆、精度、长时程等短板定位，不测单一总榜首 | 仿真 42 个标准任务加 12 个随机任务共 54 个，另有 18 个真机任务；30 余种策略已上报（定义见论文第 3 节） | Isaac Sim 5.1 + IsaacLab 2.3 / 多机器人本体 | 五维能力成功率：记忆、精度、长时程、开放理解等分维报告（定义见论文第 3 节评估设置） | 基线平均约 8% 至 13%（2026-07，出处：论文主结果表，如 Hy-Embodied 类基线平均约 13.07% 与 8.80% 双列；各维度细项见分章 08） | 高：仿真加真机两套，复现入口见 `benchmarks/RoboDojo/README.md` 与分章 08 |
| VLA-Harness | 一次接入、多处评测的矩阵型框架，测跨模型横评与复现保真，不测单一操作技能榜首 | 约 14 个仿真榜单适配器，6 类模型服务；榜单页聚合 657 条结果覆盖 17 个榜单（定义见论文 Fig.1 与第 1 节） | WebSocket 加消息打包协议，各榜单容器隔离 / 依所连榜单本体而定 | 分集到任务再到榜单三级聚合 + 复现偏差报告（定义见论文第 2 至 3 节） | 聚合 657 条结果（2026-03，出处：论文 Fig.1）；复现警示：单个未记录参数可摆动 55 个百分点，如 LIBERO 案例从 97.8% 降至 42%（出处：论文 Sec.III）；并行评估最高约 47 倍加速，如 2000 轮 LIBERO 约 18 分钟（出处：论文摘要与 Fig.1） | 中：需容器与多榜单配置，复现入口见 `benchmarks/vla-evaluation-harness/README.md` 与分章 09 |

表注：

- 规模数字以本地仓库 commit 为准，定义以论文为准；每格溯源见行内出处，分章给出仓库相对路径与论文章节号。
- SOTA 格格式为分数（日期，出处），出处为论文表号、图号或章节，或官方榜单页；未公开格同样给出整理日期与缺口原因，不臆测。
- 不确定编号警示：arXiv:2112.03227 ⚠️待点开确认（CALVIN，原因：版本号未核实）；arXiv:2406.02523 ⚠️待点开确认（RoboCasa 原版，原因：与仓库引用段对应关系未核实）；arXiv:2302.04659 ⚠️待点开确认（ManiSkill2，原因：版本号未核实）。三者凡出现均已标记。
- 一句话定位与选型表述参考教程站总览（<https://benchmark.linkseek.net.cn/>），事实口径以论文与仓库为准。

## 选型三问

1. 你要测长程、泛化、真实迁移中的哪一个？对应先看哪一篇？
   - 测终身与持续学习：先看 01 LIBERO（遗忘矩阵与前向迁移），再看 04 RoboCasa365（组合泛化与终身阶段）。
   - 测语言长时序：先看 02 CALVIN（5 步链式与跨环境），再看 06 BEHAVIOR-1K（千种家务符号化长程）。
   - 测真实策略排名保真：先看 03 SimplerEnv（Pearson 与 MMRV 保序性），再看 09 Harness（跨榜单复现偏差）。
   - 测大规模并行与吞吐：先看 05 ManiSkill（图形处理器并行与跨本体）。
   - 测双臂模仿与泛化：先看 07 RoboTwin 2.0（五轴域随机剖面）。
   - 测能力短板剖面：先看 08 RoboDojo（五维能力分维报告）。
   - 要横向扫榜：先看 09 Harness（矩阵聚合与复现清单）。
2. 你的算力与真机条件能负担哪一档复现成本？排除哪几篇？
   - 仅有单卡与纯仿真：优先 01、02、03；暂缓 06、08。
   - 有图形处理器并行需求：纳入 05；有双臂仿真需求：纳入 07。
   - 无真机：08 的 18 个真机任务只读结论不复现；03 与 09 的真实对照部分只读论文结论。
   - 要做大规模数据：04 的 2000 小时级数据先读分章再定资源。
3. 你的观测与动作接口与哪个榜单最接近？从哪一篇开始复现？
   - RGB 与深度加语言指令、桌面操作：从 01 或 02 开始。
   - 真机策略加仿真评估：从 03 开始，核对视觉匹配与变体聚合设置。
   - 厨房移动操作：从 04 开始。
   - 点云与多本体：从 05 开始，核对观测模态与动作空间章节。
   - 家务符号化目标：从 06 开始，核对 BDDL 定义。
   - 双臂协同：从 07 开始，核对双臂构型与域随机轴。
   - 多能力剖面：从 08 开始，核对五维划分与训练数据设置。
   - 多模型横评：从 09 开始，核对榜单适配器与聚合配置。

## 术语表

- BDDL：家务任务符号化定义语言，描述物体、初始状态与目标逻辑谓词；BEHAVIOR-1K 用其判定任务完成。出处理念见 BEHAVIOR-1K 论文定义节，复现入口见分章 06。
- LH-MTLC：语言条件长时序链式评估协议，一次指令下连续完成 5 个子任务；CALVIN 用其报告链长与分步成功率。出处理念见 CALVIN 论文评估节，复现入口见分章 02。
- SR_k：至少连续完成 k 步的子任务序列占比，k 取 1 至 5；CALVIN 主指标。出处理念见 CALVIN 论文评估表，复现入口见分章 02。
- MMRV：排错的最大真实代价，衡量仿真排名与真实排名不一致时的代价；SimplerEnv 用其与 Pearson 共同度量保序性。出处理念见 SimplerEnv 论文评估节，复现入口见分章 03。
- Pearson 相关：仿真分数与真实分数的线性相关系数；SimplerEnv 用其度量仿真评估是否保序。出处理念见 SimplerEnv 论文 Fig.1 与评估节。
- Visual Matching：真机背景叠加的视觉对齐做法，把真实外观带入仿真以缩小视觉差距；SimplerEnv 关键手段。出处理念见 SimplerEnv 论文方法节。
- Variant Aggregation：多变体汇总，把同一任务的外观与位姿变体分数汇总为稳定排名；SimplerEnv 用其降低单变体噪声。出处理念见 SimplerEnv 论文评估节。
- BDDL 谓词满足与效率分：BEHAVIOR-1K 完成度加效率的双指标，前者看逻辑目标是否达成，后者看步数与动作代价。出处理念见 BEHAVIOR-1K 论文实验节。
- EGL：图形处理器离屏渲染后端；CALVIN 等榜单用其做无屏仿真。出处理念见教程站附录与各仓文档，复现入口见分章 02。
- XPolicyLab：策略服务与评测适配层；RoboDojo 与 RoboTwin 用其统一仿真与真机接口。出处理念见 RoboDojo 论文框架节，复现入口见分章 07 与 08。

## 复现入口指引（只给入口，不写步骤）

- 总览对照源：`benchmarks/papers-overview.md`（12 篇论文总览与 3 个不确定编号说明）。
- 分章对照：01 对应 `benchmarks/LIBERO/`，02 对应 `benchmarks/calvin/`，03 对应 `benchmarks/SimplerEnv/`，04 对应 `benchmarks/robocasa/`，05 对应 `benchmarks/ManiSkill/`，06 对应 `benchmarks/BEHAVIOR-1K/`，07 对应 `benchmarks/RoboTwin/`，08 对应 `benchmarks/RoboDojo/`，09 对应 `benchmarks/vla-evaluation-harness/`。各仓复现入口以仓内 `README.md` 与 `docs/` 为准，细节见各分章复现路径节。
- 教程站长文对照（仅作表述参考）：<https://benchmark.linkseek.net.cn/> 总览一句话定位、选型三问与附录术语复现清单。
- 版本固定提醒：复现前先在各分章记录论文版本与仓库 commit，再核对配置文件与数据划分；跨库比较注明对应关系。
