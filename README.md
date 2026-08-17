# Market Data Reconciliation

I built this to practice a core piece of financial data work: checking whether two independent sources actually agree with each other. The idea came from a bigger trade reconciliation project I'm working on, scoped down to something I could finish in a day.

**Live dashboard:** [Two-Source Price Reconciliation on Tableau Public](https://public.tableau.com/app/profile/mohammad.nazim.rehman/viz/Two-SourcePriceReconciliationYahooFinancevsAlphaVantage/Dashboard1)

## The idea

Pull daily closing prices for the same set of stocks from two different providers (Yahoo Finance and Alpha Vantage), then use SQL to compare them and flag anything that doesn't line up. If a price from one source is more than 0.5% off from the other, it gets flagged as a mismatch. I also checked for cases where one source has a date the other doesn't, since that's a different kind of problem than a price disagreement.

Tickers: AAPL, MSFT, GOOGL, AMZN, NVDA, TSLA, GME, CVNA
Range: about 2 months of daily data

## Making sure the logic actually works

Before trusting the comparison against real data, I ran it against a fake test case first, a price pair I deliberately set 2% apart. It got flagged correctly. That mattered because the real comparison came back completely clean, and I wanted to know that was a real result and not just a query silently doing nothing.

## Result

344 ticker-days compared. 0 mismatches. 0 missing dates on either side.

Yahoo Finance and Alpha Vantage agreed on every single close price in this window. That's not really a surprise for large, liquid stocks like these — both vendors are pulling from similarly clean data. Where you'd expect two providers to actually disagree is around stock splits or dividends, where "adjusted" vs "unadjusted" pricing conventions differ. That's the natural next step if I extend this.

## Getting a couple of the data sources working was its own thing

Worth noting: my original plan was Yahoo Finance vs. Stooq, but Stooq now blocks plain API requests behind a JavaScript check, so that was a dead end. Alpha Vantage's free tier also changed recently, full historical data now requires a paid plan, so I had to switch to the `compact` output size and pace the requests to stay under the daily limit. Small thing, but a good reminder that "free API" doesn't always mean stable.

## How to run it

Open `Python_Notebook.ipynb` in Google Colab and run the cells top to bottom. You'll need a free Alpha Vantage API key, get one instantly at alphavantage.co/support/#api-key, no email confirmation needed. Everything else runs in the notebook itself, no local setup.

## Stack

Python, SQL (SQLite), pandas, yfinance, Tableau

## Files

- `Python_Notebook.ipynb` — pulls both data sources, runs the SQL comparison, validates the logic, exports results
- `reconciliation_detail.csv` — every ticker/date row compared, with the diff and match status
- `reconciliation_summary.csv` — mismatch rate and average deviation, rolled up by ticker
- `README.md` — this file

## What I'd do next

- Compare adjusted vs. unadjusted closing prices, more likely to actually surface a real mismatch
- Add a third data source and see how you'd resolve a disagreement between three providers instead of two
- Try the same comparison on intraday data instead of daily closes, where timing differences matter more

---


