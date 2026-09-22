# ClawAgent

A self-hosted AI assistant dashboard: chat with your own Claude Code over Telegram, and have it watch the things you care about — market prices, Reddit, Twitter, people, web intel — pinging you when something happens.

> **Attribution**: This repository is based on [Gideon AI](https://github.com/terryds/gideon-ai) (MIT License, Copyright (c) 2026 Terry Djony), introduced module by module across multiple PRs. The LICENSE file is kept intact.

## Architecture

```
                    ┌────────────────────┐
  You ── Telegram ──┤  ClawAgent panel    ├── Claude Code (yours)
                    │                    │
                    │  • Market signals   │── Binance, Yahoo Finance
                    │  • Reddit tracking  │── reddit.com (via proxy)
                    │  • Twitter tracking │── twitter-cli
                    │  • Exa people search│── Exa API
                    │  • Info signals     │── Perplexity
                    └────────────────────┘
```

Runs entirely on your own machine: SQLite for storage, Bun + React in a single process. No accounts, no SaaS.

> Single-user design: meant for one person on a VPS. The linked Telegram chat effectively has shell access (see the original project's Security notes), so don't share it with other users.

## Modules & PRs

| PR | Module | Contents |
|----|--------|----------|
| #1 | Project scaffolding | Build config, dependencies, env template |
| #2 | Persistence layer | `server/db.ts` SQLite schema and read/write helpers |
| #3 | Telegram relay | Bot send/receive, message listening, Claude Code invocation |
| #4 | Market signals | Trading pairs, price fetching, polling alerts |
| #5 | Reddit tracker | Keyword monitoring |
| #6 | Twitter tracker | Keyword monitoring |
| #7 | Exa + info signals | People search, scheduled Perplexity digests |
| #8 | API server | `server/index.ts` aggregate endpoints |
| #9 | Frontend dashboard | React monitoring UI |

## Quick start

### Requirements

- Bun ≥ 1.3.12
- Claude Code CLI (`npm install -g @anthropic-ai/claude-code`, log in once)
- Telegram Bot Token (get one free from [@BotFather](https://t.me/BotFather))

### Install & run

```bash
cp .env.example .env   # fill in your config
bun install
bun run dev            # dev mode
# or
bun run build && bun start
```

Open `http://localhost:3000` in your browser and follow the two-step setup wizard.

## License

MIT — see [LICENSE](./LICENSE). Original author: Terry Djony (Gideon AI).
