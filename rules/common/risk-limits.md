---
description: Risk limit rules for all trading strategies and portfolio operations
---

# Risk Limits

These rules apply to every strategy, trade recommendation, and portfolio operation. No exceptions.

## Position Size Limits

- Maximum single position: 5% of portfolio NAV for liquid assets, 2% for illiquid assets.
- Maximum sector concentration: 30% of portfolio in any single GICS sector.
- Maximum country concentration: 40% of portfolio in any single non-domestic market.
- Maximum single-name notional: must not exceed 5% of the stock's 20-day average daily volume.

## Leverage Limits

- Maximum gross leverage: 2.0x NAV for equity strategies, 4.0x for fixed income, 1.5x for multi-asset.
- Maximum net exposure: +/- 50% of NAV for market-neutral strategies.
- Leverage must be calculated including derivative notional exposure, not just cash deployed.

## Drawdown Triggers

- At -5% drawdown from peak: mandatory risk review, reduce gross exposure by 20%.
- At -10% drawdown from peak: halt new position initiation, close worst-performing positions.
- At -15% drawdown from peak: full portfolio de-risk to 50% of normal exposure.
- At -20% drawdown from peak: escalate to portfolio manager, consider full liquidation.

## Correlation Limits

- No two positions in the portfolio should have a trailing 60-day correlation above 0.85.
- Maximum effective portfolio concentration (using correlation-adjusted HHI) must stay below 0.15.
- Monitor stock-bond correlation and adjust hedging assumptions when correlation regime shifts.

## Stop-Loss Requirements

- Every position must have a defined stop-loss before entry. No exceptions.
- Stop-loss must be set based on volatility (e.g., 2x ATR) or technical levels, not arbitrary percentages.
- Trailing stops must be updated at least daily for active strategies.
- Mental stops are not acceptable. All stops must be systematic and documented.

## Pre-Trade Risk Check

Before any trade or strategy recommendation, verify:

1. Position size is within limits.
2. Sector and factor concentration remain within bounds.
3. Portfolio drawdown has not triggered any de-risk thresholds.
4. Stop-loss is defined and documented.
5. Correlation with existing positions has been assessed.
6. Liquidity is sufficient for the intended position size.
7. Margin requirements are met with adequate buffer (minimum 150% of maintenance margin).
