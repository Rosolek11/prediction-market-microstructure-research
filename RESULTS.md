# Research results and iteration

## Initial paper sample

| Metric | Result |
|---|---:|
| Closed positions | 125 |
| Winning / losing | 58 / 67 |
| Win rate | 46.4% |
| Simulated PnL | -18.9249 units |
| Mean result per position | -0.1514 units |
| Profit factor | 0.938 |
| Maximum drawdown | 63.2595 units |
| Longest loss streak | 7 |
| Median simulated fill delay | 501 ms |
| 95th-percentile fill delay | 518 ms |

The negative initial result was useful: it exposed where the signal was
insufficiently selective.

## Strongest findings

### Weak edge

The `5%–7.49%` edge bucket produced:

- 44 positions;
- `-80.9591` units of PnL;
- `-1.8400` units per position;
- profit factor `0.43`.

### Entry price

The `0.60–0.69` entry-price bucket produced:

- 54 positions;
- `-86.4253` units of PnL;
- `-1.6005` units per position;
- profit factor `0.50`.

### Direction asymmetry

| Direction | Positions | PnL | Win rate |
|---|---:|---:|---:|
| Up | 48 | +17.6262 | 52.1% |
| Down | 77 | -36.5512 | 42.9% |

This does not justify disabling one direction, but it supports separate
calibration by direction.

### Impulse magnitude

Performance was not monotonic as the ATR multiple increased. Simply requiring a
larger impulse was therefore rejected as an adequate fix.

## Revised research model

The following changes were introduced:

- edge threshold increased from `5%` to `7.5%`;
- maximum entry price reduced from `0.90` to `0.60`;
- maximum signal age set to `1500 ms`;
- 30-second pre-impulse compression gate added;
- compression threshold set to `2.5 × ATR`.

On the original sample, retaining only entries with edge of at least `7.5%`
would have changed simulated PnL from `-18.9249` to `+62.0342` units. Combining
entry price below `0.60` with edge of at least `7.5%` produced `+62.3838` units
across 24 positions.

These are in-sample observations and may be overfit. They are hypotheses to be
validated on new data, not reported as expected future returns.

## Engineering finding

One signal in the initial data was filled after approximately 139 seconds
because the original simulator had a minimum delay but no maximum signal age.
The new TTL gate rejects this class of stale execution.

