<div align="center">

# nvm 中文翻译版

**[中文版] nvm — Node 版本管理器:符合 POSIX 标准的 bash 脚本,一条命令安装、切换、管理多个 Node.js 版本**

[![原项目](https://img.shields.io/badge/原项目-nvm--sh--nvm-blue?style=flat-square&logo=github)](https://github.com/nvm-sh/nvm)
[![中文文档](https://img.shields.io/badge/中文文档-README.zh--CN.md-orange?style=flat-square)](README.zh-CN.md)
[![GitHub Stars](https://img.shields.io/github/stars/nvm-sh/nvm?style=flat-square&label=原项目Stars)](https://github.com/nvm-sh/nvm/stargazers)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

> 这是 [nvm-sh/nvm](https://github.com/nvm-sh/nvm) 的中文翻译版本。
> 完整源代码请访问原项目:https://github.com/nvm-sh/nvm

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 📖 项目简介

nvm(Node Version Manager)是一个符合 POSIX 标准的 bash 脚本工具,用于在一台机器上管理多个 Node.js 版本。它按用户安装、按 shell 会话加载,让你可以在不同项目间用一条命令自由切换 Node 版本,互不干扰。nvm 面向 Unix、macOS 以及 Windows WSL 等 POSIX 兼容环境;原生 Windows 用户可参考 nvm-windows 等替代方案。

## ✨ 主要特性

- 一条命令安装任意 Node.js 版本:`nvm install node` 装最新版,`nvm install 20` 装 20.x 最新,`nvm install 14.7.0` 装精确版本
- `nvm use <版本>` 在多个版本间即时切换,当前 shell 立即生效
- 支持 `.nvmrc`:项目根目录放一个版本号文件,`nvm use` / `nvm install` 自动读取
- `nvm alias default` 设置默认版本,新开的终端自动使用
- 完整支持 LTS:`nvm install --lts`、`lts/*`、`lts/argon` 等写法
- 升级时迁移全局包:`nvm install --reinstall-packages-from=<旧版本>`,可同时加 `--latest-npm`
- `nvm run` / `nvm exec` / `nvm which`:以指定版本运行脚本、执行命令或定位可执行文件路径
- 支持 `system` 别名,随时切回系统自带的 Node;`nvm deactivate` 恢复原 PATH
- 支持 Node/io.js 二进制镜像源(`NVM_NODEJS_ORG_MIRROR` 等),还支持 `--offline` 离线安装
- 自带 bash 补全、自定义配色(`nvm set-colors`)

## 📁 文件说明

| 文件 | 说明 |
|:-----|:-----|
| README.md | 本文件(中文简介) |
| README.zh-CN.md | 详细中文文档(章节标题、安装/使用说明与代表性命令的完整汉化) |

## 🚀 快速开始

1. 用安装脚本安装 nvm(当前版本 v0.40.7,以原项目 README 为准):

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
```

或使用 wget:

```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
```

2. 脚本会把 nvm 克隆到 `~/.nvm`,并把初始化代码追加到你的 shell 配置文件(`~/.bashrc`、`~/.bash_profile`、`~/.zshrc` 或 `~/.profile`)。
3. 关闭并重新打开终端,或手动执行 `source ~/.bashrc` 让配置生效。
4. 验证安装:`command -v nvm`,输出 `nvm` 即成功(注意:`which nvm` 不适用,因为 nvm 是 shell 函数而非可执行文件)。
5. 安装并使用最新 LTS 版:

```sh
nvm install --lts
nvm use --lts
```

6. 设置默认版本:`nvm alias default node`。
7. 日常使用:在项目根目录写一个 `.nvmrc`(内容为版本号),进入项目后执行 `nvm use` 即自动切换。

国内下载慢可配置镜像:`export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/`。需要代部署或定制环境,请加微信 uaycar。

完整源代码与最新版本请访问原项目:https://github.com/nvm-sh/nvm

## 📞 联系方式

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

本项目为 [nvm-sh/nvm](https://github.com/nvm-sh/nvm) 的中文翻译版本,所有代码版权归原项目作者所有,遵循其原始许可证。

**如果觉得有用,请给原项目点个 Star!** ⭐
