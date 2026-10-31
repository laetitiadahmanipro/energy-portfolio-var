 ## Market Risk of an Energy Portfolio (Python)

A market risk analysis of a fictional energy portfolio: 50% Brent crude oil and 50% US natural gas, with a value of USD 10 million.

The project covers daily returns, Value at Risk (VaR), Expected Shortfall (ES), Monte Carlo simulation, backtesting, stress tests and PnL attribution.

## Data

- Source: Yahoo Finance, using the `yfinance` library
- Series: Brent futures (`BZ=F`) and Henry Hub natural gas futures (`NG=F`)
- Period: January 2020 to October 2026 (1,718 daily returns)

## Methods

1. Daily returns of each asset and of the portfolio (constant 50/50 weights)
2. Historical and parametric (normal) VaR at 95% and 99%, 1 day
3. Expected Shortfall (ES) for both methods
4. Monte Carlo VaR and ES (100,000 simulations, normal and Student-t distributions)
5. Backtesting with a rolling 250-day window and the Kupiec test
6. Stress tests with simple price shocks
7. PnL attribution by asset (worst days and yearly PnL)

## Main results

| | Brent | Natural gas | Portfolio |
|---|---|---|---|
| Daily volatility | 2.79% | 4.88% | 2.92% |
| Worst day | -24.4% | -47.5% | -22.1% |

| 1-day risk measure | 95% VaR | 95% ES | 99% VaR | 99% ES |
|---|---|---|---|---|
| Historical | 4.31% | 6.36% | 7.28% | 10.30% |
| Parametric (normal) | 4.69% | 5.91% | 6.68% | 7.67% |
| Monte Carlo (normal) | 4.68% | 5.90% | 6.70% | 7.62% |
| Monte Carlo (Student-t) | 4.31% | 6.48% | 7.60% | 10.46% |

On a USD 10 million portfolio, the historical 99% VaR is about USD 728k and the historical 99% ES is about USD 1.03M.

| Backtest (1,468 days) | Breaches | Expected | Breach rate | Kupiec p-value |
|---|---|---|---|---|
| Historical 95% | 75 | 73.4 | 5.11% | 0.849 |
| Parametric 95% | 61 | 73.4 | 4.16% | 0.127 |
| Historical 99% | 24 | 14.7 | 1.63% | 0.025 |
| Parametric 99% | 26 | 14.7 | 1.77% | 0.007 |

| Stress scenario | Portfolio return | PnL (USD) |
|---|---|---|
| Brent -20% | -10.0% | -1.0M |
| Gas -30% | -15.0% | -1.5M |
| Brent -20% and Gas -30% | -25.0% | -2.5M |
| Worst day in the data (29 Jan 2026) | -22.05% | -2.2M |

## Key findings

- Natural gas is almost twice as volatile as Brent, so it drives most of the portfolio risk.
- At 99%, the normal distribution underestimates tail risk: the parametric ES (7.67%) is far below the historical ES (10.30%). Energy returns have fat tails.
- A Monte Carlo simulation with a Student-t distribution (fat tails) gets much closer to the historical results (99% ES of 10.46%).
- Both models pass the 95% backtest but fail at 99% (Kupiec test, p < 0.05). The parametric model is the worst.
- Breaches are clustered in crisis periods (late 2021 to mid 2022, early 2026): a 250-day window reacts too slowly when volatility jumps.
- The worst real day (-USD 2.2M) is about three times the 99% VaR. VaR describes normal days, not crises, so stress tests are needed.
- Gas explains most of the worst days since 2022. The worst days of 2020 come from Brent.

## Assumptions and limitations

- Fictional portfolio, rebalanced to 50/50 every day. This is not a real trading result.
- Yahoo Finance continuous futures are used. Contract rolls can create price jumps that are not real market moves.
- Only linear exposure to futures. No options and no Greeks.
- The Student-t model uses 4 degrees of freedom, chosen as an assumption and not estimated from the data.
- Monte Carlo and stress tests assume a constant correlation between Brent and gas.
- The Kupiec test counts breaches but does not test whether they are clustered.
- Historical VaR depends on the chosen period (2020 to 2026 includes Covid and the 2022 energy crisis).

## How to run

Open `energy_portfolio_var.ipynb` in Google Colab, or install the packages in `requirements.txt` and run it in Jupyter.

## Possible next steps

- Volatility models (EWMA or GARCH) to react faster in crises
- Add electricity prices
- Options pricing and Greeks (separate project)
