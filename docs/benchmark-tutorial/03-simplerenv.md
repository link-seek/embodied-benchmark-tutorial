# 03 SimplerEnv：真实策略的 real-to-sim 排名复现榜单

> 模板来源：`.opencode/skills/benchmark-survey/`（七段结构复用 `templates/chapter.md`，引用格式复用 `templates/citation.md`）。表述口径与 `00-overview.md` 一致。

## 版本头

- 论文：Li et al., arXiv 预印本 2024，arXiv:2405.05941，<https://arxiv.org/abs/2405.05941>（版本细节以 arXiv 页面为准）
- 本地仓库：`benchmarks/SimplerEnv@06accac`
- 整理日期：2026-09-20
- 口径说明：定义以论文为准，数字以仓库 commit 为准

## 定位

- SimplerEnv 是 real-to-sim 评估榜单：策略在真实数据上训练，在专用仿真环境中评估，检验仿真评估的相对排名是否与真实排名保序，不测新策略训练榜首（来源：论文第 1 节与图 1）。
- 核心主张是不需要完整数字孪生：只要仿真评估与真实评估强相关，就能为策略迭代提供可靠的改进信号；作者明确仿真永远是真实的不完美代理，不取代真实评估（来源：论文第 3.1 节问题形式化段）。
- 一句话定位表述参考教程站总览与 SimplerEnv 对应章节，事实口径以论文与仓库为准（对照源见总览 `00-overview.md` 复现入口指引节）。

## 任务规模

- Google Robot 与 WidowX 两类本体约 10 个任务族：前者含拿可乐罐、拿任意物体、移近、开关抽屉、闭合抽屉内放置 6 组，后者含勺放毛巾、胡萝卜放盘、叠方块、茄子放入篮 4 组（来源：仓库 `benchmarks/SimplerEnv/README.md` 环境对照表；环境注册见 `benchmarks/SimplerEnv/simpler_env/__init__.py` 的 `ENVIRONMENTS` 与 `ENVIRONMENT_MAP`）。
- 细分变体含可乐罐横 / 竖 / 立姿、抽屉上 / 中 / 下层等子任务（来源：仓库 README 环境节）。
- 论文报告约 1500 轮配对 sim-and-real 评估，覆盖 RT-1、RT-1-X、RT-2-X、Octo 等开源通用策略（来源：论文图 1 题注与第 6.1 节实验设置）。
- 仓库真实与仿真分数对照表见 `benchmarks/SimplerEnv/simpler_env/utils/metrics.py` 的 `REAL_PERF` 与 `SIMPLER_PERF`（Google Robot 6 策略、WidowX 3 策略）。

## 引擎与本体

- 物理仿真基于 SAPIEN，环境代码基于 ManiSkill2 真机映射分支；另有 ManiSkill3 图形处理器并行分支，速度约为 ManiSkill2 版的 10 至 15 倍（来源：论文第 5 节；仓库 `benchmarks/SimplerEnv/README.md` 开篇段）。
- 操作本体为 Google Robot 与 WidowX 两套真机配置，机器人 URDF 来自公开仓库或 ROS 导出，相机内参用交互式工具对齐真实评估视频帧（来源：论文第 5 节；资产结构见仓库 `benchmarks/SimplerEnv/ManiSkill2_real2sim/` 与 README 代码结构节）。
- 论文同时在 Isaac Sim 中复现结论，说明方法不绑定单一仿真器（来源：论文第 6 节问答 5）。

## 观测动作

- 观测以图像为主输入，仓库示例从 ManiSkill2 观测字典取图送入策略，支持长时程任务的子任务指令推进（来源：仓库 `benchmarks/SimplerEnv/README.md` 上手示例节；推理主入口见 `benchmarks/SimplerEnv/simpler_env/main_inference.py`）。
- 动作语义为 7 维：位置增量 3、轴角旋转增量 3、夹爪 1（来源：仓库 README 上手示例节注释）。
- 控制频率 Google Robot 环境 3Hz、Bridge 环境 5Hz，仿真频率约 500Hz；单环境在消费级卡上渲染约 3500 步每秒，约为真实评估 7 倍速（来源：仓库 README 环境节；论文第 5 节）。

## 指标与基线

- 成功率：各任务仿真与真实成功率直接对照（来源：论文第 6.2 节与图 6、图 7；数值对照见仓库 `metrics.py` 的 `REAL_PERF` / `SIMPLER_PERF`）。
- Pearson 相关：仿真分数与真实分数的线性相关系数，越高越好，取值负 1 至 1（来源：论文第 3.2 节；计算入口见仓库 `benchmarks/SimplerEnv/tools/calc_metrics.py`）。
- MMRV（排错的最大真实代价）：把仿真排错两个策略的代价记为二者真实分数差，按策略取最坏再平均，越低越好，取值 0 至 1；它弥补 Pearson 只看线性拟合、对小噪声过敏感的两处短板（来源：论文第 3.2 节与图 3）。
- 两套评估做法：Visual Matching（真实背景绿幕叠加 + 前景物体与机械臂纹理匹配）与 Variant Aggregation（多外观多光照变体汇总平均以降单变体噪声）（来源：论文第 4.2 节与图 5；仓库 README 开篇两种设置节）。
- 保序性证据（2024-05，出处：论文图 6，Visual Matching 设置，Google Robot 四任务）：拿可乐罐 MMRV=0.031、r=0.976；移近 MMRV=0.111、r=0.855；开关抽屉 MMRV=0.055、r=0.915；开抽屉放苹果 MMRV=0.000、r=0.969。
- 未公开单一成功率榜首（2026-09-20 整理，出处：论文图 6 与图 7 的配对对照、各策略分值见仓库 `metrics.py`，不转述二手排名）。
- 消融结论：控制差距（系统辨识前后开环复现对比，论文图 4）与视觉差距均显著影响保序性；物体质心、摩擦等物理属性取简化近似仍可保序（来源：论文第 6 节问答 3、问答 4）。

## 复现路径

- 复现入口以仓库文档为准，不写部署步骤：
  - `benchmarks/SimplerEnv/README.md`（上手示例、环境对照表、两种评估设置、代码结构四节）
  - `benchmarks/SimplerEnv/simpler_env/__init__.py`（`ENVIRONMENTS` 与 `ENVIRONMENT_MAP` 环境注册）
  - `benchmarks/SimplerEnv/simpler_env/main_inference.py`（主推理入口）与 `simpler_env/simple_inference_visual_matching_prepackaged_envs.py`（预打包环境简易推理）
  - `benchmarks/SimplerEnv/scripts/`（各任务 visual_matching / variant_agg 推理配置）
  - `benchmarks/SimplerEnv/tools/calc_metrics.py`（MMRV 与 Pearson 复算）与 `simpler_env/utils/metrics.py`（真实与仿真分数对照）
  - `benchmarks/SimplerEnv/ADDING_NEW_ENVS_ROBOTS.md`（新增环境与本体指引）与 `benchmarks/SimplerEnv/example.ipynb`（交互示例）
  - 论文 arXiv 页 <https://arxiv.org/abs/2405.05941>（版本头已列，此处复列为复现入口）与仓库首页 <https://github.com/simpler-env/SimplerEnv>

## 局限与对我们的意义

- 局限（来源：论文第 3.1 节、第 5 节与第 6 节问答 4）：仿真永远是不完美代理，只能补位不能取代真实评估；物体密度、摩擦取常识近似，质心与动摩擦等精细物理属性被简化；铰接物体（如抽屉柜）建模是全流程中最费人力的一环；覆盖刚体为主的可乐罐、抽屉类任务，柔性与复杂接触任务外推性未验证。
- 对我们的意义：适合真实策略的检查点选择、失败模式预判与分布偏移敏感性分析，Visual Matching 与 Variant Aggregation 两套做法可直接复用到我们自己的 real-to-sim 管线；不适合从零训练新策略的榜首争夺（该需求看 05 分章），无真机时真实对照部分只读论文结论。
