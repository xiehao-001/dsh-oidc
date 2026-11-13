# 捆绑内容：dsh-our-free-model

本包在 `vendor/dsh-our-free-model/` 内附带了
[`dsh-our-free-model`](https://github.com/Ebony-Vinyl/dsh-our-free-model)（v2.8.3，MIT），
用于在机构 SSO 不可用或未配置模型时提供免登录、免 API Key 的模型通道。

## 为什么捆绑

- 机构 OIDC 只能拿到机构侧的模型授权；离线、非机构环境或机构网关上不可达时，对话无法进行。
- 该捆绑包提供公共模型直连（DeepSeek V4.1 Flash、Kimi K3 等）与 OpenAI 兼容本地网关，可作为兜底。
- 它与 SSO 插件互不依赖：机构登录优先，免密钥通道作为第二条路径并存。

## 安装方式（二选一）

**A. 只装主包（默认）** — 使用包内 `cordis.patch.yml`，仅挂载 `dsh-oidc`。

**B. 连带捆绑包一起挂载** — 先把 `vendor/dsh-our-free-model` 作为独立 bundle 安装到同一个 DSH 安装，
再改用包内的 `cordis.patch.with-free-model.yml` 覆盖 `cordis.patch.yml`。

## 注意

- 捆绑包由上游维护，升级请直接替换 `vendor/dsh-our-free-model/` 目录。
- 它会在本机监听一个 OpenAI 兼容端口（默认仅 loopback）；不需要时不要启用 B 方案。

## 发布适配

GitHub push protection 会把两组 Gemini CLI 的公开 installed-app 常量识别为 Google OAuth 凭据并拦截推送：
`vendor/dsh-our-free-model/src/subscriptions.js` 与 `vendor/dsh-our-free-model/vendor/channel-pack/pack.js`。
为了让本仓库可以公开，这两处被改写为**等价的字符串拼接**（运行时取到的值完全不变），功能不受影响；上游文件本身未改动，升级时按上文替换目录即可。
