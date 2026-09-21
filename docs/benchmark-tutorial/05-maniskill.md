# 05 ManiSkill：图形处理器并行的大规模操作基准

> 模板来源：`.opencode/skills/benchmark-survey/`（七段结构复用 `templates/chapter.md`，引用格式复用 `templates/citation.md`）。表述口径与 `00-overview.md` 一致，本篇 ManiSkill2 与 ManiSkill3 分段写清，分数注明出自哪一篇。

## 版本头

- 论文（ManiSkill2）：Gu et al., ICLR 2023，arXiv:2302.04659 ⚠️待点开确认（ManiSkill2，原因：版本号未核实），<https://arxiv.org/abs/2302.04659>
- 论文（ManiSkill3）：Tao et al., RSS 2025，arXiv:2410.00425，<https://arxiv.org/abs/2410.00425>
- 本地仓库：`benchmarks/ManiSkill@62ff3a5`（两代同仓：ManiSkill3 对应 `mani_skill>=3.0.0`，ManiSkill2 对应 `mani_skill==0.5.3` 及 `v0.5.3` 标签，见仓库 `README.md` 引用节）
- 整理日期：2026-09-20
- 口径说明：定义以论文为准，数字以仓库 commit 为准

## 定位

- ManiSkill 是面向可泛化操作技能的大规模仿真基准，核心是度量并行吞吐与跨本体泛化，回答“策略能否在物体拓扑几何变化与多本体下保持成功”这类问题，不测家务语义规划（来源：ManiSkill2 论文第 1 节；ManiSkill3 论文第 I 节）。
- ManiSkill2 解决通用可泛化操作技能的统一评测，ManiSkill3 在此之上解决图形处理器并行仿真加渲染的规模化与异构并行（来源：ManiSkill2 论文摘要；ManiSkill3 论文摘要与第 III 节）。
- 一句话定位表述参考教程站 ManiSkill 章节（<https://benchmark.linkseek.net.cn/>），事实口径以论文与仓库为准。

## 任务规模

- ManiSkill2：20 个操作任务族，2000 余个物体模型，400 万帧以上演示；覆盖静止与移动底座、单臂与双臂、刚体与柔体四类（来源：ManiSkill2 论文摘要与第 2.1 节；arXiv:2302.04659 ⚠️待点开确认，ManiSkill2，原因：版本号未核实）。
- ManiSkill2 任务分组：6 个柔体操作（填充、悬挂、挖掘、倾倒、捏塑、书写）、3 个毫米级孔轴装配（单孔、充电插头、套件组装）、5 个六自由度取放（含 YCB 与 EGAD 物体集及杂乱场景）、5 个关节物体与避障任务（含推椅、移桶、开柜门抽屉、转水龙头）（来源：ManiSkill2 论文第 2.1 节）。
- ManiSkill3：12 类任务域、20 余种机器人本体开箱即用，数百万帧演示（来源：ManiSkill3 论文摘要与第 III-A 节；总览表口径为约 12 类域、20 余种本体）。
- ManiSkill3 演示来源三轨：易任务用运动规划与稠密奖励强化学习生成，难任务用少量遥操作演示（约 10 条）加在线模仿学习（RLPD、RFCL）扩增，另支持轨迹回放改观测与奖励（来源：ManiSkill3 论文第 III-G 节）。

## 引擎与本体

- ManiSkill2：SAPIEN 物理仿真，自研 Warp 图形处理器 MPM 柔体求解器并与刚体双向耦合，异步渲染加渲染服务器，单卡 16 进程下 RGBD 输入 PPO 约 2000 FPS（来源：ManiSkill2 论文第 3 节与第 4 节，Table 1）。
- ManiSkill3：SAPIEN 并行渲染系统，PhysX 图形处理器仿真，仿真加渲染最高 30000 FPS 以上，显存占用约为同类平台的二分之一到三分之一（来源：ManiSkill3 论文摘要与第 III-B 节，Figure 2、Figure 4）。
- ManiSkill3 独有异构图形处理器仿真：各并行环境可仿真不同几何、不同物体数、不同自由度关节（如每环境不同柜子与抽屉），并行一次渲染（来源：ManiSkill3 论文第 III-C 节与 Figure 6）。
- 本体：ManiSkill2 覆盖静止臂、移动底座、双臂与夹爪组合；ManiSkill3 开箱支持 20 余种（含四足、浮动夹爪、人形、灵巧手），统一资源描述与控制器配置（来源：ManiSkill2 论文第 2.1 节；ManiSkill3 论文第 III-F 节）。

## 观测动作

- ManiSkill2 观测三模态：点云、RGBD、特权状态；双相机（基座相机加腕部相机）；目标位置类任务在点云中拼接目标 cues 点（来源：ManiSkill2 论文第 5.2 节实验设置段）。
- ManiSkill2 动作（控制器即动作空间）：关节位置、增量关节位置、增量末端位姿等，可按机器人部件混搭（如底座速度、臂任务空间、夹爪关节空间各配其一），并支持演示动作空间转换到目标动作空间（来源：ManiSkill2 论文第 2.2 节）。
- ManiSkill3 观测与渲染：RGB、深度、分割并行渲染，另支持体素与点云格式；支持千级相机域随机化（位姿、光照、纹理）与 640 乘 480 真实分辨率数字孪生（来源：ManiSkill3 论文第 III-B 节与 Figure 5）。
- ManiSkill3 控制：图形处理器并行关节位置控制与逆解控制，虚拟现实遥操作系统支持长时程与精细任务演示采集（来源：ManiSkill3 论文第 III-D、III-F 节）。

## 指标与基线

- ManiSkill2 指标：各任务族成功率，训练与测试物体集分离报告泛化（测试集结果见附录 F，来源：ManiSkill2 论文第 5 节首段与第 2.1 节各任务评估协议段；arXiv:2302.04659 ⚠️待点开确认，ManiSkill2，原因：版本号未核实）。
- ManiSkill2 代表性基线（2023-02，出处：ManiSkill2 论文第 5.1、5.2 节）：感知规划执行 Sense-Plan-Act 在 PickSingleYCB 上 Contact-GraspNet 加运动规划成功率 43.24%（74 物体乘 5 轮，质心距目标 2.5 厘米内算成功），在 AssemblingKits 上 Transporter Networks 按严格插入口径成功率 18%（100 轮）；行为克隆在装配与捏塑书写类任务上接近零成功；DAPG 加 PPO（2500 万步）在 PickCube 点云智能体上达 0.94±0.03，高精度装配仍近零。
- ManiSkill3 真实评估分数（2024-10，出处：ManiSkill3 论文第 III-E 节与第 IV-D 节）：PickCube 立方体抓取仿真训练零样本部署真实，3 次训练、每次真实评 8 轮，共 22 次成功 24 次试验，平均成功率 91.6%（小样本 22/24，含不同尺寸颜色与初始位姿）。
- ManiSkill3 真实孪生对照（2024-10，出处：ManiSkill3 论文第 III-E 节）：4 个 SIMPLER 数字孪生任务上 Octo 与 RT-1X 的仿真真实成功率相关系数 0.9284，平均最大排序违背值 0.0147。
- ManiSkill3 速度基线（2024-10，出处：ManiSkill3 论文第 IV-C 节）：NatureCNN 骨干 PPO 在 PickCube 上 256 并行环境约 1 小时训 1500 万样本（RTX 4090），数字孪生评估约为真实速度 60 到 100 倍。

## 复现路径

- 复现入口以仓库文档为准，不写部署步骤：
  - `benchmarks/ManiSkill/README.md`（ManiSkill3 主入口，ManiSkill2 代码定位 `v0.5.3` 标签节）
  - `benchmarks/ManiSkill/README.md` 引用节（ManiSkill3 用 `taomaniskill3` 条目，ManiSkill2 用 `gu2023maniskill2` 条目，两代引用分开）
  - `benchmarks/ManiSkill/CITATION_MS2.cff` 与 `CITATION.cff`（两代引用元数据）
  - `benchmarks/ManiSkill/mani_skill/`（环境、智能体、传感器、轨迹、向量化等模块目录）
  - 在线文档 <https://maniskill.readthedocs.io/> 与演示画廊（见仓库 README 外链节）

## 局限与对我们的意义

- 局限：ManiSkill2 高精度装配与精细柔体变形对现有模仿与强化学习仍极难（多任务成功率近零，来源：ManiSkill2 论文第 5.2 节）；ManiSkill3 软体环境非批量并行、视觉触觉仿真走专用分支（来源：ManiSkill3 论文第 IV-D 节后段与第 VI 节）。
- 对我们的意义：适合作为并行吞吐与跨本体泛化的主基准——有点云需求从 ManiSkill2 的 PickSingleYCB 与控制器转换系统入手，要图形处理器规模化则切 ManiSkill3 异构并行与域随机化；有真实机械臂时优先复用 PickCube 数字孪生范式（91.6% 为小样本 22/24，不宜外推为通用成功率）；不做家务语义规划时不选本榜单（总览表复现成本记为中，需图形处理器并行）。
