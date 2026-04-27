---
name: psm-causal-inference
description: Use when the user needs propensity score methods for causal inference, including propensity score matching (PSM), inverse probability weighting (IPW), overlap/common support checks, caliper or nearest-neighbor matching, covariate balance diagnostics using standardized mean differences, ATT/ATE estimation, sensitivity analysis, and clear reporting for observational treatment effect studies in Python, R, Stata, or SQL-backed analytics workflows.
---

# PSM Causal Inference

Use this skill for observational treatment effect questions where the user wants to compare treated and untreated units after adjusting for observed confounding with propensity scores.

## Workflow

1. Define the estimand before code:
   - Treatment, outcome, unit of analysis, time window, and eligible population.
   - Target estimand: ATT for effect on treated units, ATE for the whole population, or ATC for untreated units.
   - Confounders must be pre-treatment variables only.

2. Build the propensity model:
   - Estimate `P(T = 1 | X)` with logistic regression, regularized logistic regression, gradient boosting, random forest, or another calibrated classifier.
   - Do not include post-treatment variables, mediators, colliders, or outcome-derived features.
   - Inspect overlap/common support before matching or weighting.

3. Choose adjustment strategy:
   - Nearest-neighbor matching for interpretable matched cohorts.
   - Caliper matching when poor matches would bias estimates; common default is `0.2 * SD(logit(propensity_score))`.
   - Matching with replacement when control pool is small.
   - IPW/stabilized weights when the goal is ATE and overlap is adequate.
   - Trimming when propensity scores are near 0 or 1 and positivity is violated.

4. Validate balance:
   - Report standardized mean differences (SMD) before and after adjustment.
   - Aim for absolute SMD below `0.1`; stricter studies may use `0.05`.
   - Check variance ratios for continuous covariates and category balance for categorical covariates.
   - Produce a love plot or balance table when useful.

5. Estimate treatment effect:
   - Use matched-pair outcome differences for 1:1 ATT matching.
   - Use regression on the matched/weighted sample for adjusted estimates when appropriate.
   - Use robust, bootstrap, or cluster-aware standard errors depending on the design.
   - Report uncertainty: confidence intervals, p-values if needed, and practical effect size.

6. Stress test the conclusion:
   - Try alternative propensity specifications.
   - Vary caliper width and matching ratio.
   - Compare matching with IPW or doubly robust estimation when feasible.
   - State residual risks from unmeasured confounding.

## Reporting Checklist

- Estimand, inclusion criteria, treatment timing, and outcome window.
- Propensity model covariates and why they are pre-treatment confounders.
- Overlap/common support diagnostics.
- Matching or weighting choices, including caliper, replacement, ratio, and discarded units.
- Balance before and after adjustment.
- Treatment effect estimate with uncertainty.
- Sensitivity analyses and unmeasured-confounding caveats.

## Guardrails

- PSM adjusts only for observed confounders.
- Good predictive propensity scores are not enough; balance is the goal.
- Avoid matching on variables caused by treatment.
- Do not present matched sample results as population-wide ATE unless the design supports it.
- If overlap is weak, say the causal question is not well supported by the data.
