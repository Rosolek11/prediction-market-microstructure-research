# Research methodology

## Research question

Can a sudden move in an external reference price identify a short-lived stale
quote in a five-minute binary prediction market?

## Data collected

Each decision-relevant event records:

- local timestamp;
- source WebSocket timestamp;
- order-book bids and asks;
- visible depth;
- fee parameters;
- reference price and market threshold;
- ATR and impulse values;
- compression ratio;
- model and market probabilities;
- calculated edge and expected value;
- requested and actual execution delay;
- simulated price, quantity, fee, and slippage;
- entry, mark, exit, and settlement information.

## Execution assumptions

The paper model is intentionally pessimistic:

1. a signal does not fill immediately;
2. the engine waits for the configured latency;
3. the first later book is used for execution;
4. all requested quantity must be visible within the price limit;
5. fees are included;
6. stale signals are rejected;
7. losing positions can lose the complete outlay.

## Evaluation

Results are segmented by:

- direction;
- entry-market price;
- model probability;
- remaining time;
- impulse divided by ATR;
- edge after fees;
- execution delay;
- slippage;
- close reason.

The intended next stage is chronological walk-forward evaluation:

1. select parameters on an earlier training window;
2. freeze the parameters;
3. evaluate on a later unseen window;
4. compare the revised model with the original model running in shadow mode;
5. use official market outcomes for final resolution.

## What is not claimed

- The study does not demonstrate guaranteed arbitrage.
- A profitable in-sample filter is not treated as proof of future performance.
- Model probability is not treated as the payout probability.
- Top-of-book prices are not assumed to be fully executable.
- A local reference-price proxy is not considered a substitute for official
  market resolution.

