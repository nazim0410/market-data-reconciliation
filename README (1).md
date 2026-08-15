# Market Data Reconciliation

A scoped-down data reconciliation exercise: pull the same tickers'
daily closing prices from two independent free data providers —
**Yahoo Finance** and **Alpha Vantage** — and use SQL to compare them,
flag discrepancies beyond a tolerance threshold, and surface the
results in Tableau.

This demonstrates the core reconciliation *pattern* used in capital
markets back-office work — comparing two independently-sourced
records of the same thing, classifying the differences, and
quantifying data quality — scoped to a single sitting.

**Live dashboard:** [Two-Source Price Reconciliation on Tableau Public](https://public.tableau.com/app/profile/mohammad.nazim.rehman/viz/Two-SourcePriceReconciliationYahooFinancevsAlphaVantage/Dashboard1)

---

## What it does

- Pulls daily close prices for 8 tickers (AAPL, MSFT, GOOGL, AMZN, NVDA, TSLA, GME, CVNA) over a 2-month window from Yahoo Finance and Alpha Vantage
- Loads both into an in-memory SQLite database and runs a SQL join comparing ticker/date pairs, computing absolute and percentage price differences, and flagging mismatches beyond a 0.5% tolerance
- Separately checks for **coverage gaps** — dates present in one source but missing from the other, a different class of issue than a value mismatch
- **Validates the detection logic itself** with a controlled test case (a deliberately-wrong price pair) before trusting the result against real data — this is what makes a "zero mismatches" finding credible rather than a silent failure
- Produces summary and detail tables, exported to CSV and visualized in Tableau

## Result

**344 ticker-days compared, 0 mismatches, 0 coverage gaps.**

Yahoo Finance and Alpha Vantage report identical unadjusted daily closes for all 8 tickers across the full window. This is a genuine finding, not a null result from broken logic — the detection query was validated against a synthetic 2% price gap (correctly flagged as `MISMATCH`) before being run against live data, confirming the comparison logic is sound.

In practice, cross-vendor price discrepancies typically arise from adjusted-vs-unadjusted close conventions, differing consolidated-tape sources, or refresh timing near market close — none of which were present for these liquid, large-cap names in this window. A natural extension (noted below) is comparing adjusted vs. unadjusted closes directly, which reliably surfaces real divergence around dividend/split dates.

## Run it

Open `run_in_colab.ipynb` in [Google Colab](https://colab.research.google.com) and run the cells top to bottom. You'll need a free Alpha Vantage API key (instant, no email confirmation — https://www.alphavantage.co/support/#api-key). No local setup or database server required.

## Stack

`Python` `SQL (SQLite)` `pandas` `yfinance` `Tableau`

## Files

- `run_in_colab.ipynb` — Full pipeline: fetch, compare, validate, export
- `reconciliation_detail.csv` — Every compared ticker/date row with diff and status
- `reconciliation_summary.csv` — Mismatch rate and deviation stats by ticker
- `README.md` — this file

## Next steps

- Compare adjusted vs. unadjusted close prices to surface real, explainable discrepancies around corporate actions
- Extend to a third data source for majority-vote-style discrepancy resolution
- Add intraday price comparison, where timing-driven divergence is more common than on daily closes
