# Customer Spending Regression — End-to-end Excel modeling from correlation to elasticities

**Key relationships**

| View | Metric | Result |
|---|---:|---:|
| Correlation (Discount ↔ Total Amount Spent) | r | 0.4063 |
| Simple Linear Regression (y=Total Amount Spent; x=Discount) | Slope | 0.7507 |
|  | Intercept | 127.2927 |
|  | R² | 0.1651 |
| Multiple Regression (y=Total; x1=Discount, x2=Product Price) | Coef. Discount | 0.1051 |
|  | Coef. Product Price | 1.6740 |
|  | R² | 0.4715 |
| Log–Log Model (y=ln Total; x1=ln Discount, x2=ln Product Price) | Coef. ln(Discount) | 0.0651 |
|  | Coef. ln(Product Price) | 0.9461 |
|  | Intercept | 0.3866 |
|  | R² | 0.6189 |

**Distribution diagnostics**

| Variable | Skewness |
|---|---:|
| Discount | 1.872 |
| Product Price | 0.635 |
| Total Amount Spent | 1.583 |

---

## Case Description
An e-commerce retailer (“Trendy Shopper”) wants to understand what drives **Total Amount Spent** per transaction and to build a simple predictive model using Excel. The dataset includes discount levels, product prices, and limited customer/product attributes.

---

## Tasks
- Quantify the relationship between **Discount** and **Total Amount Spent**.
- Build and interpret a **simple** and a **multiple** linear regression.
- Diagnose non-normality/heteroscedasticity and apply **log transforms**.
- Extend to an **advanced model** with demographics/regions/categories (one-hot encoding).
- Produce predictions and basic model quality checks in Excel.

---

## Accounting/Analytics Steps (aligned to the data)
1. **Load & clean**
   - Import `Regression Analysis Dataset.xlsx` (Sheets: Task 1–5).
   - Select numeric columns; drop empty rows (e.g., headers/spacers).
   - Observations available: ~20,000; used after cleaning in multi-variate steps: **19,964**.

2. **Correlation (Task 1)**
   - Compute `r = 0.4063` between **Discount** and **Total Amount Spent**.
   - Visualize via scatter (Excel Quick Analysis).

3. **Simple regression (Task 2)**
   - Model: `Total = 127.2927 + 0.7507·Discount` (R²=0.1651).
   - Interpretation: Each 1 unit increase in Discount associates with **+0.751** in spend (holding else constant by construction).

4. **Multiple regression (Task 3)**
   - Model: `Total = −1.4789 + 0.1051·Discount + 1.6740·Product Price` (R²=0.4715).
   - Residual funnel vs Product Price ⇒ evidence of heteroscedasticity.

5. **Transform & re-fit (Task 3)**
   - Use `ln` on all three variables.
   - Model: `ln(Total) = 0.3866 + 0.0651·ln(Discount) + 0.9461·ln(Product Price)` (R²=0.6189).
   - Coefficients are **elasticities**: +1% in price ↦ **+0.946%** in spend; +1% in discount ↦ **+0.065%** in spend.

6. **Advanced regression (Task 4)**
   - Add predictors: **Age, Gender, User Region, Product Category** (one-hot, drop first).
   - Core elasticities remain ~unchanged; overall R² ≈ **0.619**.
   - Non-price/discount controls show limited incremental explanatory power here.

7. **Predictive modeling (Task 5)**
   - Compute fitted log-spend and compare to actual log-spend (scatter: Actual vs Predicted).
   - In-sample MSE (log scale) ≈ **0.338**; identify outliers for potential curation.

---

## Trial Balance / Data Summary
**Rows & completeness**

| Item | Count |
|---|---:|
| Raw transactions (across tasks) | 20,000 |
| Used in multi-variate/log models (after NA handling) | 19,964 |

**Reconciliation checks**

| Check | Result |
|---|---|
| Sign(r Discount, Spend) matches sign(β_Discount) in simple & multiple models | ✅ |
| R² improves after log transform (0.4715 → 0.6189) | ✅ |
| Elasticity interpretation consistent across Task 3 & Task 4 | ✅ |

---

## Financial Statements / Results (model outputs)
- **Key elasticities (log–log model)**  
  - **Price elasticity of spend**: **0.946** (strong driver)  
  - **Discount elasticity of spend**: **0.065** (modest but positive)
- **Fit quality**  
  - R² ≈ **0.619** (log scale)  
  - In-sample MSE (log scale) ≈ **0.338**
- **Business takeaway**  
  - Price levels explain most variance in basket spend; discounts help, but with lower elasticity. Demographic/region/category effects are comparatively minor in this setup.

---

## What I Learned
- Log transforms can materially improve linearity and stabilize variance, lifting R² from ~0.47 to ~0.62.
- Price dominates spend variability in this dataset; discount is supportive, not primary.
- Adding demographic/region/category controls may add limited value unless interactions or non-linearities are modeled.
