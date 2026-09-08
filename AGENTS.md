# nvm 编码代理指南

> 🌐 本文档由 [nvm-sh/nvm](https://github.com/nvm-sh/nvm) 翻译,英文原版见原项目。
>
> ℹ️ 注:原文超过 10000 字符,本页为全文翻译;所有命令与代码块保持原样。

本文档为 AI 编码代理在处理 Node 版本管理器(nvm)代码库时提供指导。

## 概述

nvm 是一个 Node.js 版本管理器,实现为一个符合 POSIX 标准的函数,可跨多种 shell(sh、dash、bash、ksh、zsh)工作。代码库以 shell 脚本为主,强调可移植性与兼容性。

### 核心架构

- **主脚本**:`nvm.sh` - 包含全部核心功能与主 `nvm()` 函数
- **安装脚本**:`install.sh` - 负责下载并安装 nvm 自身
- **执行包装器**:`nvm-exec` - 允许以指定的 Node.js 版本运行命令
- **Bash 补全**:`bash_completion` - 为 bash 用户提供 Tab 补全
- **测试**:完整测试套件位于 `test/` 目录,使用 [urchin](https://www.npmjs.com/package/urchin) 测试框架

## 关键文件及其用途

### `nvm.sh`
核心功能文件,包含:
- 主 `nvm()` 函数(约第 3000 行起)
- 所有内部辅助函数(以 `nvm_` 为前缀)
- install、use、ls 等命令的实现
- shell 兼容性逻辑
- POSIX 兼容工具函数

### `install.sh`
通过 curl/wget/git 处理 nvm 安装:
- 从 GitHub 下载 nvm
- 建立目录结构
- 配置 shell 集成
- 同时支持 git clone 与脚本下载两种方式

### `nvm-exec`
简单的包装脚本,作用:
- 以 `--no-use` 标志 source nvm.sh
- 通过 `NODE_VERSION` 环境变量或 `.nvmrc` 切换到指定 Node 版本
- 以该 Node 版本执行所给命令

## 顶层 nvm 命令与内部函数

### 核心命令

#### `nvm install [version]`
- **内部函数**:`nvm_install_binary()`、`nvm_install_source()`、`nvm_download_artifact()`
- 下载并安装指定的 Node.js 版本
- 安装完成后自动 `nvm use` 该版本
- 支持 LTS 版本、版本范围、内置别名(如 `node`、`stable`)以及用户自定义别名
- 可安装二进制包,也可从源码编译
- 从源码编译时,可接受附加参数并传递给编译任务

#### `nvm use [version]`
- **内部函数**:`nvm_resolve_alias()`、`nvm_version_path()`、`nvm_change_path()`
- 将当前 shell 切换到指定的 Node.js 版本
- 更新 PATH 环境变量
- 支持集成 `.nvmrc` 文件

#### `nvm ls [pattern]`
- **内部函数**:`nvm_ls()`、`nvm_tree_contains_path()`
- 列出已安装的 Node.js 版本
- 支持模式匹配与过滤
- 显示当前版本与别名

#### `nvm ls-remote [pattern]`
- **内部函数**:`nvm_ls_remote()`、`nvm_download()`、`nvm_ls_remote_index_tab()`
- 列出 nodejs.org 与 iojs.org(或环境变量配置的镜像)上的可用 Node.js 版本
- 支持 LTS 过滤与模式匹配
- 按需下载版本索引

#### `nvm alias [name] [version]`
- **内部函数**:`nvm_alias()`、`nvm_alias_path()`
- 创建以别名命名、内容为映射版本的文本文件
- 特殊别名:`default`、`node`、`iojs`、`stable`、`unstable`(注意:`stable` 与 `unstable` 已弃用,源自 node v1 之前的发布计划)
- 存放于 `$NVM_DIR/alias/` 目录

#### `nvm current`
- **内部函数**:`nvm_ls_current()`
- 显示当前激活的 Node.js 版本
- 若使用系统 Node.js,则返回 "system"

#### `nvm which [version]`
- **内部函数**:`nvm_version_path()`、`nvm_resolve_alias()`
- 显示指定 Node.js 版本的可执行文件路径
- 解析别名与版本字符串

### 工具命令

#### `nvm cache clear|dir`
- 管理已下载二进制与源码的缓存
- 清空缓存或显示缓存目录路径

#### `nvm debug`
- 用于排查问题的诊断信息
- 显示环境、工具版本与路径

#### `nvm deactivate`
- 移除 nvm 对当前 shell 的修改
- 恢复原始 PATH

#### `nvm unload`
- 将 nvm 从 shell 环境中彻底移除
- 取消设置所有 nvm 函数与变量

### 内部函数分类

#### 版本解析
- `nvm_resolve_alias()` - 将别名解析为版本号
- `nvm_version()` - 查找最匹配的本地版本
- `nvm_remote_version()` - 查找最匹配的远程版本
- `nvm_normalize_version()` - 规范化版本字符串
- `nvm_version_greater()` - 比较版本号大小
- `nvm_version_greater_than_or_equal_to()` - 带相等判断的版本比较
- `nvm_get_latest()` - 从列表中获取最新版本

#### 安装辅助
- `nvm_install_binary()` - 下载并安装预编译二进制包
- `nvm_install_source()` - 从源码编译 Node.js
- `nvm_download_artifact()` - 下载压缩包或二进制包
- `nvm_compute_checksum()` - 校验下载完整性
- `nvm_checksum()` - 校验和验证包装器
- `nvm_get_mirror()` - 获取相应的下载镜像
- `nvm_get_arch()` - 判断系统架构

#### 路径管理
- `nvm_change_path()` - 版本切换时更新 PATH
- `nvm_strip_path()` - 从 PATH 中移除 nvm 路径
- `nvm_version_path()` - 获取某版本的安装路径
- `nvm_version_dir()` - 获取版本目录名
- `nvm_prepend_path()` - 安全地前插 PATH

#### Shell 检测与兼容性
- `nvm_is_zsh()` - 检测 zsh shell
- `nvm_is_iojs_version()` - 判断版本是否为 io.js
- `nvm_get_os()` - 检测操作系统
- `nvm_supports_source_options()` - 检查 shell 是否支持 source 选项

#### 网络与远程操作
- `nvm_download()` - 通用下载函数
- `nvm_ls_remote()` - 列出远程版本
- `nvm_ls_remote_iojs()` - 列出远程 io.js 版本
- `nvm_ls_remote_index_tab()` - 解析远程版本索引

#### 工具函数
- `nvm_echo()`、`nvm_err()` - 输出函数
- `nvm_has()` - 检查命令是否存在
- `nvm_sanitize_path()` - 清理路径中的敏感数据
- `nvm_die_on_prefix()` - 校验 npm prefix 设置
- `nvm_ensure_default_set()` - 确保已设置 default 别名
- `nvm_auto()` - 依据 .nvmrc 自动切换版本

#### 别名管理
- `nvm_alias()` - 创建或列出别名
- `nvm_alias_path()` - 获取别名文件路径
- `nvm_unalias()` - 删除别名
- `nvm_resolve_local_alias()` - 解析本地别名

#### 列举与显示
- `nvm_ls()` - 列出本地版本
- `nvm_ls_current()` - 显示当前版本
- `nvm_tree_contains_path()` - 检查路径是否位于 nvm 树内
- `nvm_format_version()` - 格式化版本显示

## 运行测试

### 测试框架
nvm 使用 [urchin](https://www.npmjs.com/package/urchin) 测试框架进行 shell 脚本测试。

### 测试结构
```
test/
├── fast/           # 快速单元测试
├── slow/           # 集成测试
├── sourcing/       # shell source 测试
├── install_script/ # 安装脚本测试
├── installation_node/ # Node 安装测试
├── installation_iojs/ # io.js 安装测试
└── common.sh       # 共享测试工具
```

### 运行测试

#### 安装依赖
```bash
npm install  # Installs urchin, semver, and replace tools
```

#### 运行全部测试
```bash
npm test               # Runs tests in the current shell only (sh, bash, dash, zsh)
make test              # Runs tests in default shells (sh, bash, dash, zsh)
make test-sh           # Runs tests only in sh
make test-bash         # Runs tests only in bash
make test-dash         # Runs tests only in dash
make test-zsh          # Runs tests only in zsh
make SHELLS=ksh test   # Runs tests only in ksh (experimental, see issue #574)
```

#### 运行指定测试套件
```bash
npm run test/fast                # Runs fast tests in the current shell
npm run test/slow                # Runs slow tests in the current shell
npm run test/sourcing            # Runs sourcing tests in the current shell
npm run test/install_script      # Runs install script tests in the current shell
npm run test/installation        # Runs installation tests (node + iojs) in the current shell
npm run test/installation/node   # Runs Node installation tests in the current shell
npm run test/installation/iojs   # Runs io.js installation tests in the current shell
make TEST_SUITE=fast test        # Only fast tests
make TEST_SUITE=slow test        # Only slow tests
make SHELLS=bash test            # Only bash shell
```

#### 运行单个测试
```bash
./test/fast/Unit\ tests/nvm_get_arch     # Run single test (WARNING: This will exit/terminate your current shell session)
./node_modules/.bin/urchin test/fast/                        # Run fast test suite
./node_modules/.bin/urchin 'test/fast/Unit tests/nvm_get_arch'  # Run single test safely without shell termination
./node_modules/.bin/urchin test/slow/                        # Run slow test suite
./node_modules/.bin/urchin test/sourcing/                    # Run sourcing test suite
./node_modules/.bin/urchin test/install_script/              # Run install script test suite
./node_modules/.bin/urchin test/installation_node/           # Run Node installation test suite
./node_modules/.bin/urchin test/installation_iojs/           # Run io.js installation test suite
```

#### Lint 与文档检查
```bash
npm run eclint               # Checks EditorConfig compliance
npm run doctoc:check         # Verifies README table of contents
npm run dockerfile_lint      # Lints the Dockerfile
npm run test:check-exec      # Checks test files have executable permission
npm run test:check-nonexec   # Checks non-test files don't have executable permission
npm run markdown-link-check  # Validates markdown links (requires markdown-link-check)
```

### 测试编写准则
- 测试应在所有受支持的 shell(sh、bash、dash、zsh、ksh)中可运行
- 定义并使用 `die()` 函数处理测试失败
- 在 cleanup 函数中完成测试后的清理
- 必要时对外部依赖进行 mock
- mock 文件放在 `test/mocks/` 目录
- mock 文件只能通过现有的 `update_test_mocks.sh` 脚本更新,任何新增 mock 都必须加入该脚本

## Shell 环境搭建

### 受支持的 Shell
- **bash** - 完整功能支持
- **zsh** - 完整功能支持
- **dash** - 基础 POSIX 支持
- **sh** - 基础 POSIX 支持
- **ksh** - 有限支持(实验性)

### 安装 Shell 环境

#### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install bash zsh dash ksh
# sh is typically provided by dash or bash and is available by default
```

#### macOS
```bash
# bash and zsh are available by default, bash is not the default shell for new user accounts
# Install other shells via Homebrew
brew install dash ksh
# For actual POSIX sh (not bash), install mksh which provides a true POSIX sh
brew install mksh
```

#### 手动 Shell 测试
```bash
# Test in specific shell
bash -c "source nvm.sh && nvm --version"
zsh -c "source nvm.sh && nvm --version"
dash -c ". nvm.sh && nvm --version"
sh -c ". nvm.sh && nvm --version"          # On macOS: mksh -c ". nvm.sh && nvm --version"
ksh -c ". nvm.sh && nvm --version"
```

### 各 Shell 的注意事项
- **zsh**:需要临时取消设置几乎所有非默认 zsh 选项,以恢复 POSIX 兼容性
- **dash**:功能集有限,避免使用 bash 特有语法
- **ksh**:部分功能可能不可用,主要用于兼容性测试

## CI 环境详情

### GitHub Actions 工作流

#### `.github/workflows/tests.yml`
- 在多个 shell 与多个测试套件上运行测试
- 使用 `script` 命令模拟真实 TTY
- 矩阵策略覆盖 shell × 测试套件组合
- 非 bash shell 排除 install_script 测试

#### `.github/workflows/shellcheck.yml`
- 使用 shellcheck 对所有 shell 脚本进行 lint
- 针对多种 shell 目标测试(bash、sh、dash、ksh)
  - 注意:因 [shellcheck 的限制](https://github.com/koalaman/shellcheck/issues/809),未包含 zsh
- 使用 Homebrew 安装最新版 shellcheck

#### `.github/workflows/lint.yml`
- 运行额外的 lint 与格式检查
- 校验文档与代码风格

### Travis CI(旧版)
- 配置于 `.travis.yml`
- 在多个 Ubuntu 版本上测试
- 通过 apt 包安装 shell 环境

### CI 测试执行
```bash
# Simulate CI environment locally
unset TRAVIS_BUILD_DIR  # Disable Travis-specific logic
unset GITHUB_ACTIONS    # Disable GitHub Actions logic
make test
```

## 本地搭建 shellcheck

### 安装

#### macOS(Homebrew)
```bash
brew install shellcheck
```

#### Ubuntu/Debian
```bash
sudo apt-get install shellcheck
```

#### 从源码/二进制
```bash
# Download from https://github.com/koalaman/shellcheck/releases
wget https://github.com/koalaman/shellcheck/releases/download/latest/shellcheck-latest.linux.x86_64.tar.xz
tar -xf shellcheck-latest.linux.x86_64.tar.xz
sudo cp shellcheck-latest/shellcheck /usr/local/bin/
```

### 用法

#### Lint 主文件
```bash
shellcheck -s bash nvm.sh
shellcheck -s bash install.sh
shellcheck -s bash nvm-exec
shellcheck -s bash bash_completion
```

#### 跨 Shell 类型 Lint
```bash
shellcheck -s sh nvm.sh      # POSIX sh
shellcheck -s bash nvm.sh    # Bash extensions
shellcheck -s dash nvm.sh    # Dash compatibility
shellcheck -s ksh nvm.sh     # Ksh compatibility
```

#### nvm 中常见的 shellcheck 指令
- `# shellcheck disable=SC2039` - 允许在 POSIX 模式下使用 bash 扩展
- `# shellcheck disable=SC2016` - 允许单引号内出现字面 `$`
- `# shellcheck disable=SC2001` - 允许使用 sed 代替参数展开
- `# shellcheck disable=SC3043` - 允许使用 `local` 关键字(bash 扩展)

### 修复 shellcheck 问题
1. **加引号**:始终为变量加引号:用 `"${VAR}"` 而非 `$VAR`
2. **POSIX 兼容**:可移植部分避免使用 bash 特有特性
3. **数组用法**:使用 `set --` 处理位置参数,而非 POSIX 不支持的数组
4. **局部变量**:先用 `local FOO` 声明,再在下一行初始化(后者是为了支持 ksh)

## 开发最佳实践

### 代码风格
- 使用 2 空格缩进
- 遵循 POSIX shell 规范以保证可移植性
- 内部函数以 `nvm_` 为前缀
- 输出使用 `nvm_echo` 而非 `echo`
- 错误信息使用 `nvm_err`

### 兼容性
- 在所有受支持的 shell 中测试改动
- 核心功能避免使用 bash 特有特性
- 需要针对 zsh 的特殊行为时,使用 `nvm_is_zsh` 检测
- 在测试中 mock 外部依赖

### 性能
- 缓存高开销操作(如远程版本列表)
- 使用局部变量避免作用域污染
- 尽量减少子进程调用
- 对可选功能实现懒加载

### 调试
- 使用 `nvm debug` 命令获取环境信息
- 开发期间用 `set -x` 开启详细输出
- 使用 `NVM_DEBUG=1` 环境变量测试
- 检查 `$NVM_DIR/.cache` 排查缓存数据问题

## 常见陷阱

1. **PATH 修改**:nvm 会大范围修改 PATH;恢复时务必小心
2. **Shell source**:nvm 必须 source,不能当作脚本直接执行
3. **版本解析**:别名、部分版本号与特殊关键字之间的交互较为复杂
4. **平台差异**:需处理 Linux、macOS 与其他 Unix 系统之间的差异
5. **网络依赖**:许多操作需要联网获取版本列表
6. **并发访问**:多个 shell 同时安装版本时可能发生冲突

## Windows 支持

nvm 可通过多种兼容层在 Windows 上运行:

### WSL2(适用于 Linux 的 Windows 子系统)
- nvm 全部功能可用
- **重要**:请确保使用 WSL2 而非 WSL1 - 最新步骤参见 [Microsoft 的 WSL2 安装指南](https://docs.microsoft.com/en-us/windows/wsl/install)
- 从 Microsoft Store 安装 Ubuntu 或其他 Linux 发行版
- 在 WSL2 内按 Linux 安装说明操作

### Cygwin
- Windows 上的 POSIX 兼容环境
- 从 [cygwin.com](https://www.cygwin.com/install.html) 下载并运行安装程序
- 安装时勾选这些包:bash、curl、git、tar、wget
- 可能需要额外的 PATH 配置

### Git Bash(MSYS2)
- 随 Git for Windows 附带
- 相比完整 Linux 环境功能受限
- 因路径转换问题,部分功能可能无法使用,包括:
  - 二进制解压路径可能被错误转换
  - 符号链接创建可能失败
  - 部分 shell 特有功能行为可能不同
  - 文件权限处理与 Unix 系统不同

### Windows 安装说明

#### WSL2(推荐)
1. 按 Microsoft 官方指南安装 WSL2:https://docs.microsoft.com/en-us/windows/wsl/install
2. 从 Microsoft Store 安装 Ubuntu 或你偏好的 Linux 发行版
3. 在 WSL2 内按标准 Linux 流程安装

#### Git Bash
1. 从 https://git-scm.com/download/win 安装 Git for Windows(含 Git Bash)
2. 打开 Git Bash 终端
3. 运行 nvm 安装脚本

#### Cygwin
1. 从 https://www.cygwin.com/install.html 下载并安装 Cygwin
2. 安装时包含 bash、curl、git、tar、wget 包
3. 在 Cygwin 终端中运行 nvm 安装

本指南可帮助 AI 编码代理理解 nvm 代码库结构、测试流程与开发环境搭建要求。
