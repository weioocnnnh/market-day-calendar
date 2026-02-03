# 部署到 Cloudflare Workers（静态站点）

以下说明用于将本项目作为静态站点部署到 Cloudflare Workers（利用 Workers Sites）。

前提
- Node.js 与 npm 已安装
- 已有 Cloudflare 账户，并创建 API Token（或使用 `wrangler login`）

安装 Wrangler（Cloudflare CLI）

```bash
npm install -g wrangler
# 或者在项目里做为 devDependency：
# npm install -D wrangler
```

配置
1. 在 `wrangler.toml` 中替换 `account_id` 为你的 Cloudflare Account ID，或通过 CI 环境变量注入。
2. `workers_dev = true` 会在 `*.workers.dev` 子域上发布（开发/预览用）。使用自定义域需要配置 `zone_id` 与路由。

本地构建与发布

```bash
# 生成静态文件到 dist/
npm run build

# 登录 wrangler（如果你还没登录）
wrangler login

# 发布到 workers（会上传 dist/ 目录内容）
npm run deploy
```

注意
- 若要在 CI 中使用，请将 `account_id` 与 `api_token` 作为秘密环境变量，并在发布时通过 `wrangler publish` 的环境变量或 `wrangler.toml` 引用它们。
- 本项目使用 Vite，默认构建目录为 `dist/`，`wrangler.toml` 已将 `site.bucket` 指向 `./dist`。

故障排查
- 若发布失败，先确认 `npm run build` 成功并生成 `dist/index.html`。
- 使用 `wrangler whoami` 验证当前登录账号。

更多信息
- Wrangler 文档：https://developers.cloudflare.com/workers/cli-wrangler
- Workers Sites 文档：https://developers.cloudflare.com/workers/platform/sites
