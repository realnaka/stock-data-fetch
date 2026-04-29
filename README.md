# 📊 Stock Data Fetch — One Skill, All Markets

**Free · Multi-Asset · Real-Time · AI-Native**

Stop juggling a dozen API keys and broken Python libraries. **One command** fetches prices across US stocks, China A-shares, Hong Kong, crypto, commodities, and global indices — with automatic failover when a source goes down.

```bash
snapshot NVDA       # US stock via Finnhub — price + P/E + earnings + news
snapshot 600519.SH  # A-share via Tencent → Sina fallback
snapshot BTC        # Crypto via OKX → Hyperliquid backup
snapshot GOLD       # Commodity via Hyperliquid + Finnhub futures
```

---

## 🎯 Why This Exists

AI trading agents are only as good as their data. But financial data is a mess:

- **yfinance breaks constantly** — rate limits, SSL errors, N/D returns
- **Free APIs disappear** — no SLA, no warning
- **Every market is different** — US stocks use Finnhub, China uses Tencent, crypto uses OKX
- **Network restrictions** — Binance blocked in some regions, Google News times out behind firewalls

**Stock Data Fetch solves this with a battle-tested, multi-layered fallback architecture.**

---

## 🏗️ Architecture

```
snapshot TICKER
    │
    ├─ US Stocks ──────── Finnhub (quote + fundamentals + earnings)
    │
    ├─ China A-Shares ── Tencent qt.gtimg.cn → Sina hq.sinajs.cn
    │
    ├─ Hong Kong ─────── Tencent qt.gtimg.cn → Sina hq.sinajs.cn
    │
    ├─ Crypto ────────── Binance → OKX → Hyperliquid → OKX DEX
    │
    ├─ Commodities ──── Hyperliquid spot tokens + Finnhub futures
    │
    └─ News ──────────── Marketaux API (entity-linked, sentiment-tagged)
```

**Every path has at least one fallback.** If a source fails, the next one takes over automatically.

---

## 📦 What You Get

| Asset Class | Data Available | Sources |
|-------------|---------------|---------|
| 🇺🇸 US Stocks | Price, P/E, PB, Market Cap, Revenue, EPS, News | Finnhub |
| 🇨🇳 A-Shares | Real-time price, OHLCV, Volume | Tencent, Sina |
| 🇭🇰 HK Stocks | Real-time price, OHLCV, Volume | Tencent, Sina |
| 🪙 Crypto (230+) | Real-time price, 24h change | OKX, Hyperliquid |
| 🥇 Commodities | Gold, Oil, Silver, Copper, NatGas | Hyperliquid, Finnhub |
| 📰 News | Entity-linked, sentiment-tagged | Marketaux |

All data written to structured JSON with git version history.

---

## 🚀 Install (30 Seconds)

### Prerequisites

```bash
# Python 3.11+
python3 --version

# Clone the monorepo
git clone https://github.com/realnaka/trading-agent.git
cd trading-agent
pip install -e .
```

### API Keys (Free Tier)

```bash
# Get free keys:
# Finnhub: https://finnhub.io/register (60 calls/min free)
# Marketaux: https://www.marketaux.com/ (100 calls/day free)

export FINNHUB_API_KEY="your_key"
export MARKETAUX_API_KEY="your_key"
# Tencent, Sina, OKX, Hyperliquid — no key needed
```

### Test It

```bash
snapshot NVDA --no-commit
snapshot BTC --no-commit
snapshot 600519.SH --no-commit
```

---

## 🤖 Use with AI Agents

### Claude Code / Cursor / Codex

Add the skill to your agent's skills directory:

```bash
cp SKILL.md ~/.claude/skills/stock-data-fetch.md
```

### Hermes Agent

```bash
cp SKILL.md ~/.hermes/skills/data-science/stock-data-fetch/SKILL.md
```

The agent will automatically discover and use the data sources documented in the skill.

---

## 🌍 Network Resilience

We operate behind the Great Firewall and in restricted corporate networks. Every source has been verified:

| Source | Status | Notes |
|--------|--------|-------|
| Tencent qt | ✅ | Most reliable for China markets |
| Sina Finance | ✅ | Requires Referer header |
| Finnhub | ✅ | US stocks primary |
| OKX | ✅ | Crypto primary (Binance 451 blocked) |
| Hyperliquid | ✅ | 7×24 perp prices, 230+ coins |
| Marketaux | ✅ | News with entity linking |
| yfinance | ❌ | Unreliable — rate limited |
| akshare | ❌ | eastmoney.com blocked |
| Google News | ❌ | Timeout in restricted networks |
| Binance | ❌ | HTTP 451 in some regions |

---

## 📄 License

MIT — Free for personal and commercial use.

---

## ⭐ Star This Repo

If this saves you from another `yfinance` error at 2 AM, give it a star. Pull requests welcome for new data sources.
