# Agentic Inbox

运行在 Cloudflare Workers 上的自托管邮件客户端，用独立邮箱存储与 AI Agent 完成检索、草拟和发送。

[上游 Cloudflare 一键部署](https://deploy.workers.cloudflare.com/?url=https://github.com/cloudflare/agentic-inbox) · [上游项目](https://github.com/cloudflare/agentic-inbox) · [English](./README.en.md)

![Agentic Inbox 界面](./demo_app.png)

## 功能

- 通过 Cloudflare Email Routing 收件、Email Service 发件，支持会话、搜索、附件与文件夹
- 每个邮箱使用独立的 Durable Object 与 SQLite，附件存入 R2
- 内置邮件 Agent，可读取、搜索、草拟和发送邮件
- 新邮件可自动生成草稿，发送前仍需人工确认
- 使用 Cloudflare Access 保护 Web 界面与 MCP 接口

## 使用

需要 Cloudflare 账号、已启用 Email Routing 的域名、R2、Workers AI、Email Service 与 Cloudflare Access。

```bash
npm install
npm run dev
```

在 `wrangler.jsonc` 配置自己的域名；R2 存储桶尚不存在时先创建：

```bash
npx wrangler r2 bucket create agentic-inbox
npm run deploy
```

生产环境需要配置 Cloudflare Access 的 `POLICY_AUD` 与 `TEAM_DOMAIN`。部署后为域名建立转发到 Worker 的 catch-all Email Routing 规则，并配置 Email Service；缺少 Access 配置时，生产请求会被拒绝。

## 说明

Cloudflare Access 是唯一信任边界。通过同一 Access 策略的用户可以访问全部邮箱；MCP 客户端也可通过 `mailboxId` 操作任意邮箱，本项目不提供逐邮箱授权。本地开发会跳过 Access 验证。

邮件和附件存放于 Cloudflare；AI 功能通过配置的 Workers AI 服务处理邮件内容。不要将真实邮件或凭据写入仓库和公开问题报告。

## 版权说明

项目依据 [Apache License 2.0](./LICENSE) 发布。上游版权归 Cloudflare, Inc. 及其贡献者所有；应保留已有上游通知，个人品牌、素材和用户数据不在许可范围内。

许可边界见[许可范围](./LICENSE_SCOPE.md)。
