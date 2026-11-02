# 📊 Stock & ETF Checklist

**Live app:** https://stock-checklist-akshaj.streamlit.app/

Most beginner investors buy on hype and sell in a panic. This tool helps you invest by your own rules instead. Run any stock or ETF through a checklist of tests, see the data behind each result, and backtest those rules against simply buying the index.

## Features

- **Checklist:** enter a ticker and get pass/fail results for each test, with the reason behind each one.
- **Score and summary:** see how many tests passed, along with the current price and the 52-week high and low.
- **Price chart:** one year of price history.
- **Backtesting (in progress):** see how a rule would have performed over the last 10 years compared with buy and hold, including the worst drawdown.

## First backtest: the 200-day moving average rule

The rule is to own the asset only while its price is above its 200-day average, and to hold cash otherwise. Starting amount: $10,000. Period: 10 years, ending November 2026.

| Ticker | Buy & hold | 200-day rule | Buy & hold max drawdown | 200-day rule max drawdown |
|--------|-----------:|-------------:|------------------------:|--------------------------:|
| VOO    | $42,375    | $26,940      | -34.0%                  | -19.6%                    |
| QQQ    | $63,444    | $38,551      | -35.1%                  | -22.3%                    |
| AAPL   | $132,023   | $43,038      | -38.5%                  | -35.3%                    |

**What this shows:** the rule trailed buy and hold on returns for all three tickers. On the two index funds, it cut the worst drop by about a third, and that trade-off can matter for an investor who might panic-sell. On a single stock like AAPL, the rule gave up most of the gains and barely reduced the drawdown. The first real finding of this project is that a simple rule rarely beats holding the index, and that a tool should show this honestly.

**Assumptions:** prices are adjusted for dividends and splits. Signals act on the next day's close to avoid look-ahead bias. Cash earns 0%, and there are no trading costs or taxes.

## How it works

- `checks.py` holds every test as a plain function that takes `(info, hist)` and returns `("pass" | "fail" | "n/a", detail)`. All tests are registered in one `TESTS` dictionary.
- `app.py` is the Streamlit interface. It loops over `TESTS`, so adding a test takes one function and one line.
- `backtest.py` uses vectorized pandas (`rolling`, `shift`, `cumprod`) to simulate a rule across 10 years of daily prices.
- Data comes from Yahoo Finance via `yfinance`, cached for an hour.

## Run locally

```bash
git clone https://github.com/akshaj1717/stock-checklist.git
cd stock-checklist
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

## Disclaimer

This is an educational and analysis tool, not financial advice. Past performance does not predict future results.
