# Nifty 50 Forecasting — An Honest Backtest

Can past Nifty 50 prices predict tomorrow? I built the full pipeline and let the data answer.

## What I built
- Loaded 8,720 trading days of Nifty 50 OHLC data (1990–2026)
- Feature engineering: 5/10-day moving averages, daily % change, trading range, volatility
- Chronological train/test split (train on old data, test on recent — never shuffled)
- Random Forest baselines for three tasks

## Honest results
| Task | Model | Naive baseline | Verdict |
|------|-------|----------------|---------|
| Predict tomorrow's price | MAE 6,737 pts (37%) | — | ? Failed — trees can't extrapolate beyond training range |
| Predict tomorrow's % move | MAE 0.803% | 0.743% (always predict 0%) | ? No edge over naive |
| Predict up/down | 51.2% accuracy | 54.3% (always predict "up") | ? No edge over naive |

## Why it failed (the real lesson)
1. **Trees can't extrapolate.** Trained on Nifty 300–11,000, tested on 12,000–24,000.
2. **Daily moves are mostly noise.** Past prices carry almost no signal — that is market efficiency.
3. **The market drifts upward**, so "always predict up" is a brutally strong baseline.

## Takeaway
The market is efficient at the daily scale — past prices alone don't predict tomorrow.
The value of this project is the honest pipeline: real features, correct time-series
validation, and refusing to fake good numbers.

## Run it
1. Download the daily file (`NIFTY 50_day.csv`) from the [Nifty 50 minute-data Kaggle dataset](https://www.kaggle.com/datasets/debashis74017/nifty-50-minute-data)
2. Open `nifty-forecast.ipynb` in Colab and run all cells

## Stack
Python, pandas, scikit-learn (RandomForest), Colab
