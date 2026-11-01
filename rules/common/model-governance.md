---
description: Model governance rules for validation, documentation, and review of quantitative models
---

# Model Governance

All quantitative models used for trading, risk management, or portfolio construction must follow these governance standards.

## Model Validation Requirements

- Every model must have a documented validation report before deployment.
- Validation must be performed by an agent or reviewer independent of the model developer.
- Validation must include: theoretical soundness review, implementation verification, and performance benchmarking.
- Models must be re-validated annually or when market regime changes are detected.

## Backtesting Standards

- Walk-forward analysis with at minimum 3 non-overlapping out-of-sample periods is required.
- In-sample and out-of-sample performance must be reported separately. Never report blended results.
- Transaction costs, slippage, and market impact must be included in all backtest results.
- Backtest results must include confidence intervals, not just point estimates.
- The deflated Sharpe ratio (accounting for number of strategies tested) should be reported.

## Overfitting Checks

- Monitor the ratio of in-sample to out-of-sample Sharpe. Decay greater than 40% suggests overfitting.
- Limit the number of free parameters relative to the number of independent observations.
- Apply regularization (L1, L2) to any model with more than 5 parameters.
- Use combinatorial purged cross-validation for strategies with overlapping holding periods.
- Document every strategy variant tested. The more variants tested, the higher the bar for significance.

## Parameter Stability

- Key model parameters must be stable across rolling estimation windows.
- A parameter that changes sign or magnitude by more than 50% across windows is unreliable.
- Perform sensitivity analysis: model performance should degrade gracefully with parameter perturbation.
- Avoid cliff effects where small parameter changes cause large performance shifts.

## Documentation Requirements

Every model must have documentation covering:

1. **Purpose** — What decision does this model support?
2. **Methodology** — Mathematical specification, assumptions, and limitations.
3. **Data** — Input data sources, cleaning procedures, and quality checks.
4. **Validation** — Out-of-sample results, robustness checks, and known weaknesses.
5. **Parameters** — All parameters, how they were estimated, and their stability.
6. **Monitoring** — What metrics are tracked to detect model degradation.
7. **Contingency** — What happens if the model fails or produces unreasonable outputs.

## Model Review Process

- New models require sign-off from the risk-manager agent before deployment.
- Material model changes (methodology, parameters, data sources) require re-validation.
- Monthly model performance monitoring reports must be generated.
- Models that underperform their validation benchmarks for 3 consecutive months must be reviewed.
- A model inventory must be maintained listing all active models, their owners, and last validation date.
