# 需要先构建

本包在源仓库里没有提交 `lib/` 构建产物（或入口文件缺失），当前只是可发布的源码包：

- 入口声明：`lib/index.js`
- `exports` 键数：12
- 需要：`在 EduWork 仓库内执行其 tsdown/esbuild 构建脚本`

## 为什么没有产物

构建依赖 `@deepseek-ai/dsh-*` 一系列内部包（例如 `dsh-session`、`dsh-client-ui-*`），
这些在公共 npm registry 上并不齐全，无法在仓库外用独立 `pnpm install` 复现。

## 怎么办

1. 在 EduWork 源码树（`extracted/EduWork-main`）里执行该包的构建脚本，得到 `lib/`；
2. 把 `lib/` 覆盖回本目录；
3. 重新运行 `node tools/validate-publish.mjs`，本包的告警应变成通过。

在此之前请不要安装本包：DSH 会在加载 bundle 时报入口不存在。
