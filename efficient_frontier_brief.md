# LFC MarketView: Efficient Frontier

## Project Type
Solo Jupyter Notebook

## Overview
Build a portfolio optimizer based on Modern Portfolio Theory. You start with a simple question: if you split your money between two stocks, which mix gives the best return for the risk you take? You then scale up to a portfolio of several Italian stocks and draw the efficient frontier, the set of portfolios that give the highest return for each level of risk.

This project teaches you the idea behind almost every asset allocation decision in the industry: diversification. Combining assets that do not move perfectly together lowers risk without necessarily lowering return. It is designed as a first project, so no prior finance or advanced Python is required.

## What You'll Build
A complete Jupyter notebook implementing:
1. Download price data for Italian large-cap stocks (FTSE MIB)
2. Calculate returns, volatility and correlations
3. Build the risk/return curve for a two-stock portfolio
4. Simulate 5,000 random multi-stock portfolios and draw the efficient frontier
5. Compare the optimal portfolio vs an equal-weight portfolio and the FTSE MIB

## Important
The use of Generative AI (Claude) is encouraged, however it remains essential that the student understands and is able to explain without the use of the technology any design choices/implementations in the code.

## Learning Objectives
- Download and clean financial data with pandas and yfinance
- Calculate daily and annualized returns and volatility
- Understand correlation and why it drives diversification
- Use basic matrix operations in NumPy (weights × covariance matrix)
- Calculate and interpret the Sharpe ratio
- Understand the limits of optimizing on past data

## PART 1: Data, Returns & Correlation

**Goal**: Download stock data and describe each stock's risk and return

**Stocks to Use** (Borsa Italiana, Yahoo Finance tickers):
```
Energy:       ENI.MI     (Eni)
Banks:        ISP.MI     (Intesa Sanpaolo)
              UCG.MI     (UniCredit)
Utilities:    ENEL.MI    (Enel)
Luxury/Auto:  RACE.MI    (Ferrari)
Insurance:    G.MI       (Generali)
Benchmark:    FTSEMIB.MI (FTSE MIB index)
```

**Core Formulas**:
```python
daily_returns   = prices.pct_change()
annual_return   = daily_returns.mean() * 252
annual_vol      = daily_returns.std() * sqrt(252)
```

**Tasks**:
- Download 5+ years of daily prices for all stocks
- Plot normalized prices (all start at 1) on the same chart
- Calculate daily returns, annualized return and annualized volatility
- Build a correlation heatmap
- Identify the most and least correlated pairs

**Deliverables**:
- [ ] Normalized price chart for all stocks
- [ ] Table: annual return and volatility per stock
- [ ] Correlation heatmap
- [ ] README: Explain what volatility measures and what correlation measures

## PART 2: The Two-Stock Portfolio

**Goal**: See diversification happen with just two assets

**Portfolio Formulas** (weight w in stock A, 1 − w in stock B):
```python
port_return = w * ret_A + (1 - w) * ret_B
port_vol    = sqrt(w**2 * vol_A**2 + (1-w)**2 * vol_B**2
                   + 2 * w * (1-w) * corr_AB * vol_A * vol_B)
```

**Tasks**:
- Pick two stocks with low correlation
- Loop over weights from 0% to 100% in 1% steps
- Calculate portfolio return and volatility for each weight
- Plot the risk/return curve and label the two 100% points
- Find and mark the minimum-variance portfolio
- Repeat with two highly correlated stocks and compare the curves

**Deliverables**:
- [ ] Risk/return curve for a low-correlation pair
- [ ] Minimum-variance portfolio marked with its weights
- [ ] Same curve for a high-correlation pair
- [ ] README: Explain why the curve bends, and why it bends less when correlation is high

## PART 3: Multi-Asset Efficient Frontier

**Goal**: Find the best portfolios from all 6 stocks and test them

**Metrics to Calculate**:
```python
Return      = weights @ annual_returns
Volatility  = sqrt(weights.T @ cov_matrix @ weights)
Sharpe      = (Return - risk_free) / Volatility   # risk_free = 2%
Max DD      = max peak-to-trough loss
```

**Visualizations**:
1. Scatter plot of 5,000 random portfolios coloured by Sharpe ratio
2. Max-Sharpe and min-variance portfolios marked with stars
3. Pie or bar chart of the weights of each optimal portfolio
4. Growth of €10,000: max-Sharpe vs equal-weight vs FTSE MIB

**Tasks**:
- Calculate the annualized covariance matrix
- Generate 5,000 random weight combinations (weights sum to 1, no shorting)
- Calculate return, volatility and Sharpe for each
- Plot the frontier and mark the two optimal portfolios
- Backtest the optimal weights vs equal-weight vs FTSE MIB
- Analyse: are the "optimal" weights sensible? Would you invest in them?

**Deliverables**:
- [ ] Efficient frontier scatter plot
- [ ] Max-Sharpe and min-variance weights printed and charted
- [ ] Growth of €10k chart vs FTSE MIB
- [ ] Performance comparison table
- [ ] README: Explain the Sharpe ratio and the weaknesses of optimizing on past data


**Performance Table Example**:
| Metric | Max Sharpe | Min Variance | Equal Weight | FTSE MIB |
|--------|------------|--------------|--------------|----------|
| Annual Return | 18.4% | 9.1% | 13.2% | 10.5% |
| Volatility | 21% | 15% | 19% | 20% |
| Sharpe | 0.78 | 0.47 | 0.59 | 0.43 |
| Max DD | -32% | -27% | -36% | -38% |

**Optional Slides** (3 slides):
1. Theory: Diversification, correlation and the efficient frontier
2. Key results: Frontier chart, optimal weights, growth chart
3. Insights: What works, what does not, real-world limitations

## Resources

**Learning Python**:
- Kaggle Python course: https://www.kaggle.com/learn/python
- Kaggle Pandas course: https://www.kaggle.com/learn/pandas

**Key Concepts**:
- Returns: pandas.DataFrame.pct_change()
- Covariance matrix: pandas.DataFrame.cov()
- Random weights: numpy.random.default_rng().random(), then divide by the sum
- Matrix multiplication: the @ operator in NumPy
- Optimization (stretch): scipy.optimize.minimize()
