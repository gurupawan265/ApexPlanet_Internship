# Exploratory Data Analysis (EDA) Report
**ApexPlanet Software Pvt. Ltd. — Data Analytics Internship, Task 2**
**Dataset:** ApexPlanet_Cleaned_Dataset (1,000 orders × 19 columns, cleaned in Task 1)

---

## 1. Descriptive Statistics & Univariate Analysis

### Numerical Variables

| Metric | Age | Quantity | Unit_Price (₹) | Total_Sales (₹) |
|---|---|---|---|---|
| Mean | 41.35 | 5.44 | 25,486.78 | 139,399.44 |
| Median | 41 | 5 | 25,398.74 | 108,594.03 |
| Std Dev | 13.68 | 2.84 | 14,179.40 | 114,100.05 |
| Min | 18 | 1 | 145.78 | 437.34 |
| Max | 65 | 10 | 49,997.53 | 493,677.50 |
| Skew | 0.04 | 0.06 | -0.03 | 0.99 |

- **Age** and **Quantity** are roughly symmetric (skew ≈ 0) — uniform-ish distributions across their ranges.
- **Total_Sales** is right-skewed (0.99) — a long tail of high-value orders pulls the mean (₹1,39,399) well above the median (₹1,08,594).
- See `charts/01_age_distribution.png`, `02_sales_distribution.png`, `03_quantity_distribution.png`.

### Categorical Variables

| Column | Top Values |
|---|---|
| Gender | Male: 511, Female: 489 — near-even split |
| City | Patna (135), Kolkata (133), Mumbai (131) lead; 13 rows "Unknown" |
| Category | Electronics (354) dominates order count; Education (178), Furniture (159) |
| Product | Mobile (184), Book (178), Laptop (170) most frequently ordered |
| Age_Group | 36-45 (223) and 26-35 (219) are the largest cohorts |
| Revenue_Tier | Medium (359), Low (273), High (259), Premium (109) |

See `charts/04` through `08` for visual breakdowns, and `09_monthly_sales_trend.png` / `10_top_products.png` for time and product views.

---

## 2. SQL for Business Questions

Seven business questions were answered using SQL (SQLite, with a normalized 3-table schema: `orders`, `customers`, `products`) — see `part2_sql_business_questions.py` for full queries and `SQL_Query_Results.xlsx` for results.

| # | Question | Key Finding |
|---|---|---|
| Q1 | Top 5 products by revenue? | Laptop (₹2.54 Cr) and Mobile (₹2.53 Cr) lead, both Electronics |
| Q2 | Monthly sales trend? | Peaked in March (₹1.31 Cr); dipped Aug–Sep (~₹0.92–0.94 Cr) |
| Q3 | Highest-revenue city? | Patna (₹1.93 Cr), Kolkata (₹1.89 Cr), Bengaluru (₹1.88 Cr) |
| Q4 | Age group with highest AOV? | 18-25 surprisingly leads (₹1,43,836 avg), closely followed by 36-45 |
| Q5 | Category performance by gender? | Males outspend in Electronics & Grocery; Females lead in Education & Fashion |
| Q6 | Top 5 customers by lifetime value? | Customer_345 (Kolkata) leads with ₹8.67 lakh across 2 orders |
| Q7 | Premium-tier Electronics orders? | 41 orders ≥₹3,00,000, nearly all Laptops/Mobiles at qty 7–10 |

---

## 3. Multivariate Analysis & Correlation

**Correlation matrix (numerical variables):**

|  | Age | Quantity | Unit_Price | Total_Sales |
|---|---|---|---|---|
| Age | 1.00 | -0.03 | -0.01 | 0.00 |
| Quantity | -0.03 | 1.00 | 0.02 | **0.65** |
| Unit_Price | -0.01 | 0.02 | 1.00 | **0.69** |
| Total_Sales | 0.00 | 0.65 | 0.69 | 1.00 |

**Key relationships:**
- **Quantity ↔ Total_Sales (r = 0.65)** and **Unit_Price ↔ Total_Sales (r = 0.69)**: both strong positive drivers — revenue is jointly explained by how much customers buy and at what price point, not dominated by either alone.
- **Age ↔ everything**: correlations ≈ 0 across the board. Age does not meaningfully predict spend, price preference, or quantity purchased.
- See `charts/11_correlation_heatmap.png`, `12` and `13` (scatter plots), `14`/`15` (box plots), `16` (City × Category heatmap), `17` (pair plot), `18` (grouped bars).

**Notable cuts:**
- City × Category heatmap shows Electronics is the top revenue driver in *every* city — not a regional fluke.
- Box plots show Premium-tier orders have a visibly wider spread for males than females, suggesting more high-ticket purchase variance among male customers.

---

## 4. Dashboard Mock-up

A 4-slide KPI dashboard proposal was built in PowerPoint (`Sales_KPI_Dashboard_Mockup.pptx`):
1. Title slide
2. **8 Proposed KPIs** with rationale (Total Revenue, AOV, Orders & Units Sold, Unique Customers, Revenue by Category, Revenue Tier Mix, Monthly Trend, Top City/Product)
3. **Dashboard mock-up** — KPI cards + monthly trend line, category donut, city bar chart, revenue-tier bar chart (all native, editable PowerPoint charts)
4. **Key Insights summary** tying EDA findings back to the KPI choices

---

## Files in this deliverable
- `part1_univariate_analysis.py` — descriptive stats + 10 charts
- `part2_sql_business_questions.py` — 7 SQL queries + results export
- `part3_multivariate_correlation.py` — correlation matrix + 8 charts
- `SQL_Query_Results.xlsx` — all query outputs, one sheet per question
- `Sales_KPI_Dashboard_Mockup.pptx` — dashboard mock-up deck
- `charts/` — all 18 PNG visualizations referenced above
