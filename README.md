# Institutional SSO & Model Access

`@eduwork/dsh-oidc` · v0.3.4 · MulanPSL-2.0 AND MIT

Signs users in to DeepSeek Harness with standards-based OIDC — authorization code with PKCE over a browser or loopback callback — and turns the resulting access token into authorized model access. After one institutional login the DSH provider reads the authorized model catalog, keeps the token fresh, and sends model requests with the current access token, so no institutional model API key is ever requested or stored. Personal API keys stay in their own settings.

It is written for institutions that already run an OIDC identity provider and a compatible model gateway (LiteLLM native OAuth, experimental oidc-llm 0.1, or identity-only OIDC), and for anyone who has to deploy such a login into a local DSH Web profile. The package also carries restricted institutional branding, and the desktop backend additionally manages product branding through the desktop bridge.

## Highlights

- Runs the OIDC authorization-code flow with PKCE, a browser or loopback callback, token refresh, sign-out and DSH provider registration.
- Validates the ID token signature, issuer, audience, nonce and UserInfo subject in OIDC mode; LiteLLM native follows its own contract without guessing token types or downgrading the protocol.
- Checks token lifetime before each request, refreshes near expiry, and merges concurrent refreshes into a single one.
- Cancels in-flight model requests on sign-out, clears local authorization, and makes a best-effort revocation of the refresh token.
- Reads the authorized model catalog after login and registers models by declared `type`, so only `llm` models enter the conversation and a later catalog refresh does not overwrite the user's own model choice.
- Supports a desktop backend (`backend: desktop`, desktop credential storage, `desktopServices.openExternal`, temporary 127.0.0.1 callback) and a local Web backend (`backend: web`, WebServer bound to 127.0.0.1 on its actual port).
- Accepts only restricted branding data — no remote code is loaded — and reuses the DSH provider for model catalogs, JSON/SSE transport, reasoning levels and attachments.

## Install

Install this directory as a local DSH plugin bundle. From a shell:

```bash
dsh plugin --profile web add /absolute/path/to/dsh-oidc
```

Or add the directory through the **Plugins** panel in DSH (local bundle). Restart the DSH app after installation; plugins are activated from the bundle patch (`cordis.patch.yml`).

## Configuration

The bundle patch inserts the plugin row with `profilePathEnv: EDUWORK_OIDC_PROFILE` and `allowEmptyProfiles: true`, so the profile file is normally supplied through that environment variable before DSH starts:

| Key | Default | Meaning |
| --- | --- | --- |
| `profilePathEnv` | `EDUWORK_OIDC_PROFILE` | Environment variable naming the absolute path of the enterprise profile JSON |
| `profile` | unset | Inline trusted profile object |
| `profiles` | unset | Inline list of trusted profile objects |
| `allowEmptyProfiles` | `true` in the patch (otherwise the plugin refuses to start) | Permit starting with no profile configured |
| `backend` | `web` | `web` for the local Web host, `desktop` for the EduWork Electron host |
| `uiMode` | `standard` | Account UI mode; `external` and `models-only` are also accepted |
| `manageProductBrand` | `true` | Let the plugin apply product branding from the profile |
| `configFile` | unset | `{ path, examplesPath }` for opening the configuration file from the desktop host |
| `authorizedOrigins` | unset | Extra origins authorized for gateway calls |

## Bundled: dsh-our-free-model

This package also ships a third-party plugin directory at `vendor/dsh-our-free-model/`: [`dsh-our-free-model`](https://github.com/Ebony-Vinyl/dsh-our-free-model) v2.8.3, distributed under its own MIT license (Copyright (c) 2026 Ebony-Vinyl, see `vendor/dsh-our-free-model/LICENSE` and its `README.md`). It provides a key-less, login-free model channel (public models plus an OpenAI-compatible local gateway) as a fallback when institutional SSO or a configured model is unavailable. It is maintained upstream, keeps its own license and attribution, and is **not** ECNU/EduWork code.

Two bundle patches are included. `cordis.patch.yml` is the default and mounts only `@eduwork/dsh-oidc`. `cordis.patch.with-free-model.yml` mounts the same plugin and additionally inserts a second plugin row, `id: our-free-model` / `name: 'dsh-our-free-model'`. To use it, first install `vendor/dsh-our-free-model` as its own bundle into the same DSH installation, then replace `cordis.patch.yml` with the bundled `cordis.patch.with-free-model.yml`; the second row only resolves when that package is present. The bundled channel listens on a local OpenAI-compatible port (loopback by default), so leave the second row out if you do not need it, and upgrade by replacing the `vendor/dsh-our-free-model/` directory from upstream.

## Requirements

- DeepSeek Harness >=0.2.1 <0.3.0
- Node >=24.18.0
- Peer packages: @deepseek-ai/cordis, @deepseek-ai/dsh-api-remotes, @deepseek-ai/dsh-attachment, @deepseek-ai/dsh-authorization, @deepseek-ai/dsh-brand, @deepseek-ai/dsh-client-ui-conversation, @deepseek-ai/dsh-client-ui-renderer, @deepseek-ai/dsh-client-ui-settings, @deepseek-ai/dsh-client-ui-settings-general, @deepseek-ai/dsh-client-ui-settings-models, @deepseek-ai/dsh-client-ui-sidebar, @deepseek-ai/dsh-client-ui-theme, @deepseek-ai/dsh-client-locale, @deepseek-ai/dsh-credentials, @deepseek-ai/dsh-fs, @deepseek-ai/dsh-host-webserver, @deepseek-ai/dsh-launch-environment, @deepseek-ai/dsh-llm, @deepseek-ai/dsh-llm-pi-ai, @deepseek-ai/dsh-sandbox, @deepseek-ai/dsh-scope, @deepseek-ai/dsh-session, @deepseek-ai/dsh-settings, @deepseek-ai/dsh-timeout, @deepseek-ai/dsh-typert-protocol, @earendil-works/pi-ai, react, react-dom

## Notes and limits

- The deployer must supply a compatible gateway and the public profile configuration. Profiles are trusted JSON (capped at 512 KiB) validated against a strict key allow-list; `backend: native` and the old Key Binding model flows are removed, so old profiles fail explicitly and must be migrated first.
- The experimental oidc-llm 0.1 integration is a draft and disabled by default. Only models declared as `llm` reach the conversation, so a server that returns ids only needs `provider.models[].type` filled in before upgrading; specialized and untyped models stay out.
- This is a single-machine client: the Web backend listens on loopback only and must not be exposed. Sign-out clears local authorization and cancels in-flight requests, but the server-side validity of already issued access tokens remains under the server's control.
- This directory is a source snapshot: no `lib/` build output is included, so the `lib/index.js` entry declared in `package.json` does not exist yet. Build the package in the EduWork tree before installing, otherwise DSH reports a missing bundle entry.

## 中文文档

[README.zh-CN.md](./README.zh-CN.md)

## License

MulanPSL-2.0 AND MIT — see [LICENSE](./LICENSE) and, where present, [LICENSE-MIT](./LICENSE-MIT). The bundled `vendor/dsh-our-free-model` directory keeps its own MIT license and attribution. Extracted from the ECNU EduWork distribution (https://github.com/ECNU/EduWork); upstream copyright and license notices are preserved.
