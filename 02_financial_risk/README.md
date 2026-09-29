# Financial Markets & Risk Analytics

A quantitative finance project that ingests historical multi-asset market data into PostgreSQL, and applies Monte Carlo simulation and Value-at-Risk (VaR) methodologies to quantify portfolio downside risk.

## Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Methodology](#methodology)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Results](#results)
- [Technologies Used](#technologies-used)
- [License](#license)

## Overview
This project analyzes risk across a diversified asset portfolio (equities, tech growth, gold, and long-term treasuries) by:
1. Ingesting historical daily price data via `yfinance` and persisting it in a PostgreSQL database.
2. Computing daily log returns for each asset.
3. Estimating downside risk using **Historical VaR**, **Parametric (Normal) VaR**, and **Conditional VaR / Expected Shortfall (CVaR)** at the 95% and 99% confidence levels.
4. Simulating 10,000 future price paths using **Geometric Brownian Motion (GBM)** Monte Carlo simulation over a 30-day horizon.

## Dataset
- **Source**: Yahoo Finance (via `yfinance`)
- **Tickers**:
  - `SPY` — S&P 500 ETF (large-cap US equities)
  - `QQQ` — NASDAQ 100 ETF (tech growth)
  - `GLD` — Gold ETF (commodities / safe haven)
  - `TLT` — 20+ Year US Treasury Bond ETF (fixed income)
- **Date Range**: 2021-01-01 to 2026-01-01
- **Frequency**: Daily adjusted close prices
- **Storage**: PostgreSQL table `market_prices` (columns: `ticker`, `price_date`, `adj_close`, `prev_close`, `log_return`)

## Project Structure
```
02_financial_risk/
├── data_ingestion.ipynb      # Fetches price data, computes log returns, loads into PostgreSQL
├── risk_simulation.ipynb     # VaR/CVaR calculations + Monte Carlo GBM simulation
└── README.md
```

## Methodology

### 1. Data Ingestion (`data_ingestion.ipynb`)
- Downloads adjusted close prices for `SPY`, `QQQ`, `GLD`, `TLT` from 2021–2026.
- Reshapes data from wide to long format (`melt`).
- Computes daily log returns: `r_t = ln(P_t / P_{t-1})` per ticker, using a lagged (`shift(1)`) prior close.
- Writes the resulting `market_prices` table to a PostgreSQL database (`portfolio_db`).

### 2. Risk Simulation (`risk_simulation.ipynb`)
- Queries historical `SPY` log returns from PostgreSQL.
- Calculates **Historical VaR/CVaR** using empirical return percentiles (5th and 1st).
- Calculates **Parametric VaR** assuming a normal return distribution (`scipy.stats.norm.ppf`).
- Runs a **10,000-path Monte Carlo simulation** of future SPY prices over a 30-trading-day horizon using GBM:
  - `S_t = S_{t-1} * exp((μ - 0.5σ²)dt + σ√dt · Z)`
- Visualizes simulated price paths alongside the mean projected trajectory.

## Installation & Setup

### Prerequisites
- Python 3.9+
- PostgreSQL running locally (or update `DB_URL` accordingly)

### Install dependencies
```powershell
pip install pandas numpy yfinance sqlalchemy psycopg2-binary scipy matplotlib
```

### Configure database
Update the connection string in both notebooks to match your local PostgreSQL credentials:
```python
DB_URL = "postgresql://<user>:<password>@localhost:5432/portfolio_db"
```

## Usage
Run the notebooks in order:
```powershell
jupyter notebook data_ingestion.ipynb
jupyter notebook risk_simulation.ipynb
```
1. `data_ingestion.ipynb` must be run first to populate the `market_prices` table.
2. `risk_simulation.ipynb` then queries that table to perform risk analysis.

## Results
For a **$1,000,000** SPY portfolio, the notebook reports:
- Historical 95% and 99% VaR & CVaR (Expected Shortfall)
- Parametric (Normal distribution) 95% and 99% VaR
- A 30-day forward Monte Carlo simulation (10,000 paths) with visualized price trajectories and mean path

These metrics quantify the potential loss under normal market conditions (VaR) and the expected loss in the worst-case tail scenarios (CVaR), forming the basis for portfolio risk management decisions.

## Technologies Used
- **Python**: pandas, numpy, matplotlib, scipy
- **Data Source**: yfinance
- **Database**: PostgreSQL, SQLAlchemy
- **Statistical Methods**: Log returns, Parametric & Historical VaR, CVaR/Expected Shortfall, Geometric Brownian Motion Monte Carlo simulation

## License
MIT