# 09 VLA-Harness：一次接入、多处评测的矩阵型框架

> 模板来源：`.opencode/skills/benchmark-survey/`（七段结构复用 `templates/chapter.md`，引用格式复用 `templates/citation.md`）。表述口径与 `00-overview.md` 一致。

## 版本头

- 论文：Choi et al., arXiv 预印本 2026，arXiv:2603.13966，<https://arxiv.org/abs/2603.13966>（vla-eval 统一评测框架；榜单聚合 657 条结果）
- 本地仓库：`benchmarks/vla-evaluation-harness@c233542`（v0.6.0 版系，含 `configs/` 榜单与模型服务配置与 `docs/reproductions/` 复现记录，来源：仓库 README Latest News 节）
- 整理日期：2026-09-20
- 口径说明：定义以论文为准，数字以仓库 commit 为准；本篇不测单一操作技能榜首，只讲横评矩阵与复现保真

## 定位

- VLA-Harness（仓库名 vla-evaluation-harness，论文名 vla-eval）是矩阵型评测框架，核心是跨模型横评与复现保真，回答“同一模型在多个榜单上是否稳定、他人分数能否复现”这类问题，不测单一操作技能榜首（来源：论文摘要与第 1 节）。
- 设计对标语言模型的 lm-evaluation-harness：模型推理与榜单执行经 WebSocket 加消息打包协议解耦，榜单侧容器隔离，集成复杂度从模型数乘榜单数降为两者之和（来源：论文第 2 节架构段）。
- 一句话定位表述参考教程站 Harness 章节（<https://benchmark.linkseek.net.cn/>），事实口径以论文与仓库为准。

## 任务规模

- 约 14 个仿真榜单适配器（论文 Fig.1 与 Table I 口径：SimplerEnv、LIBERO、CALVIN 三个经跨代码库复现验证，其余为已集成待验证），6 类模型服务（OpenVLA、pi0/pi0-FAST、GR00T N1、X-VLA、CogACT 等，来源：论文第 2.2 节与 Table I）。
- 榜单页聚合 657 条结果覆盖 17 个榜单，源自 1704 篇引用 tracked 榜单的论文（来源：论文 Fig.1 与第 4 节榜单节）。
- 仓库 `configs/` 现有榜单配置约 20 个目录（含 LIBERO、CALVIN、SimplerEnv、RoboCasa、RoboCasa365、RoboTwin、RoboDojo 等）与 10 余个模型服务目录，数量多于论文 Table I 的 14 加 6（来源：仓库 `benchmarks/vla-evaluation-harness/configs/benchmarks/` 与 `configs/model_servers/` 目录；新增榜单以仓库为准，论文口径见论文第 2.2 节）。
- 复现矩阵：6 个 VLA 代码库（OpenVLA、pi0.5、OpenVLA-OFT、GR00T N1.6、DB-CogACT、X-VLA）在 LIBERO、CALVIN、SimplerEnv 三榜单上的对照（来源：论文 Table II）。

## 引擎与本体

- 通信协议 WebSocket 加消息打包序列化，每条消息带类型（观测、动作、轮次起止）、评估载荷、序列号、时间戳（来源：论文第 2.1 节架构段）。
- 榜单侧 Docker 容器隔离：镜像 4.7GB 到 35.6GB，动作维度 6 到 14 维，发布于 ghcr.io 带版本标签，场景与纹理资产随镜像捆绑（来源：论文 Table I 与第 2.1 节声明式配置段）。
- 模型侧单文件 uv 脚本：实现单个 `predict(obs, ctx)` 方法（约 50 行），依赖经 PEP 723 内联元数据声明，`vla-eval serve` 自动建隔离环境（来源：论文第 2.1 节模型服务段与 Listing 1）。
- 本体依所连榜单而定：框架本身不固定本体，榜单容器内各用各的仿真器与机器人（来源：论文第 2.2 节榜单表与仓库 `configs/benchmarks/` 各榜单配置目录）。

## 观测动作

- 观测封装：榜单侧四方法接口（reset、step、make_obs、get_step_result）在容器内实现，统一转观测载荷发模型服务（来源：论文第 2.1 节榜单集成段）。
- 动作下发：模型服务返回动作向量，框架自动做动作分块与可选批量推理（`max_batch_size`，来源：论文第 2.1 节模型服务段）。
- 评估配置：两份 YAML（榜单配置加模型服务配置）驱动一次评测，`vla-eval serve` 起模型服务，`vla-eval run` 跑榜单客户端（来源：论文第 2.1 节声明式配置段；仓库 `configs/benchmarks/` 与 `configs/model_servers/`）。
- 结果产物：每次运行落结构化 JSON，记录框架版本、榜单配置、逐轮指标，可据单配置文件精确重放（来源：论文第 2.1 节声明式配置段末段）。

## 指标与基线

- 聚合方式三级：分集到任务再到榜单逐级汇总，另附复现偏差报告；榜单页对不可比配置（SimplerEnv 三机器人配置、CALVIN 两切分、LIBERO 四或五分集）立规范协议（来源：论文第 2 至 4 节）。
- 复现矩阵（2026-03，出处：论文 Table II，三榜单固定种子与版本化镜像）：
  - LIBERO：X-VLA 97.4%（偏差 -0.7）、pi0.5 97.7%（+0.8）、OpenVLA-OFT 96.7%（-0.4）、GR00T N1.6 94.9%（-2.1）、DB-CogACT 94.7%（-0.2）、OpenVLA 76.2%（-0.3）。
  - CALVIN（链长）：X-VLA 4.30（-0.13）、DB-CogACT 4.02（-0.04）。
  - SimplerEnv：X-VLA 94.8%（-1.0）、DB-CogACT 63.5%（-6.0）、GR00T N1.6 59.7%（-8.0，Google Robot 视觉匹配，其余为 WidowX）。
- 复现偏差警示（出处：论文第 3.2 节）：单个未记录参数可摆动 55 个百分点——X-VLA 在 LIBERO 上用错本体状态源从 97.8% 掉到 42%；四轴与增量动作模式混淆可致 0%；四元数归一化口径差异致 LIBERO-Goal 97% 到 83%、LIBERO-Long 95% 到 56%；缺 OpenVLA 中心裁剪（scale 0.9）约掉 3 个百分点；GR00T 缺末端位姿本体输入在 SimplerEnv 上从 30%~55% 掉到 0%。
- 并行加速：2000 轮 LIBERO 约 18 分钟（对比串行约 14 小时），最高约 47 倍加速（2026-03，出处：论文摘要、Fig.2 与 Fig.3；CogACT-7B 在 H100 上环境吞吐 32.6 倍加批量推理 2.8 倍合成）。
- 榜单覆盖洞察：509 余模型中 81% 只测过 1 个榜单，仅约 6% 测过 3 个及以上（出处：论文 Fig.5 与第 4.2 节）。

## 复现路径

- 复现入口以仓库文档为准，不写部署步骤：
  - `benchmarks/vla-evaluation-harness/README.md`（总入口：安装节 `pip install vla-eval`、双终端快速开始节、并行评测节）
  - `benchmarks/vla-evaluation-harness/configs/benchmarks/`（榜单配置：`libero/`、`simpler/`、`calvin/`、`robotwin/`、`robocasa365/`、`robodojo/` 等）
  - `benchmarks/vla-evaluation-harness/configs/model_servers/`（模型服务配置：`openvla/`、`pi0/`、`groot/`、`oft/`、`xvla/`、`lerobot/` 等）
  - `benchmarks/vla-evaluation-harness/docs/reproductions/`（复现记录：`openvla.md`、`xvla.md`、`lerobot.md`、`robodojo.md` 等 seven-plus 文件，见仓库该目录）
  - 榜单页 <https://allenai.github.io/vla-evaluation-harness/leaderboard>（见论文第 4 节与 README Latest News 节）
  - 架构说明 `benchmarks/vla-evaluation-harness/docs/architecture.md`（见 README 动机节外链）

## 局限与对我们的意义

- 局限：审计只覆盖 6 个代码库与 3 个仿真榜单，真机迁移未覆盖；榜单页结果摘自已发表论文、未经独立验证；支持指标限任务成功率，子任务进度、效率、安全维度暂不支持（来源：论文第 4 节局限段与第 5 节结论段）。
- 对我们的意义：适合作为多模型横评的入口框架——先用两命令（serve 加 run）跑通 LIBERO 复现矩阵核对偏差，再按需接入 RoboTwin、RoboDojo 等榜单；引用他人分数前先查规范协议是否可比，不可比配置不并表；单参数 55 个百分点摆动的教训是复现必须锁死本体状态源、动作模式、四元数口径三项（总览表复现成本记为中）。
