---
description: Data integrity rules for all quantitative analysis and backtesting
---

# Data Integrity

These rules ensure that all analysis, backtests, and trading decisions are based on clean, unbiased data. Data quality is the foundation of every quantitative process.

## Survivorship Bias

- Always use point-in-time constituent lists for index-based strategies. Never use current index members to backtest historical periods.
- Include delisted securities with their delisting returns. Excluding them inflates backtest performance.
- When using fundamental data, ensure the universe includes companies that went bankrupt, merged, or were acquired.
- Flag any analysis that uses a fixed universe without accounting for changes over time.

## Look-Ahead Bias

- Never use data that would not have been available at the time of the trading decision.
- Fundamental data must be stamped with the filing date (when it became public), not the period-end date.
- Analyst estimates must use the as-of-date consensus, not revised figures.
- Index rebalance decisions must use announcement dates, not effective dates.
- Any feature engineering or signal construction must be causal (only past data used to predict future).

## Point-in-Time Data

- All data must be stored and accessed in point-in-time format.
- Macro data releases must use the initial release value, not revised values, for historical analysis.
- Corporate action adjustments must be applied forward from the event date, never retroactively adjusted in historical data.
- Rating changes, index additions/deletions, and analyst coverage changes must be timestamped to the announcement.

## Backfill Bias

- Newly added data series must not be backfilled into historical analysis without explicit documentation.
- When a data vendor adds historical data for a previously uncovered security, mark the coverage start date and only use data from that point forward in backtests.
- ETF data should not be used before the ETF had meaningful AUM and trading volume.

## Data Cleaning Requirements

Before any analysis, the following cleaning steps are mandatory:

1. **Gap detection** — Identify and document any missing data points. Forward-fill only if appropriate for the data type (prices yes, volumes no).
2. **Outlier detection** — Flag and review returns exceeding 3 standard deviations or 25% in a single day.
3. **Corporate action adjustment** — Verify that splits, dividends, and spinoffs are properly adjusted.
4. **Currency consistency** — Ensure all prices are in a consistent currency or properly converted using point-in-time exchange rates.
5. **Timestamp alignment** — Align data from different sources to the same timezone and market calendar.
6. **Duplicate detection** — Check for and remove duplicate records.
7. **Cross-source validation** — When possible, validate data against a second independent source.
