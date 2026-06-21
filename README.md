## Regression-Based Business Insights & Model Interpretation

## Business Problem Summary

A retail chain's leadership team wants to understand what factors are driving monthly sales performance across its stores. They are considering several possible actions — increasing marketing spend, improving inventory availability, adjusting discounting strategy, reallocating staff, or prioritizing certain store types or regions for future investment. This analysis uses regression to identify which factors are most strongly associated with monthly sales and to provide an evidence-based business recommendation.

## Dataset Description

The dataset (`data/business_regression_data.xlsx`) contains **320 monthly records** for **80 stores** over **4 months (January–April 2025)**. It includes:

- **store_id** — unique store identifier
- **month** — reporting month
- **region** — East, North, South, West
- **store_type** — High Street, Mall, Residential, Airport
- **marketing_spend** — monthly marketing spend (₹)
- **footfall** — monthly visitor count
- **avg_discount_pct** — average discount percentage for the month
- **staff_count** — number of store staff
- **inventory_availability_pct** — average product availability (%)
- **competitor_distance_km** — distance to nearest competitor (6 missing values, imputed with median)
- **holiday_flag** — whether the month included a major holiday (0/1)
- **customer_rating** — average customer rating (8 missing values, imputed with median)
- **monthly_sales** — target/dependent variable (₹)
- **monthly_profit** — not used as a predictor (closely related to sales/cost structure rather than an independent driver; excluded to avoid circularity)

## Dependent and Independent Variables

- **Dependent variable:** `monthly_sales`
- **Independent variables used in the final model:** `footfall`, `marketing_spend`, `inventory_availability_pct`, `staff_count`, and dummy variables for `store_type` and `region`
- **Variables tested but not used in the final model:** `avg_discount_pct` (not statistically significant), `competitor_distance_km` and `customer_rating` (weak/near-zero correlation with monthly_sales, see Task 1 notes below)
- **Variables not useful for regression:** `store_id` (identifier, not a predictor), `month` (only 4 time points, used qualitatively rather than as a regression input), `monthly_profit` (outcome-adjacent variable, not an independent driver)

## Regression Approach

Two types of models were built:

1. **Four simple linear regressions**, each using one independent variable at a time against `monthly_sales` (footfall, marketing_spend, staff_count, avg_discount_pct), to understand each variable's standalone explanatory power.
2. **One multiple linear regression**, combining the strongest numerical predictors (footfall, marketing_spend, inventory_availability_pct, staff_count) with dummy variables for store_type and region, to capture the combined and more realistic picture of what drives sales.

All regression statistics (slope, intercept, R², p-values, t-stats) were computed using **live Excel formulas** (`SLOPE`, `INTERCEPT`, `RSQ`, `CORREL`, `LINEST`, `TDIST`) in `analysis/regression_workbook.xlsx`, cross-validated against an independent Python (statsmodels OLS) calculation to confirm accuracy.

## Dummy Variable Approach

- **store_type**: reference category = **High Street** (most frequent, 116/320 rows). Dummies created: `is_Mall`, `is_Residential`, `is_Airport`.
- **region**: reference category = **East** (most frequent, 104/320 rows). Dummies created: `is_North`, `is_South`, `is_West`.

Only 3 dummies per categorical variable (not 4) to avoid the dummy variable trap / perfect multicollinearity with the intercept. Full explanation in `outputs/model_equations.md`.

## Model Comparison Summary

| Model | R-squared | Significant? |
|---|---|---|
| Simple: footfall | 0.7363 | Yes |
| Simple: marketing_spend | 0.1672 | Yes |
| Simple: staff_count | 0.6523 | Yes |
| Simple: avg_discount_pct | 0.0083 | No |
| **Multiple Regression (Final)** | **0.8296** | Mostly Yes |

Full comparison with business usefulness and limitations: `analysis/model_comparison.md`.

## Final Model Selected

**Multiple Regression**: `monthly_sales ~ footfall + marketing_spend + inventory_availability_pct + staff_count + store_type dummies + region dummies` (R² = 0.8296). Selected because it explains substantially more variance than any single-variable model and reflects the combination of levers leadership actually controls. Full equation and coefficient interpretation: `outputs/model_equations.md`.

## Business Recommendation

Leadership should prioritize **footfall-driving investments, inventory availability improvements, and staffing adequacy** over blanket increases in marketing spend, since these show the strongest, most reliable associations with sales in this data. Marketing spend matters but shows weaker and inconsistent returns at high spend levels (see residual analysis). Full recommendation, risks, and the association-vs-causation discussion: `outputs/final_recommendation.md`.

## Assumptions and Limitations

- Missing values in `competitor_distance_km` (6 rows) and `customer_rating` (8 rows) were imputed using the column median.
- Data spans only 4 months — seasonal patterns outside Jan–Apr are not captured.
- Footfall and staff_count are moderately correlated (VIF ≈ 6.45), so their individual coefficients in the multiple regression should be interpreted with some caution even though the overall model fit is strong.
- Regression results show statistical **association**, not proof of causation. See `outputs/final_recommendation.md` for full discussion.
- `is_Mall` and `is_North` coefficients in the final model were not statistically significant (p > 0.05) and should not be treated as confirmed effects.


│   ├── residuals_preview.png
│   └── model_comparison_preview.png
└── README.md
```
