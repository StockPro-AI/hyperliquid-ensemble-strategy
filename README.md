# Hyperliquid 6-Signal Ensemble Strategy

> Ported from the [Hyperliquid Autoresearch](https://github.com/semioushe/hyperliquid-autoresearch) project (103 experiments, autonomous AI agent loop).  
> **Backtest: Sharpe 21.4 | Max Drawdown 0.3% | Period: 2024-07-01 to 2025-03-31**

---

## Strategy Overview

The strategy combines **6 independent signals** into a majority-vote ensemble. An entry is triggered only when **at least 4 of 6 signals agree** on direction (long or short).

| # | Signal | Parameters |
|---|--------|------------|
| 1 | Momentum | `close - close[20]` |
| 2 | Very-Short Momentum | `close - close[5]` |
| 3 | EMA Crossover | EMA(9) vs EMA(21) |
| 4 | RSI | RSI(8) > or < 50 |
| 5 | MACD | MACD(12,26,9) line vs signal |
| 6 | BB Compression | BB squeeze + price above/below basis |

### Entry Rules
- **Long**: >= 4/6 signals bullish AND no cooldown active
- **Short**: >= 4/6 signals bearish AND no cooldown active

### Exit Rules
- **ATR Trailing Stop**: 5.5x ATR(14) trailing stop
- **RSI Extreme Exit**: RSI(8) > 75 closes longs, RSI(8) < 25 closes shorts
- **Cooldown**: 3 bars after any exit before new entry is allowed

---

## Files

| File | Description |
|------|-------------|
| `ensemble_strategy.pine` | Pine Script v5 strategy for TradingView |
| `gunbot_ensemble_config.json` | Gunbot custom strategy config (>= v27) |

---

## Pine Script (TradingView)

1. Open TradingView > Pine Script Editor
2. Paste the contents of `ensemble_strategy.pine`
3. Click **Add to chart**
4. Recommended timeframe: **1H**
5. Works on BTC, ETH, SOL perpetuals

**Key inputs (all adjustable in strategy settings):**

```
Vote Threshold : 4    (min signals for entry)
ATR Multiplier : 5.5  (trailing stop distance)
RSI Length     : 8
RSI OB Exit    : 75
RSI OS Exit    : 25
EMA Fast/Slow  : 9 / 21
Momentum Len   : 20
Cooldown Bars  : 3
```

---

## Gunbot Config

1. Copy `gunbot_ensemble_config.json` into your Gunbot `config/` directory
2. Merge the `ENSEMBLE_6SIGNAL` strategy block into your existing `strategies` section
3. Assign pairs as shown in the `pairs` block (adjust exchange accordingly)
4. **Note:** Gunbot does not natively compute all 6 signals internally — the JSON documents the parameter targets. For full signal logic, wire up via the Gunbot **custom strategy JavaScript** interface or use TradingView alerts forwarded to Gunbot via webhook.

---

## Backtest Results (Original Python Backtest)

| Metric | Value |
|--------|-------|
| Sharpe Ratio (annualized) | **21.4** |
| Max Drawdown | **0.3%** |
| Experiments run | 103 |
| Start Capital | $100,000 |
| Assets | BTC, ETH, SOL |
| Period | 2024-07-01 to 2025-03-31 |
| Baseline Sharpe (simple momentum) | 2.724 |

> **Disclaimer:** Past backtest performance does not guarantee future results. Use at your own risk. Not financial advice.

---

## Credits

- Original research: [semioushe/hyperliquid-autoresearch](https://github.com/semioushe/hyperliquid-autoresearch)
- Port & implementation: [StockPro-AI](https://github.com/StockPro-AI)
