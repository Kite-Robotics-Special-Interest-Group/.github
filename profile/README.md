<div align="center">

# Kite Robotics Special Interest Group

**机器人系统的跨学科协作组织**

模型与推理 · 电控 · 机械 · 感知与数据 · 平台工具 —— 从算法到整机。

</div>

---

## 组织定位

本组织把「模型算法 → 电控 → 机械结构 → 整机联调」放在同一套仓库与流程下协作。
组织内会有多个子系统仓库，**不只有模型与推理**；各组在自己的仓库里开发，跨组接口在组织层对齐。

## 子系统与仓库

| 方向 | 典型内容 | 仓库命名建议 | 状态 |
| --- | --- | --- | --- |
| 模型与推理 | VLA 模型、端侧运行时、量化与部署 | `kite`（主仓库）、`*-runtime` | ✅ 进行中 |
| 电控 | 主控固件、电机/传感器驱动、总线与通信协议 | `ec-*` | 待建立 |
| 机械 | 结构设计、CAD、装配与加工图 | `mech-*` | 待建立 |
| 感知与数据 | 相机与标定、数据采集/标注/清洗 | `perception-*`、`data-*` | 待建立 |
| 平台与工具 | 仿真、测试台、CI、公共脚本与文档 | `tools-*`、`docs-*` | 待建立 |

> 新仓库请按上面前缀命名（小写 + 短横线），并在下方「仓库索引」登记一行。

## 仓库索引

| 仓库 | 说明 | 负责方向 | 可见性 |
| --- | --- | --- | --- |
| [Kite](https://github.com/Kite-Robotics-Special-Interest-Group/Kite) | 端侧 VLA 模型与运行时（模型、量化、部署、ASR） | 模型与推理 | 私有 |

## 通用约定（所有仓库适用）

1. **每个仓库必须有 README**：目标、负责人、当前状态、快速开始、相关文档链接。
2. **接口变更要跨组对齐**：机械接口尺寸、电控通信协议、模型输入输出契约——任何变更都要在 PR 里写清影响面，并请对应方向负责人评审。
3. **Issue / PR 走模板**：组织级模板在本仓库提供（各仓库可覆盖）；流程为「分支 → PR → 至少 1 人 Approve + CI 绿勾 → Squash 合并」，不直接推 `main`。
4. **大文件不进 Git**：模型权重、数据集、视频、超大 CAD/加工文件走网盘；小而必须版本化的二进制用 Git LFS。
5. **硬件也版本化**：结构 / PCB / 固件 / 模型各自的发布用 tag + CHANGELOG 记录；同一阶段的三件套（结构版本、电控版本、固件/模型版本）在对应仓库 `docs/` 里维护一张**兼容性表**。
6. **文档中文、代码标识符英文**；commit message 用 `feat:/fix:/docs:/refactor:/test:/chore:`。
7. **默认私有**：公开任何内容（代码、图纸、数据、方案）前先问管理员。

## 快速开始

| 你的方向 | 从这里开始 |
| --- | --- |
| 模型 / 推理 / 部署 | [Kite 教程](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/tutorial.md)（从零到板端跑通） |
| 电控 / 机械 / 感知 | 对应仓库建立后补「新人上手指南」；在此之前先读本页「通用约定」与 [CONTRIBUTING.md](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/CONTRIBUTING.md) |
| 工具 / 环境 | Docker 优先（跨设备一致）；Git 入门见 [Kite 的 Git 教程](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/git-guide.md) |

## 加入与分工

1. 联系管理员 [@muzirin](https://github.com/muzirin)，说明你的方向与可投入时间
2. 加入对应的 GitHub Team（如 `model` / `ec` / `mech`），获得相应仓库权限
3. 到对应仓库的 Issues 领任务（新手优先 `good first issue`）
4. 第一次提交前读一遍 [CONTRIBUTING.md](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/CONTRIBUTING.md)

## 组织维护

- 管理员：[@muzirin](https://github.com/muzirin)
- 组织级公共文件（本主页、Issue/PR 模板、协作规范、行为准则）都在 **[.github](https://github.com/Kite-Robotics-Special-Interest-Group/.github)** 仓库维护
