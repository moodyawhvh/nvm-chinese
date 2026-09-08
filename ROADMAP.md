# nvm 路线图

> 🌐 本文档由 [nvm-sh/nvm](https://github.com/nvm-sh/nvm) 翻译,英文原版见原项目。

以下是 `nvm` 计划中的主要功能清单:

- [x] 重写安装代码路径,以支持[从源码](https://github.com/nvm-sh/nvm/issues/1188)安装 `io.js` 和 `node` `v4+`。
  - 其中包括[复用已下载且校验和匹配的压缩包](https://github.com/nvm-sh/nvm/issues/1193),这对性能和带宽都是不错的优化。
- [ ] 通过可选启用的环境变量支持,列出、下载并安装 `node` 的[候选发布版本](https://github.com/nvm-sh/nvm/issues/779)以及[每日构建版](https://github.com/nvm-sh/nvm/issues/1053)。
- [ ] [`nvm update`](https://github.com/nvm-sh/nvm/issues/400):支持 `nvm` 自身自动更新
- [ ] [v1.0.0](https://github.com/nvm-sh/nvm/milestone/1),包括将 [npm 上的 nvm](https://github.com/nvm-sh/nvm/issues/304) 更新为可正确自动安装 nvm 的版本
