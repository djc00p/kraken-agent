# 📈 Kraken Agent

[![ClawHub Skill](https://img.shields.io/badge/ClawHub-Skill-blue)](https://clawhub.ai/djc00p/kraken-agent) [![Agent Skill](https://img.shields.io/badge/Agent-Skill-blue)](#) [![Kraken](https://img.shields.io/badge/Kraken-5741D9.svg)](https://www.kraken.com) [![MCP](https://img.shields.io/badge/MCP-Server-4CAF50.svg)](https://modelcontextprotocol.io) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Kraken Agent** discovers and uses the official Kraken CLI — an AI-native trading tool covering crypto, stocks, forex, and derivatives.

Built for agents: structured JSON output, consistent error envelopes, a built-in MCP server, and **paper trading that needs no API key**.

---

> ⚠️ **Real money warning.** This skill can reach a live Kraken account with real funds. Market data and paper trading are safe and need no API key. Any command run with credentials present — `kraken setup`, authenticated `kraken mcp`, or live order commands — can place real orders and lose real money. **Start in paper trading.** Confirm with the user before any authenticated or order-placing command, and never place a live order without an explicit request to trade.

---

## 📊 What It Covers

| Asset class | Examples |
|-------------|----------|
| Crypto spot | BTC, ETH, SOL — 1,400+ pairs |
| xStocks (tokenized) | AAPL, NVDA, TSLA, SPY, QQQ |
| Forex | EUR/USD, GBP/USD, USD/JPY |
| Perpetual futures | 317 contracts, up to 50x margin |
| Earn / staking | Flexible and bonded strategies |

---

## 🔧 Install

See the [official install instructions](https://github.com/krakenfx/kraken-cli#installation) in the repo. Single binary, no runtime dependencies.

Verify:

```bash
kraken --version
```

---

## 🎮 Paper Trading (No API Key Needed)

A safe sandbox against live prices — no account, no real money:

```bash
kraken paper init --balance 10000 -o json
kraken paper buy BTCUSD 0.01 -o json
kraken paper status -o json
kraken paper history -o json
kraken paper reset -o json
```

Futures paper trading is also available: `kraken futures paper`.

---

## 📉 Live Market Data (No API Key Needed)

```bash
kraken ticker BTCUSD -o json
kraken orderbook BTCUSD --count 10 -o json
kraken ohlc BTCUSD --interval 60 -o json
```

---

## 🔌 MCP Server

Built-in Model Context Protocol server over stdio — works with Claude, Codex, Gemini CLI, Cursor, and OpenClaw:

```bash
kraken mcp                    # market + account + paper (default)
kraken mcp -s market,paper    # read-only, no auth needed
kraken mcp -s all             # all services (dangerous calls require acknowledged=true)
```

Add to your MCP client config:

```json
{
  "mcpServers": {
    "kraken": {
      "command": "kraken",
      "args": ["mcp", "-s", "market,paper"]
    }
  }
}
```

---

## 🔐 Authentication (Live Trading)

> ⚠️ **These credentials give full control of the account's trading balance.** A key that cannot withdraw funds can still open, modify, and close real orders, and those orders can lose real money. Treat the key as sensitive account access. Confirm with the user before running any authenticated command, and never place a live order without an explicit request to trade.

```bash
# Interactive setup — prompts for the key, keeps it out of shell history
kraken setup

# Or via environment variables
export KRAKEN_API_KEY="your-key"
export KRAKEN_API_SECRET="your-secret"
```

Get API keys at <https://www.kraken.com> (Settings → API).

### Credential handling

* **Never pass keys directly on the CLI** — use `kraken setup` or environment variables to avoid shell history exposure.
* **Prefer `kraken setup` over exported env vars** for a persistent setup. It stores credentials in the tool's own config file with restrictive permissions rather than in shell history or a `.env` file you might commit.
* **If you use environment variables**, set them only for the current shell session, and do not write them into a file that could be committed. Do not echo them, print them, or include them in logs.
* **Scope the key.** Where Kraken offers it, restrict the key to the minimum permissions needed, and disable withdrawal access unless the user explicitly needs it.
* **Never reuse a key that has appeared in a log, chat, or commit.** Rotate it at <https://www.kraken.com> (Settings → API) instead.

---

## 🤖 Agent Integration Tips

* Always use `-o json` — stdout is always valid JSON on success
* Route on the `error` field in error envelopes, not `message`
* Paper trading is the right starting point for any new agent setup
* The repo's `skills/` folder contains 50 goal-oriented workflow packages
* `AGENTS.md` and `CONTEXT.md` in the repo cover rate limits, error handling, and invocation patterns
* Prefer the read-only MCP scope (`kraken mcp -s market,paper`) unless the user explicitly needs account access. `-s all` enables order-capable services — confirm before using it
* Stay on paper trading (`kraken paper ...`) until the user explicitly asks to trade live

---

**Repo:** <https://github.com/krakenfx/kraken-cli>

**Published by [@djc00p](https://github.com/djc00p)**
