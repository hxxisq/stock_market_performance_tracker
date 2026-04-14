# Stock Market Performance Tracker

**An end-to-end ETL pipeline analyzing 5 years of financial data with advanced SQL and Python visualization**

![Python](https://img.shields.io/badge/Python-3.8+-blue)
![SQLite](https://img.shields.io/badge/Database-SQLite-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## Project Overview

This project demonstrates a complete **data engineering workflow**: collecting raw financial data from APIs, transforming it into a normalized relational database, and deriving insights through SQL analysis and visualization.

**Real-world challenge solved:** Financial APIs return data in wide, denormalized formats that are difficult to query at scale. Building a proper database schema and writing efficient SQL queries reveals meaningful patterns in stock performance.

### Key Features
- **5 years** of historical stock data (~6,300 trading records)
- **5 finance sector stocks** (JPM, GS, BAC, V, MA)
- **Advanced SQL:** Window functions, rolling averages, aggregations
- **3 visualizations:** Trend analysis, moving averages, price comparisons
- **Normalized database design** with foreign keys

---

## Stocks Analyzed

| Ticker | Company | Avg Price (2025) |
|--------|---------|------------------|
| JPM | JPMorgan Chase | $275.22 |
| GS | Goldman Sachs | $676.34 |
| BAC | Bank of America | $46.35 |
| V | Visa | $341.56 |
| MA | Mastercard | $554.96 |

---

## Technical Stack

| Component | Technology |
|-----------|------------|
| **Data Source** | yfinance API |
| **Data Processing** | pandas |
| **Database** | SQLite3 |
| **SQL** | Advanced queries (window functions, rolling averages) |
| **Visualization** | matplotlib, seaborn |
| **Language** | Python 3.8+ |

---

## Project Structure

```
stock-market-tracker/
├── README.md                              # This file
├── Stock_Market_Performance_Tracker.ipynb  # Main notebook
├── requirements.txt                        # Dependencies
└── data/
    └── stock_tracker.db                   # SQLite database (generated)
```

---

## Getting Started

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/stock-market-tracker.git
cd stock-market-tracker
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Run the notebook**
```bash
jupyter notebook Stock_Market_Performance_Tracker.ipynb
```

---

## Visualizations Generated

### 1. Six-Month Price Trends
Line chart showing all 5 stocks over the last 6 months
- **Insight:** JPM and V price movements are correlated
- **Insight:** GS exhibits highest volatility

### 2. Rolling Average Analysis (JPM)
Close price vs 30-day moving average
- **Insight:** Moving averages smooth daily noise and reveal underlying trends
- **Insight:** Moving averages lag behind price spikes

### 3. 2025 Average Price Comparison
Bar chart comparing average closing prices across all stocks
- **Insight:** GS had the highest average price ($676.34)
- **Insight:** BAC had the lowest average price ($46.35)

---

## Key Queries

### Average Price by Stock (2025)
```sql
SELECT 
    s.ticker,
    ROUND(AVG(ph.close), 2) as avg_price
FROM price_history ph
JOIN stocks s ON ph.stock_id = s.stock_id
WHERE ph.date >= '2025-01-01'
  AND ph.date < '2026-01-01'
GROUP BY s.ticker
ORDER BY avg_price DESC
```

### 30-Day Rolling Average (JPM)
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
ORDER BY ph.date
```

---

## Key Insights

1. **Price Correlation:** JPM and Visa (V) stock prices move together
2. **Volatility:** Goldman Sachs (GS) exhibits highest price volatility
3. **Trends:** 30-day moving averages reveal underlying market trends masked by daily noise
4. **Price Range:** Finance stocks range from $46 (BAC) to $676 (GS)

---

## Architecture

```
Raw Data (yfinance)
        ↓
    Wide Format (Date × Stocks)
        ↓
   Data Reshape (pandas melt)
        ↓
    Long Format (Date-Stock pairs)
        ↓
   SQLite Database
        ├── stocks (reference table)
        └── price_history (fact table)
        ↓
   SQL Queries
        ├── Aggregations
        ├── Window Functions
        └── Rolling Averages
        ↓
   Visualizations
        ├── Line charts
        ├── Overlays
        └── Bar charts
```

---

## Extending This Project

### Short-term
- Add more stocks or sectors
- Calculate additional metrics (volatility, correlation)
- Create more visualizations (heatmaps, scatter plots)

### Medium-term
- Add technical indicators (RSI, MACD, Bollinger Bands)
- Implement comparative analysis with market indices (S&P 500)
- Create a web dashboard (Streamlit/Flask)

### Long-term
- Migrate from SQLite to PostgreSQL/MySQL
- Add machine learning for price prediction
- Build a real-time data pipeline (Apache Airflow)
- Deploy as a production service

---

## Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Data in wide format (columns per stock) | Used pandas `.melt()` to reshape to long format |
| Efficient rolling averages | Learned SQL window functions with `ROWS BETWEEN` |
| Matplotlib date labels overlapping | Increased figure size and rotated labels |
| Understanding window functions | Built examples with visual output to clarify |

---

## Dependencies

```
yfinance==0.2.32      # Financial data API
pandas==2.0.3         # Data manipulation
matplotlib==3.7.2     # Visualization
seaborn==0.12.2       # Enhanced matplotlib
sqlite3               # Database (built-in)
```

See `requirements.txt` for exact versions.

---

## Learning Outcomes

By working through this project, I understood:
- How real-world financial data flows through an ETL pipeline
- Database design principles (normalization, foreign keys)
- Advanced SQL (window functions, rolling averages)
- Data visualization best practices
- Time-series analysis fundamentals

---

## Notes for Interviewers

This project demonstrates:
1. **Full-stack data engineering:** Data → Database → Analysis → Visualization
2. **SQL mastery:** Window functions and rolling averages are advanced concepts
3. **Problem-solving:** Reshaped wide data, optimized queries, debugged issues
4. **Communication:** Well-documented code and clear visualizations

---

## Contributing

Found a bug or have an improvement? Feel free to:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request

---

## Author

**Benjamin Shado**  
Data Analyst | Data Scientist 
[GitHub](https://github.com/hxxisq) | [LinkedIn](https://linkedin.com/in/benjaminshado)

---

## Resources

- [yfinance Documentation](https://github.com/ranaroussi/yfinance)
- [SQLite Window Functions](https://www.sqlite.org/windowfunctions.html)
- [Pandas Reshaping Data](https://pandas.pydata.org/docs/reference/reshaping.html)
- [Matplotlib Documentation](https://matplotlib.org/stable/contents.html)

---

**Last Updated:** April 14, 2026
