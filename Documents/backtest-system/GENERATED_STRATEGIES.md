# Generated Trading Strategies — Crypto Perps (Binance USDT-M)

> Generated using STRATEGY_GENERATION_PROMPT.md

---

## Category A: Cross-sectional factors (beyond momentum) — 1h

### Strategy 1: Low-Volatility Anomaly

**Type:** Cross-sectional / Factor

**Timeframe:** 1h

**Hypothesis:** Low-volatility assets tend to outperform high-volatility assets in crypto markets due to lower risk premium and mean-reversion in volatility. This is the inverse of the traditional risk-return relationship.

**Universe:** BTC, ETH, SOL, AVAX, ADA, LINK, NEAR, SUI, XRP, DOGE (10 assets from 1h data)

**Signal:** 20-period realized volatility (standard deviation of hourly returns)

**Ranking:** Ascending (lowest volatility = highest rank)

**Positions:** Long top-3 lowest vol, short bottom-3 highest vol, equal weight (16.67% each)

**Rebalancing:** Every 24 hours (daily) at 00:00 UTC

**Estimated costs:** ~8 bps per rebalance (6 positions, 11-14 bps RT costs, partial turnover)

**Why not noise:** Low-vol anomaly is well-documented across asset classes. In crypto, high vol often signals distress or speculation, while low vol indicates stability that attracts flows.

**Main risk:** Regime shift where high-vol assets suddenly outperform (e.g., during major bull runs)

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv` for all 10 assets

---

### Strategy 2: Relative Liquidity Momentum

**Type:** Cross-sectional / Factor

**Timeframe:** 1h

**Hypothesis:** Assets with improving liquidity (volume/volatility ratio) attract more capital and outperform those with deteriorating liquidity. Liquidity begets liquidity in crypto markets.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP (10 assets)

**Signal:** 24-hour average of (volume / ATR) ratio, normalized by 30-day median

**Ranking:** Descending (highest liquidity score = highest rank)

**Positions:** Long top-3, short bottom-3, equal weight

**Rebalancing:** Every 24 hours at 00:00 UTC

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Liquidity is a self-reinforcing property. More liquid assets have tighter spreads, less slippage, and attract more trading activity.

**Main risk:** Sudden liquidity shocks (exchange issues, delistings)

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv`

---

### Strategy 3: Beta-to-BTC Rotation

**Type:** Cross-sectional / Factor

**Hypothesis:** Low-beta assets (relative to BTC) outperform during market stress, while high-beta assets outperform during expansions. Rotating between them captures the beta spread.

**Universe:** ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP, DOGE (10 altcoins)

**Signal:** 30-day rolling beta to BTC (covariance with BTC returns / BTC variance)

**Ranking:** Absolute deviation from 1.0 (target beta-neutral portfolio)

**Positions:** Long 3 lowest beta, short 3 highest beta, weights inversely proportional to beta

**Rebalancing:** Weekly (every 168 bars)

**Estimated costs:** ~6 bps per rebalance (lower turnover)

**Why not noise:** Beta is a fundamental risk factor. The spread between high and low beta assets should be mean-reverting.

**Main risk:** BTC dominance regime where all alts move together regardless of beta

**Data needed:** `data/binance_klines_1h/BTC_1h.csv` + all altcoin 1h files

---

## Category B: Relative value / spread — 1h or 15m

### Strategy 4: BTC/ETH Ratio Z-Score

**Type:** Relative value / Spread

**Timeframe:** 1h

**Hypothesis:** The BTC/ETH ratio oscillates around its historical mean. When the ratio deviates significantly, it tends to revert due to pairs trading and arbitrage activity.

**Universe:** BTC, ETH

**Signal:** Z-score of BTC/ETH price ratio: (current_ratio - 30-day_mean) / 30-day_std

**Ranking:** N/A (direct spread signal)

**Positions:** When z-score > +1.5: short BTC, long ETH. When z-score < -1.5: long BTC, short ETH. Equal weight.

**Rebalancing:** When z-score crosses ±1.5 or returns to 0

**Estimated costs:** ~10 bps per trade (2 assets, full turnover)

**Why not noise:** BTC and ETH are highly correlated but have different market dynamics. The ratio has shown mean-reverting properties historically.

**Main risk:** Structural break in BTC/ETH relationship (e.g., ETH adoption as collateral)

**Data needed:** `data/binance_klines_1h/BTC_1h.csv`, `data/binance_klines_1h/ETH_1h.csv`

---

### Strategy 5: Volatility Convergence to BTC

**Type:** Relative value / Volatility

**Timeframe:** 1h

**Hypothesis:** Altcoin volatilities tend to converge to BTC's volatility over time. When an altcoin's volatility deviates significantly from BTC's, it tends to revert.

**Universe:** ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP (8 alts)

**Signal:** Volatility spread = alt_20h_vol - BTC_20h_vol, normalized by 30-day std of spread

**Ranking:** Absolute z-score of volatility spread

**Positions:** Long 2 alts with most negative spread (low vol vs BTC), short 2 alts with most positive spread (high vol vs BTC), equal weight

**Rebalancing:** Daily at 00:00 UTC

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Altcoin volatility is anchored to BTC as the market leader. Deviations are typically temporary.

**Main risk:** Market-wide volatility expansion/contraction that affects all assets similarly

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv` for BTC + 8 alts

---

### Strategy 6: Relative Drawdown Reversal

**Type:** Relative value / Momentum

**Timeframe:** 1h

**Hypothesis:** During market corrections, assets that have fallen the least show relative strength and tend to outperform those that have fallen the most in the subsequent recovery.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** Maximum drawdown from peak over last 24 hours

**Ranking:** Ascending (smallest drawdown = highest rank)

**Positions:** Long top-3 (least drawdown), short bottom-3 (most drawdown), equal weight

**Rebalancing:** Daily at 00:00 UTC, but only when market is in correction mode (BTC down >5% from 24h high)

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Relative strength during drawdowns persists into recoveries as capital rotates to safer assets first.

**Main risk:** Continued downtrend where weak assets keep underperforming

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv`

---

## Category C: Carry / income — 1h

### Strategy 7: Funding Rate Differential

**Type:** Carry / Relative value

**Timeframe:** 1h

**Hypothesis:** Funding rate differentials between pairs create arbitrage opportunities. Long the asset with most negative funding (perps trading at discount) and short the asset with most positive funding (perps trading at premium).

**Universe:** BTC, ETH, HYPE, JUP, SOL, SPX, SUI, XRP (8 symbols with funding data)

**Signal:** Current funding rate from `data/binance_funding/{SYM}_funding.json`

**Ranking:** Funding rate ascending (most negative = highest rank for longs, most positive = highest rank for shorts)

**Positions:** Long 2 most negative funding, short 2 most positive funding, equal weight

**Rebalancing:** Every 8 hours (at funding settlement: 00:00, 08:00, 16:00 UTC)

**Estimated costs:** ~6 bps per rebalance (funding is paid/received at settlement)

**Why not noise:** Funding rates reflect the cost of carry. The spread between rates should converge over time.

**Main risk:** Basis risk - price movements can outweigh funding income

**Data needed:** `data/binance_funding/{SYM}_funding.json` + `data/binance_klines_1h/{SYM}_1h.csv`

---

### Strategy 8: Funding + Momentum Gate

**Type:** Carry / Factor combination

**Timeframe:** 1h

**Hypothesis:** Funding carry works better when combined with momentum. Only enter long positions when funding is negative AND momentum is positive (perps at discount but price rising).

**Universe:** BTC, ETH, HYPE, JUP, SOL, SPX, SUI, XRP

**Signal:** Funding rate + 24h ROC (Rate of Change)

**Ranking:** Combined score: funding_z + momentum_z (both normalized)

**Positions:** Long top-2 with negative funding AND positive momentum. Short top-2 with positive funding AND negative momentum. Equal weight.

**Rebalancing:** Every 8 hours at funding settlement

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Combines two independent signals. Negative funding means shorts are paying longs, and positive momentum confirms the direction.

**Main risk:** Sudden momentum reversal wiping out funding gains

**Data needed:** `data/binance_funding/{SYM}_funding.json` + `data/binance_klines_1h/{SYM}_1h.csv`

---

## Category D: Volatility — 1h or 15m

### Strategy 9: Volatility Risk Premium

**Type:** Volatility / Cross-sectional

**Timeframe:** 1h

**Hypothesis:** Assets with lower realized volatility tend to have lower implied volatility, creating a risk premium. Going long low-realized-vol and short high-realized-vol captures this premium.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** 24-hour realized volatility

**Ranking:** Ascending (lowest realized vol = highest rank)

**Positions:** Long top-3 lowest realized vol, short bottom-3 highest realized vol, equal weight

**Rebalancing:** Daily at 00:00 UTC

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Volatility risk premium is a well-established phenomenon in derivatives markets.

**Main risk:** Volatility regime shift (e.g., all assets become highly volatile simultaneously)

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv`

---

### Strategy 10: Volatility Breakout Cross-Sectional

**Type:** Volatility / Momentum

**Timeframe:** 15m

**Hypothesis:** Assets whose volatility is expanding relative to the market tend to continue that trend. Long assets with expanding volatility, short those with contracting volatility.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** 24-period (6h) volatility change: current_24h_vol - previous_24h_vol

**Ranking:** Descending (greatest vol expansion = highest rank)

**Positions:** Long top-3 expanding vol, short bottom-3 contracting vol, equal weight

**Rebalancing:** Every 6 hours (4x daily)

**Estimated costs:** ~12 bps per rebalance (higher frequency)

**Why not noise:** Volatility tends to cluster. Expanding volatility often precedes continued momentum.

**Main risk:** Mean reversion in volatility (expansion followed by contraction)

**Data needed:** `data/binance_klines_15m/{SYM}_15m.csv`

---

## Category E: Microstructure proxies (OHLCV only) — 5m or 15m

### Strategy 11: Volume Profile Accumulation

**Type:** Microstructure proxy / Cross-sectional

**Timeframe:** 5m

**Hypothesis:** Assets with high volume on up-bars (accumulation) show buying interest and tend to outperform. Assets with high volume on down-bars (distribution) show selling pressure.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** 24-period (2h) accumulation score: sum(volume * sign(close - open)) / sum(volume)

**Ranking:** Descending (highest accumulation = highest rank)

**Positions:** Long top-3 highest accumulation, short bottom-3 lowest (most distribution), equal weight

**Rebalancing:** Every 2 hours

**Estimated costs:** ~10 bps per rebalance

**Why not noise:** Volume profile directly reflects order flow. Accumulation/distribution patterns are predictive of future price action.

**Main risk:** Fake volume (wash trading, bot activity)

**Data needed:** `data/binance_klines_5m/{SYM}_5m.csv`

---

### Strategy 12: Wick Ratio Rejection

**Type:** Microstructure proxy / Cross-sectional

**Timeframe:** 15m

**Hypothesis:** Long lower wicks indicate buying support (rejection of lower prices). Long upper wicks indicate selling pressure (rejection of higher prices).

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** Wick ratio = (close - low) / (high - low) for lower wick, (high - close) / (high - low) for upper wick. Combined: lower_wick_ratio - upper_wick_ratio

**Ranking:** Descending (strongest buying rejection = highest rank)

**Positions:** Long top-3 highest wick score, short bottom-3 lowest, equal weight

**Rebalancing:** Every 4 hours

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Wick patterns reflect intraday supply/demand imbalances that often persist.

**Main risk:** Whipsaws in ranging markets

**Data needed:** `data/binance_klines_15m/{SYM}_15m.csv`

---

### Strategy 13: Close Location Value

**Type:** Microstructure proxy / Cross-sectional

**Timeframe:** 5m

**Hypothesis:** Assets closing in the top third of their range show strength, while those closing in the bottom third show weakness.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** Close location = (close - low) / (high - low), categorized into terciles

**Ranking:** Descending (highest close location = highest rank)

**Positions:** Long top-3 (closing in top tercile), short bottom-3 (closing in bottom tercile), equal weight

**Rebalancing:** Every 1 hour

**Estimated costs:** ~10 bps per rebalance

**Why not noise:** Close location is a simple but effective proxy for intraday momentum.

**Main risk:** Reversals after strong closes (profit-taking)

**Data needed:** `data/binance_klines_5m/{SYM}_5m.csv`

---

## Category F: Taker flow (5m taker data) — 5m or 15m

### Strategy 14: Taker Buy Ratio

**Type:** Microstructure proxy / Order flow

**Timeframe:** 5m

**Hypothesis:** High taker buy ratio indicates aggressive buying (market orders lifting asks). Assets with highest taker buy ratio tend to outperform.

**Universe:** BTC, ETH, HYPE, JUP, SOL, SUI, XRP (7 symbols with taker data)

**Signal:** 12-period (1h) average of taker_buy_base_vol / total_volume

**Ranking:** Descending (highest taker buy ratio = highest rank)

**Positions:** Long top-2 highest ratio, short bottom-2 lowest ratio, equal weight

**Rebalancing:** Every 1 hour

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Taker buy ratio reflects real aggressive buying/selling pressure, not just limit orders.

**Main risk:** Spoofing or fake order flow

**Data needed:** `data/binance_klines_5m_taker/{SYM}_5m.csv`

---

### Strategy 15: Taker Volume Momentum

**Type:** Microstructure proxy / Momentum

**Timeframe:** 5m

**Hypothesis:** Increasing taker buy volume signals growing buying interest. Assets with increasing taker buy volume tend to continue rising.

**Universe:** BTC, ETH, HYPE, JUP, SOL, SUI, XRP

**Signal:** 12-period change in taker_buy_base_vol (current 12-period avg - previous 12-period avg)

**Ranking:** Descending (greatest increase = highest rank)

**Positions:** Long top-2 increasing, short bottom-2 decreasing, equal weight

**Rebalancing:** Every 1 hour

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Momentum in taker volume is more reliable than price momentum alone.

**Main risk:** Volume spikes without follow-through

**Data needed:** `data/binance_klines_5m_taker/{SYM}_5m.csv`

---

### Strategy 16: Aggressor Imbalance

**Type:** Microstructure proxy / Order flow

**Timeframe:** 5m

**Hypothesis:** When buyers are more aggressive than sellers (taker buy vol > taker sell vol), price tends to rise. This imbalance can be used cross-sectionally.

**Universe:** BTC, ETH, HYPE, JUP, SOL, SUI, XRP

**Signal:** Aggressor imbalance = (taker_buy_base_vol - (total_volume - taker_buy_base_vol)) / total_volume

**Ranking:** Descending (most buyer-aggressive = highest rank)

**Positions:** Long top-2 most buyer-aggressive, short bottom-2 most seller-aggressive, equal weight

**Rebalancing:** Every 30 minutes

**Estimated costs:** ~12 bps per rebalance (higher frequency)

**Why not noise:** Directly measures who is pushing the price.

**Main risk:** High-frequency noise, bid-ask bounce

**Data needed:** `data/binance_klines_5m_taker/{SYM}_5m.csv`

---

## Category G: Cross-sectional scalping — 5m or 1m

### Strategy 17: High-Frequency Relative Momentum

**Type:** Scalping / Cross-sectional

**Timeframe:** 5m

**Hypothesis:** Relative momentum at high frequency captures short-term performance differences. Ranking assets by 1h return every 5m and trading the spread.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** 12-period (1h) ROC, updated every 5m

**Ranking:** Descending (highest 1h return = highest rank)

**Positions:** Long top-2, short bottom-2, equal weight. Hold for 5 minutes only.

**Rebalancing:** Every 5 minutes

**Estimated costs:** ~15 bps per rebalance (very high frequency, but small position sizes)

**Why not noise:** Cross-sectional scalping avoids directional risk. The relative ranking contains information even at high frequency.

**Main risk:** Transaction costs eating into profits, slippage

**Data needed:** `data/binance_klines_5m/{SYM}_5m.csv`

---

### Strategy 18: Relative Volume Spike

**Type:** Scalping / Volume

**Timeframe:** 5m

**Hypothesis:** Assets with sudden volume spikes relative to their own history attract attention and tend to move more than others in the short term.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** Volume spike score = current_5m_volume / 20-period (1.67h) average volume

**Ranking:** Descending (highest spike = highest rank)

**Positions:** Long top-2 highest spike, short bottom-2 lowest spike, equal weight. Hold for 15 minutes.

**Rebalancing:** Every 5 minutes

**Estimated costs:** ~15 bps per rebalance

**Why not noise:** Volume spikes often precede price moves. Cross-sectional approach diversifies idiosyncratic risk.

**Main risk:** Volume spikes without price follow-through (false breakouts)

**Data needed:** `data/binance_klines_5m/{SYM}_5m.csv`

---

### Strategy 19: Cross-Asset Lead-Lag

**Type:** Scalping / Lead-lag

**Timeframe:** 5m

**Hypothesis:** BTC often leads altcoin movements. When BTC moves first, altcoins tend to follow. Trading the lagging altcoins based on BTC's direction.

**Universe:** BTC (leader), ETH, SOL, AVAX, ARB, LINK (followers)

**Signal:** 5m return correlation with BTC over last 12 periods (1h)

**Ranking:** Descending (highest correlation = most responsive to BTC)

**Positions:** When BTC 5m return > 0: long top-3 most correlated alts. When BTC 5m return < 0: short top-3 most correlated alts. Equal weight.

**Rebalancing:** Every 5 minutes

**Estimated costs:** ~12 bps per rebalance

**Why not noise:** Lead-lag relationships are structurally embedded in crypto markets due to BTC's dominance.

**Main risk:** Decoupling of alts from BTC

**Data needed:** `data/binance_klines_5m/{SYM}_5m.csv`

---

## Category H: Combinations / meta-strategies

### Strategy 20: Momentum + Volatility (Sharpe Ranking)

**Type:** Factor combination

**Timeframe:** 1h

**Hypothesis:** Ranking by Sharpe ratio (return/volatility) instead of raw return produces more stable results by penalizing volatile assets.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** 24h Sharpe ratio = (24h return) / (24h volatility)

**Ranking:** Descending (highest Sharpe = highest rank)

**Positions:** Long top-3, short bottom-3, equal weight

**Rebalancing:** Daily at 00:00 UTC

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Sharpe ratio normalizes for risk, reducing the impact of outliers.

**Main risk:** Low-volatility regime where all Sharpe ratios are compressed

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv`

---

### Strategy 21: Momentum + Funding Gate

**Type:** Factor + Carry combination

**Timeframe:** 1h

**Hypothesis:** Momentum works better when aligned with funding. Only enter long when momentum is positive AND funding is negative (perps at discount). Only short when momentum is negative AND funding is positive.

**Universe:** BTC, ETH, HYPE, JUP, SOL, SPX, SUI, XRP

**Signal:** 24h ROC + current funding rate

**Ranking:** Combined: momentum_z + (-funding_z) for longs, momentum_z + funding_z for shorts

**Positions:** Long top-2 with positive momentum AND negative funding. Short top-2 with negative momentum AND positive funding. Equal weight.

**Rebalancing:** Every 8 hours at funding settlement

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Combines trend-following with carry, two independent edges.

**Main risk:** Funding and momentum diverging

**Data needed:** `data/binance_funding/{SYM}_funding.json` + `data/binance_klines_1h/{SYM}_1h.csv`

---

### Strategy 22: Multi-Timeframe Momentum

**Type:** Multi-TF combination

**Timeframe:** 5m (execution), 1h (signal)

**Hypothesis:** Combining 5m microstructure signals with 1h momentum creates a more robust signal. Short-term noise is filtered by longer-term trend.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** Combined score = 0.6 * 1h_ROC_z + 0.4 * 12-period_5m_ROC_z

**Ranking:** Descending (highest combined score = highest rank)

**Positions:** Long top-3, short bottom-3, equal weight

**Rebalancing:** Every 1 hour

**Estimated costs:** ~10 bps per rebalance

**Why not noise:** Multi-timeframe approach reduces whipsaws from single-timeframe signals.

**Main risk:** Conflicting signals between timeframes

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv` + `data/binance_klines_5m/{SYM}_5m.csv`

---

### Strategy 23: Ensemble Factor Ranking

**Type:** Ensemble / Meta-strategy

**Timeframe:** 1h

**Hypothesis:** Combining multiple factors (momentum, volatility, liquidity) into an ensemble ranking produces more stable and robust signals than any single factor.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** Ensemble score = 0.4 * momentum_z + 0.3 * liquidity_z + 0.2 * vol_z + 0.1 * beta_z

**Ranking:** Descending (highest ensemble score = highest rank)

**Positions:** Long top-3, short bottom-3, equal weight

**Rebalancing:** Daily at 00:00 UTC

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Ensemble methods reduce the variance of individual factor signals.

**Main risk:** Overfitting if weights are not properly calibrated

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv`

---

### Strategy 24: Regime-Switching Factor Weights

**Type:** Regime-switching / Meta-strategy

**Timeframe:** 1h

**Hypothesis:** Different factors perform better in different market regimes. Use BTC's 20-day trend to determine regime and adjust factor weights accordingly.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP

**Signal:** 
- Bull regime (BTC 20-day ROC > 0): 0.5 * momentum + 0.3 * liquidity + 0.2 * volatility
- Bear regime (BTC 20-day ROC < 0): 0.4 * liquidity + 0.3 * volatility + 0.3 * beta

**Ranking:** Descending (highest regime-adjusted score = highest rank)

**Positions:** Long top-3, short bottom-3, equal weight

**Rebalancing:** Daily at 00:00 UTC

**Estimated costs:** ~8 bps per rebalance

**Why not noise:** Market regimes have different dynamics. Adapting to regimes improves robustness.

**Main risk:** Regime misclassification, whipsaws at regime boundaries

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv`

---

### Strategy 25: Cross-Sectional Momentum with Volatility Filter

**Type:** Factor combination with filter

**Timeframe:** 1h

**Hypothesis:** The validated MP-18 strategy can be improved by adding a volatility filter. Only trade assets whose 24h volatility is within 1 standard deviation of the median volatility.

**Universe:** BTC, ETH, SOL, AVAX, ARB, LINK, NEAR, SUI, XRP, JUP, HYPE

**Signal:** 168-bar ROC (same as MP-18)

**Ranking:** Descending, but only for assets with 336-bar ROC > 0 AND volatility within [median_vol - std_vol, median_vol + std_vol]

**Positions:** Long top-2, short bottom-2, equal weight, rebalance every 336 bars

**Rebalancing:** Every 336 bars (14 days at 1h)

**Estimated costs:** ~6 bps per rebalance (lower frequency)

**Why not noise:** MP-18 is the only validated strategy. Adding a volatility filter reduces tail risk from extreme volatility events.

**Main risk:** Filtering out the best opportunities during high-volatility periods

**Data needed:** `data/binance_klines_1h/{SYM}_1h.csv`