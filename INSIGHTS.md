# Insights: Trader Performance vs. Bitcoin Market Sentiment

*Analysis of 211,218 Hyperliquid trades (32 accounts, 246 assets, May 2023 – May 2025)
matched against the daily Bitcoin Fear & Greed Index.*

## 1. Win rate diverges sharply by sentiment regime — average PnL doesn't

| Sentiment | Trades | Avg PnL/trade | Median PnL/trade | Win Rate |
|---|---:|---:|---:|---:|
| Fear | 40,214 | $101.86 | $6.36 | **84.4%** |
| Neutral | 18,159 | $71.20 | $4.58 | 82.4% |
| Greed | 46,029 | $105.70 | $6.49 | 82.5% |

![Average PnL by bucket](figures/avg_pnl_by_bucket.png)
![Win rate by bucket](figures/winrate_by_bucket.png)

A **chi-square test on win rate (Fear vs. Greed) is highly significant** (χ² = 60.0,
p = 9.5 × 10⁻¹⁵), but a **Welch's t-test on mean PnL is not** (t = -0.40, p = 0.69). In other
words: trades are meaningfully more likely to close in profit during Fear, but the *average
dollar size* of wins and losses balances out similarly across regimes. This points to a
fatter right tail of large winners during Greed periods offsetting a lower hit rate.

![PnL distribution violin](figures/pnl_distribution_violin.png)

The distribution plot confirms this — the bulk of the Greed distribution isn't dramatically
different from Fear's, but Greed carries a wider spread.

## 2. Traders in this dataset are contrarian, not momentum-driven

![Long short lean](figures/long_short_lean.png)

- During **Fear**, **63.8%** of new positions opened are **longs**.
- During **Greed**, the lean flips: **56.6%** of new positions opened are **shorts**.

This is a "buy the dip, fade the rally" pattern — the opposite of a momentum-chasing
retail stereotype. Combined with the higher Fear-regime win rate, this contrarian
positioning appears to be paying off more often than not for this cohort.

## 3. Position sizing scales up during Fear

![Avg size by bucket](figures/avg_size_by_bucket.png)

| Sentiment | Avg trade size (USD) | Median trade size (USD) |
|---|---:|---:|
| Fear | $7,182 | $749 |
| Neutral | $4,783 | $548 |
| Greed | $4,574 | $552 |

Average trade size during Fear is **~57% larger** than during Greed — traders are putting
more capital to work exactly when the market is more fearful, consistent with
conviction-buying or averaging into weakness.

## 4. Fine-grained sentiment view

![Finegrained PnL](figures/finegrained_pnl.png)

Breaking the 5-way classification out fully shows the pattern isn't perfectly monotonic —
Extreme Fear and Extreme Greed both show elevated average PnL relative to the milder
categories, suggesting the *extremes* of sentiment (not just the Fear/Greed direction) are
where the more interesting trade outcomes cluster.

## 5. Daily aggregate view

![Daily PnL vs FG](figures/daily_pnl_vs_fg.png)

- Correlation between the raw Fear & Greed **value** and daily total closed PnL: **r = -0.08**
  (p = 0.07, not significant).
- Correlation between the Fear & Greed value and daily total **volume**: **r = -0.26**
  (p = 4.2 × 10⁻⁹, significant).

Aggregate daily profitability doesn't track the sentiment index in any strong linear way,
but **trading activity (dollar volume) picks up as sentiment gets more fearful** — reinforcing
finding #3 at the daily-aggregate level, not just the per-trade level.

## 6. PnL concentration across accounts

![Top accounts by bucket](figures/top_accounts_by_bucket.png)

Total profitability is concentrated in a handful of the 32 accounts. Importantly, most top
performers generate meaningful PnL in **both** Fear and Greed regimes — success here looks
more like consistent execution than a bet on a single sentiment regime.

---

## Strategy Implications

1. **Use sentiment as a risk/sizing filter, not a standalone directional signal.**
   The weak correlation between the FG index and daily aggregate PnL means sentiment alone
   is a poor timing signal. But the **win-rate gap** and **sizing pattern** suggest it's a
   reasonable input for *position sizing and risk appetite* — e.g., scaling size up
   modestly as fear increases, in line with what the most active traders here already do.
2. **Lean contrarian, with discipline.** The data supports a "size up on fear, trim/short
   into greed" posture over a momentum-chasing one — but because *average* PnL is similar
   across regimes, this should be paired with strict per-trade risk controls (stop-loss,
   max position size) rather than relying on regime alone to produce outsized returns.
3. **Watch the extremes.** Extreme Fear and Extreme Greed both showed elevated average PnL
   versus the milder categories — a rule that increases conviction specifically at the
   tails of the sentiment index (rather than treating "Fear" and "Greed" as uniform bands)
   may capture more of the effect than a simple three-bucket rule.
4. **Don't expect a strong daily-aggregate signal.** If building a systematic strategy,
   sentiment is better applied at the trade/position level (sizing, entry bias) than as a
   predictor of next-day aggregate PnL, where the correlation is weak and not significant.

## Caveats & Limitations

- The trader sample is small (32 accounts) and may not generalize to the broader market —
  these are Hyperliquid derivatives traders, a specific and likely sophisticated segment.
- "Win rate" is computed on closing trades (`closed_pnl != 0`); trades that are still open
  at the end of the dataset window aren't reflected in these PnL stats.
- The Fear & Greed Index is a Bitcoin-specific sentiment measure but is applied here across
  246 different traded assets — sentiment on BTC may not transmit equally to every asset.
- Correlation/significance results describe this historical sample and are not a guarantee
  of future performance; this is exploratory analysis, not a backtested trading strategy.
