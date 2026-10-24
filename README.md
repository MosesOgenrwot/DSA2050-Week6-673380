# DSA 2050 – Week 6 Guided Regression Lab

**Student:** Moses Ogenrwot

**Student ID:** `673380`

## Lab objective
Estimate `Sales_KES` from advertising spend, store visits, discount, region, customer segment and product category, and test whether regression models meaningfully beat simple baselines. The workflow uses a leakage-safe pipeline, a 20% untouched test set, training-only 5-fold cross-validation, residual analysis and subgroup error checks.

## Models compared
Mean baseline, median baseline, Linear Regression, Ridge Regression (alpha = 1), shallow Decision Tree (max_depth = 4, min_samples_leaf = 8). An unrestricted deep tree was also fitted to demonstrate overfitting, and Ridge alpha values 0.1, 1, 10, 100 were examined as the extension challenge.

| Model | Test MAE (KES) | Test RMSE | Test R² | CV MAE (train only) |
|---|---|---|---|---|
| Mean baseline | 39,197 | 48,716 | −0.01 | – |
| Median baseline | 39,147 | 48,649 | −0.01 | – |
| Linear Regression | 13,758 | 17,470 | 0.870 | 15,977 |
| **Ridge Regression** | **13,714** | **17,459** | **0.870** | **15,971** |
| Decision Tree (depth 4) | 23,776 | 28,768 | 0.648 | 24,498 |

## Best model
**Ridge Regression (alpha = 1).** It is practically tied with Linear Regression, and both clearly beat the baselines and the tree.

**Selected model MAE:** ≈ **KES 13,714** on the test set (≈ KES 15,971 in 5-fold CV on the training set), roughly 8–9% of mean sales.

## Two key findings
1. Ridge/Linear Regression cut MAE by about 65% versus the baselines (≈39,100 → ≈13,700) and raise R² from ≈0 to ≈0.87. Cross-validation gives the same ranking. `StoreVisits` and `AdvertisingSpend_KES` are the strongest drivers; removing `StoreVisits` collapses test R² to ≈0.24.
2. Errors are not uniform. The largest errors are mostly over-predictions at low store-visit counts (MAE ≈19,300 for visits < 300 vs ≈10,700 otherwise), and MAE differs by customer segment (Retail ≈15,600, Corporate ≈14,300, SME ≈10,500). The unrestricted tree reached training MAE 0 but test MAE ≈26,800, so it clearly overfits.

## Three limitations
1. Small dataset (320 rows, 64 test rows), so metrics and especially subgroup errors are noisy and depend on the split.
2. Likely teaching/synthetic data with few predictors and no date, seasonality, price, stock or competition information, so real-world performance would probably be worse.
3. Prediction is not causation, and the model may drift as business conditions change, so it should be validated on newer data before deployment.

## Files
- `DSA2050_Week6_Guided_Regression_Lab.ipynb` – full notebook (all cells run top to bottom, outputs included)
- `DSA2050_Week6_Sales_Data.csv` – dataset
- `README.md`
