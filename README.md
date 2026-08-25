# Agentic Inbox

运行在 Cloudflare Workers 上的自托管邮件客户端，用独立邮箱存储与 AI Agent 完成检索、草拟和发送。

[Cloudflare 部署](https://deploy.workers.cloudflare.com/?url=https://github.com/cloudflare/agentic-inbox) · [上游项目](https://github.com/cloudflare/agentic-inbox)

![Agentic Inbox 界面](./demo_app.png)

## 功能

- 通过 Cloudflare Email Routing 收发邮件，支持会话、搜索、附件与文件夹
- 每个邮箱使用独立的 Durable Object 与 SQLite，附件存入 R2
- 内置邮件 Agent，可读取、搜索、草拟和发送邮件
- 新邮件可自动生成草稿，发送前仍需人工确认
- 使用 Cloudflare Access 保护 Web 界面与 MCP 接口

## 开始

需要 Cloudflare 账号、已启用 Email Routing 的域名、R2、Workers AI、Email Service 与 Cloudflare Access。

```bash
npm install
npm run dev
```

在 `wrangler.jsonc` 配置域名，并创建 R2 存储桶：

```bash
npx wrangler r2 bucket create agentic-inbox
npm run deploy
```

部署后为域名建立转发到 Worker 的 catch-all 规则，并配置 Email Service 与 Cloudflare Access。

## 安全边界

Cloudflare Access 是唯一信任边界。通过同一 Access 策略的用户可以访问全部邮箱；MCP 客户端也可通过 `mailboxId` 操作任意邮箱，本项目不提供逐邮箱授权。

## 许可

[Apache License 2.0](./LICENSE)

上游代码版权归 Cloudflare, Inc. 及其贡献者所有，并依据 Apache-2.0 提供。本仓库中的原创修改（如有）不改变上游版权、通知或许可证。

完整归属与适用范围见 [NOTICE](./NOTICE) 与 [LICENSE_SCOPE.md](./LICENSE_SCOPE.md)。
