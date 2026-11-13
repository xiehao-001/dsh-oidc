# 机构登录与模型接入（SSO）（@eduwork/dsh-oidc）

> 标准 OIDC 登录、机构品牌、按令牌授权的模型访问，并捆绑免密钥模型包。
>
> Standards-based OIDC login, institutional branding, token-authorized model access, bundled with a key-less model pack.

## 功能

支持 PKCE 授权码流程、刷新令牌与退出登录；不保存机构 API Key，模型凭据由身份令牌换取。捆绑 dsh-our-free-model（免登录免 Key 的公共模型通道），机构不可用时可作为兜底。附带 OpenAI 兼容网关，可把 DSH 模型暴露给外部工具。

## 安装

```bash
# 在 DSH 中把本目录作为本地 bundle 安装（绝对路径）
```

安装后由 DSH 插件系统加载，条目 id 为 `dsh-oidc`。

## 依赖

- `@deepseek-ai/cordis`
- `@deepseek-ai/dsh-api-remotes`
- `@deepseek-ai/dsh-attachment`
- `@deepseek-ai/dsh-authorization`
- `@deepseek-ai/dsh-brand`
- `@deepseek-ai/dsh-client-ui-conversation`
- `@deepseek-ai/dsh-client-ui-renderer`
- `@deepseek-ai/dsh-client-ui-settings`
- `@deepseek-ai/dsh-client-ui-settings-general`
- `@deepseek-ai/dsh-client-ui-settings-models`
- `@deepseek-ai/dsh-client-ui-sidebar`
- `@deepseek-ai/dsh-client-ui-theme`
- `@deepseek-ai/dsh-credentials`
- `@deepseek-ai/dsh-fs`
- `@deepseek-ai/dsh-host-webserver`
- `@deepseek-ai/dsh-launch-environment`
- `@deepseek-ai/dsh-llm`
- `@deepseek-ai/dsh-llm-pi-ai`
- `@deepseek-ai/dsh-sandbox`
- `@deepseek-ai/dsh-scope`
- `@deepseek-ai/dsh-session`
- `@deepseek-ai/dsh-settings`
- `@deepseek-ai/dsh-timeout`
- `@deepseek-ai/dsh-typert-protocol`
- `@earendil-works/pi-ai`
- `react`
- `react-dom`
- `@deepseek-ai/dsh-client-locale`

## 兼容性

- `engines.dsh`: `>=0.2.1 <0.3.0`
- 与 EduWork 发行版同源，来源于 ECNU/EduWork 仓库。

## 注意

源码包未包含构建产物 lib/，发布前需在包目录执行 pnpm install && pnpm build（详见 BUILD.md）。

## 许可

MulanPSL-2.0 AND MIT（与上游 EduWork 一致）。
