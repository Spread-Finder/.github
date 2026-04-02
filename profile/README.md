<p align="center">
  <h1 align="center">Spread Finder</h1>
  <p align="center">
    Real-time cryptocurrency arbitrage detection across CEX and DEX exchanges
  </p>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/exchanges-10_CEX_|_4_DEX-green" alt="Exchanges">
  <img src="https://img.shields.io/badge/scan_cycle-~100ms-orange" alt="Scan Cycle">
  <img src="https://img.shields.io/badge/status-active-brightgreen" alt="Status">
</p>

---

Spread Finder finds real arbitrage opportunities between cryptocurrency exchanges and delivers actionable alerts with net profit estimates - not raw spreads that disappear on execution.

## What You Get

- [x] **Price monitoring across 15 venues** - 11 centralized exchanges + 4 DEX aggregators tracked simultaneously
- [x] **Validated opportunities** - every alert is confirmed through order book depth analysis at your target trade size
- [x] **Net profit estimates** - trading fees, withdrawal fees, network costs, and slippage already deducted
- [x] **Instant Telegram alerts** - opportunities delivered to your phone in real time
- [x] **Web dashboard** - live opportunities, exchange status, historical analytics
- [x] **Smart filtering** - deduplication, cooldown periods, configurable thresholds to cut the noise
- [x] **Sub-second detection** - full market scan in ~100ms

## Supported Exchanges

| Exchange | Real-time Prices | Orderbook Depth | Status |
|----------|:----------------:|:---------------:|:------:|
| Binance | + | + | Full |
| Bybit | + | + | Full |
| OKX | + | + | Full |
| KuCoin | + | + | Full |
| Gate.io | + | + | Full |
| MEXC | + | + | Full |
| Bitget | + | + | Full |
| HTX | + | + | Full |
| Kraken | + | + | Limited |
| BingX | + | + | Limited |
| Phemex | + | + | Limited |

**DEX aggregators:** Jupiter, Raydium, OKX DEX, 1inch

## How It Works

1. **Collect** - dedicated workers stream prices from every exchange via WebSocket and REST
2. **Detect** - scanner finds price differences across all trading pairs in milliseconds
3. **Validate** - top candidates go through order book depth analysis at your target amount
4. **Calculate** - net profit estimated after all fees, withdrawal costs, and slippage
5. **Alert** - profitable opportunities delivered via Telegram and displayed on the dashboard

## Disclaimer

This software is for educational and research purposes. Cryptocurrency trading involves substantial risk of loss. The authors are not responsible for any financial losses incurred through the use of this software. Always do your own research before making trading decisions.

---

<sub>Spread Finder - real-time market analysis built for speed and precision.</sub>
