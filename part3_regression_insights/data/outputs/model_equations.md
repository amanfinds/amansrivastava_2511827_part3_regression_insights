# Model Equations

## Dummy Variable Approach

Two categorical variables were converted into dummy (0/1) variables: **store_type** and **region**.

- **store_type** has four categories: High Street, Mall, Residential, Airport. **High Street** was used as the **reference category** (it is the most frequent category in the data, 116 of 320 rows), so three dummy variables were created: `is_Mall`, `is_Residential`, `is_Airport`.
- **region** has four categories: East, North, South, West. **East** was used as the **reference category** (most frequent, 104 of 320 rows), so three dummy variables were created: `is_North`, `is_South`, `is_West`.

Only three dummies were created per categorical variable (not four) specifically to **avoid the dummy variable trap** — including a dummy for every category plus an intercept creates perfect multicollinearity, since the reference category's effect is already captured by the intercept. Every coefficient on a dummy variable should be read as "the difference in monthly sales compared to a High Street store in the East region, holding all other variables constant."

## Simple Regression Equations

**Model 1 — footfall:**

```
monthly_sales = 446,410.58 + 35.678 × footfall
```
R² = 0.7363, p < 0.001

**Model 2 — marketing_spend:**

```
monthly_sales = 560,777.35 + 2.1296 × marketing_spend
```
R² = 0.1672, p < 0.001

**Model 3 (supplementary) — staff_count:**

```
monthly_sales = 400,551.47 + 16,984.03 × staff_count
```
R² = 0.6523, p < 0.001

**Model 4 (supplementary, not significant) — avg_discount_pct:**

```
monthly_sales = 697,835.63 − 138,730.45 × avg_discount_pct
```
R² = 0.0083, p = 0.104 (not statistically significant — coefficient should not be trusted or acted on)

## Multiple Regression Equation (Final Model)

```
monthly_sales =  103,838.09
               + 28.829   × footfall
               + 1.1736   × marketing_spend
               + 3,003.33 × inventory_availability_pct
               + 2,790.49 × staff_count
               + 22,531.68 × is_Airport
               + 12,191.54 × is_Mall            (not statistically significant, p = 0.062)
               − 17,402.35 × is_Residential
               + 7,601.23  × is_North           (not statistically significant, p = 0.281)
               + 20,900.94 × is_South
               + 18,892.97 × is_West
```

R² = 0.8296, F-statistic = 150.39, n = 320

## Coefficient Explanations (Business Language)

- **Intercept (₹103,838.09):** the model's baseline prediction for a High Street store in the East region with zero footfall, zero marketing spend, zero inventory availability, and zero staff — a theoretical reference point, not a realistic store, but mathematically necessary to anchor the equation.
- **footfall (+28.83):** each additional visitor in a month is associated with about ₹28.83 more in monthly sales, holding everything else constant. This is the largest practical driver in the model given footfall's typical range (1,000–13,000 visitors/month).
- **marketing_spend (+1.17):** each additional rupee of marketing spend is associated with about ₹1.17 more in monthly sales — a positive but modest return, holding other factors constant.
- **inventory_availability_pct (+3,003.33):** each one-percentage-point increase in product availability is associated with about ₹3,003 more in monthly sales. Since this ranges from 71% to 99% in the data, improving availability from a poor month to a strong month could be associated with a meaningful sales gain.
- **staff_count (+2,790.49):** each additional staff member is associated with about ₹2,790 more in monthly sales, holding footfall and other factors constant.
- **is_Airport (+22,531.68):** Airport stores are associated with about ₹22,532 higher monthly sales than a comparable High Street store in the East region, all else equal.
- **is_Mall (+12,191.54, not significant):** Mall stores show a positive but statistically weak association (p = 0.062, just above the conventional 0.05 cutoff) — leadership should treat this as suggestive, not confirmed.
- **is_Residential (−17,402.35):** Residential stores are associated with about ₹17,402 lower monthly sales than a comparable High Street store, all else equal.
- **is_North (+7,601.23, not significant):** North region shows no statistically reliable difference from East once other factors are controlled for (p = 0.281).
- **is_South (+20,900.94):** South region stores are associated with about ₹20,901 higher monthly sales than East region stores, all else equal.
- **is_West (+18,892.97):** West region stores are associated with about ₹18,893 higher monthly sales than East region stores, all else equal.

## Final Model Selected and Reason

The **multiple regression model** was selected as the final model. It explains substantially more variation in monthly sales (R² = 0.8296) than any single-variable model (best simple model R² = 0.7363), and it reflects the reality that store performance is driven by a *combination* of marketing, staffing, inventory, and store characteristics rather than any single lever. Full reasoning and trade-offs are documented in `analysis/model_comparison.md`.
