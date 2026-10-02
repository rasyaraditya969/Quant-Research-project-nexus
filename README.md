# NEXUS-QRM
## Macro, News & Market Quant Research Engine

> **NEXUS-QRM (Quantitative Regime & Market Intelligence)** is a quantitative research and systematic market analysis framework designed to study market regimes, momentum, volatility, cross-asset relationships, sector rotation, and rule-based portfolio strategies.

The project is built as a research framework rather than a single trading strategy. It combines market data, quantitative signals, macroeconomic regime analysis, portfolio construction, backtesting, statistical validation, and robustness testing into a single research pipeline.

---

## Research Objective

The primary objective of NEXUS-QRM is to investigate whether systematic market signals can provide useful information about:

- Market momentum
- Volatility regimes
- Risk-on / risk-off conditions
- Sector leadership
- Cross-asset relationships
- Macro-economic regimes
- Portfolio allocation
- Strategy robustness

The framework is designed to evaluate both **signal behavior** and **strategy performance**, rather than relying only on historical returns.

---

# 1. Research Architecture

```text
                         MARKET DATA
                              │
                              ▼
                  ┌─────────────────────┐
                  │   DATA COLLECTION   │
                  │   Yahoo Finance     │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ FEATURE ENGINEERING │
                  │                     │
                  │ Momentum             │
                  │ Volatility           │
                  │ VIX                  │
                  │ Breadth              │
                  │ Relative Strength    │
                  │ Trend                │
                  └──────────┬──────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        MARKET REGIME   SECTOR ROTATION  CROSS-ASSET
              │              │              │
              └──────────────┼──────────────┘
                             ▼
                  ┌─────────────────────┐
                  │   SIGNAL ENGINE      │
                  │                     │
                  │ Momentum             │
                  │ Trend                │
                  │ Relative Strength    │
                  │ Low Volatility       │
                  │ Macro Overlay        │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ PORTFOLIO / MODEL   │
                  │ CONSTRUCTION        │
                  └──────────┬──────────┘
                             │
                             ▼
                       BACKTEST ENGINE
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        Benchmarking    Transaction      Risk Metrics
                         Costs
              │              │              │
              └──────────────┼──────────────┘
                             ▼
                  ┌─────────────────────┐
                  │ ROBUSTNESS TESTING  │
                  │                     │
                  │ Parameter Search     │
                  │ Walk-Forward         │
                  │ Out-of-Sample       │
                  │ Bootstrap            │
                  │ Monte Carlo          │
                  └──────────┬──────────┘
                             │
                             ▼
                     RESEARCH REPORT

2. Research Universe

The framework covers multiple financial-market categories.

Equity Indices
S&P 500
Nasdaq-100
Dow Jones Industrial Average
Russell 2000
Volatility
VIX
Macro / Rates
US 10-Year Treasury yield proxy
US 2-Year Treasury yield proxy
US Dollar Index
Commodities
Gold
Silver
Crude Oil
Copper
Equity Sectors
Technology
Financials
Energy
Health Care
Industrials
Consumer Discretionary
Consumer Staples
Materials
Communication Services
Utilities
Real Estate

The research universe is configurable and can be expanded to additional instruments.

3. Data Pipeline

Historical market data is collected using yfinance.

The default research period begins in:

2018-01-01

The data pipeline performs:

Historical data retrieval
Adjusted price extraction
Multi-asset alignment
Missing-value handling
Return calculation
Feature construction

Periodic return:

$$ R_t = \frac{P_t}{P_{t-1}} - 1 $$

where:

\(P_t\) = price at time \(t\)
\(P_{t-1}\) = previous-period price
\(R_t\) = periodic return
4. Quantitative Feature Engineering

NEXUS-QRM transforms raw market prices into quantitative features used throughout the research pipeline.

4.1 Momentum

The framework calculates price momentum across multiple lookback horizons.

The general momentum formulation is:

$$ M_t^{(n)} = \frac{P_t}{P_{t-n}}-1 $$

where:

\(M_t^{(n)}\) = momentum over \(n\) periods
\(P_t\) = current price
\(P_{t-n}\) = price \(n\) periods earlier

The research includes multiple horizons such as:

5-day
10-day
20-day
30-day
60-day
4.2 Realized Volatility

Annualized realized volatility is calculated from daily returns:

$$ \sigma_{annual} = \sigma_{daily}\sqrt{252} $$

Rolling volatility is used to evaluate changes in market risk and as part of the regime framework.

4.3 VIX Analysis

VIX is incorporated as a market volatility and risk-regime variable.

The framework also calculates a rolling VIX z-score:

$$ Z_t = \frac{VIX_t-\mu_t}{\sigma_t} $$

This provides a normalized measure of current volatility relative to its recent history.

4.4 Sector Breadth

Sector breadth measures the proportion of sector ETFs trading above their 50-day moving average.

$$ Breadth_t = \frac{\#\{Sector_i > MA50_i\}}{N} $$

This provides a market-participation measure that can be incorporated into the regime analysis.

5. Macro / Market Regime Model

NEXUS-QRM constructs a transparent market-regime score using:

S&P 500 momentum
US Dollar momentum
S&P 500 volatility
Sector breadth

The conceptual growth score is:

$$ GrowthScore = Z(Momentum) - Z(DXY) - Z(Volatility) + Z(Breadth) $$

The framework uses this score to study market conditions such as:

Expansion / Risk-On
        │
        ▼
    Transition
        │
        ▼
Contraction / Risk-Off

The regime framework is a quantitative research heuristic and is not intended to represent an official economic recession classification.

6. Sector Rotation Model

NEXUS-QRM includes a systematic sector-rotation model covering 11 US equity sectors.

The model evaluates sectors using four quantitative factors:

Momentum              35%
Relative Strength     25%
Trend                 20%
Low Volatility        20%

The composite score is:

$$ Score = 0.35(Momentum) + 0.25(RelativeStrength) + 0.20(Trend) + 0.20(LowVolatility) $$

The framework then applies a macro-regime overlay to the sector-selection process.

7. Cross-Sectional Normalization

Factor values are normalized cross-sectionally using z-scores.

$$ Z_{i,t} = \frac{X_{i,t}-\mu_t} {\sigma_t} $$

This allows factors with different numerical scales to be combined into a unified scoring framework.

8. Portfolio Construction

The sector rotation model ranks the available sectors and selects the highest-ranked securities.

Default configuration:

Selected sectors: 3
Rebalancing frequency: Monthly

The selected sectors are weighted using inverse volatility.

$$ w_i = \frac{1/\sigma_i} {\sum_j 1/\sigma_j} $$

This approach assigns relatively lower portfolio weight to higher-volatility sectors and relatively higher weight to lower-volatility sectors.

9. Backtesting Framework

The portfolio backtest evaluates historical strategy behavior using target portfolio weights.

The implementation separates signal generation from portfolio returns.

Conceptually:

Signal at t
     │
     ▼
Target Portfolio Weight
     │
     ▼
Shift Forward
     │
     ▼
Portfolio Return

Portfolio weights are shifted by one trading day before being applied to subsequent asset returns in order to reduce look-ahead bias.

10. Transaction Cost Modeling

Transaction costs are incorporated into the portfolio backtest.

Portfolio turnover is estimated from changes in portfolio weights:

$$ Turnover_t = \sum_i |w_{i,t}-w_{i,t-1}| $$

Transaction cost:

$$ Cost_t = Turnover_t \times \frac{TC_{bps}}{10,000} $$

The sector-rotation implementation uses a configurable transaction-cost assumption, with the current model using:

Transaction Cost = 5 bps
11. Benchmarking

The sector rotation model can be evaluated against passive alternatives including:

Equal-Weight Sector Portfolio

An equal-weight allocation across the sector universe.

SPY Buy & Hold

A passive S&P 500 equity benchmark.

The objective is to compare systematic sector allocation with simpler portfolio construction approaches.

12. Performance Metrics

The framework evaluates multiple dimensions of strategy performance.

Total Return
$$ R_{total} = \prod_{t=1}^{T}(1+R_t)-1 $$
CAGR
$$ CAGR = \left(\frac{V_T}{V_0}\right)^{1/Y}-1 $$
Annualized Volatility
$$ \sigma_{annual} = \sigma_{daily}\sqrt{252} $$
Sharpe Ratio
$$ Sharpe = \frac{E[R_p-R_f]} {\sigma(R_p-R_f)} \sqrt{252} $$
Sortino Ratio

The Sortino ratio evaluates returns relative to downside deviation.

Maximum Drawdown
$$ DD_t = \frac{V_t}{\max(V_1,\ldots,V_t)}-1 $$

Maximum drawdown is the minimum observed drawdown.

Calmar Ratio
$$ Calmar = \frac{CAGR}{|MaximumDrawdown|} $$
Win Rate
$$ WinRate = \frac{\#(R_t>0)}{N} $$
Profit Factor
$$ ProfitFactor = \frac{\sum PositiveReturns} {|\sum NegativeReturns|} $$
13. Transparent SPX Regime Strategy

The project also contains a transparent SPX regime strategy designed to make the signal construction auditable.

The strategy combines:

20-day SPX momentum
Volatility filter
Macro / growth regime score
Long Condition
SPX 20D Momentum > 0
AND
Growth Score > 0
AND
SPX Volatility < Rolling Median Volatility
Short Condition
SPX 20D Momentum < 0
AND
Growth Score < 0

The resulting position is shifted forward before calculating strategy returns.

14. Parameter Optimization

NEXUS-QRM includes parameter exploration using a development/training sample.

Momentum lookbacks:

5
10
20
30
60

Volatility lookbacks:

10
20
30
60

The optimization evaluates:

Sharpe Ratio
CAGR
Maximum Drawdown

The final holdout period is kept separate from the initial parameter-search process.

15. Walk-Forward Validation

Walk-forward analysis is used to evaluate whether parameter selections remain useful on unseen observations.

The process follows:

Historical Training Window
          │
          ▼
Parameter Selection
          │
          ▼
Unseen Test Window
          │
          ▼
Record Out-of-Sample Result
          │
          ▼
Roll Forward
          │
          ▼
Repeat

Default configuration:

Training Window = 756 trading days
Testing Window  = 126 trading days

For each walk-forward period, parameters are selected using the preceding training window and then evaluated on the following unseen test window.

16. Out-of-Sample Holdout

The framework also reserves a final holdout period for independent evaluation.

Conceptually:

Historical Dataset
│
├─────────────────────────┐
│                         │
│     Development         │
│                         │
├─────────────────────────┤
│                         │
│     Final Holdout       │
│                         │
└─────────────────────────┘

The holdout is intended to provide an additional check against evaluating the model exclusively on observations used during research and parameter development.

17. Bootstrap / Monte Carlo Analysis

NEXUS-QRM includes bootstrap-based simulation using historical strategy returns.

The process:

Collect historical strategy returns.
Resample returns with replacement.
Generate simulated equity paths.
Calculate terminal-equity distributions.
Analyze selected percentiles.

Default:

Simulations = 2,000

The framework reports:

5th Percentile
50th Percentile
95th Percentile

of simulated terminal equity.

The simulation is intended as a robustness analysis and not as a prediction of future performance.

18. News & Sentiment Research

The project contains an optional financial-news research module.

The research pipeline is:

Google News RSS
      │
      ▼
Headline Collection
      │
      ▼
Text Processing
      │
      ▼
VADER Sentiment
      │
      ▼
Sentiment Score
      │
      ▼
Positive / Neutral / Negative

The research module can monitor topics including:

S&P 500
Nasdaq
Federal Reserve
Inflation
CPI
PCE
NFP
Treasury yields
Gold
Oil
US Dollar
Technology
Semiconductor markets

The current implementation is a research prototype.

For institutional deployment, the framework would require a more robust and timestamp-accurate financial-news data source.

19. Event Study

The project also contains an event-study component for analyzing market behavior around specified dates.

Conceptually:

Event Date
    │
    ├── -5 Days
    ├── -4 Days
    ├── -3 Days
    ├── -2 Days
    ├── -1 Day
    │
    ├── EVENT
    │
    ├── +1 Day
    ├── +2 Days
    ├── +3 Days
    ├── +4 Days
    └── +5 Days

The objective is to study average market returns around an event window.

For higher-frequency institutional research, exact event timestamps and intraday data would be required.

20. Research Outputs

NEXUS-QRM produces several categories of quantitative outputs.

Market Diagnostics
Momentum
Realized volatility
VIX
Regime score
Sector breadth
Sector Analytics
Sector ranking
Composite factor scores
Relative strength
Volatility
Portfolio weights
Portfolio Analytics
Equity curve
CAGR
Sharpe Ratio
Sortino Ratio
Maximum Drawdown
Calmar Ratio
Win Rate
Profit Factor
Turnover
Robustness Analysis
Parameter sensitivity
Walk-forward validation
Out-of-sample testing
Final holdout
Bootstrap simulation
Monte Carlo analysis
News Research
Headline sentiment
Sentiment distribution
Event-window analysis
21. Technology Stack
Python
│
├── NumPy
├── Pandas
├── SciPy
├── Scikit-learn
├── Statsmodels
├── Matplotlib
├── yfinance
├── Feedparser
├── VADER Sentiment
└── pandas-datareader

Research environment:

Google Colab
Jupyter Notebook
Python 3.x
Git / GitHub
22. Repository Structure
Quant-Research-project-nexus/
│
├── README.md
│
├── Quant research/
│   └── NEXUS_QR_Quant_Research_Colab.ipynb
│
├── sector_rotation_quant.py
│
└── .gitignore

Recommended future research structure:

quant-nexus/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   └── NEXUS_QR_Quant_Research_Colab.ipynb
│
├── src/
│   ├── data.py
│   ├── features.py
│   ├── signals.py
│   ├── portfolio.py
│   ├── backtest.py
│   └── risk.py
│
├── results/
│   ├── equity_curve.png
│   ├── drawdown.png
│   ├── parameter_sensitivity.png
│   ├── walk_forward.png
│   └── monte_carlo.png
│
└── data/
23. Reproducibility

Install the required Python packages:

pip install numpy pandas scipy scikit-learn statsmodels matplotlib yfinance feedparser vaderSentiment pandas-datareader

The main research notebook can be executed in:

Google Colab
Jupyter Notebook

The standalone sector-rotation model can be executed with:

python sector_rotation_quant.py

The sector-rotation script generates research outputs including:

sector_target_weights.csv
sector_scores.csv
sector_rotation_backtest.csv
24. Research Workflow

The complete research workflow is:

HYPOTHESIS
    │
    ▼
MARKET DATA
    │
    ▼
DATA PROCESSING
    │
    ▼
FEATURE ENGINEERING
    │
    ▼
SIGNAL CONSTRUCTION
    │
    ▼
REGIME ANALYSIS
    │
    ▼
PORTFOLIO CONSTRUCTION
    │
    ▼
BACKTEST
    │
    ▼
BENCHMARK
    │
    ▼
TRANSACTION COSTS
    │
    ▼
PARAMETER ANALYSIS
    │
    ▼
WALK-FORWARD
    │
    ▼
OUT-OF-SAMPLE
    │
    ▼
BOOTSTRAP / MONTE CARLO
    │
    ▼
RESEARCH CONCLUSION
25. Research Philosophy

NEXUS-QRM follows a research-first quantitative framework.

The purpose is not simply to find a parameter combination that performs well historically.

Instead, the framework attempts to investigate:

Whether a signal persists across different periods
Whether performance survives unseen data
How sensitive the model is to parameter changes
How transaction costs affect results
How portfolio risk behaves
Whether performance is concentrated in a particular market regime
Whether simulated return paths materially change the observed risk profile

The research therefore emphasizes:

Signal
+
Validation
+
Robustness
+
Risk

rather than historical return alone.

26. Research Limitations

The current research framework has several limitations.

Historical Data

Historical relationships may not persist under future market conditions.

Data Quality

Market-data providers may contain missing observations, revisions, adjustments, or other data-quality issues.

Transaction Costs

Real-world execution costs may differ from the assumptions used in the backtest.

Slippage

The current framework does not fully model instrument-specific market impact, liquidity, and execution latency.

Regime Classification

The macro regime score is a transparent quantitative heuristic and should not be interpreted as an official economic recession classifier.

News Data

The current news module uses RSS-based headlines and basic sentiment analysis. It is not equivalent to institutional-grade financial-news infrastructure.

Parameter Risk

Parameter optimization can introduce overfitting if model development and evaluation periods are not properly separated.

Backtest Risk

Backtested results should not be interpreted as evidence of guaranteed future performance.

27. Future Research

Potential extensions include:

Multi-asset portfolio optimization
Volatility targeting
Risk parity
Dynamic position sizing
Portfolio-level VaR / CVaR
Factor attribution
Regime-conditioned alpha
Purged time-series validation
Embargoed validation
Improved transaction-cost modeling
Instrument-specific slippage
Futures contract handling
Contract-roll methodology
Macro-surprise analysis
CPI / PCE / NFP event studies
FOMC event studies
Intraday event studies
Advanced NLP for financial news
Entity extraction
News embeddings
Live signal monitoring
Production-grade backtesting
28. Author

Rasya Raditya Anggara

Undergraduate Researcher
Quantitative Finance · Systematic Research · Financial Markets

Research Interests
Quantitative Research
Systematic Trading
Financial Data Science
Statistical Modeling
Portfolio Analytics
Risk Modeling
Market Regime Analysis
Macro & Cross-Asset Research
29. Disclaimer

This repository is intended for quantitative research and educational purposes only.

The research results produced by this framework are based on historical data and model assumptions. Historical backtests do not guarantee future performance.

This project does not constitute investment advice or a recommendation to buy or sell any financial instrument.

Any real-world deployment should involve additional validation, data-quality controls, execution modeling, transaction-cost analysis, risk limits, and independent out-of-sample testing.
