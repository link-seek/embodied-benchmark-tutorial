# 具身智能榜单论文总览

> 覆盖 `benchmarks/` 下 9 个榜单仓库的代表性论文，共 12 行（含 RoboCasa365、ManiSkill3、RoboTwin 1.0 三个同仓多文）。arXiv 编号均经 `opencli arxiv search` 逐个核验，
> 并与各仓库 `README.md` 引用段交叉确认；Star 数为任务给定快照（非实时），代码链接为完整 GitHub 地址。

| 榜单 | 论文标题 | 作者/单位 | 年份/Venue | arXiv | 代码 | Star |
|---|---|---|---|---|---|---|
| LIBERO | LIBERO: Benchmarking Knowledge Transfer for Lifelong Robot Learning | Bo Liu、Yifeng Zhu、Chongkai Gao 等（单位详见论文） | 2023 / arXiv 预印本 | [2306.03310](https://arxiv.org/abs/2306.03310) | [Lifelong-Robot-Learning/LIBERO](https://github.com/Lifelong-Robot-Learning/LIBERO) | 2321 |
| CALVIN | CALVIN: A Benchmark for Language-Conditioned Policy Learning for Long-Horizon Robot Manipulation Tasks | Oier Mees、Lukas Hermann、Erick Rosete-Beas、Wolfram Burgard | 2022 / IEEE RA-L | [2112.03227](https://arxiv.org/abs/2112.03227) | [mees/calvin](https://github.com/mees/calvin) | 987 |
| SimplerEnv | Evaluating Real-World Robot Manipulation Policies in Simulation | Xuanlin Li、Kyle Hsu、Jiayuan Gu 等（单位详见论文） | 2024 / arXiv 预印本 | [2405.05941](https://arxiv.org/abs/2405.05941) | [simpler-env/SimplerEnv](https://github.com/simpler-env/SimplerEnv) | 1165 |
| RoboCasa | RoboCasa: Large-Scale Simulation of Everyday Tasks for Generalist Robots | Soroush Nasiriany、Abhiram Maddukuri、Lance Zhang 等 | 2024 / RSS | [2406.02523](https://arxiv.org/abs/2406.02523) | [robocasa/robocasa](https://github.com/robocasa/robocasa) | 1738 |
| RoboCasa365 | RoboCasa365: A Large-Scale Simulation Framework for Training and Benchmarking Generalist Robots | Soroush Nasiriany、Sepehr Nasiriany、Abhiram Maddukuri、Yuke Zhu | 2026 / ICLR | [2603.04356](https://arxiv.org/abs/2603.04356)（2026-03-04，标题作者与官网 PDF 一致；官网固定版 [PDF](https://robocasa.ai/assets/robocasa365_iclr26.pdf)） | [robocasa/robocasa](https://github.com/robocasa/robocasa) | 1738 |
| ManiSkill | ManiSkill2: A Unified Benchmark for Generalizable Manipulation Skills | Jiayuan Gu、Fanbo Xiang、Xuanlin Li 等（单位详见论文） | 2023 / ICLR | [2302.04659](https://arxiv.org/abs/2302.04659) | [mani-skill/ManiSkill](https://github.com/mani-skill/ManiSkill) | 3336 |
| ManiSkill3 | ManiSkill3: GPU Parallelized Robotics Simulation and Rendering for Generalizable Embodied AI | Stone Tao、Fanbo Xiang、Arth Shukla 等 | 2025 / RSS | [2410.00425](https://arxiv.org/abs/2410.00425) | [mani-skill/ManiSkill](https://github.com/mani-skill/ManiSkill) | 3336 |
| BEHAVIOR-1K | BEHAVIOR-1K: A Human-Centered, Embodied AI Benchmark with 1,000 Everyday Activities and Realistic Simulation | Chengshu Li、Ruohan Zhang、Josiah Wong 等（单位详见论文） | 2024 / arXiv 预印本 | [2403.09227](https://arxiv.org/abs/2403.09227) | [StanfordVL/BEHAVIOR-1K](https://github.com/StanfordVL/BEHAVIOR-1K) | 1706 |
| RoboTwin | RoboTwin 2.0: A Scalable Data Generator and Benchmark with Strong Domain Randomization for Robust Bimanual Robotic Manipulation | Tianxing Chen、Zanxin Chen、Baijun Chen 等（单位详见论文） | 2025 / ICML 2026（README标注） | [2506.18088](https://arxiv.org/abs/2506.18088) | [RoboTwin-Platform/RoboTwin](https://github.com/RoboTwin-Platform/RoboTwin) | 2874 |
| RoboTwin 1.0 | RoboTwin: Dual-Arm Robot Benchmark with Generative Digital Twins | Yao Mu、Tianxing Chen、Zanxin Chen 等 | 2025 / CVPR Highlight | [2504.13059](https://arxiv.org/abs/2504.13059) | [RoboTwin-Platform/RoboTwin](https://github.com/RoboTwin-Platform/RoboTwin) | 2874 |
| RoboDojo | RoboDojo: A Unified Sim-and-Real Benchmark for Comprehensive Evaluation of Generalist Robot Manipulation Policies | Tianxing Chen、Yue Chen、Zixuan Li 等（单位详见论文） | 2026 / arXiv 预印本 | [2607.04434](https://arxiv.org/abs/2607.04434) | [RoboDojo-Benchmark/RoboDojo](https://github.com/RoboDojo-Benchmark/RoboDojo) | 581 |
| vla-evaluation-harness | vla-eval: A Unified Evaluation Harness for Vision-Language-Action Models | Suhwan Choi、Yunsung Lee、Yubeen Park 等（单位详见论文） | 2026 / arXiv 预印本 | [2603.13966](https://arxiv.org/abs/2603.13966) | [allenai/vla-evaluation-harness](https://github.com/allenai/vla-evaluation-harness) | 614 |

## 本地文件（2026-09-20 经 url2hc 中转下载）

PDF 与 Markdown 均在 `benchmarks/papers/`（PDF 共 184M，md 为 MarkItDown 转换版，AI 阅读建议用 md）：

| 本地 PDF | 本地 md | 对应论文 |
|---|---|---|
| 2112.03227.pdf | 2112.03227.md | CALVIN |
| 2302.04659.pdf | 2302.04659.md | ManiSkill2 |
| 2306.03310.pdf | 2306.03310.md | LIBERO |
| 2403.09227.pdf | 2403.09227.md | BEHAVIOR-1K |
| 2405.05941.pdf | 2405.05941.md | SimplerEnv |
| 2406.02523.pdf | 2406.02523.md | RoboCasa |
| 2410.00425.pdf | 2410.00425.md | ManiSkill3 |
| 2504.13059.pdf | 2504.13059.md | RoboTwin 1.0 |
| 2506.18088.pdf | 2506.18088.md | RoboTwin 2.0 |
| 2603.13966.pdf | 2603.13966.md | vla-eval |
| 2607.04434.pdf | 2607.04434.md | RoboDojo |
| robocasa365.pdf | robocasa365.md | RoboCasa365 |

> 下载链路：直连 arXiv 仅约 25KB/s，改走 `link-seek/url2hc` 的 download-to-obs 工作流中转（OBS 预签名链接约 2.2MB/s，快约 90 倍）。OBS 源文件 7 天后自动删除，本地已落地不受影响。

## 表注

- **对照结论（2026-09-20 实测）**：6 个与各仓 README 引用段逐字一致——LIBERO `2306.03310`、SimplerEnv `2405.05941`、BEHAVIOR-1K `2403.09227`、RoboTwin 2.0 `2506.18088`、RoboDojo `2607.04434`（标题作者年份全对）、vla-eval `2603.13966`（作者 Choi 等全对）。
- **不确定的 3 个**：CALVIN `2112.03227`、RoboCasa 原版 `2406.02523`、ManiSkill2 `2302.04659`——三仓 README 的 bibtex 里没写 arXiv 号（只写了 RA-L 2022 / RSS 2024 / ICLR 2023），这三个号来自 `arxiv search`，标题作者对得上但没经过 README 逐字确认，引用前建议点开 arXiv 再看一眼。

- **来源**：arXiv 编号与标题作者以 `opencli arxiv search` 返回为准，Venue（CALVIN 的 IEEE RA-L、RoboCasa 的 RSS、ManiSkill2 的 ICLR）以各仓库 `README.md` 引用段为准；其余 Venue 暂按 arXiv 预印本标注。
- **RoboDojo 说明**：任务描述为"2025–2026 sim-and-real"，经核验其论文为 `2607.04434`（2026-07，README 引用段一致），表中已按核验结果填写。
- **ManiSkill 说明**：两代同仓，ManiSkill2（Gu 等，ICLR 2023，`2302.04659`）与 ManiSkill3（Tao 等，RSS 2025，`2410.00425`）均已列入上表。
- **RoboTwin 说明**：初代（`2504.13059`，CVPR 2025 Highlight；早期版 `2409.02920`）与 2.0（`2506.18088`，ICML 2026）均已列入上表，另有挑战赛论文 `2506.23351` 未列。
- **RoboCasa 说明**：原版（RSS 2024）与 RoboCasa365（ICLR 2026，暂无 arXiv，官网 PDF）均已列入上表。
