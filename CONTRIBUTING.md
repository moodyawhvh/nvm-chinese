# 贡献指南

> 🌐 本文档由 [nvm-sh/nvm](https://github.com/nvm-sh/nvm) 翻译,英文原版见原项目。

:+1::tada: 首先,感谢你抽出时间为 `nvm` 做贡献!:tada::+1:

我们非常喜欢 pull request 和 issue,它们是我们的最爱。

以下是为 `nvm` 贡献的一套准则。`nvm` 由 [@LJHarb](https://github.com/ljharb) 维护,托管在 GitHub 上。这些内容大多是指导方针,而非硬性规则。请自行斟酌判断,并欢迎通过 pull request 对本文档提出修改建议。

不过在提交之前,请先审阅以下内容:

# 我能如何贡献?

参与贡献的方式有很多,下面是一些我们希望得到帮助的方向。

## 处理已有 issue

你可以考虑协助处理那些等待关注的 issue——找一找带 "help wanted" 标签的任务。

### 如何提交一份(高质量的)Bug 报告?:bug:

请描述问题,并附上帮助维护者复现问题的额外细节:

* **使用清晰、描述性强的标题**来概括这个问题。

* **尽可能详细地描述复现该问题的确切步骤**。例如,先说明你在终端里使用的具体命令。列步骤时,**不要只说你做了什么,还要说明你是怎么做的**。比如,如果你把光标移到了行尾,请说明你用的是鼠标、键盘快捷键还是命令;如果是命令,是哪一条?
* **提供具体示例来演示这些步骤**。请附上相关文件或 GitHub 项目的链接,或者可直接复制粘贴的代码片段。如果要在 issue 中贴代码片段,请使用 [Markdown 代码块](https://help.github.com/articles/markdown-basics/#multiple-lines)。
* **描述你按步骤操作后观察到的行为**,并明确指出该行为的问题所在。
* **说明你原本期望看到的行为,以及为什么。**
* **尽量提供充分的上下文**,以便他人验证并最终修复该问题。包括尽可能详细地说明你的环境信息,这样我们能更容易地确认问题。

## 文档

我们欢迎任何愿意改进文档的贡献——补充缺失的信息,或让文档更加一致、连贯。

# 开发环境

完整的安装、升级与故障排查说明,请参阅 [README](README.md),其中按不同操作系统给出了对应指引。

# 风格指南 / 编码约定

### Pull Request

#### 创建 pull request 之前

  - 请附上测试。带测试的改动会被很快合并。
  - 请手动确认你的改动在 `bash`、`sh`/`dash`、`ksh` 和 `zsh` 中都能正常工作。快速测试虽然会跑这些 shell,但再手动验证一遍总是好的。
  - 请保持空白符一致——2 空格缩进,所有文件末尾保留换行符,等等。
  - 每次更新你的 PR 时,请在默认分支的最新提交之上 rebase。没人喜欢合并提交(merge commit)。

即使以上要求没有全部满足,也欢迎照样提交 PR/issue!也许会有其他人受到启发,自愿帮你补完。

#### 如何创建 pull request

新建分支:

```
git checkout -b issue1234
```

把改动提交到你的分支,并写一条符合我们[规范](#提交信息)的清晰提交信息:

```
git commit -a
```

在发送 pull request 之前,请先 rebase 到上游源码最新代码,确认你的代码运行在最新版本之上:

```
git fetch upstream
git rebase upstream/main
```

验证你的改动:

```
npm test
```

推送你的改动:

```
git push origin issue1234
```

发送 [pull request](https://docs.github.com/en/pull-requests),按评审意见完成修改,然后等待合并。

### 提交信息

* 提交信息第一行(摘要)不超过 72 个字符。
* 描述你做了什么时,使用现在时("Add feature" 而不是 "Added feature")和祈使语气("Move cursor to..." 而不是 "Moves cursor to...")。
* 如果你的 PR 解决了某个 issue,请在提交正文中引用它。
* 其余约定见[这里](https://gist.github.com/ljharb/772b0334387a4bee89af24183114b3c7)。

#### 提交信息示例

```
[Tag]: Short description of what you did

Longer description here if necessary

Fixes #1234
```

> **注意:** 如果一次提交有多个作者,请在提交信息中添加共同作者:

```
Co-authored-by: Name Here <email@here>
```


# 行为准则
[行为准则](https://github.com/nvm-sh/nvm/blob/HEAD/CODE_OF_CONDUCT.md)

# 在哪里可以求助?
如果你有任何问题,请联系 [@LJHarb](mailto:ljharb@gmail.com)。

# 开发者来源证明 1.1(Developer's Certificate of Origin 1.1)

向本项目做出贡献,即表示我证明:

  - 该贡献由我全部或部分创作,我有权按文件中所示的开源许可证提交;或
  - 该贡献基于先前的工作,据我所知该工作受适当的开源许可证保护,且我有权依据该许可证,将经我修改后的作品按同一开源许可证提交(除非我被允许以其他许可证提交),一如文件中所示;或
  - 该贡献由已作出上述 (a)、(b) 或 (c) 证明的其他人直接提供给我,且我未对其进行修改。
  - 我理解并同意:本项目及该贡献是公开的,贡献记录(包括我随贡献提交的所有个人信息,包括我的签署)将被永久保存,并可在符合本项目或相关开源许可证的前提下再分发。
