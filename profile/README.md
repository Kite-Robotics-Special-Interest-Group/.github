<div align="center">

# Kite Robotics SIG

**端侧轻量级 VLA（Vision-Language-Action）模型与运行时**

让多模态机器人策略模型真正跑在资源受限的边缘设备上。

</div>

---

## 我们在做什么

| 方向 | 内容 |
| --- | --- |
| 模型端侧化 | X-VLA 单本体固化（soft prompt 融合）→ fp16 → 固定形状 ONNX |
| 芯片部署 | 后摩漫界 M50：HMQuantool 量化 → TCIM 编译 `.hmm` → `tcim_lite` 推理（实测端到端 **236 ms / 4.2 Hz**） |
| 语音交互 | 板端中文 ASR：**Qwen3-ASR + llama.cpp**（CPU），量化/线程调优结论已记录 |
| 工程化 | 容器化开发环境、可复现测试与 CI、面向新手的 Git / 环境 / 部署教程 |

## 仓库

| 仓库 | 说明 |
| --- | --- |
| [Kite](https://github.com/Kite-Robotics-Special-Interest-Group/Kite) | 主仓库：模型、运行时、脚本、文档（**私有**，需管理员邀请） |

## 快速开始

```bash
# 1) 获取代码（私有仓库：先让管理员邀请你的 GitHub 账号）
git clone https://github.com/Kite-Robotics-Special-Interest-Group/Kite.git
cd Kite

# 2) 容器内跑通（推荐：依赖与路径固定，换设备一致）
docker build -t kite-dev:latest .
docker run --rm -v "$PWD:/kite" kite-dev:latest python -m pytest -q
```

新手请直接从 **[教程：从零到板端跑通](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/tutorial.md)** 开始。

## 关键文档

- 教程（推荐入口）：[docs/tutorial.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/tutorial.md)
- 环境搭建：[docs/setup.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/setup.md)、[容器化 docs/container.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/container.md)
- 运行时：量化/编译 [docs/runtime-onnx.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/runtime-onnx.md)、板端部署 [docs/runtime-m50-deploy.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/runtime-m50-deploy.md)、ASR [docs/runtime-asr-qwen3.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/runtime-asr-qwen3.md)
- 平台事实：[HardwareInfo.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/HardwareInfo.md)
- 协作规范：[CONTRIBUTING.md](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/CONTRIBUTING.md)、[Git 教程](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/git-guide.md)

## 目标平台

- **主机**：飞腾腾锐 D3000M（aarch64，8 核 FTC862，LPDDR5，12 GB）
- **加速卡**：后摩漫界 M50（存算一体，160 TOPS INT8，PCIe Gen4 ×4，12–48 GB）
- **软件栈**：Ubuntu 22.04 / Kylin V11、后摩驱动 V1.5.0 + Runtime SDK、llama.cpp、ONNX Runtime

## 参与方式

- 新手先读教程与协作流程，再从 Issue 里领任务（优先 `good first issue`）
- 一切改动走「功能分支 → PR → 至少 1 人 Approve + CI 绿勾 → Squash 合并」，不要直接推 `main`
- 大文件（权重、数据集、`.hmm`）走网盘，不进 Git
- 组织管理员：[@muzirin](https://github.com/muzirin)

## 致谢

- [X-VLA](https://huggingface.co/2toINF/X-VLA-0.9B-WidowX)（Apache-2.0，上游开源模型）
- [Qwen3-ASR](https://huggingface.co/ggml-org/Qwen3-ASR-0.6B-GGUF)（MIT，阿里 Qwen）
- 后摩智能（M50 工具链与 Runtime SDK，由供方提供）
