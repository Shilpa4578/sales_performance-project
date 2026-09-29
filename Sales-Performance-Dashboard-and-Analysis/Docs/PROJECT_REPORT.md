# Project Report: Sales Performance Dashboard and Analysis

## 1. Introduction
This report documents a full-stack data analytics project analyzing 12,000 retail sales transactions from January 2023 through December 2025. The project was built across four tools — Python, Excel, Power BI, and Tableau — to demonstrate end-to-end analytics proficiency, with every figure cross-validated at each stage of the pipeline.

## 2. Problem Statement
A retail business operating across 5 regions, 5 product categories, and 3 customer segments needed a clear, trustworthy view of where its revenue and profit were strongest and weakest, whether the business was growing, what seasonal patterns existed, and whether pricing/discount practices were affecting profitability — in order to make evidence-based decisions rather than assumptions.

## 3. Project Objectives
- Clean and validate a 12,000-row retail transaction dataset
- Calculate core KPIs: Total Sales, Total Profit, Total Orders, Total Quantity, Average Order Value, Profit Margin
- Analyze performance by product, category, region, customer segment, and time period
- Build a professional, formula-driven Excel workbook
- Build an interactive, multi-page Power BI dashboard with a proper data model and DAX measures
- Establish a validated Tableau data connection as a third BI-tool demonstration
- Produce evidence-based, quantified business insights and recommendations

## 4. Dataset Description
| Attribute | Detail |
|---|---|
| Source file | `retail_sales_12000.csv` |
| Records | 12,000 transactions |
| Columns (raw) | 14 |
| Columns (after cleaning) | 19 |
| Date range | 2023-01-01 to 2025-12-31 |
| Regions | Central, East, North, South, West |
| Categories | Clothing, Electronics, Furniture, Home & Kitchen, Office Supplies |
| Products | 25 individual SKUs |
| Customer segments | Consumer, Corporate, Small Business |
| Payment methods | Cash, Credit Card, Debit Card, Net Banking, UPI |

## 5. Data Cleaning and Preparation
Phase 1 validation confirmed the dataset was already high quality: zero missing values across any of the 14 raw columns, zero fully duplicate rows, and zero duplicate Order IDs. All numeric fields (Sales, Cost, Profit, Quantity, Unit_Price, Discount) contained only valid, non-negative values, and Sales/Profit values reconciled correctly against their component formulas (Sales = Quantity × Unit Price × (1 − Discount); Profit = Sales − Cost) within floating-point rounding tolerance.

Cleaning steps applied:
- Converted `Order_Date` from text to a proper date type
- Rounded Sales/Cost/Profit to 2 decimals to remove floating-point noise
- Derived `Year`, `Month`, `Month_Name`, `Year_Month`, and per-order `Profit_Margin_%` columns
- Investigated statistical outliers (784 in Sales, 876 in Profit, via the IQR method) and confirmed these were legitimate high-value transactions, not data entry errors — retained in the dataset

## 6. Exploratory Data Analysis
Performed in Python (`01_sales_analysis.ipynb`) using Pandas and Matplotlib. Covered: core KPI calculation, monthly and yearly sales/profit trends, month-over-month growth rate, category and product performance ranking (including Top 10 / Bottom 10 products), regional performance, customer segment performance, seasonal trend analysis by calendar month, and a discount-vs-profit-margin correlation analysis. Every calculation was independently re-verified with integrity checks confirming that category, region, product, and segment sales all sum back to the total sales figure exactly.

## 7. Excel Analysis
Built in `02_sales_analysis.xlsx` across 9 sheets: Raw Data and Cleaned Data (converted into native Excel Tables with proper date typing and currency/percentage formatting), a formula-driven KPI Summary (live `SUM`/`COUNTA`/`DIVIDE`-equivalent formulas, not hardcoded values), 10 pivot-style summary tables built with live `SUMIFS` formulas (Sales and Profit each broken down by Region, Category, Product, Month, and Customer Segment), 4 additional analysis sheets, and a 6-chart dashboard sheet. The workbook was recalculated via a LibreOffice-based validation pass with zero formula errors, and every KPI and pivot total was independently cross-checked against the Python results with 100% agreement.

## 8. Power BI Dashboard
Built on a proper star-schema data model: a `Fact_Sales` table (the cleaned 12,000-row dataset) related to a purpose-built `Dim_Date` calendar table (marked as an official Date table to support time-intelligence functions), joined on Order Date with a one-to-many, single-direction relationship. Nine DAX measures were created in a dedicated `_Measures` table: Total Sales, Total Profit, Total Orders, Total Quantity, Average Order Value, Profit Margin, Monthly Sales, Monthly Profit, and Sales Growth % (using `DATEADD` time intelligence).

Five report pages were built:
1. **Executive Overview** — 5 KPI cards, Monthly Sales Trend, Sales by Region, Sales by Category, Top 10 Products
2. **Product Analysis** — Top 10/Bottom 10 Products, Sales by Category, Profit by Category, Sales-vs-Profit scatter chart, Quantity by Product
3. **Regional Analysis** — Sales/Profit by Region, Monthly Regional Trend (multi-line by region), a Region-vs-Category matrix with data bars, and an interactive Region slicer
4. **Customer Analysis** — Sales/Profit by Segment, Average Order Value by Segment, Orders by Segment, and an interactive Customer Segment slicer
5. **Forecast & Insights** — a 6-month sales forecast using Power BI's native Analytics-pane forecasting (95% confidence interval, seasonality = 12), a historical trend line, and a business insights text panel

Final formatting included a custom color theme (matching the Excel dashboard's palette for visual consistency across the whole portfolio), consistent fonts, synced slicers across pages, a navigation button bar on every page, and a mobile layout for the Executive Overview. The report was self-checked and confirmed to show Total Sales and Total Profit matching the validated totals, with no relationship errors in the model.

## 9. Tableau Dashboard
Tableau Desktop was connected to the validated `Cleaned Data` table from the Phase 3 Excel workbook. The connection was verified: 12,000 rows, all 19 columns present, and the first record spot-checked against every prior phase. Field types were reviewed and corrected where needed (Order_Date confirmed as a true Date with a working Year/Quarter/Month/Day hierarchy; Year and Month converted from Measures to Dimensions; Profit_Margin_% deliberately left unformatted to avoid a percentage-display multiplication bug). Seven calculated KPI fields were built, mirroring the same logic used in Python, Excel, and Power BI: Total Sales, Total Profit, Total Orders (via `COUNTD`), Total Quantity, Average Order Value, Profit Margin, and a Sales Growth % table calculation. `SUM(Sales)` and `SUM(Profit)` were confirmed matching the validated totals exactly.

**This is stated as a partial component of the project.** The worksheet-level chart building, the 5 planned dashboard pages, filters, and final formatting were not completed. Tableau's role in this project is to demonstrate cross-platform BI proficiency at the data-connection and calculated-field level.

## 10. KPI Analysis
| KPI | Value | Formula Basis |
|---|---|---|
| Total Sales | $9,779,538.30 | Sum of all order Sales values |
| Total Profit | $3,087,335.94 | Sum of all order Profit values |
| Total Orders | 12,000 | Distinct count of Order IDs |
| Total Quantity | 48,120 | Sum of all order Quantities |
| Average Order Value | $814.96 | Total Sales ÷ Total Orders |
| Overall Profit Margin | 31.57% | Total Profit ÷ Total Sales |

These six figures were calculated identically (and verified to match) across Python, Excel, and Power BI.

## 11. Forecasting
Forecasting was implemented using Power BI's built-in Analytics-pane forecasting feature, which applies exponential smoothing to the Monthly Sales Trend visual. Configuration: 6-month forecast length, 95% confidence interval, and seasonality explicitly set to 12 (rather than left on auto-detection), informed by the confirmed recurring December-peak/February-trough pattern found in the historical data. No separate statistical or Python-based forecasting model was built for this project — this scope is stated explicitly to avoid overstating the forecasting component.

## 12. Key Business Insights
1. Sales declined slightly over the 3-year window (2023: $3.30M → 2025: $3.20M, ~3%) while profit margin held steady at 31.57% — indicating a volume problem, not a margin/efficiency problem.
2. Electronics accounts for 45% of total sales ($4.40M) — more than double the next category (Furniture, $2.95M) — representing meaningful category concentration risk.
3. The top 3 products by sales (Smartphone, Headphones, Laptop) are all Electronics; the 2 weakest products (Paper Pack, Printer) are both Office Supplies — product performance mirrors category performance.
4. Regional sales range from $2.20M (North) to $1.73M (East), but profit margin is consistent across all 5 regions (31.3%–31.9%) — the regional gap is one of sales volume, not efficiency.
5. Consumer drives the most total sales and profit; Small Business has the highest average order value ($842.64) despite the fewest orders — two distinct customer behaviors requiring different strategies.
6. December is consistently the strongest month and February the weakest, in every year of the dataset — a reliable, plannable seasonal pattern.
7. The correlation between Discount and Profit Margin % is 0.02 — discounting is not measurably eroding profitability in this dataset.
8. Every one of the 12,000 orders is profitable — the minimum single-order profit is $5.94, with zero loss-making transactions.
9. 784 sales-value and 876 profit-value statistical outliers were identified and confirmed as legitimate large orders (up to $4,718.76), likely linked to the Corporate/Small Business segments given their higher order values.
10. Month-over-month growth swings between roughly -23% and +32%, but averages approximately +1% — volatility consistent with seasonality rather than instability, but confirming the near-flat overall trajectory.
11. Even the weakest category (Office Supplies) carries a profit margin (~31.4%) nearly identical to the company-wide average — reinforcing that the business's unit economics are sound everywhere; the opportunity is volume, not pricing or cost structure.

## 13. Business Recommendations
1. Invest in demand generation and market expansion in underperforming regions (East, Central) and categories (Clothing, Office Supplies), since their profit margins are already on par with top performers — the constraint is sales volume, not efficiency.
2. Protect and continue investing in proven strengths: the Electronics category, the North region, and the December seasonal peak.
3. Apply a segment-specific strategy: volume and retention-focused marketing for Consumer; a higher-touch, consultative approach for Small Business to grow order frequency while preserving its already-strong order value.
4. Use the confirmed seasonal pattern to plan inventory, staffing, and marketing timing in advance, with particular attention to building demand around the February trough.
5. Preserve the current pricing and cost discipline (zero loss-making orders) as an explicit guardrail when introducing new discount programs, since discounting has not been shown to hurt margin at the levels currently used.

## 14. Tools and Technologies
- **Python:** Pandas, NumPy, Matplotlib (data cleaning, KPI calculation, exploratory visualization)
- **Excel:** openpyxl (programmatic workbook construction), native formulas (SUMIFS, COUNTA, SUM), native Excel Tables and chart objects, LibreOffice-based recalculation validation
- **Power BI:** Power Query (data transformation), DAX (9 measures), star-schema data modeling, native Analytics-pane forecasting
- **Tableau:** Excel data source connection, field-type validation, calculated fields (including a table calculation for Sales Growth %)

## 15. Challenges and Solutions
- **Challenge:** Native Excel PivotTables and openpyxl-generated charts occasionally lost formatting or rendering integrity when round-tripped through automated recalculation. **Solution:** switched to formula-driven pivot-equivalent summary tables (SUMIFS-based) for reliability, and explicitly set chart series colors rather than relying on default theming, which resolved a white-on-white line rendering bug found during validation.
- **Challenge:** A horizontal bar chart (Top 10 Products) initially plotted in the wrong visual order. **Solution:** rather than relying on axis-orientation settings (which caused other rendering issues), built a small reverse-order helper data table specifically for that chart.
- **Challenge:** Keeping numbers consistent across four different tools built at different times. **Solution:** every phase's KPIs were re-derived independently and cross-checked against the previous phase's validated numbers before being accepted, rather than copied forward assumptively.

## 16. Conclusion
This project delivers a complete, cross-validated retail analytics workflow — from a raw 12,000-row CSV through Python analysis, a formula-driven Excel workbook, and a fully interactive 5-page Power BI dashboard, with an additional Tableau data-connection component. Every headline figure, from the $9.78M total sales down to the 0.02 discount-margin correlation, was independently verified at multiple stages, reflecting a deliberate, accuracy-first approach to the analysis.

## 17. Future Scope
- Complete the remaining Tableau worksheets and dashboard pages to match the Power BI scope
- Add a Python-based statistical forecasting model (e.g. Prophet or ARIMA) to compare against the Power BI native forecast
- Extend customer segmentation with RFM (Recency, Frequency, Monetary) analysis if customer-level purchase history is expanded
- Automate the data refresh pipeline across Excel and Power BI if the dataset is extended with new time periods
