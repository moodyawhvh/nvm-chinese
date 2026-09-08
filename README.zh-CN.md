<div align="center">

# nvm 中文文档

[![原项目](https://img.shields.io/badge/原项目-nvm--sh--nvm-blue?style=flat-square&logo=github)](https://github.com/nvm-sh/nvm)
[![英文原版](https://img.shields.io/badge/英文原版-nvm--sh--nvm-blueviolet?style=flat-square)](https://github.com/nvm-sh/nvm)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

**Node Version Manager — 符合 POSIX 标准的 bash 脚本,用于管理多个 Node.js 版本(当前版本 v0.40.7)**

> 本文档是 [nvm-sh/nvm](https://github.com/nvm-sh/nvm) 官方 README 的中文翻译,采用"章节标题 + 导语 + 使用说明 + 代表性命令"的精翻方式,内容以英文原版为准。
> 代部署 / 定制服务 / 技术咨询 请添加微信:uaycar

</div>

---

## 目录

- [简介](#简介)
- [安装与更新](#安装与更新)
- [验证安装](#验证安装)
- [重要说明](#重要说明)
- [使用说明](#使用说明)
- [长期支持版(LTS)](#长期支持版lts)
- [安装时迁移全局包](#安装时迁移全局包)
- [.nvmrc 与默认版本](#nvmrc-与默认版本)
- [使用镜像源(国内加速)](#使用镜像源国内加速)
- [常见问题排查](#常见问题排查)
- [版权声明](#版权声明)

## 简介

`nvm` 让你可以通过命令行快速安装并使用不同版本的 [Node.js](https://nodejs.org)。

```sh
$ nvm install 24
Now using node v24.14.0 (npm v11.9.0)
$ nvm use 22
Now using node v22.22.1 (npm v10.9.4)
```

就这么简单!nvm 是按用户安装、按 shell 会话加载的版本管理器,可运行于任何符合 POSIX 标准的 shell(sh、dash、ksh、zsh、bash),典型平台包括各类 Unix、macOS 和 Windows WSL。

## 安装与更新

运行安装脚本即可**安装**或**更新** nvm:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
```

或:

```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
```

脚本会把 nvm 仓库克隆到 `~/.nvm`,并尝试把下面这段初始化代码追加到正确的 profile 文件(`~/.bashrc`、`~/.bash_profile`、`~/.zshrc` 或 `~/.profile`);如果写错了文件,可设置 `$PROFILE` 环境变量后重跑:

```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

补充说明:

- 若存在 `$XDG_CONFIG_HOME`,nvm 文件会安装到该目录下。
- 在安装命令后加 `--no-use`,可先只加载 nvm 而不自动启用默认版本。
- 可用 `NVM_SOURCE`、`NVM_DIR`、`PROFILE`、`NODE_VERSION` 自定义安装来源、目录、profile 与版本;`NVM_DIR` 末尾不要带斜杠。
- 安装器会自动选用 `git`、`curl` 或 `wget` 之一。
- 设置 `PROFILE=/dev/null` 可让安装器不改动 shell 配置(例如已通过 zsh 插件加载 nvm 时)。
- 也支持 Git 方式安装(`git clone https://github.com/nvm-sh/nvm.git ~/.nvm` 后 checkout 最新 tag 并 source `nvm.sh`,要求 git ≥ 1.7.10)和完全手动安装,详见原 README。
- Docker 中安装:非交互 bash 不会加载 profile,可通过 `BASH_ENV` 指定被 source 的脚本,CI/CD 场景示例见原 README。

## 验证安装

```sh
command -v nvm
```

输出 `nvm` 即安装成功。注意 `which nvm` 无效——nvm 是一个被 source 进 shell 的函数,不是可执行文件。Linux 下若提示 `nvm: command not found`,重开一个终端或 `source ~/.bashrc` / `source ~/.zshrc` 即可。

## 重要说明

- 源码编译安装 Node 需要 C++ 编译环境(macOS 用 Xcode,Debian/Ubuntu 装 `build-essential` 和 `libssl-dev`)。
- Windows:可通过 WSL 使用 nvm,Git Bash(MSYS)或 Cygwin 下通常也能工作;原生替代品有 [nvm-windows](https://github.com/coreybutler/nvm-windows)、[nodist](https://github.com/marcelklehr/nodist)、[nvs](https://github.com/jasongin/nvs)(均非本项目开发)。
- 不支持 Fish shell(社区有 bass、nvm.fish 等替代方案);**不支持 Homebrew 安装方式**。
- macOS 上用 nvm 后全局安装包不再需要 `sudo`;若存在 `~/.npmrc`,确保其中没有 `prefix` 配置。
- Apple Silicon(M 系芯片):Node 从 v16.0.0 起提供 arm64 包,v14.17.0 起支持源码编译 arm64,过旧版本可能安装失败。

## 使用说明

```sh
nvm install node          # "node" 是最新版本的别名
nvm install 14.7.0        # 安装指定版本
nvm alias my_alias v14.4.0 # 自定义别名(不能含空格和斜杠)
```

第一个安装的版本会成为默认版本。其他常用命令:

```sh
nvm ls-remote             # 列出可安装的远程版本
nvm use node              # 切换到某版本(当前 shell 生效)
nvm run node --version    # 以指定版本直接运行 node
nvm exec 4.2 node --version # 在子 shell 中以指定版本执行任意命令
nvm which 12.22           # 显示指定版本 node 可执行文件路径
nvm ls                    # 列出本地已安装版本
nvm deactivate            # 取消 nvm 的 PATH 影响
nvm uninstall <版本>      # 卸载某版本
```

在 `nvm install` / `nvm use` / `nvm run` / `nvm exec` / `nvm which` 等命令中,除了 `14.7`、`12.22.1` 这类版本号,还可以用这些特殊别名:

- `node`:最新版 Node.js;`iojs`:最新版 io.js
- `system`:系统安装的 Node(`nvm use system`)
- `current`:当前 shell 正在使用的版本(不受 `.nvmrc` 影响)
- `stable` / `unstable`:历史遗留别名,现已无实际区分意义

## 长期支持版(LTS)

Node 官方有 LTS 计划。别名和 `.nvmrc` 中可用 `lts/*` 表示最新 LTS 线,`lts/argon` 表示 "argon" 线。以下命令均支持 LTS 参数:

```sh
nvm install --lts          # 安装最新 LTS
nvm use --lts=argon        # 使用指定 LTS 线
nvm ls-remote --lts        # 列出远程 LTS 版本
nvm install --reinstall-packages-from=current 'lts/*'  # 升级到最新 LTS 并迁移全局包
```

本地 nvm 连接 nodejs.org 时会自动维护 LTS 别名文件(`$NVM_DIR/alias/lts`),请勿手工修改。

## 安装时迁移全局包

安装新版本并把旧版本的全局 npm 包迁移过去:

```sh
nvm install --reinstall-packages-from=node node
```

也可以从指定版本迁移:`nvm install --reinstall-packages-from=5 6`。迁移包**不会**顺带升级 npm;需要升级 npm 时加 `--latest-npm` 标志,或随时执行 `nvm install-latest-npm`。已安装版本之间迁移包可单独执行:

```sh
nvm use 22.22.2
nvm reinstall-packages 22.20.0
```

另外:新版本安装时可用 `--offline` 完全离线安装(使用本地缓存);把常用全局包一行一个写进 `$NVM_DIR/default-packages`,以后每次安装新版本都会自动装上。

## .nvmrc 与默认版本

在项目根目录创建 `.nvmrc`,内容写版本号(如 `20.11.0` 或 `lts/*`),然后:

```sh
nvm use        # 读取 .nvmrc 并切换
nvm install    # 读取 .nvmrc 并安装
```

给新终端设置默认版本:

```sh
nvm alias default node   # 最新已安装版本
nvm alias default 18     # 最新已安装的 18.x
```

还可以在 shell 配置中添加钩子,进入含 `.nvmrc` 的目录时自动执行 `nvm use`,bash/zsh/fish 的写法见原 README 的 "Deeper Shell Integration" 一节。

## 使用镜像源(国内加速)

通过环境变量把下载源指向镜像站:

```sh
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
nvm install node
```

io.js 对应 `NVM_IOJS_ORG_MIRROR`;私有镜像可用 `NVM_AUTH_HEADER` 传递 Authorization 头。设置 `NVM_SYMLINK_CURRENT=true` 可让 `nvm use` 维护 `current` 符号链接(多标签页同时用可能产生竞态)。其他环境变量(`NVM_COLORS`、`NVM_RC_VERSION` 等)见原 README 的 Environment variables 一节。

## 常见问题排查

- **`command -v nvm` 没反应**:配置文件没被加载——重开终端,或 `source ~/.bashrc` / `source ~/.zshrc` / `. ~/.profile`。
- **macOS 提示找不到 git / 安装失败**:先安装 Xcode Command Line Tools;zsh 用户若没有 `~/.zshrc`,先 `touch ~/.zshrc` 再重跑安装脚本。
- **下载慢或失败**:配置 `NVM_NODEJS_ORG_MIRROR` 镜像源。
- **"npm does not support Node.js"**:按原 README 建议回退到旧版本、卸载问题版本,再带 `--latest-npm` 重新安装。
- **Alpine Linux**:需额外安装 bash 依赖包,3.13+ 与 3.5–3.12 的处理方式见原 README。

更多内容(bash 补全、自定义颜色、CI 用法、Docker 开发环境、卸载方法等)请阅读英文原版 README。

---

## 版权声明

<div align="center">

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

本项目为 [nvm-sh/nvm](https://github.com/nvm-sh/nvm) 的中文翻译文档,原项目代码与原始文档的版权归其作者所有,遵循原项目 License 章节所列许可证。

**如果觉得有用,请给原项目点个 Star!** ⭐

</div>
