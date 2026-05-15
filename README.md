# Stock Market Performance Tracker

End-to-end ETL pipeline collecting, storing, and analyzing 5 years of financial data across 5 stocks.

---

## What it does

Pulls historical price data from the yfinance API, reshapes it into a normalized SQLite database, then runs SQL window functions to compute rolling averages and trend analysis — turning raw price data into actionable insights.

**Stocks tracked:** JPM · GS · BAC · V · MA (~6,300 trading records)

---

## Key queries

**30-day rolling average**
```sql
SELECT
    ph.date,
    ph.close,
    ROUND(AVG(ph.close) OVER (
        ORDER BY ph.date
        ROWS BETWEEN 29 PRECEDING AND CURRENT ROW
    ), 2) as rolling_30day_avg
FROM price_history ph
JOIN stocks s ON ph.stock_id = s.stock_id
WHERE s.ticker = 'JPM'
```

**Average price by stock (2025)**
```sql
SELECT s.ticker, ROUND(AVG(ph.close), 2) as avg_price
FROM price_history ph
JOIN stocks s ON ph.stock_id = s.stock_id
WHERE ph.date >= '2025-01-01'
GROUP BY s.ticker
ORDER BY avg_price DESC
```

---

## Pipeline

```
yfinance API → pandas (reshape) → SQLite DB → SQL queries → visualizations
```

---

## Stack

`Python` `SQL` `SQLite` `Pandas` `yfinance` `Matplotlib` `Seaborn`

---

## Run it

```bash
git clone https://github.com/hxxisq/stock-market-tracker.git
cd stock-market-tracker
pip install -r requirements.txt
jupyter notebook Stock_Market_Performance_Tracker.ipynb
```
