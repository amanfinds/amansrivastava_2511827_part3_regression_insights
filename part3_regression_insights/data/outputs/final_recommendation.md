# Final Recommendation

## Which Factors Appear Most Strongly Associated With Monthly Sales?

Based on the final multiple regression model (R² = 0.8296), four operational factors show a statistically significant, positive association with monthly sales:

1. **Footfall** — the single strongest driver (coefficient ≈ +28.83 per visitor; also the strongest simple regression on its own, R² = 0.7363).
2. **Inventory availability %** — each 1-point increase in product availability is associated with ≈ +₹3,003 in monthly sales.
3. **Staff count** — each additional staff member is associated with ≈ +₹2,790 in monthly sales.
4. **Marketing spend** — positively associated with sales (≈ +₹1.17 per rupee spent), but with a smaller practical effect than footfall, staffing, or inventory.

In addition, **store format and region matter**: Airport, South, and West locations show significantly higher predicted sales than a comparable High Street store in the East region, while Residential stores show significantly lower predicted sales.

## Which Variables Should Leadership Focus On?

- **Footfall-driving initiatives** — anything that increases store visits (location strategy, local marketing, partnerships, store visibility) has the largest modeled association with sales.
- **Inventory availability** — this is a directly controllable operational lever with a clear, significant, positive relationship to sales. Stockouts or low availability appear costly.
- **Staffing levels** — adequate staffing is associated with higher sales, plausibly through better customer service and reduced lost sales from long queues or unavailable assistance.
- **Store format and regional strategy** — when prioritizing new locations or renovations, Airport and South/West region locations show the strongest structural sales association.

## Which Variables Should Not Be Over-Interpreted?

- **avg_discount_pct** was not statistically significant in the simple regression (p = 0.104) and was excluded from the final model. There is no reliable evidence in this data that discounting drives sales.
- **is_Mall** (p = 0.062) and **is_North** (p = 0.281) were not statistically significant in the final model. Treat any "Mall stores do better" or "North region underperforms" narrative as unconfirmed by this data — it could be due to chance.
- **marketing_spend**, while statistically significant, has a modest coefficient relative to footfall and staffing. The residual analysis (see `analysis/residual_analysis.md`) found that several of the largest under-performing stores had *unusually high* marketing spend, suggesting diminishing or inconsistent returns at high spend levels rather than a simple "more is better" relationship.

## What Business Action Would You Recommend?

1. **Prioritize footfall-driving investments** over blanket increases in marketing budget — footfall has by far the largest modeled association with sales, and marketing spend alone explains relatively little (R² = 0.1672 in isolation).
2. **Audit inventory availability at underperforming stores.** Since availability has a clear, significant, positive relationship with sales, stores below the ~88% average availability are good candidates for supply-chain attention.
3. **Review staffing allocation,** especially at stores with high footfall but lower staff counts, since both variables matter and the model suggests staffing has its own independent positive effect.
4. **Investigate marketing efficiency store-by-store** rather than increasing spend uniformly — the residual analysis flagged specific stores (e.g. STR-1007, STR-1012, STR-1023) with high spend but lower-than-predicted sales, worth a closer look before allocating more budget there.
5. **Use Airport, South, and West performance as a benchmark** when evaluating Residential and High Street locations, and study the outperforming stores identified in the residual analysis (e.g. STR-1073, STR-1028) as potential best-practice case studies.

## What Risks or Limitations Should Leadership Keep in Mind?

- This dataset covers only **4 months (Jan–Apr 2025)** across 80 stores. Seasonal effects beyond this window (e.g. festive season, summer) are not captured.
- **Footfall and staff_count are moderately correlated** with each other (VIF ≈ 6.45 in the workbook's VIF check), so it is hard to fully separate "more staff causes more sales" from "busier stores are simply staffed more heavily." The model's overall fit is reliable, but individual coefficients for these two variables should be read as approximate, not precise.
- The model explains about 83% of the variation in sales — a strong fit, but **17% remains unexplained** by the variables in this dataset (e.g. local competition, weather, staff experience, specific promotions are not captured).
- Two variables (competitor_distance_km and customer_rating) had minor missing data (6 and 8 rows respectively out of 320) and were filled with the column median for any sheet calculations that needed complete data; this is a reasonable but imperfect fix.

## Why Does Regression Show Association But Not Automatically Prove Causation?

Regression measures how strongly variables move together across the data that exists — it does not, by itself, prove that changing one variable will *cause* a change in another. Three independent risks should be kept in mind: 

1. **Reverse causality** — busier, higher-performing stores may simply *receive* more marketing budget and staff because leadership already expects them to do well, rather than the spend and staffing *causing* the sales.
2. **Omitted variables** — true drivers not in this dataset (local competition, store manager quality, neighborhood income levels, weather, foot traffic from nearby events) could be influencing both sales and the predictors simultaneously, creating an association that doesn't reflect a direct cause-and-effect link.
3. **Correlation between predictors** — since footfall and staff_count move together, it's hard to know whether staffing truly increases sales, or whether well-staffed stores simply tend to be the larger, naturally busier ones to begin with.

For these reasons, this analysis should be treated as a strong, evidence-based **starting point for hypothesis generation and prioritization** — not as definitive proof that any single action (e.g. "hire 5 more staff at every store") will produce a predictable increase in sales. Wherever possible, leadership should validate the highest-priority recommendations (e.g. inventory improvements, staffing adjustments) with a controlled pilot or test-and-learn approach in a subset of stores before rolling out company-wide.
