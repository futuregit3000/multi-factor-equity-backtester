# Interview Guide

## 30-second explanation

> I built a point-in-time multi-factor equity backtester in Python. It ranks a
> 50-stock U.S. large-cap universe each month using momentum, value, quality,
> and low-volatility signals. I used SEC filing dates to prevent fundamental
> look-ahead bias, standardized the factors cross-sectionally, selected the top
> 15 stocks, allowed weights to drift between rebalances, and deducted
> transaction costs. I compared the strategy with SPY using return, risk,
> drawdown, attribution, turnover, and subperiod robustness metrics.

## Two-minute explanation

The project has four main layers:

1. **Data pipeline:** adjusted prices for returns, unadjusted prices for market
   capitalization, SEC Company Facts for accounting data, and FRED for the
   three-month Treasury yield.
2. **Signal construction:** 12–1 momentum, value, quality, and low-volatility
   components are winsorized and standardized within each monthly
   cross-section.
3. **Portfolio simulation:** the top 15 stocks receive equal target weights.
   Signals are formed at month end and executed at the following trading-day
   close. Weights drift between rebalances and costs are charged on gross
   traded notional.
4. **Evaluation:** CAGR, volatility, Sharpe, Sortino, maximum drawdown, Calmar,
   alpha, beta, tracking error, information ratio, VaR, Expected Shortfall,
   subperiod tests, and transaction-cost sensitivity.

## Questions you should be ready to answer

### Why use 12–1 momentum?

It captures medium-term price continuation while excluding the most recent
month, which can behave differently because of short-term reversal effects.

### Why winsorize the factor data?

Accounting ratios and returns can contain extreme values. Winsorization limits
their influence without removing the securities entirely.

### Why use z-scores?

The raw factors use different units. Z-scores place them on comparable
cross-sectional scales before combining them.

### How did you prevent look-ahead bias?

A fundamental fact was eligible only when both its accounting period end and
its SEC filing date were on or before the monthly signal date. Signals were
then executed on the next trading day.

### Why use adjusted and unadjusted prices differently?

Adjusted prices are appropriate for total-return calculations because they
account for splits and distributions. Market capitalization should be
estimated with the contemporaneous unadjusted share price.

### How are transaction costs modeled?

Cost equals gross traded notional multiplied by the selected basis-point rate.
Gross traded notional includes both purchases and sales.

### Why let portfolio weights drift?

Real portfolio weights change as holdings earn different returns. Resetting
weights every day would incorrectly assume costless daily rebalancing.

### What is the strongest limitation?

The fixed 50-stock universe uses present-day knowledge. That introduces
survivorship and selection bias and can make historical results look better
than a truly investable point-in-time universe.

### Did the strategy always outperform?

No. It outperformed over the full period and in the first two subperiods, but
SPY outperformed during 2023–2025. Mentioning this is important because it shows
the factor model is regime dependent.

### What would you improve first?

Use historical index constituents and delisted securities, then rebuild the
fundamental layer from quarterly point-in-time filings to create
trailing-twelve-month values.

## Results to remember

- Common period: February 2016 through December 2025
- Four-factor CAGR: 17.40%
- SPY CAGR: 15.45%
- Four-factor Sharpe: 0.903
- SPY Sharpe: 0.768
- Four-factor maximum drawdown: -26.57%
- SPY maximum drawdown: -33.72%
- Four-factor beta: 0.846
- Base transaction cost: 10 basis points
- Average one-way monthly turnover: 18.63%

Treat these as historical notebook results, not forecasts.

## A strong closing statement

> The project taught me that a backtest is not only a return calculation. The
> difficult parts are point-in-time data alignment, realistic timing,
> transaction-cost accounting, cross-sectional normalization, and honest
> interpretation of bias and regime dependence.
