# CF Panel — Cloudflare 第三方管理面板

单文件 Cloudflare Worker，浏览器里管理你的 Cloudflare 账号：Workers、Zones/DNS、KV、D1、R2、Pages，支持 OAuth / API Token / Global Key 三种登录方式。

## 部署

1. 把 `worker.js` 粘贴到 Worker 的 Edit code，部署
2. （可选）设置页 → 子域名设置；如需 OAuth 登录，按教程配置 OAuth 客户端
3. 如需预置 OAuth Client ID：在 Worker 的 Settings → Variables 添加环境变量 `OAUTH_CLIENT_ID`

## 版本

- v2.5（2026-10-15）：设置页移除 OAuth 配置卡片；路由加载并行化
- v2.4：OAuth Client ID 改由环境变量预置
- v2.3：`OAUTH_CLIENT_ID` 环境变量支持
- v2.2：scope 改用 `workers-scripts.admin`（修复 invalid_scope）
- v2.1：修复重部署清空 secrets 等 bug

## 教程

- OAuth 客户端配置：见 `cf-oauth-client-setup-tutorial-v3.html`（另行分发）
