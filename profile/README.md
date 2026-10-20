<div align="center">

# Kite Robotics Special Interest Group

**机器人系统的跨学科协作组织**

模型与推理 · 电控 · 机械 · 感知与数据 · 平台工具

</div>

---

## 我们在做什么

我们想打通一条完整的链路：**让模型看懂画面、听懂指令、在真实的机器人上做出动作。**

现在这条链路上有几件正在做的事：

- **把 VLA 模型放到端侧**：X-VLA 的权重经过融合、fp16、量化、编译，跑在一块没有独立显卡的板子上
  （最近一次实测：端到端 **236 ms / 动作**，约为主机 CPU 基线的 105 倍）
- **让机器人听懂人话**：板端中文语音识别，纯 CPU 运行
- **把跨专业的接口对齐**：结构尺寸、通信协议、模型输入输出，谁改了什么、影响谁，都写在仓库里

组织里不只有模型和推理：电控、机械、感知、工具都是一等公民，各自有仓库、各自的负责人。

> 这里也可以是很多人做具身智能的第一站。不要求你先会什么，愿意动手就行；
> 文档和代码尽量留给下一届能用，而不是每次从零开始。

## 一点罗曼史

> All our dreams can come true, if we have the courage to pursue them.
> 只要我们有勇气去追求，所有的梦想都可以成真。
> —— 沃尔特·迪士尼

机器人不是从工厂里长出来的，它比工业更早。

公元一世纪，亚历山大港的希罗写过自动门、会唱歌的鸟；中国的《列子》里，工匠偃师造出过一个能歌善舞、还会朝观众眨眼的人。那时候没有电机，没有计算机，甚至还没有"机器人"这个词，可人们已经在想：能不能让东西自己动起来。

一九二〇年，恰佩克的剧本《罗梭的万能工人》第一次把 robot 写进语言，它来自捷克语 robota——劳役、苦工。二十二年后，阿西莫夫写下机器人三定律；一九六一年，Unimate 站上通用汽车的流水线；再往后是火星车、ASIMO、Atlas，直到今天，大模型开始试着"看懂"世界。

这条线上一直有两股力量。一股是工程：材料、控制、算力，一代人做一点，慢得像在石头上刻字。另一股是想象：我们总希望身边有一个能理解我们、替我们做事的对象。工程让想象有了落点，想象让工程值得做下去。

我们今天做的事很小：让一个模型在几十瓦的板子上，完成一次"看见—理解—行动"。我们不敢说它有多完美——延迟还不够低，精度还没测全，它离"一个真正的伙伴"还有很长的路。但我们可以问心无愧地说：仓库里的每一条命令都跑过，每一个数字都测过，踩过的坑也都写下来了。

谨以此，献给每一个愿意在这条路上多走一步的人。

我有一个愿望，有一天，这份文档能被下一届改得比我们写的更好。
我有一个愿望，有一天，国产的芯片和模型能一起，把这件事做到世界前面。
我有一个愿望，有一天，某个在这里跑通第一个模型的人，会做出我们今天想象不到的东西。

这件事，会是由我们一起做完的。我等着你们的好消息。

—— Kite Robotics SIG，2026 年 10 月，于吉林长春

## 进展与里程碑

| 日期 | 里程碑 | 摘要 |
| --- | --- | --- |
| 2026-10-16 | **M0 · 模型首次上板跑通** | X-VLA 的 `.hmm` 在 **飞腾 D3000M + 后摩漫界 M50** 上完成首次真机推理：**236 ms / 动作（4.23 Hz，约 105× CPU 基线）**；同日板端中文 ASR（Qwen3-ASR + llama.cpp）跑通 · [记录](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/issues/8) · [预发布](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/releases/tag/v0.1.0-m50-first-run) |

> 新里程碑的格式照着这一行写就行：日期、一句结论、可复现的数字、链接。

## 子系统与仓库

| 方向 | 典型内容 | 仓库命名建议 | 状态 |
| --- | --- | --- | --- |
| 模型与推理 | VLA 模型、端侧运行时、量化与部署 | `kite`（主仓库）、`*-runtime` | 进行中 |
| 电控 | 主控固件、电机/传感器驱动、总线与通信协议 | `ec-*` | 待建立 |
| 机械 | 结构设计、CAD、装配与加工图 | `mech-*` | 待建立 |
| 感知与数据 | 相机与标定、数据采集/标注/清洗 | `perception-*`、`data-*` | 待建立 |
| 平台与工具 | 仿真、测试台、CI、公共脚本与文档 | `tools-*`、`docs-*` | 待建立 |

新仓库请按上面前缀命名（小写 + 短横线），并在下面的索引里补一行。

## 仓库索引

| 仓库 | 说明 | 方向 | 可见性 |
| --- | --- | --- | --- |
| [Kite](https://github.com/Kite-Robotics-Special-Interest-Group/Kite) | 端侧 VLA 模型与运行时（模型、量化、部署、ASR） | 模型与推理 | 私有 |
| [Kite-Open](https://github.com/Kite-Robotics-Special-Interest-Group/Kite-Open) | 公开镜像：soft prompt 工具、运行时抽象、容器与文档 | 模型与推理 | 公开 |

## 怎么开始

| 你的情况 | 从这里开始 |
| --- | --- |
| 外部访客，只想看可公开的部分 | [Kite-Open](https://github.com/Kite-Robotics-Special-Interest-Group/Kite-Open) |
| 第一次用 GitHub | [零基础入门](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/docs/onboarding.md)（装工具、克隆、提交、发 PR） |
| 模型 / 推理 / 部署 | [从零教程](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/tutorial.md) · [M0 复现清单](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/reproduce-m0.md) |
| 电控 / 机械 / 感知 | 先读 [零基础入门](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/docs/onboarding.md) 与 [贡献规范](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/CONTRIBUTING.md)；新仓库用 [复现文档模板](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/docs/reproduction-template.md) 起手 |
| 要写文档 / 做交接 | [如何写复现文档](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/docs/reproduction-guide.md) |
| 工具与环境 | Docker 优先（Windows 上依赖 WSL2，见 [WSL 入门](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/docs/wsl.md)） |

## 协作约定

1. **每个仓库都要有 README**：目标、负责人、当前状态、复现方式、快速开始。
2. **接口变更要跨组对齐**：机械尺寸、通信协议、模型输入输出——PR 里写清影响面，并请对应方向负责人看过。
3. **Issue / PR 走模板**：分支 → PR → 至少 1 人 Approve + CI 绿勾 → Squash 合并，不直接推 `main`。
4. **大文件不进 Git**：权重、数据集、视频、超大 CAD/加工文件走网盘；小而必须版本化的二进制用 Git LFS。
5. **硬件也版本化**：结构 / PCB / 固件 / 模型各自打 tag，并维护一张各版本能否装在一起的兼容性表。
6. **文档中文、代码标识符英文**；commit 用 `feat:/fix:/docs:/refactor:/test:/chore:`。
7. **默认私有**：公开任何内容（代码、图纸、数据、方案）前先问管理员。

## 加入

1. 联系管理员 [@muzirin](https://github.com/muzirin)，说明方向和能投入的时间
2. 加入对应的 GitHub Team（`model` / `ec` / `mech` 等），获得仓库权限
3. 到仓库的 Issues 领任务，新手优先 `good first issue`
4. 第一次提交前读一遍 [CONTRIBUTING.md](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/CONTRIBUTING.md)

## 组织维护

组织级公共文件（本主页、Issue/PR 模板、协作规范、行为准则）都在
[.github](https://github.com/Kite-Robotics-Special-Interest-Group/.github) 仓库维护。
