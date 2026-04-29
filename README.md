# 📊 Stock Data Fetch — One Skill, All Markets

**Free · Multi-Asset · Real-Time · AI-Native**

Stop juggling a dozen API keys and broken Python libraries. **One command** fetches prices across US stocks, China A-shares, Hong Kong, crypto, commodities, and global indices — with automatic failover when a source goes down.

```bash
snapshot NVDA       # US stock via FMP — quote + EV/EBITDA + growth + earnings
snapshot 600519.SH  # A-share via Tencent → Sina fallback
snapshot BTC        # Crypto via OKX → Hyperliquid backup
snapshot GOLD       # Commodity via Hyperliquid + Finnhub futures
```

---

## 🎯 Why This Exists

AI trading agents are only as good as their data. But financial data is a mess:

- **Free APIs break constantly** — rate limits, deprecated endpoints, N/D returns
- **Every market uses different sources** — US stocks need FMP, China needs Tencent, crypto needs OKX
- **Network restrictions are real** — some regions block Binance, corporate firewalls block eastmoney.com
- **No single free API covers fundamentals well** — FMP gives you growth rates and EV metrics, Finnhub gives you PE/PB/ROE

**Stock Data Fetch solves this with a battle-tested, multi-layered fallback architecture.**

---

## 🏗️ Architecture

```
snapshot TICKER
    │
    ├─ US Stocks ──────── FMP → Finnhub
    │                     ├─ FMP: quote + EV/EBITDA + growth + cash flow
    │                     └─ Finnhub: PE/PB/ROE + 52-week range
    │
    ├─ China A-Shares ── Tencent qt.gtimg.cn → Sina hq.sinajs.cn
    │
    ├─ Hong Kong ─────── Tencent qt.gtimg.cn → Sina hq.sinajs.cn
    │
    ├─ Crypto ────────── OKX → Hyperliquid → OKX DEX
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
| 🇺🇸 US Stocks | Price, EV/EBITDA, FCF Yield, Growth Rates, Revenue, EPS, Sector, Beta | FMP → Finnhub |
| 🇨🇳 A-Shares | Real-time price, OHLCV, Volume | Tencent → Sina |
| 🇭🇰 HK Stocks | Real-time price, OHLCV, Volume | Tencent → Sina |
| 🪙 Crypto (230+) | Real-time price, 24h change | OKX → Hyperliquid |
| 🥇 Commodities | Gold, Oil, Silver, Copper, NatGas | Hyperliquid → Finnhub |
| 📰 News | Entity-linked, sentiment-tagged | Marketaux |

**US stocks get the richest data**: 14+ metrics including EV/EBITDA, FCF yield, revenue growth YoY, EPS growth YoY, 5Y CAGR, sector classification, and beta — all from free-tier APIs.

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
# FMP: https://financialmodelingprep.com/developer/docs (free, 250 calls/day)
# Finnhub: https://finnhub.io/register (60 calls/min free)
# Marketaux: https://www.marketaux.com/ (100 calls/day free)

export FMP_API_KEY="your_key"
export FINNHUB_API_KEY="your_key"
export MARKETAUX_API_KEY="your_key"
# Tencent, Sina, OKX, Hyperliquid — no key needed
```

### Test It

```bash
snapshot NVDA --no-commit
snapshot 600519.SH --no-commit
snapshot BTC --no-commit
snapshot GOLD --no-commit
```

---

## 🤖 Use with AI Agents

### Claude Code / Cursor / Codex

```bash
cp SKILL.md ~/.claude/skills/stock-data-fetch.md
```

### Hermes Agent

```bash
cp SKILL.md ~/.hermes/skills/data-science/stock-data-fetch/SKILL.md
```

The agent will automatically discover and use the data sources documented in the skill.

---

## 🌍 Verified Data Sources

Every source tested in production behind restricted networks:

| Source | Market | What It Provides | Key Needed |
|--------|--------|-----------------|------------|
| FMP | US Stocks | Quote, EV/EBITDA, FCF Yield, Growth, Earnings, Sector | ✅ Free |
| Finnhub | US Stocks | Quote, PE/PB/ROE, 52-Week Range | ✅ Free |
| Tencent qt | China A / HK | Real-time quote | ❌ None |
| Sina Finance | China A / HK | Real-time quote (fallback) | ❌ None |
| OKX | Crypto | Real-time, 200+ pairs | ❌ None |
| Hyperliquid | Crypto + Commodities | 7×24 perp prices, 230+ coins | ❌ None |
| Marketaux | News | Entity-linked, sentiment-tagged | ✅ Free |

---

## 📄 License

MIT — Free for personal and commercial use.

---

## ⭐ Star This Repo

If this saves you from another broken free API at 2 AM, give it a star. Pull requests welcome for new data sources.
