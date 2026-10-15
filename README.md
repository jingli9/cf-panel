# Cloudflare 第三方管理面板

单文件 Cloudflare Module Worker（`worker.js`），前端只调用同源 `/api`，
所有 Cloudflare API 请求都由 Worker 后端代理转发，浏览器不再直连 `api.cloudflare.com`（无 CORS 问题）。

## 功能

- **Workers**：列表、部署/编辑/删除、workers.dev 开关、环境变量、Secrets 批量更新、
  KV / D1 / R2 绑定、账号级自定义域名、版本列表
- **批量创建 Worker**：多账号批量部署、KV 模板、GitHub 链接自动转换、自动创建 KV/D1、执行日志
- **KV**：命名空间 CRUD、键分页/前缀搜索、单键 CRUD、批量写入/删除
- **D1**：数据库 CRUD、参数化查询、Raw 模式、导出备份
- **R2**：存储桶管理、对象浏览/上传/下载/删除
- **域名/DNS**：Zone 管理、记录 CRUD、批量导入
- **Pages**：项目管理、部署记录、触发/重试/回滚/删除部署、自定义域名
- **用量统计**：GraphQL 今日请求、按 Worker 排行
- **设置**：workers.dev 子域名、鉴权方式查看、反馈入口
- **OAuth 登录**：点"使用 Cloudflare 账号登录"，走官方 OAuth 2.0 + PKCE 授权，
  不用创建/粘贴 API Token；token 快过期自动刷新；多账号切换互不干扰

## 新功能说明（v2.1 → v2.5）

- **v2.5**
  - 设置页删除整个 OAuth 登录配置卡片，界面更干净
  - Workers 列表路由加载改为并行（原逐 zone 串行，zone 多时明显变快）
- **v2.4**
  - OAuth Client ID 改由部署环境变量 `OAUTH_CLIENT_ID` 统一预置，设置页不再手动填写
- **v2.3**
  - 支持 `OAUTH_CLIENT_ID` 环境变量（32 位 hex 校验，未配置则保持内置默认）
- **v2.2**
  - OAuth scope 改用 `workers-scripts.admin`，修复 `invalid_scope` 报错（Cloudflare OAuth 界面无 read/write 勾选项）
- **v2.1**
  - 修复重新部署时清空 secrets 的数据丢失 bug（读不到值的 secret 以空值上传问题）
  - Pages 资源哈希改为原始字节，与 wrangler 口径一致

## 部署教程

### Cloudflare 官网部署（推荐，单文件粘贴）

**1. 创建 Worker**

- 登录 https://dash.cloudflare.com → 左侧「Workers 和 Pages」→「创建」→「创建 Worker」
- 名字自定 → 点「部署」

**2. 粘贴代码**

- 进到 Worker 页面 → 右上角「编辑代码」
- 把编辑器里的示例代码全删掉
- 用记事本打开本地的 `worker.js`，全选复制，粘贴进去（文件约 455KB，粘贴时页面可能卡几秒，耐心等）
- 点「保存并部署」

**3. 设置访问密码（强烈建议）**

- Worker 页面 →「设置」→「变量和机密」→「添加」
- 类型选「机密」，名称填 `ACCESS_PASSWORD`，值填你的面板密码 → 保存

**4. （可选）预置 OAuth Client ID**

- 同一位置添加变量：名称 `OAUTH_CLIENT_ID`，值填你的 OAuth 客户端 ID → 保存
- 不配置则使用内置默认；OAuth 登录方式见下方

**5. 访问**

绑定自定义域名或直接用 `https://<worker名>.<子域>.workers.dev` 打开，输入访问密码登录，
再添加 Cloudflare 凭据（推荐 OAuth 登录，最省事）。

## 鉴权方式

- **OAuth 2.0 登录（推荐）**：登录页点"使用 Cloudflare 账号登录"，
  跳转 Cloudflare 官方授权页点允许即可，不用创建 API Token、不用复制粘贴密钥
- **API Token**：Bearer 鉴权，按最小权限配置
- **Global API Key（旧版兼容）**：拥有账号全部权限，请妥善保管

OAuth 说明：标准 Authorization Code + PKCE（S256）流程；access token 过期前 2 分钟
自动用 refresh token 续期；多账号切换互不干扰。
Client ID 通过部署环境变量 `OAUTH_CLIENT_ID` 预置（公开标识，可放心填写）。

## OAuth 客户端配置

自建 OAuth 客户端（用于"使用 Cloudflare 账号登录"）：

👉 **[OAuth 客户端配置图文教程](https://jingli9.github.io/cf-panel/oauth-setup-tutorial.html)**（从零创建 → 域名验证 → 设为公开 → 接入面板）

## 已知限制

- R2 对象级操作需要对应权限
- Secrets 批量更新、Analytics 等接口需要 Token 具备对应权限
- 未设置 `ACCESS_PASSWORD` 时面板无密码保护，公网部署务必设置

## 反馈

问题反馈 / 交流：https://t.me/yifang_chat
