# Quantitative Portfolio Simulator

A **comprehensive Python-based equity portfolio management system** that implements and compares multiple quantitative trading strategies using real market data. Built around pandas and NumPy for high-performance financial analysis, the simulator tests algorithmic trading approaches on a $5M virtual fund across a universe of 10 major tech stocks during 2018.

## Project Overview

The simulator evaluates **three distinct rebalancing strategies** over a full trading year, incorporating real-world complexities like dividend payments, transaction timing, and market volatility. Each strategy rebalances portfolios at configurable intervals (default: 5 trading days) and tracks performance through comprehensive Mark-to-Market (MTM) analysis.

**Key Features:**
- **Real market data integration** from Yahoo Finance for 10 major stocks (IBM, MSFT, GOOG, AAPL, AMZN, META, NFLX, TSLA, ORCL, SAP)[1]
- **Multiple trading strategies** with performance comparison capabilities
- **Dividend handling** with automatic reinvestment calculations
- **Custom high-tech index** creation for benchmark comparison
- **Multi-currency analysis** supporting USD/JPY conversions
- **Comprehensive visualization** with matplotlib/seaborn integration

## Repository Structure

```
Python_project/
├── Datasets/              → Historical stock data (CSV files)
│   ├── AAPL.csv          → Apple Inc. 2018 daily prices
│   ├── AMZN.csv          → Amazon.com Inc. data
│   ├── GOOG.csv          → Alphabet Inc           → International Business Machines
│   ├── META.csv          → Meta Platforms Inc.
│   ├── MSFT.csv          → Microsoft Corporation
│   ├── NFLX.csv          → Netflix Inc.
│   ├── ORCL.csv          → Oracle Corporation
│   ├── SAP.csv           → SAP SE
│   └── TSLA.csv          → Tesla Inc.
├── Project.ipynb         → Initial project specification & skeleton
├── Project_Test1.ipynb   → Development notebook with strategy prototypes
├── Stock_Price_Analysis_PreFinal.ipynb → Complete implementation
├── README.md             → Basic project description
└── .gitignore           → Python-specific exclusions
```

## Trading Strategies

### Strategy 1: Momentum Trading (Buy High)
Selects the **5 highest-priced stocks** on each rebalancing day, following momentum theory that high-performing assets continue their upward trajectory[1].

### Strategy 2: Contrarian Trading (Buy Low) 
Identifies stocks with the **largest percentage drops** in adjusted close prices over the rebalancing period, implementing a "buy the dip" approach that capitalizes on temporary market corrections[1].

### Strategy 3: Contrarian Premium (Buy Rising)
Targets stocks showing the **highest percentage gains** in adjusted close prices, betting on continued upward momentum in already-rising securities[1].

## Core Components

### Data Management
The system consolidates historical data into a unified DataFrame with 20 columns (Close + Adj Close for each stock), enabling efficient vectorized operations for portfolio calculations[1].

```python
# Core data structure spans 250 trading days in 2018
all_stocks = pd.DataFrame(columns=[
    'ibm_Close', 'ibm_AdjClose', 'msft_Close', 'msft_AdjClose',
    # ... continues for all 10 stocks
])
```

### Portfolio Mechanics

**Initial Setup:** $5M fund split equally into 5 stock positions ($1M each) on January 2, 2018[1].

**Rebalancing Logic:**
1. Liquidate all current positions at market close prices
2. Apply selected strategy to identify 5 target stocks  
3. Redistribute total cash equally among chosen securities
4. Purchase maximum whole shares, maintaining cash remainder

**Mark-to-Market Calculation:**
```
MTM_t = cash_t + Σ(shares_k,t × close_price_k,t)
```

### Dividend Processing
Sophisticated dividend detection compares Close/Adj Close price ratios to identify dividend ex-dates, automatically crediting cash accounts with appropriate amounts based on shareholdings[1].

### Performance Analytics

**High-Tech Index:** Simple average of all 10 stocks' daily close prices, serving as a market benchmark[1].

**Percentage Change Normalization:** Both MTM and index values converted to percentage changes relative to January 2, 2018 baseline for direct comparison.

**Multi-Currency Analysis:** USD/JPY conversion capabilities for international performance assessment.

## Key Functions

| Function | Purpose | Inputs | Returns |
|----------|---------|---------|---------|
| `strategy_top_5()` | Highest-priced stock selection | day_number | stocks_to_buy dict |
| `buy_stocks()` | Portfolio allocation & purchase | stocks_to_buy, total_cash | no_of_stocks, remaining_cash |
| `totalCash_afterSell()` | Liquidation value calculation | no_of_stocks, day, cash | total_cash |
| `max_StockChange()` | Price change identification | past_day, current_day, direction | sorted change list |
| `div()` | Dividend detection & crediting | day, holdings, cash | updated_cash |
| `mtm()` | Mark-to-market valuation | holdings, cash, day | portfolio_value |

## Getting Started

### Prerequisites
- **Python 3.7+** with scientific computing stack
- **pandas ≥ 1.3.0** for data manipulation
- **numpy ≥ 1.21.0** for numerical operations  
- **matplotlib ≥ 3.4.0** for visualization
- **seaborn** for enhanced plotting capabilities

### Installation & Usage

```bash
# Clone the repository
git clone https://github.com/KaivDev4434/Python_project.git
cd Python_project

# Ensure datasets are present
ls Datasets/  # Should show 10 CSV files

# Launch Jupyter environment
jupyter notebook Stock_Price_Analysis_PreFinal.ipynb
```

### Running Analysis

Execute the main analysis function and select your preferred strategy:

```python
# Interactive strategy selection
stock_analysis()
# Choose: 1 (Buy High), 2 (Buy Low), or 3 (Buy Rising)
```

The system will:
1. Process 250 trading days of market data
2. Execute rebalancing at specified intervals  
3. Track daily MTM performance
4. Generate comparative visualizations
5. Output final portfolio value and percentage returns

## Results & Performance

The simulator demonstrates **strategy-dependent performance variations** with:
- **Strategy 2 (Buy Low)** showing consistent outperformance during 2018's volatile market conditions
- **Final portfolio values** ranging from $4.76M to $5.38M depending on chosen approach[1]
- **Comprehensive benchmarking** against the custom high-tech index

Visualization capabilities include normalized percentage change plots comparing portfolio performance against market benchmarks, enabling clear assessment of alpha generation.

## Extensions & Customization

**Configurable Parameters:**
- Rebalancing frequency (1-249 days)
- Portfolio size and initial allocation
- Stock universe composition  
- Currency conversion rates

**Potential Enhancements:**
- Transaction cost modeling
- Risk-adjusted return metrics (Sharpe ratio, maximum drawdown)
- Monte Carlo simulation capabilities
- Real-time data integration via API
- Machine learning-based strategy optimization

The modular architecture supports easy extension for additional strategies, alternative assets, or enhanced risk management features.
