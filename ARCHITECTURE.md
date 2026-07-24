# Architecture

## Design goals

The research platform was designed around five constraints:

1. minimize avoidable decision latency;
2. retain enough context to explain every entry and exit;
3. simulate executable fills rather than top-of-book fantasy fills;
4. keep market-data collection independent from trading decisions;
5. fail safely when data becomes stale or incomplete.

## Main components

### Reference-price feed

The reference feed is processed continuously and independently of five-minute
market boundaries. Historical bars are retained between market rotations, so
ATR does not reset when a new market begins.

### Volatility model

The engine builds five-second OHLC bars and calculates Wilder ATR(14):

```text
ATR(t) = (ATR(t-1) × (n - 1) + TR(t)) / n
```

Only closed bars update ATR. A stationary price inside a forming candle cannot
increase the indicator.

### Compression indicator

The pre-impulse indicator examines six complete five-second bars immediately
before the impulse window. It calculates:

```text
compression_ratio = 30-second high-low range / current ATR
```

The current impulse candle is excluded. This prevents the breakout from
contaminating the measurement of the consolidation that preceded it.

### Order-book engine

The engine maintains both sides of the binary market from public WebSocket
updates. Simulated trades walk visible depth and include the market fee
schedule.

### Execution simulator

Paper orders use:

- a configurable pessimistic delay;
- a maximum acceptable price;
- full-or-kill semantics;
- a maximum signal age;
- no retrospective fill when depth disappears.

This prevents the backtest from assuming that a quote seen at signal time
remains executable later.

### Observability

The dashboard exposes:

- current reference price and market threshold;
- ATR, impulse, compression ratio, and gate status;
- both order books and available depth;
- model probability and market probability;
- edge after fees;
- open and closed paper positions;
- entry and exit timestamps;
- realized and mark-to-market PnL;
- structured operational and decision logs.

## Operational model

The service runs under a restricted Linux service account with:

- automatic restart supervision;
- an explicit memory limit;
- restricted filesystem access;
- persistent strategy settings;
- continuous compressed research records;
- disk-usage monitoring and archival procedures.

Deployment coordinates and production access details are deliberately excluded
from this public case study.

