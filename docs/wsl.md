# WSL 入门：在 Windows 上使用 Linux

> **面向**：没接触过 Linux 的成员（Windows 用户）。
> **目标**：装好 WSL、会在 Windows ↔ Linux 之间切换、知道项目文件放哪、能跑 Python 与脚本。
> **什么时候需要**：工具只支持 Linux（某些导出/编译脚本）、或想和板端（Linux）保持一致的命令。
> **和 Docker 的关系**：Windows 上的 Docker Desktop **依赖 WSL2**——用 Docker 就要装 WSL2（但不必进 Ubuntu 手动操作）。
> 本文讲的是**第二种用法**：直接进入 Ubuntu 装环境、跑脚本（工具只支持 Linux 或要与板端命令一致时）。

---

## 0. 先理解三件事

1. **WSL = Windows 里的一个完整 Linux**（默认 Ubuntu），有独立的软件包管理（`apt`）和文件系统
2. **两套文件系统**：Windows 的 `C:`/`D:` 在 WSL 里是 `/mnt/c`、`/mnt/d`；WSL 自己的家目录是 `~`（`/home/你的用户名`）
3. **命令不一样**：下面是 Windows 用户最常混淆的对照

| 想做什么 | Windows（PowerShell） | WSL / Linux |
| --- | --- | --- |
| 列文件 | `dir` | `ls -l` |
| 进目录 | `cd D:\工作` | `cd /mnt/d/工作` |
| 看文件内容 | `type a.txt` | `cat a.txt` |
| 复制 | `copy a b` | `cp a b` |
| 删除 | `del a` | `rm a` |
| 查命令所在位置 | `where python` | `which python3` |
| 加可执行权限 | （无此概念） | `chmod +x run.sh` |
| 安装软件 | `winget` / 安装包 | `sudo apt install 包名` |
| 以管理员运行 | 右键"以管理员身份" | 命令前加 `sudo` |

> WSL 里**没有** `C:\` 这种路径；盘符写成 `/mnt/c/`，路径分隔符是 `/`。

---

## 1. 安装（约 10 分钟）

在 **Windows PowerShell（管理员）** 里：

```powershell
wsl --install                 # 安装 WSL2 + 默认 Ubuntu（Win10 2004+ / Win11）
wsl --install -d Ubuntu       # 如果只想要 Ubuntu
wsl --version                 # 显示版本
wsl -l -v                     # 发行版列表：VERSION 应为 2，STATE 为 Running/Stopped
```

首次启动 Ubuntu 会让你设置 **Linux 用户名和密码**（与 Windows 账号无关，**记牢**，`sudo` 要用）。

如果 `wsl --install` 失败：在"启用或关闭 Windows 功能"里勾选 **适用于 Linux 的 Windows 子系统** 与 **虚拟机平台**，重启后再试；仍失败用 `wsl --update`。

常用管理命令（在 PowerShell 里执行）：

```powershell
wsl                          # 进入默认发行版
wsl -d Ubuntu                # 进入指定发行版
wsl --shutdown               # 全部停止（改配置 / 搬磁盘前先执行）
wsl --manage Ubuntu --move E:\WSL\Ubuntu    # 把发行版搬到别的盘（C 盘紧张时）
```

---

## 2. 打开方式与互相访问

| 场景 | 做法 |
| --- | --- |
| 在 WSL 里进入项目目录 | `cd /mnt/d/JLU_tools/Kite` |
| 在 WSL 里进入 Windows 桌面 | `cd /mnt/c/Users/<你的用户名>/Desktop` |
| 在资源管理器里打开 WSL 家目录 | 地址栏输入 `\\wsl.localhost\Ubuntu\home\<用户名>` |
| 用 VS Code 编辑 WSL 里的文件 | 在 WSL 终端里进目录后执行 `code .`（需装 Remote - WSL 扩展） |
| 在 Windows 里跑一条 Linux 命令 | `wsl -- ls -l` |
| 在 WSL 里跑 Windows 程序 | `explorer.exe .`（打开当前目录） |

---

## 3. 常用命令（够用清单）

```bash
pwd                          # 当前目录
ls -lh                       # 列文件（含大小）
cd ..                        # 上一级
mkdir -p a/b/c               # 建多层目录
cp -r src dst                # 复制目录
mv a b                       # 移动/改名
rm -rf build                 # 删目录（慎用）
cat file | head -n 20        # 看前 20 行
grep -n "关键字" file         # 搜索
chmod +x run.sh && ./run.sh  # 赋权并执行
sudo apt update              # 更新软件源
df -h; free -h; nproc        # 磁盘 / 内存 / CPU 核数
ps aux | grep python         # 看进程
kill <PID>                   # 结束进程
```

- `Ctrl+C`：中断当前命令；`Ctrl+D`：退出
- 路径里**不要**用反斜杠；空格用引号包起来：`"/mnt/d/My Folder"`

---

## 4. Python 环境（最容易卡住的地方）

新装的 Ubuntu 常常**没有 pip 和 venv**，直接 `python3 -m venv .venv` 会报 `ensurepip is not available`：

```bash
sudo apt update
sudo apt install -y python3-pip python3-venv git
python3 -V                     # 建议 ≥3.10
python3 -m venv .venv
source .venv/bin/activate       # 激活（命令行前出现 (.venv)）
pip install -r requirements.txt
deactivate                      # 退出虚拟环境
```

**没有 sudo 权限时**，用 `uv`（单文件、不需要 root）：

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"
uv venv --python 3.11 ~/.venvs/kite
source ~/.venvs/kite/bin/activate
uv pip install -r requirements.txt
```

> 我们的导出脚本（`scripts/export_onnx_fp16.py`）需要 `torch + transformers==4.51.3`，在 WSL 里按上面建好环境即可。

---

## 5. Git 与文件：几个必须知道的点

1. **仓库放哪**：放 Windows 盘（`/mnt/d/...`）方便用 Windows 编辑器；放 WSL 家目录（`~/`）读写更快。
   大量文件读写（编译、训练）建议放 `~/`，然后 `git clone` 一份到 WSL 里。
2. **行尾（CRLF/LF）**：仓库根有 `.gitattributes`（`*.sh eol=lf`）。**不要用记事本**改脚本；
   编辑统一 UTF-8（VS Code 右下角可切换）。如果脚本报 `\r: command not found`：
   ```bash
   sed -i 's/\r$//' 脚本.sh
   ```
3. **执行权限**：`/mnt` 下的文件权限由 Windows 接管，`chmod +x` 可能不生效 →
   需要执行的脚本放到 WSL 家目录，或直接 `bash 脚本.sh`。
4. **身份与登录**：在 WSL 里第一次 `git push` 可能要求登录 →
   推荐 `gh auth login`（浏览器授权），或使用 Personal Access Token。
   ```bash
   git config --global user.name "你的名字"
   git config --global user.email "你的GitHub邮箱"
   ```

---

## 6. 网络（国内常见问题）

- WSL **默认不能**直接使用 Windows 的 `127.0.0.1:7897` 代理（那是 Windows 的本地回环）。
  两个办法：
  1. **用 Windows 宿主 IP**：`cat /etc/resolv.conf | grep nameserver` 拿到 IP，然后
     ```bash
     export HTTPS_PROXY=http://<那个IP>:7897
     export HTTP_PROXY=$HTTPS_PROXY
     ```
  2. **Win11 镜像网络**：在 `C:\Users\<你>\.wslconfig` 写
     ```ini
     [wsl2]
     networkingMode=mirrored
     ```
     然后 `wsl --shutdown` 再进，`127.0.0.1` 就能用了
- pip 慢：用国内镜像（见各仓库 `docs/setup.md`）
- DNS 异常：`sudo sh -c 'echo "nameserver 223.5.5.5" > /etc/resolv.conf'`

---

## 7. Docker 与 WSL

- 装了 Docker Desktop 后，到 **Settings → Resources → WSL Integration** 勾选你的发行版，
  之后 WSL 里就能直接 `docker`（否则会提示 `docker: command not found`）
- Docker 数据盘默认在 `C:`，**不要放到外接盘**（掉线会让 Docker 引擎起不来）；
  空间紧张时在 **Settings → Resources → Advanced → Disk image location** 改位置

---

## 8. 踩坑清单（我们实际遇到过的）

| 现象 | 原因 / 处理 |
| --- | --- |
| `wsl --install` 失败或卡住 | 未开启虚拟化 / 网络问题：`wsl --update`，检查 BIOS 虚拟化 |
| `python3 -m venv` 报 `ensurepip is not available` | 没装 `python3-venv`（见 §4） |
| `pip: command not found` | 没装 `python3-pip`，或用 `python3 -m pip` |
| `docker: command not found`（WSL 里） | Docker Desktop 未开 WSL Integration（§7） |
| 代理不生效 | 用了 `127.0.0.1`（§6） |
| `\r: command not found` | 文件是 CRLF 行尾（§5） |
| C 盘越来越满 | WSL 虚拟磁盘会增长：`wsl --manage Ubuntu --move <盘>`（先 `wsl --shutdown`） |
| 在 Windows 里找不到 WSL 文件 | 用 `\\wsl.localhost\Ubuntu\...`（§2） |
| 提示 `请勿在 /mnt 下编译`（构建很慢） | 把工程复制到 `~/` 再编译 |

---

## 9. 一分钟自检

```bash
uname -a                         # 应显示 Linux ... aarch64/x86_64（WSL2）
nproc && free -h && df -h ~      # CPU / 内存 / 家目录空间
python3 -V && python3 -m pip -V  # Python 与 pip
git --version
ls /mnt/c > /dev/null && echo "能访问 Windows C 盘"
```

四项都正常，就可以按仓库文档继续了。遇到问题先看 §8，再在 Issue 里贴**完整报错**。
