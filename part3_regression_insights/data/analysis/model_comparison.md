# Model Comparison

This document compares all regression models built for this analysis: four simple linear regression
models and one multiple linear regression model. All R-squared and p-values below are pulled live
from `analysis/regression_workbook.xlsx` (sheets `simple_regression` and `multiple_regression`).

## Comparison Table

| Model | Dependent Variable | Independent Variable(s) | R-squared | Significant Variables (p < 0.05) | Business Usefulness | Limitations |
|---|---|---|---|---|---|---|
| **Simple Regression 1** | monthly_sales | footfall | 0.7363 | footfall (p ≈ 4.75e-94) | High — footfall alone explains ~74% of sales variation. Useful as a quick, single-metric health check for a store. | Ignores marketing, staffing, store type, and region. Cannot tell leadership *why* footfall is high or low, or what action drives footfall up. |
| **Simple Regression 2** | monthly_sales | marketing_spend | 0.1672 | marketing_spend (p ≈ 2.48e-14) | Low-moderate — confirms marketing spend matters, but explains under 17% of sales variation on its own. Not reliable for forecasting. | Too narrow to support a marketing budget decision by itself; omits stronger drivers like footfall and staffing. |
| **Simple Regression 3** | monthly_sales | staff_count | 0.6523 | staff_count (p ≈ 6.25e-75) | High — staffing levels track closely with sales. Useful for staffing-related conversations. | Likely overlaps with footfall (busier stores both attract more staff allocation and more footfall), so the relationship may partly reflect store size rather than a pure staffing effect. |
| **Simple Regression 4** | monthly_sales | avg_discount_pct | 0.0083 | None (p ≈ 0.104) | Very low — this variable alone tells leadership almost nothing about sales. | Statistically not significant in isolation. Should not be used to justify discounting decisions on its own. |
| **Multiple Regression (FINAL MODEL)** | monthly_sales | footfall, marketing_spend, inventory_availability_pct, staff_count, store_type dummies (is_Airport, is_Mall, is_Residential), region dummies (is_North, is_South, is_West) | **0.8296** | footfall, marketing_spend, inventory_availability_pct, staff_count, is_Airport, is_Residential, is_South, is_West (all p < 0.05) | **Highest** — explains ~83% of sales variation using a realistic combination of operational levers leadership actually controls (marketing, staffing, inventory) plus structural store characteristics (type, region). This is the model used for business recommendations. | is_Mall (p ≈ 0.062) and is_North (p ≈ 0.281) are not statistically significant and should not be over-interpreted. Footfall and staff_count are moderately correlated with each other (VIF ≈ 6.45), so their individual coefficients should be read with some caution even though the overall model is reliable. |

## Why the Multiple Regression Model Was Selected as Final

1. **Highest explanatory power.** R² of 0.8296 vs. a maximum of 0.7363 for any single-variable model — an improvement of roughly 9 percentage points of explained variance.
2. **Reflects real business decisions.** Leadership does not act on footfall or staff_count in isolation; they act on a *combination* of marketing spend, staffing, inventory policy, and store-format/region choices. The multiple regression mirrors this reality.
3. **Controls for confounding.** By including store_type and region together with the operational variables, the model isolates the effect of marketing spend, inventory, and staffing more cleanly than any simple regression could, which only looks at one variable at a time and risks mistaking correlation for the full picture.
4. **Most coefficients are statistically significant**, giving confidence that the relationships are not due to chance.

## Key Caveats Carried Into the Final Recommendation

- **is_Mall** and **is_North** did not reach standard significance (p > 0.05) — these are treated as inconclusive rather than confirmed effects.
- **avg_discount_pct** was tested and dropped from the final model after Simple Regression 4 showed it was not a significant driver of sales on its own.
- Regression shows **association**, not proof of cause and effect. This distinction is discussed in full in `outputs/final_recommendation.md`.
