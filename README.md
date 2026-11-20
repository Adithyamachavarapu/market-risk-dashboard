# Market Risk Dashboard

A Power BI report that tracks risk for an equal-weight portfolio of 12 NIFTY 50 stocks, using seven years of daily NSE prices from Yahoo Finance (Jan 2019 – Dec 2025).

![Dashboard Preview](images/dashboard.png)

## What it does

The report answers the questions a risk team asks every morning:
- How much could we lose on a bad day? (VaR and Expected Shortfall)
- Does the risk model actually work? (rolling backtest with Kupiec test)
- Which stocks carry the most risk? (risk contribution, beta, correlation)

## Data

- **Source:** Yahoo Finance daily prices via yfinance Python library
- **Stocks:** 12 large-cap Indian companies across 8 sectors + NIFTY 50 index
- **Period:** 2 Jan 2019 to 30 Dec 2025, 1,726 trading days
- **Raw rows:** 22,477 (13 securities x trading days)

## Key numbers

| Metric | Value |
|--------|-------|
| 1-day VaR 95% | 1.36% (Rs 13,609 on Rs 10 lakh) |
| Expected Shortfall 95% | 2.37% (Rs 23,719) |
| VaR breaches | 76 vs 73.75 expected (passes Kupiec, p = 0.79) |
| Portfolio beta vs NIFTY | 0.92 |
| Max drawdown | -34.59% (COVID, March 2020) |
| Top risk contributor | Tata Steel (12.1% of risk, 8.3% of money) |

## Tools used

- **Python** (pandas, NumPy, SciPy): data download, cleaning, returns, rolling windows, statistical tests
- **Power BI Desktop**: star schema, 36 DAX measures, what-if parameter, 3 report pages
- **Matplotlib**: supporting charts

## How it was built

1. Downloaded prices for 13 tickers with yfinance, saved raw CSV
2. Cleaned: removed 3 missing prices, 36 non-trading-day rows, kept 4 extreme moves after checking by hand
3. Computed daily returns, portfolio return, drawdown, rolling volatility, rolling 250-day VaR with one-day shift to avoid look-ahead bias
4. Ran statistical tests (Jarque-Bera, Kupiec, beta, risk contribution) and saved results to JSON
5. Built star schema in Power BI with 6 tables, 36 measures, 3 report pages

## Report pages

- **Overview** — total return, CAGR, volatility, Sharpe, beta, growth-of-100 line, return by year
- **Risk Monitor** — VaR/ES cards, backtest line chart, return histogram, drawdown, rolling volatility
- **Stock Drilldown** — risk contribution table, return vs volatility scatter, correlation matrix

## Limitations

- Survivorship bias: picked today's NIFTY 50 stocks, not 2019's
- Equal weights rebalanced daily, no trading costs
- One-day VaR only, fixed 6.5% risk-free rate
- Yahoo Finance data should be cross-checked with NSE before real use

## Dataset

Public data from Yahoo Finance. No proprietary information.
