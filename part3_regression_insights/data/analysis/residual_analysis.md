# Residual Analysis

**Final model used:** Multiple Regression — `monthly_sales ~ footfall + marketing_spend + inventory_availability_pct + staff_count + store_type dummies + region dummies` (R² = 0.8296)

**Method:** `predicted_sales` was calculated for all 320 store-months using the final model's intercept and coefficients (see `analysis/regression_workbook.xlsx`, sheet `prediction_residuals`). `residual = monthly_sales (actual) − predicted_sales`. A positive residual means the model under-predicted (actual sales were higher than expected); a negative residual means the model over-predicted (actual sales were lower than expected).

## Top 5 Largest Positive Residuals (Model Under-Predicts These)

| Store ID | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
|---|---|---|---|---|---|---|
| STR-1073 | 2025-03 | East | Residential | 813,316.71 | 700,782.65 | **+112,534.06** |
| STR-1028 | 2025-04 | East | Mall | 713,611.16 | 603,971.42 | **+109,639.74** |
| STR-1026 | 2025-04 | East | Mall | 625,514.04 | 519,632.05 | **+105,881.99** |
| STR-1030 | 2025-02 | West | Residential | 820,519.04 | 721,391.56 | **+99,127.48** |
| STR-1051 | 2025-02 | East | Airport | 787,715.51 | 690,488.43 | **+97,227.08** |

**What this means in business terms:** these five stores are outperforming what their footfall, marketing spend, staffing, and inventory levels would predict. Something not captured by the model — strong local management, a popular product mix, a nearby event, an effective in-store promotion, or simply excellent customer service — appears to be driving extra sales. Three of the five are East region, and two are Mall-format stores in the same April month, which may be worth a follow-up look (was there a regional or seasonal factor in April not captured by the holiday flag?).

## Top 5 Largest Negative Residuals (Model Over-Predicts These)

| Store ID | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
|---|---|---|---|---|---|---|
| STR-1017 | 2025-03 | West | High Street | 685,379.08 | 846,559.27 | **−161,180.19** |
| STR-1012 | 2025-01 | West | Residential | 595,467.60 | 728,950.13 | **−133,482.53** |
| STR-1023 | 2025-02 | South | Mall | 627,171.90 | 753,497.85 | **−126,325.95** |
| STR-1007 | 2025-02 | West | Mall | 800,451.94 | 914,908.11 | **−114,456.17** |
| STR-1009 | 2025-01 | North | Residential | 641,865.03 | 738,721.32 | **−96,856.29** |

**What this means in business terms:** these stores are underperforming relative to what their inputs would predict. Notably, **four of these five stores have unusually high marketing_spend** (STR-1007: ₹172,415.52; STR-1012: ₹138,855.51; STR-1023: ₹141,540.29 — all well above the dataset average of ≈ ₹56,279) yet still posted sales below model expectations. This is an important business signal: high marketing spend at these specific stores is not translating into sales the way the overall model expects, possibly due to local competition, poor campaign targeting, store-level execution issues, or diminishing returns on marketing spend at high levels.

## Under-Prediction vs. Over-Prediction by Store Type and Region

Because store_type and region are included as dummy variables in the model, the *average* residual within each group is mathematically forced to be approximately zero — the model is calibrated to each group on average. This means the model does **not** systematically over- or under-predict any entire region or store type as a whole. What it *does* show is meaningful **store-level and month-level variation within each group** — individual stores that are out-performing or under-performing their peers of the same type and region, which is exactly what the top-5 tables above highlight.

## Business Implications

- The negative-residual pattern around high marketing spend (STR-1007, STR-1012, STR-1023) suggests leadership should **investigate marketing efficiency at the store level** rather than assuming "more marketing spend = more sales" uniformly across all stores. The relationship may not be linear at high spend levels, or execution quality may vary by location.
- The positive-residual stores (STR-1073, STR-1028, STR-1026, STR-1030, STR-1051) could be useful **case studies** — understanding what these stores are doing right (operationally, locally, or in-store) could surface best practices to roll out elsewhere.
- These residuals are a starting point for investigation, not a final diagnosis. The regression model only includes the variables available in this dataset; local factors (competitor actions, weather, local events, staff turnover, store renovations) are not captured and could explain some of these gaps.
