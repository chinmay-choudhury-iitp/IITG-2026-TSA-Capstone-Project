# 📈 Time Series Analysis Capstone Project 2026
### Data-Driven Stock Forecasting & Virtual Portfolio Management on the NSE

> **IIT Guwahati — Consulting & Analytics Club × StockGro**
> Submitted by: Chinmay Choudhury, IIT Patna

---

## Overview

This project applies a complete time series forecasting pipeline to manage a virtual ₹10,00,000 portfolio on NSE-listed stocks via the [StockGro](https://stockgro.onelink.me/vNON/21jikjek) virtual trading platform. Three forecasting models — **ARIMA**, **Facebook Prophet**, and **LSTM** — were implemented, evaluated, and used to drive portfolio construction and live trading decisions over a 2-day window (May 14–15, 2026).

**Result:** +1.02% portfolio return (+₹10,163 P&L) over the live 2-day window, with an average live MAPE of 0.91% and 80% directional accuracy (4/5 stocks correct).

---

## Repository Structure

```
tsa-capstone-project-2026/
│
├── Python_Notebook.ipynb          # Full pipeline: fetch → preprocess → model → forecast → portfolio
├── Report.pdf                     # Final capstone project report (10 pages)
├── Visual_Dashboard.html          # Interactive dashboard (bonus deliverable)
│
├── README.md                      # This file
└── TSA_Capstone_project_PS.pdf            # Original problem statement
```

---

## Stock Universe

Five NSE-listed stocks across distinct sectors:

| Stock | Ticker | Sector | 5-Yr Gain | Avg Ann. Volatility |
|---|---|---|---|---|
| HDFC Bank | `HDFCBANK.NS` | Banking | +14.0% | 20.34% |
| Infosys | `INFY.NS` | IT | −1.3% | 23.66% |
| Sun Pharma | `SUNPHARMA.NS` | Pharma | +521.5% | 20.24% |
| Reliance Industries | `RELIANCE.NS` | Energy | +51.1% | 21.98% |
| Maruti Suzuki | `MARUTI.NS` | Auto | +77.4% | 22.39% |

Data source: Yahoo Finance via `yfinance` | Period: Jan 2021 – May 2026 | Interval: Daily

---

## Pipeline Summary

### Task 1 — Stock Universe Selection
Stocks selected across 5 sectors for diversification. Justified using rolling standard deviation (all in 20–24% annualised range) and STL-based trend analysis. Sun Pharma identified as the best risk-adjusted pick (+521.5% return at lowest volatility).

### Task 2 — Data Preprocessing
- Forward fill for missing values (zero missing confirmed)
- ADF test: all stocks non-stationary; first differencing applied (d=1 for ARIMA)
- MinMax scaling [0,1] for LSTM input
- Train/test split: 1,304 training days / 22 test days (most recent)

### Task 3 — Time Series Forecasting
Three models implemented:
- **ARIMA** — auto-selected via AIC minimisation; walk-forward validation
- **Facebook Prophet** — additive decomposition with automatic seasonality
- **LSTM** — 60-day lookback window, recurrent neural network

### Task 4 — Volatility & Trend Analysis
- **GARCH(1,1)** for time-varying volatility forecasts (used directly in portfolio sizing)
- **STL Decomposition** (period=252) for trend/seasonality/residual breakdown

### Task 5 — Portfolio Construction
50/50 blend of two strategies:
- **Strategy A (Forecast-Guided):** Capital allocated proportional to LSTM-predicted returns
- **Strategy B (Volatility-Aware):** Inverse-volatility weighting via GARCH forecasts

| Stock | Final Weight | Allocation (₹) |
|---|---|---|
| INFY.NS | 54.89% | 5,48,871 |
| MARUTI.NS | 19.85% | 1,98,525 |
| RELIANCE.NS | 15.66% | 1,56,570 |
| SUNPHARMA.NS | 13.51% | 1,35,069 |
| HDFCBANK.NS | 7.15% | 71,489 |

### Task 6 — Model Comparison

| Model | Avg MAPE | Avg Directional Accuracy |
|---|---|---|
| ARIMA | **1.28%** | 45.7% |
| LSTM | 4.23% | 46.7% |
| Prophet | 9.95% | 43.8% |

ARIMA was used for the live forecast; LSTM for portfolio weight allocation.

### Task 7 — Virtual Trading (StockGro)
- Platform: StockGro (NSE Virtual Simulator)
- Event: "Portfolio – Time Series Analysis 2026"
- Trading Window: May 14–15, 2026
- Full ₹10,00,000 deployed at market open on Day 1

### Task 8 — Live Performance vs Predictions

| Stock | Predicted (₹) | Actual Day 2 (₹) | Live MAPE | Direction |
|---|---|---|---|---|
| HDFCBANK.NS | 759.28 | 762.63 | 0.45% | ✅ |
| INFY.NS | 1,085.41 | 1,106.86 | 1.98% | ✅ |
| SUNPHARMA.NS | 1,845.28 | 1,849.30 | 0.22% | ✅ |
| RELIANCE.NS | 1,338.49 | 1,321.81 | 1.25% | ❌ |
| MARUTI.NS | 12,901.16 | 12,988.44 | 0.67% | ✅ |
| **Average** | — | — | **0.91%** | **4/5 (80%)** |

**Total Portfolio Return: +1.02% | P&L: +₹10,163**

---

## Models & Libraries

```python
# Core
yfinance          # Data fetching
pandas, numpy     # Data manipulation
statsmodels       # ARIMA, ADF test, STL decomposition
pmdarima          # auto_arima (AIC-based parameter selection)
prophet           # Facebook Prophet
tensorflow/keras  # LSTM
arch              # GARCH(1,1) volatility modelling
matplotlib        # Visualisations
scikit-learn      # MinMax scaling, evaluation metrics
```

---

## Key Results

- ARIMA outperformed Prophet and LSTM on MAPE for all 5 stocks — confirming classical models remain competitive for short-horizon large-cap forecasting
- 3/5 stocks returned ARIMA(0,1,0) (Random Walk), consistent with the Efficient Market Hypothesis
- Prophet performed worst due to trend extrapolation errors
- Portfolio returned +1.02% over 2 days, driven primarily by INFY (+1.98% on a 54.89% allocation)

---

## Reflections & Future Improvements

- **Add exogenous variables** — ARIMAX with FII flows and sentiment scores
- **Cap individual stock weights at 30%** — the 54.89% INFY allocation creates concentration risk
- **Dynamic ensemble weighting** — weight models by recent rolling accuracy rather than a fixed 50/50 blend
- **Longer trading window** — 2 days is too short to validate model quality; a 20-day window would be more robust

---

## Capstone Project Context

- **Organised by:** Consulting & Analytics Club, IIT Guwahati
- **Platform Partner:** StockGro
- **Virtual Capital:** ₹10,00,000
- **Dataset Period:** January 2021 – May 2026
- **Trading Window:** May 11–15, 2026 (any 2 consecutive days)
