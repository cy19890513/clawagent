# ClawAgent

个人自托管 AI 助手面板：通过 Telegram 跟自己的 Claude Code 对话，并让它监控你在乎的东西——行情价格、Reddit、Twitter、人物动态、网络信息——有情况就推送提醒。

> **来源说明**：本仓库基于 [Gideon AI](https://github.com/terryds/gideon-ai)（MIT License，Copyright (c) 2026 Terry Djony），按架构模块拆成多个 PR 逐步引入。LICENSE 文件完整保留。

## 架构

```
                    ┌────────────────────┐
   你 ── Telegram ──┤  ClawAgent 面板     ├── Claude Code（你自己的）
                    │                    │
                    │  • 行情信号         │── Binance、Yahoo Finance
                    │  • Reddit 追踪      │── reddit.com（经代理）
                    │  • Twitter 追踪     │── twitter-cli
                    │  • Exa 人物搜索     │── Exa API
                    │  • 信息信号         │── Perplexity
                    └────────────────────┘
```

全部跑在你自己的机器上：SQLite 存数据，Bun + React 单进程。无账号、无 SaaS。

> 单用户设计：在 VPS 上给一个人用。关联的 Telegram 聊天实际上拥有 shell 权限（见原项目的 Security notes），不要多用户共用。

## 模块与 PR

| PR | 模块 | 内容 |
|----|------|------|
| #1 | 项目脚手架 | 构建配置、依赖、环境模板 |
| #2 | 数据持久层 | `server/db.ts` SQLite 表结构与读写 |
| #3 | Telegram 中继 | Bot 收发、消息监听、Claude Code 调用 |
| #4 | 行情信号 | 交易对、行情抓取、轮询告警 |
| #5 | Reddit 追踪 | 关键词监控 |
| #6 | Twitter 追踪 | 关键词监控 |
| #7 | Exa + 信息信号 | 人物搜索、Perplexity 定时摘要 |
| #8 | API 服务 | `server/index.ts` 聚合接口 |
| #9 | 前端面板 | React 监控界面 |

## 快速开始

### 依赖

- Bun ≥ 1.3.12
- Claude Code CLI（`npm install -g @anthropic-ai/claude-code`，登录一次）
- Telegram Bot Token（找 [@BotFather](https://t.me/BotFather) 免费申请）

### 安装运行

```bash
cp .env.example .env   # 填入配置
bun install
bun run dev            # 开发模式
# 或
bun run build && bun start
```

打开浏览器访问 `http://localhost:3000`，按两步向导完成配置。

## License

MIT — 见 [LICENSE](./LICENSE)。原作者：Terry Djony（Gideon AI）。
