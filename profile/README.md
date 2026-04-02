# Spread Finder

**Real-time cryptocurrency arbitrage detection across centralized and decentralized exchanges.**

Spread Finder monitors price differences between trading venues, validates opportunities through order book depth analysis, and delivers actionable alerts - all within milliseconds.

---

## What It Does

Traditional arbitrage tools scan prices and report raw spreads. Spread Finder goes further:

- **Multi-venue price monitoring** - 10 CEX exchanges, 4 DEX aggregators, and on-ramp services tracked simultaneously
- **Two-pass validation** - fast ticker scan finds candidates, then order book depth analysis confirms real fill prices at target trade sizes
- **Net profit estimation** - accounts for trading fees, withdrawal fees, network transfer costs, and slippage before reporting an opportunity
- **Smart alerting** - deduplication, cooldown periods, and configurable filters eliminate noise

The result: alerts that represent actual executable trades, not theoretical spreads that vanish on execution.

## Architecture

```
                        Data Collection                        Analysis & Delivery
                    +---------------------+               +------------------------+
                    |                     |               |                        |
  Binance  -----+  |   Exchange Workers  |    Redis      |    Spread Scanner      |
  Bybit    -----+--| (REST + WebSocket)  |---[HOT]------>|  Pass 1: Ticker scan   |
  OKX      -----+  |   1 worker/exchange |   [WARM]      |  Pass 2: Orderbook     |
  KuCoin   -----+  |                     |   [COLD]      |         validation     |
  Gate.io  -----+  +---------------------+               +----------+-------------+
  MEXC     -----+                                                    |
  Bitget   -----+  +---------------------+               +----------v-------------+
  HTX      -----+  |                     |               |                        |
  Kraken   -----+  |   OnRamp Workers    |---[ONRAMP]--->|  Filter + Dedup        |
  BingX    -----+  |   DEX Workers       |               |  Net Profit Calculator |
                    |                     |               |  Alert Formatter       |
                    +---------------------+               +----------+-------------+
                                                                     |
                                                          +----------v-------------+
                                                          |   Telegram Alerts      |
                                                          |   Dashboard (FastAPI)  |
                                                          +------------------------+
```

**Scan cycle: ~80-145ms** using Redis-cached data (vs ~95s with direct API calls).

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Core | Python 3.11+, asyncio |
| Exchange APIs | ccxt, aiohttp, websockets |
| Data layer | Redis (3-tier: HOT/WARM/COLD), PostgreSQL |
| Web | FastAPI, Jinja2 |
| Alerts | aiogram (Telegram Bot API) |
| Validation | Pydantic v2, Decimal-based financial math |
| Tooling | uv, ruff, pytest (1989 tests) |

## Key Design Decisions

**Template Method pattern for adapters** - a shared base class handles 90% of exchange integration logic. Adding a new exchange requires ~100 lines implementing ~12 one-line hooks for API-specific parsing. All 10 exchanges follow this pattern.

**Identity vs Snapshot separation** - token metadata (symbol, contracts, networks) is separated from live price data. Identity is loaded once; snapshots refresh every second. This avoids mixing stable and volatile data in the same objects.

**Redis 3-layer model** - HOT (prices, 15s TTL), WARM (metadata, 5min TTL), COLD (contracts, 1hr TTL). Workers write, scanner reads. This decouples data collection frequency from analysis frequency.

**Decimal discipline** - all prices, fees, amounts, and spreads use Python's `Decimal` type. No floating-point arithmetic in financial calculations.

## Supported Exchanges

| Exchange | REST | WebSocket | Order Book | Notes |
|----------|:----:|:---------:|:----------:|-------|
| Binance | + | + | + | Full-featured, reference adapter |
| Bybit | + | + | + | Requires API key |
| OKX | + | + | + | Requires passphrase |
| KuCoin | + | + | + | |
| Gate.io | + | + | + | |
| MEXC | + | + | + | Maker fee 0% |
| Bitget | + | + | + | |
| HTX | + | + | + | |
| Kraken | + | + | + | Limited without API key |
| BingX | + | + | + | No contract addresses |

**DEX aggregators:** Jupiter, Raydium, OKX DEX, 1inch

## Project Structure

| Repository | Description |
|-----------|------------|
| **backend** | Core arbitrage engine, exchange adapters, scanner, API |
| **telegram-bot** | Telegram notification service |

---

<sub>Built with focus on reliability, speed, and precision in financial calculations.</sub>
