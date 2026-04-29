---
name: stock-data-fetch
description: Fetch multi-market financial data via snapshot CLI — US stocks (Finnhub), A-shares/HK (Tencent/Sina), crypto (OKX), commodities (Hyperliquid/Finnhub futures), news (Marketaux). Use for any price, fundamentals, earnings, or news query.
category: data-science
---

# Stock Data Fetch

## Quick Start

```bash
snapshot TICKER [--no-news] [--no-commit]
```

This single command handles routing to the correct data source for any market.

## Data Sources by Market

### US Stocks (NVDA, AAPL, TSLA...)
- **Finnhub** — quotes, fundamentals (PE/PB/market cap), earnings (revenue/EPS), company profile
- Data via `FINNHUB_API_KEY` env var
- Free tier: 60 calls/min

### A-Shares (600519.SH, 000001.SZ...)
1. **Tencent qt.gtimg.cn** — real-time quotes (primary)
2. **Sina hq.sinajs.cn** — real-time quotes (fallback)
- Both free, no API key required
- Note: No fundamentals/earnings available via these free APIs
- Requires `Referer` header for Sina

### HK Stocks (00700.HK, 01879...)
1. **Tencent qt.gtimg.cn** — real-time quotes (primary)
2. **Sina hq.sinajs.cn** — real-time quotes (fallback)

### Cryptocurrency (BTC, ETH, SOL...)
1. **OKX CEX** — real-time quotes (primary, confirmed working)
2. ~~Binance~~ — HTTP 451 blocked in this network
- `snapshot BTC` uses OKX automatically
- USD-denominated pairs

### Commodities & Indices
- **Hyperliquid spot tokens** — HOLD (gold), XAUT0 (Tether Gold), XAUM (Matrixdock Gold), SPX (S&P 500 perp)
- **Finnhub futures** — GC=F (gold), CL=F (crude oil), SI=F (silver), NG=F (natgas), HG=F (copper)
- `snapshot GOLD` → Hyperliquid HOLD
- `snapshot OIL` → Finnhub CL=F

### News
- **Marketaux API** — financial news with entity linking and sentiment
- Via `MARKETAUX_API_KEY` env var

## CLI Commands

```bash
# Basic snapshot
snapshot NVDA                          # Full: quote + fundamentals + earnings + news
snapshot NVDA --no-news                # Skip news (faster)
snapshot NVDA --no-commit              # Don't git commit
snapshot 600519.SH                     # A-share via Tencent
snapshot 00700.HK                      # HK stock
snapshot BTC                           # Crypto via OKX
snapshot GOLD                          # Commodity via Hyperliquid

# Check results
cat raw/snapshots/$(date +%Y-%m-%d)-NVDA.json | python3 -m json.tool
```

## Network Constraints

This environment has specific network restrictions:
- ✅ Tencent qt.gtimg.cn — confirmed working
- ✅ Sina hq.sinajs.cn — confirmed working  
- ✅ Finnhub — confirmed working
- ✅ OKX — confirmed working
- ✅ Marketaux — confirmed working
- ✅ Hyperliquid — confirmed working
- ❌ yfinance — rate limited / SSL issues
- ❌ akshare / eastmoney — proxy blocked
- ❌ Google News RSS — timeout
- ❌ Binance — HTTP 451 (legal block)
- ❌ FMP — API key expired

## Output Format

Snapshot writes JSON to `wiki/raw/snapshots/{date}-{TICKER}.json`:

```json
{
  "ticker": "NVDA",
  "retrieved_at": "2026-04-29T10:00:00Z",
  "source_apis": ["finnhub"],
  "data": {
    "quote": {"price": 213.17, "change_pct": 2.3},
    "fundamentals": {"pe_ttm": 42.7, "market_cap": 5000000000000},
    "earnings_latest": {"quarter": "2026Q1", "revenue": 22000000000},
    "news": [{"title": "...", "url": "...", "source": "Reuters"}]
  }
}
```

## Pitfalls
- Sina requires `Referer: https://finance.sina.com.cn` header
- The `snapshot` CLI is in `~/trading-agent/` repo, install via `pip install -e ~/trading-agent`
