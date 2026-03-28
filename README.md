# ROBUST LOWESS_w.s.cleveland_research_reproduction
a reproduction of the research paper of William S. Cleveland on robust locally weighted regresion
# Robust LOWESS: Reproducing Cleveland’s Locally Weighted Regression

## Overview

This project is a focused reproduction and analysis of **Robust Locally Weighted Regression (LOWESS)** as introduced by William S. Cleveland. The central objective is not merely to apply the method, but to **understand, implement, and experimentally validate** how robustness is incorporated into local regression.

While standard LOWESS captures nonlinear structure effectively, it remains sensitive to outliers. Cleveland’s contribution extends this idea by introducing an **iterative reweighting scheme**, allowing the model to resist the influence of extreme observations. This project reconstructs that idea from first principles and demonstrates its behavior through controlled experiments.

## Motivation

Many real-world datasets exhibit nonlinear relationships contaminated with noise and outliers. Traditional global models, such as linear regression, fail to capture such complexity. Even standard LOWESS, though flexible, can be distorted when local neighborhoods are dominated by anomalous points.

This raises a key question:

**Can we build a model that adapts locally while remaining resistant to outliers?**

Robust LOWESS provides a compelling answer.

## Methodology

### 1. Standard LOWESS

The baseline model performs local regression using distance-based weighting. For each target point:

* A neighborhood is selected based on a span parameter
* Weights are assigned using a tricube kernel
* A weighted least squares regression is fitted locally

This produces a smooth curve that adapts to nonlinear patterns.


### 2. Robust Extension (Core Contribution)

Cleveland’s robust formulation augments LOWESS with an additional weighting mechanism based on residuals:

1. Fit standard LOWESS
2. Compute residuals (deviation between observed and predicted values)
3. Assign lower weights to points with large residuals using a bisquare function
4. Refit the model using combined weights
5. Repeat for a few iterations

This iterative refinement reduces the influence of outliers without discarding them entirely.



## Mathematical Formulation

At each point, the model solves a weighted least squares problem:

min Σ wᵢ (yᵢ − (β₀ + β₁xᵢ))²

The weights consist of two components:

* **Distance-based weights** (tricube kernel)
* **Robust weights** (bisquare function based on residuals)

The final weight is their product, updated iteratively.



## Implementation

The project includes two implementations:

* A reference implementation using `statsmodels.lowess`
* A complete **from-scratch implementation of robust LOWESS**, including:

  * distance computation
  * kernel weighting
  * weighted regression
  * residual-based reweighting

The custom implementation closely matches the expected behavior of the original method.


## Experiments

### Nonlinear Data Modeling

A synthetic nonlinear dataset was constructed using:

### y = e^(sin(x) + cos(x))

Linear regression fails on this dataset (R² ≈ 0.065), establishing the need for non-parametric methods.



### LOWESS vs Robust LOWESS

Both models were applied to the dataset:

* LOWESS captures the nonlinear structure effectively
* Robust LOWESS produces a similar fit in the absence of strong outliers



### Outlier Analysis (Key Experiment)

To highlight the difference, structured and high-magnitude outliers were introduced.

Observations:

* Standard LOWESS is visibly distorted by clustered outliers
* Robust LOWESS remains stable and continues to follow the underlying trend

This experiment directly validates the purpose of the robust extension.



### Noise Sensitivity

Increasing noise levels were tested:

* LOWESS performs well under low noise
* Performance degrades gradually with higher noise
* Robust LOWESS provides improved stability when noise includes extreme deviations



## Key Insights

* Local regression is highly effective for modeling nonlinear data
* Distance-based weighting alone is insufficient in the presence of outliers
* Iterative residual-based reweighting significantly improves robustness
* The span parameter controls the bias–variance tradeoff
* Robust LOWESS balances flexibility with stability


## Limitations

* Computationally expensive due to repeated local fitting
* Sensitive to span parameter selection
* Does not provide a global functional form
* Limited extrapolation capability beyond observed data

## Conclusion

This project demonstrates that robustness in regression is not achieved by discarding data, but by reweighting it intelligently. Cleveland’s robust LOWESS framework provides a principled way to retain flexibility while mitigating the influence of outliers.

Reproducing this method from scratch reinforces a deeper understanding of how classical statistical ideas continue to underpin modern machine learning practice.

## Reference

## Cleveland, W. S. (1979).
"Robust Locally Weighted Regression and Smoothing Scatterplots"



