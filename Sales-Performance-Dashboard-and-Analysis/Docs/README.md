# Sales Performance Dashboard and Analysis

A full-stack data analytics portfolio project analyzing 12,000 retail transactions (2023–2025) using Python, Excel, Power BI, and Tableau — covering data cleaning, KPI analysis, regional/product/customer segmentation, and business recommendations.

---

## Project Overview

This project takes a raw retail sales dataset and builds a complete analytics pipeline across four tools: a Python/Pandas exploratory analysis, a formula-driven Excel workbook, a 5-page interactive Power BI dashboard, and an initial Tableau data connection. The goal was to demonstrate end-to-end analytics proficiency — the same validated numbers, cross-checked at every stage, carried consistently from raw CSV through to final business insights.

## Business Problem

A retail business with 12,000 transactions across 5 regions, 5 categories, and 3 customer segments needs a clear view of: where revenue and profit are strongest and weakest, whether growth is happening, what seasonal patterns exist, and where discounting or product mix decisions should change.

## Objectives

- Clean and validate a 12,000-row retail transaction dataset
- Calculate core KPIs (Sales, Profit, Orders, Quantity, AOV, Profit Margin)
- Analyze performance by product, category, region, customer segment, and month
- Build a professional Excel workbook with formulas, pivot-style summaries, and native charts
- Build an interactive multi-page Power BI dashboard with DAX measures
- Begin a Tableau connection as a third BI-tool demonstration
- Produce evidence-based business insights and recommendations

## Dataset Description

| | |
|---|---|
| File | `retail_sales_12000.csv` |
| Rows | 12,000 transactions |
| Columns | 14 (raw) → 19 (after cleaning) |
| Date range | January 1, 2023 – December 31, 2025 |
| Regions | Central, East, North, South, West (5) |
| Categories | Clothing, Electronics, Furniture, Home & Kitchen, Office Supplies (5) |
| Products | 25 individual SKUs |
| Customer segments | Consumer, Corporate, Small Business (3) |

**Data quality (verified in Phase 1):** zero missing values, zero duplicate rows, zero duplicate Order IDs, no negative or invalid values in Sales/Cost/Profit/Quantity/Discount. No loss-making orders exist anywhere in the dataset (minimum order profit: $5.94).

## Tools and Technologies

- **Python:** Pandas, NumPy, Matplotlib for data cleaning, KPI calculation, and visualization
- **Excel:** openpyxl-built workbook with live SUMIFS/COUNTA formulas, pivot-style summary tables, and native chart objects — recalculated and validated via LibreOffice
- **Power BI:** Power Query, a dedicated `_Measures` table with 9 DAX measures, a proper `Dim_Date` table with a star-schema relationship, and 5 report pages
- **Tableau:** Data source connection to the validated Excel data, field-type verification, and KPI calculated fields defined (see Tableau section below for exact scope)

## Data Analysis Workflow

```
Raw Data (CSV)
      ↓
Python / Pandas — cleaning, KPI calculation, exploratory analysis
      ↓
Excel — formula-driven validation, pivot-style summaries, charts
      ↓
Power BI — data model, DAX measures, 5-page interactive dashboard
      ↓
Tableau — data connection and validation (partial — see below)
      ↓
Business Insights & Recommendations
```

## Data Cleaning

- Converted `Order_Date` from text to proper date type
- Rounded floating-point noise in Sales/Cost/Profit to 2 decimals
- Derived `Year`, `Month`, `Month_Name`, `Year_Month`, and per-order `Profit_Margin_%` columns
- Verified 784 sales-value and 876 profit-value statistical outliers were legitimate high-value orders, not data errors — kept in the dataset

## Exploratory Analysis (Python)

Built in `01_sales_analysis.ipynb`: core KPIs, monthly/yearly trend and growth-rate analysis, category/product/region/segment breakdowns, top/bottom 10 products, seasonal trend analysis, and discount-vs-profit correlation analysis. All calculations were independently re-verified with integrity checks confirming every subgroup breakdown sums back to total sales exactly.

## Excel Analysis

`02_sales_analysis.xlsx` contains 9 sheets: Raw Data and Cleaned Data (as native Excel Tables), a formula-driven KPI Summary, 10 pivot-style summary tables (Sales/Profit × Region, Category, Product, Month, Segment — all via live `SUMIFS` formulas, not hardcoded numbers), 4 dedicated analysis sheets, and a 6-chart dashboard sheet. Every formula was recalculated and validated with zero errors, and every KPI and pivot total was cross-checked against the Python results.

## Power BI Dashboard

A 5-page interactive report built on a proper star-schema model (`Fact_Sales` + `Dim_Date`, related on Order Date, with `Dim_Date` marked as an official date table) and 9 DAX measures:

1. **Executive Overview** — 5 KPI cards, Monthly Sales Trend, Sales by Region, Sales by Category, Top 10 Products
2. **Product Analysis** — Top 10/Bottom 10 Products, Sales by Category, Profit by Category, Sales-vs-Profit scatter, Quantity by Product
3. **Regional Analysis** — Sales/Profit by Region, Monthly Regional Trend, Region-vs-Category matrix, Region slicer
4. **Customer Analysis** — Sales/Profit by Segment, Average Order Value by Segment, Orders by Segment, Segment slicer
5. **Forecast & Insights** — Monthly sales forecast (Power BI's built-in Analytics-pane forecasting, 6-month projection, 95% confidence interval, seasonality set to 12), a trend line over the 3-year actuals, and a business insights panel

The report also includes a custom color theme, synced slicers, navigation buttons across all 5 pages, and a mobile layout for the Executive Overview page. Self-checked and confirmed working, with Total Sales and Total Profit verified matching the validated totals.

## Tableau Dashboard (Partial — Honest Scope)

**What was completed:** Tableau Desktop was connected directly to the validated `Cleaned Data` table from the Phase 3 Excel workbook (12,000 rows, 19 columns confirmed). Field types were verified and corrected (Order_Date as a true Date field with a working date hierarchy, Year/Month converted to Dimensions, Profit_Margin_% left unformatted to avoid a percentage-display bug). Seven KPI calculated fields were defined — Total Sales, Total Profit, Total Orders, Total Quantity, Average Order Value, Profit Margin, and a Sales Growth % table calculation — matching the same logic used in the Python, Excel, and Power BI phases. `SUM(Sales)` and `SUM(Profit)` were confirmed to match the validated totals exactly.

**What was not completed:** the worksheet-by-worksheet chart building, the 5 dashboard pages, filter/interaction setup, and final formatting were not built out. Tableau is included in this project as evidence of cross-platform BI tool proficiency at the data-connection and calculated-field level, not as a finished third dashboard. This is stated plainly here rather than implied otherwise.

## Key KPIs

| KPI | Value |
|---|---|
| Total Sales | $9,779,538.30 |
| Total Profit | $3,087,335.94 |
| Total Orders | 12,000 |
| Total Quantity | 48,120 |
| Average Order Value | $814.96 |
| Overall Profit Margin | 31.57% |

## Key Validated Findings

- **Flat growth:** Sales declined slightly across the 3-year window (2023: $3.30M → 2025: $3.20M, ~3% decline), while profit margin held steady at 31.57% — a volume problem, not a margin problem.
- **Category concentration:** Electronics alone accounts for 45% of total sales ($4.40M), more than double the next category (Furniture, $2.95M).
- **Regional gap is not a margin issue:** North leads sales ($2.20M) and East trails ($1.73M), but profit margin is consistent across all 5 regions (31.3%–31.9%) — the gap is sales volume, not efficiency.
- **Segment behavior differs:** Consumer drives the most volume and profit; Small Business has the highest average order value ($842.64) despite the fewest orders.
- **Reliable seasonality:** December is consistently the strongest month and February the weakest, every year of the dataset.
- **Discounting is not hurting margin:** correlation between Discount and Profit Margin % is 0.02 — effectively zero.
- **No loss-making orders:** every one of the 12,000 orders is profitable (minimum order profit: $5.94).

*(Full set of 11 evidence-based insights, each with Finding → Business Meaning → Recommendation, is documented in the Business Recommendations section below and in the project report.)*

## Forecasting

Implemented via Power BI's native Analytics-pane forecasting feature (exponential smoothing) on the Monthly Sales Trend, projecting 6 months forward with a 95% confidence interval and seasonality explicitly set to 12 (informed by the confirmed December/February seasonal pattern). No separate Python or manual statistical forecasting model was built — this is stated explicitly so the forecasting claim is not overstated.

## Business Recommendations

1. Invest in demand generation and market expansion in underperforming regions (East, Central) and categories (Clothing, Office Supplies) — their profit margins are already on par with top performers, so the constraint is sales volume, not efficiency.
2. Protect and continue investing in proven strengths: Electronics category, North region, December seasonal peak.
3. Build a segment-specific strategy: volume/retention focus for Consumer, higher-touch account management for Small Business to grow order frequency.
4. Use the confirmed seasonal pattern to plan inventory, staffing, and marketing timing — particularly building demand around the February trough.
5. Preserve the current pricing/cost discipline (zero loss-making orders) as an explicit guardrail when introducing new discount programs, since discounting has not been shown to hurt margin in this data.

## Project Architecture / Workflow

```
Raw Data
   ↓
Python / Pandas
   ↓
Data Cleaning & Analysis
   ↓
Excel Validation
   ↓
Power BI Dashboard
   ↓
Tableau (data connection & validation — partial)
   ↓
Insights & Forecasting
   ↓
Business Recommendations
```

## Repository Structure

```
Sales-Performance-Dashboard-and-Analysis/
│
├── data/
│   └── retail_sales_12000.csv
│
├── python/
│   └── 01_sales_analysis.ipynb
│
├── excel/
│   └── 02_sales_analysis.xlsx
│
├── powerbi/
│   └── Sales_Performance_Dashboard.pbix
│
├── tableau/
│   └── Sales_Performance_Dashboard_partial.twbx
│
├── outputs/
│   ├── charts/
│   └── screenshots/
│
├── docs/
│   ├── project_report.md
│   └── final_insights.md
│
└── README.md
```

## How to Run / Use This Project

1. **Python:** open `python/01_sales_analysis.ipynb` in Jupyter, run all cells top to bottom. Requires `pandas`, `numpy`, `matplotlib`, `seaborn`.
2. **Excel:** open `excel/02_sales_analysis.xlsx` directly in Microsoft Excel — all formulas recalculate automatically on open.
3. **Power BI:** open `powerbi/Sales_Performance_Dashboard.pbix` in Power BI Desktop.
4. **Tableau:** open `tableau/Sales_Performance_Dashboard_partial.twbx` in Tableau Desktop — this shows the validated data connection and calculated fields, not a finished dashboard.

## Screenshots

*(See the Screenshot Checklist in the project report / portfolio materials for the exact list of images to capture and place in `outputs/screenshots/`.)*

## Future Improvements

- Complete the remaining Tableau worksheets and 5-page dashboard to match the Power BI scope
- Add a Python-based statistical forecasting model (e.g. Prophet or ARIMA) alongside the Power BI native forecast for comparison
- Expand the customer segmentation analysis with RFM (Recency, Frequency, Monetary) scoring if customer-level transaction history is extended
- Automate the Excel/Power BI refresh pipeline if the dataset is extended with new periods

## Conclusion

This project demonstrates a complete, cross-validated analytics workflow from raw data to business recommendations, using four industry-standard tools. Every number presented — from the $9.78M total sales figure down to the 0.02 discount-margin correlation — was independently verified at each stage of the pipeline, reflecting a deliberate emphasis on accuracy and cross-tool consistency over speed.
