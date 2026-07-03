# Trader Performance vs. Bitcoin Market Sentiment

Data science take-home assignment for Primetrade.ai — exploring the relationship between
Hyperliquid trader performance and the Bitcoin Fear & Greed Index to surface patterns that
could inform smarter trading strategies.

## Project Structure

```
.
├── data/
│   ├── fear_greed_index.csv      # raw: daily Bitcoin sentiment classification
│   ├── historical_data.csv       # raw: Hyperliquid trade-level data
│   ├── merged_trades.csv         # generated: trades tagged with sentiment
│   └── daily_summary.csv         # generated: date-level aggregates
├── notebooks/
│   └── trader_sentiment_analysis.ipynb   # full analysis, executed with outputs
├── src/
│   ├── data_prep.py               # cleaning + merge logic
│   ├── analysis.py                # statistics (win rate, t-tests, correlations)
│   └── visualize.py               # chart generation
├── reports/
│   ├── figures/                   # all PNG charts
│   ├── *.csv / *.json             # tidy output tables from analysis.py
│   └── INSIGHTS.md                # written findings & strategy recommendations
├── requirements.txt
└── README.md
```

## Datasets

1. **Bitcoin Fear & Greed Index** (`data/fear_greed_index.csv`) — daily sentiment score
   (0–100) and 5-way classification (Extreme Fear → Extreme Greed), Feb 2018–May 2025.
2. **Hyperliquid Historical Trader Data** (`data/historical_data.csv`) — 211,224 trade-level
   records across 32 accounts and 246 assets (May 2023–May 2025), including execution price,
   size, side, direction, closed PnL, and fees.

## Methodology

1. **Clean & merge** (`src/data_prep.py`): parse timestamps, derive a trade date, join each
   trade to that day's sentiment classification. 99.997% of trades matched (only 6 of
   211,224 trades fell outside the sentiment index's date coverage and were dropped).
2. **Bucket sentiment**: the 5-way classification is collapsed into **Fear / Neutral / Greed**
   for cleaner comparisons, while the fine-grained 5-way breakdown is also analyzed.
3. **Analyze** (`src/analysis.py`): performance metrics (avg/median PnL, win rate),
   significance tests (Welch's t-test, Mann-Whitney U, chi-square on win rate), position
   sizing, and long/short lean by sentiment regime, plus account-level breakdowns.
4. **Visualize** (`src/visualize.py`): all charts saved to `reports/figures/`.
5. **Notebook**: `notebooks/trader_sentiment_analysis.ipynb` walks through the entire
   pipeline end-to-end with narrative and embedded chart outputs — this is the primary
   deliverable to read.

## How to Reproduce

```bash
pip install -r requirements.txt

# Run the pipeline
python src/data_prep.py      # cleans & merges raw data -> data/merged_trades.csv, daily_summary.csv
python src/analysis.py       # computes stats -> reports/*.csv, reports/*.json
python src/visualize.py      # generates charts -> reports/figures/*.png

# Or just open the notebook, which reproduces everything with commentary
jupyter notebook notebooks/trader_sentiment_analysis.ipynb
```

## Headline Findings

- **Win rate is significantly higher during Fear** (84.4%) than **Greed** (82.5%)
  (chi-square p < 0.0001), even though *average PnL per trade* is not statistically
  different between regimes (Welch's t-test p ≈ 0.69).
- **Traders in this dataset are contrarian**: ~64% of new positions opened during Fear
  are longs, while ~57% of positions opened during Greed are shorts.
- **Position sizes are larger during Fear** (~$7.2k avg) than during Greed (~$4.6k avg).
- **Aggregate trading volume correlates negatively with the sentiment index**
  (r ≈ -0.26, p < 0.001) — this cohort trades more dollar volume when the market is fearful.

Full write-up with charts and strategy implications: **[`reports/INSIGHTS.md`](reports/INSIGHTS.md)**.

## Tech Stack

Python · pandas · NumPy · Matplotlib · SciPy · Jupyter
