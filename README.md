## Market Risk of an Energy Portfolio (Python)

A simple market risk analysis of a fictional energy portfolio: 50% Brent crude oil and 50% US natural gas, with a value of USD 10 million.

The project covers daily returns, Value at Risk (VaR), stress tests and PnL attribution.

## Data

- Source: Yahoo Finance, using the `yfinance` library
- Series: Brent futures (`BZ=F`) and Henry Hub natural gas futures (`NG=F`)
- Period: January 2020 to October 2026 (1,718 daily returns)

## Methods

1. Daily returns of each asset and of the portfolio (constant 50/50 weights)
2. Historical VaR at 95% and 99% (1 day)
3. Parametric VaR at 95% and 99% (normal distribution)
4. Stress tests with simple price shocks
5. PnL attribution by asset (worst days and yearly PnL)

## Main results

| | Brent | Natural gas | Portfolio |
|---|---|---|---|
| Daily volatility | 2.79% | 4.88% | 2.92% |
| Worst day | -24.4% | -47.5% | -22.1% |

| 1-day VaR | Historical | Parametric |
|---|---|---|
| 95% | 4.31% (USD 431k) | 4.69% (USD 469k) |
| 99% | 7.28% (USD 728k) | 6.68% (USD 668k) |

| Stress scenario | Portfolio return | PnL (USD) |
|---|---|---|
| Brent -20% | -10.0% | -1.0M |
| Gas -30% | -15.0% | -1.5M |
| Brent -20% and Gas -30% | -25.0% | -2.5M |
| Worst day in the data (29 Jan 2026) | -22.05% | -2.2M |

## Key findings

- Natural gas is almost twice as volatile as Brent, so it drives most of the portfolio risk.
- At 99%, the parametric VaR is lower than the historical VaR. The normal distribution underestimates extreme losses: energy returns have fat tails.
- The worst real day (-USD 2.2M) is about three times the 99% VaR. VaR describes normal days, not crises, so stress tests are needed.
- Gas explains most of the worst days since 2022. The worst days of 2020 come from Brent.

## Assumptions and limitations

- Fictional portfolio, rebalanced to 50/50 every day. This is not a real trading result.
- Yahoo Finance continuous futures are used. Contract rolls can create price jumps that are not real market moves.
- Only linear exposure to futures. No options, no Greeks, no correlation breakdown in stress tests.
- Historical VaR depends on the chosen period (2020 to 2026 includes Covid and the 2022 energy crisis).

## How to run

Open `energy_portfolio_var.ipynb` in Google Colab, or install the packages in `requirements.txt` and run it in Jupyter.

## Possible next steps

- Monte Carlo VaR
- Expected Shortfall (CVaR)
- Backtesting of the VaR model
- Add electricity prices
