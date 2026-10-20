# 零基础入门：GitHub 与团队协作

> **面向**：第一次用 GitHub 的成员（电控、机械、算法都适用）。
> **目标**：看完能独立完成「拿到仓库 → 改一个文件 → 提交 → 发 PR → 被合并」。
> **用时**：约 1–2 小时，边看边做。

---

## 0. 先建立三个概念

把 GitHub 想成**一个共享文件夹 + 一本完整的修改历史**：

| 概念 | 通俗解释 | 对应操作 |
| --- | --- | --- |
| 仓库（repo） | 项目的共享文件夹，所有人看到的都是同一份 | 克隆 / 拉取（pull） |
| 提交（commit） | 一次「存档」，**只在你自己的电脑上** | `git add` + `git commit` |
| 推送（push） | 把存档上传到 GitHub，别人才能看到 | `git push` |
| 分支（branch） | 从主线分出来的平行副本；改坏了不影响主线 | `git checkout -b ...` |
| 拉取请求（PR） | 申请「把我的分支合并进 `main`」，顺便请人检查 | 在网页上点几下 |
| Issue | 任务/缺陷的记录卡片，有编号（如 #12） | 网页上新建 |

团队铁律：**不直接改 `main`**。所有改动都走「分支 → PR → 别人看过 → 合并」。

---

## 1. 账号与权限（10 分钟）

1. 注册 GitHub：<https://github.com/signup>（建议用常用邮箱）
2. 把 **用户名** 发给管理员（[@muzirin](https://github.com/muzirin)），说明你的方向（电控 / 机械 / 模型 / 感知…）
3. 收到邀请后，在 <https://github.com/notifications> 或邮件里点 **Accept**
4. 验证：打开 <https://github.com/Kite-Robotics-Special-Interest-Group> 能看到组织
5. **开启两步验证**（2FA）：头像 → Settings → Password and authentication → Enable 2FA（组织通常强制）

> 看不到仓库 = 还没被邀请或没接受邀请。这不是你电脑的问题。

---

## 2. 安装工具（Windows 为例）

| 工具 | 必需性 | 用途 |
| --- | --- | --- |
| **Git** | 必需 | 版本管理（会自带 Git Credential Manager，登录用浏览器） |
| **GitHub Desktop** | 可选（强烈建议新手） | 图形界面：克隆、提交、推送、建分支都能点 |
| **VS Code** | 可选 | 看代码、改文档、看 diff |
| Python / Docker | 按方向 | 模型/推理方向需要；电控/机械一般不需要 |
| **WSL（可选）** | 需要用 Linux 工具时 | 在 Windows 里跑 Linux：安装、文件位置、代理与报错见 [WSL 入门](wsl.md) |

安装后验证（开始菜单搜索 `PowerShell` 或 `Git Bash`）：

```bash
git --version
git config --global user.name  "你的名字"
git config --global user.email "你的GitHub邮箱"
```

> 邮箱必须和 GitHub 账号一致，否则提交记录里认不出你。

**国内网络**：访问 GitHub 不稳时，只给 github.com 配代理（不影响其他网站）：

```bash
git config --global http.https://github.com.proxy http://127.0.0.1:7897
# 代理端口按你自己软件的实际端口改；不需要时用 --unset 删除
```

---

## 3. 拿到仓库

**方式 A（推荐）：克隆**

```bash
cd D:\你的工作目录
git clone https://github.com/Kite-Robotics-Special-Interest-Group/Kite.git
cd Kite
```

私有仓库第一次会弹窗要求登录：**点浏览器授权**（Git Credential Manager），不要输入账号密码。
GitHub Desktop 也一样：File → Clone repository → 选组织 → 选仓库。

**方式 B（不推荐）：网页 Download ZIP** —— 只能看，不能提交，也不要在这个目录里改东西。

---

## 4. 第一次修改：从网页开始（最简单，5 分钟）

适合改文档、改错别字，**不用装任何东西**：

1. 打开要改的文件（例如仓库里的 `README.md`）
2. 右上角铅笔图标 ✏️ → 直接改
3. 页面底部 **Commit changes**
4. 选择 **Create a new branch for this commit**，分支名例如 `docs/fix-typo`
5. 点 **Propose changes** → 进入 PR 页面 → 填标题/描述 → **Create pull request**

---

## 5. 本地提交：记住「黄金五步」

在仓库目录里打开终端（VS Code 里 `Ctrl+~`）：

```bash
git status                       # ① 看改了什么（随时可用）
git add 文件或.                   # ② 把改动放进「待提交」
git commit -m "docs: 修正接线说明（#12）"   # ③ 存档
git pull origin main             # ④ 先同步别人的改动（在 main 上时）
git push                         # ⑤ 上传（在自己的分支上）
```

新手最容易忘的两件事：

- 改完文件**没保存**就提交（编辑器标题上还有圆点）
- `add` 之后又改了文件 → 再 `add` 一次

---

## 6. 标准协作流程（照做即可）

```bash
# 1) 先同步最新 main
git checkout main
git pull origin main

# 2) 开自己的分支（名字：类型/编号-简述）
git checkout -b feat/12-增加急停按钮

# 3) 改文件 → 提交（可以多次）
git add .
git commit -m "feat: 增加急停按钮（#12）"

# 4) 推送
git push -u origin feat/12-增加急停按钮
```

然后在 GitHub 页面：

5. 点 **Compare & pull request** → 确认 `base: main` ← `compare: 你的分支`
6. 填描述（模板会自动出现），写 `Closes #12`
7. 右侧 **Reviewers** 选至少 1 位同事 → **Create pull request**
8. 评审通过、CI 绿勾后，点 **Squash and merge** → **Delete branch**
9. 回到本地清理：

```bash
git checkout main && git pull
git branch -d feat/12-增加急停按钮
```

### 评审时怎么配合

- 评审人可能在具体某一行留评论 → 你在**同一分支**继续改、继续 `push`，PR 会自动更新
- 讨论完点 **Resolve conversation**
- 不要因为「被提意见」就重开一个 PR；在原 PR 上迭代

---

## 7. Issue：任务从哪来

- **新建**：仓库 → Issues → New issue → 选模板（任务 / 缺陷）→ 标题格式 `[方向] 动词 + 对象`（例：`[ec] 电机驱动加入过流保护`）
- **领取**：右侧 **Assignees** 选自己，并在评论里写「我来做，预计 X 天」
- **关闭**：在 PR 描述里写 `Closes #12`，合并时自动关闭

---

## 8. 非代码文件（电控 / 机械必读）

| 内容 | 放哪 | 命名示例 |
| --- | --- | --- |
| 固件源码、配置文件、脚本 | Git 仓库 | `firmware/motor_ctrl.c` |
| 原理图 / PCB 源文件（可 diff 的格式） | Git 仓库 | `hw/mainboard-v1.2-20261020.kicad_pcb` |
| 装配图、加工图（PDF/DWG，体积不大） | Git 仓库或网盘 | `mech/arm-bracket-v1.0.pdf` |
| 大 STEP/STL、三维总装、视频、数据集 | **网盘**，仓库里只放说明 + 校验值 | — |
| 编译产物、临时导出、备份文件 | **不要提交**（`.gitignore`） | — |

**硬件也要版本化**：文件名或 `CHANGELOG` 里写清版本与日期；发布时打 tag（如 `hw-v1.2`）。
同一阶段的结构/电控/固件版本，在仓库 `docs/` 里维护一张**兼容性表**（谁和谁能装在一起）。

---

## 9. 常见错误速查

| 现象 | 原因 / 解决 |
| --- | --- |
| `repository not found` / `Authentication failed` | 没被邀请、没接受邀请，或没登录 → 见第 1、3 节 |
| `Permission denied (publickey)` | 用了 SSH 但没配密钥 → 改用 HTTPS 地址克隆 |
| `rejected ... fetch first` | 远程有你没有的提交 → 先 `git pull --rebase origin main` 再推 |
| PR 显示冲突（This branch has conflicts） | 本地 `git pull origin main` 解冲突后 push；冲突文件里删掉 `<<<<<<<` 标记 |
| `Remote: File too large` | 文件超 100 MB → 别提交，走网盘；小二进制用 Git LFS |
| 提交信息全是乱码 | 文件用 **UTF-8** 保存（记事本：另存为 → 编码 UTF-8） |
| `LF will be replaced by CRLF` 警告 | Windows 换行符提示，**可以忽略** |
| `fatal: not a git repository` | 终端不在仓库目录里 → `cd` 到仓库根目录 |
| 改了想撤销（未提交） | `git restore 文件名` |
| 提交到了错误分支 | `git reset --soft HEAD~1` 撤销提交（改动保留），切到正确分支再提交 |
| 不小心删了文件 | `git restore 文件名` 恢复 |

---

## 10. 习惯与求助

**每天**：开工先 `git pull`；收工前把做完的部分 `commit + push`（哪怕没做完，写清 WIP）。
**每次提交**只做一件事，信息写清「做了什么、为什么」。

**卡住超过 15 分钟就提问**，提问时带上：

1. 你想做什么
2. 完整的报错原文（截图或复制文本）
3. 已经试过什么

求助顺序：仓库 README/文档 → 相关 Issue → 群里 @ 对应方向负责人 → 管理员。

---

## 附：命令速查卡

```bash
git status                      # 看状态
git diff                        # 看未 add 的改动
git add 文件                    # 暂存
git commit -m "类型: 描述"       # 提交
git pull origin main            # 同步
git push                        # 推送
git log --oneline -10           # 最近提交
git checkout -b 分支名           # 新建并切换分支
git checkout main               # 切回主分支
git branch -d 分支名             # 删除已合并分支
git restore 文件                 # 撤销未提交修改
```

延伸阅读：组织级 [贡献规范](https://github.com/Kite-Robotics-Special-Interest-Group/.github/blob/main/CONTRIBUTING.md)、
模型组 [从零教程](https://github.com/Kite-Robotics-Special-Interest-Group/Kite/blob/main/docs/tutorial.md)。
