# Applying Machine Learning and Search Methods for S&P500 Stock Portfolio Forecasting and Optimization

## Overview
This project presents an end-to-end approach for predicting stock prices and optimizing investment portfolios using data from the S&P500. We combine **Deep Learning** (Long Short-Term Memory networks) and **Classical Machine Learning** (Random Forest Regressor) to forecast stock behavior, and then leverage **Search Methods** (Monte Carlo Simulation) and **Genetic Algorithms** to optimize portfolio allocations under various risk and budget constraints.

## Goals
1. **Predict stock behavior** for January 2024 based on historical data (2010–2023).  
2. **Optimize portfolios** for risk-adjusted returns using:
   - Monte Carlo Tree Search (MCTS)  
   - Genetic Algorithm  

## Key Components

### 1. Data Extraction & Preprocessing
- **Data Source**  
  - S&P500 list: Downloaded from [Wikipedia](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies).  
  - Stock price data: Collected via the [yfinance](https://pypi.org/project/yfinance/) module.  
- **Cleaning & Formatting**  
  - Removal of NaN values and inconsistent entries.  
  - Standardized date formats, columns, and data types.  
  - Ensured consistent time ranges (Jan 2010–Dec 2023).

### 2. Exploratory Data Analysis
- Verified distribution of daily closing prices (e.g., **Shapiro-Wilk** test for normality).  
- Analyzed volume trends, detecting outliers and potential volatility.  
- Filtered out tickers lacking a full 13-year record.

### 3. Model Development
1. **LSTM**  
   - Two-layer LSTM architecture, tuned for window size, dropout, and learning rate.  
   - Training on data up to 2022, validated on 2023, tested in January 2024.  
   - Used **SHAP** for explainability of feature contributions.
2. **Random Forest Regressor**  
   - Implemented for comparison.  
   - Used feature engineering (RSI, MACD, etc.) but encountered **overfitting** issues if not carefully aligned with time-series causality.  
   - Successfully identified **directional** trends but less accurate on absolute price predictions.

### 4. Portfolio Optimization
1. **Monte Carlo Simulation**  
   - Multiple simulation runs over the January 2024 period, evaluating random buy/sell decisions guided by the LSTM signals.  
   - Tracked resulting daily portfolio values vs. the S&P500 baseline.  
   - Found that while certain random runs outperformed the index, the average often underperformed a simple buy-and-hold approach.
2. **Genetic Algorithm**  
   - Used predicted returns to compute each portfolio’s **Sharpe Ratio**.  
   - Evolved allocations via selection, crossover, mutation, and elitism.  
   - Frequently yielded higher Sharpe ratios than naive equal-weight baselines.

### 5. Business Applicability
- **Decision-Support**: Aids in highlighting potential investment strategies and identifying promising stocks.  
- **Risk & Liquidity**: High-volume or stable-volatility stocks may be prioritized based on an investor’s risk tolerance.  
- **Limitations**:  
  - Only uses historical prices and technical indicators.  
  - Unaware of external factors (e.g., macroeconomic changes, earnings reports).

### 6. Ethics
- Data used is public (S&P500 / Yahoo Finance).  
- Tool is designed for transparent, responsible usage—**not** a guarantee of profits.  
- Users must combine predictions with fundamental and macro-level analysis.


## Usage
1. **Install Dependencies**  
   - Create a virtual environment, then install packages with:
     ```bash
     pip install -r requirements.txt
     ```
2. **Data Collection**  
   - Run scripts or notebooks that download S&P500 data via `yfinance`.  
   - Verify `raw_csvs` or relevant folders exist.
3. **Data Preprocessing**  
   - Execute cleaning notebooks to remove inconsistencies.  
   - Verify cleaned data is saved for consistent training.
4. **Model Training & Evaluation**  
   - **LSTM**: `lstm.ipynb`  
   - **Random Forest**: `random_forest.ipynb`
5. **Portfolio Simulation**  
   - **Monte Carlo**: `montecarlo.ipynb`  
   - **Genetic Algorithm**: `genetic_algorithm.ipynb`

## Results
- **LSTM**: Captured overall trends and outperformed naive baselines, though struggled with high-volatility stocks.  
- **Random Forest**: Reasonable at detecting *directional* changes but hampered by time-series causality constraints.  
- **Monte Carlo**: Occasionally beat the market in specific runs, average case typically below an index buy-and-hold.  
- **Genetic Algorithm**: Showed promising Sharpe ratio improvements vs. naive allocations.
