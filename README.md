# Budget Finance Analysis

## Business Problem
A company wants to understand the balance between income and expenses — specifically, whether income consistently outpaces spending, where spending is concentrated, and whether that pattern holds steady over time.

## Data Collection
**Dataset:** [BudgetWise Personal Finance Dataset](https://www.kaggle.com/datasets/mohammedarfathr/budgetwise-personal-finance-dataset) (Kaggle, publisher: mohammedarfathr)
**Size:** 15,836 transaction records
**Key fields:** Date, Transaction_Type (Income/Expense), Category, Amount, Payment_Mode, Location

This dataset was intentionally chosen for being messy and synthetic-but-realistic, with mixed currencies, inconsistent formatting, and placeholder values — providing genuine, real-world-style data cleaning practice.

## Data Cleaning
- Removed 804 fully duplicate rows
- Parsed `Date` from four mixed formats into proper datetime; unparseable dates left as missing rather than guessed
- Cleaned `Category`, `Payment_Mode`, and `Location` using fuzzy matching to collapse hundreds of typo variants (e.g., "Reent", "Rnet", "Ren" → "Rent"), with manual patches for short/ambiguous leftovers
- Extracted mixed currency symbols (₹, $) from `Amount` into a separate `Currency` column, then converted the numeric portion to a proper decimal type
- Identified `999999` as a repeated placeholder value for unknown/missing amounts (not a real transaction or genuine outlier) and converted it to missing
- Split cleaned amounts into three separate columns — USD, INR, and Unknown-currency — since the two currencies are not directly comparable and combining them would produce a misleading total
- Missing categorical values filled with "Unknown"; missing amounts left blank rather than estimated, since fabricating a financial figure would distort the very numbers this analysis depends on

## Key Insights

- **USD and INR both show strong, comparable income-to-expense ratios** (11.7x and 12.3x respectively) — income consistently and substantially outpaces spending in both currencies
- **The Unknown-currency group shows an even higher ratio (13.3x)**, but since its currency is unverified, this figure isn't directly comparable to the confirmed USD/INR groups and should be treated cautiously
- **Income amounts cluster suspiciously close together across all three currency groups** (roughly $59,000–63,000 on average, regardless of currency), while Expense amounts remain realistic and distinct per currency — this is unlikely in real financial data and points to a data-generation artifact rather than a genuine finding, and is flagged as a dataset limitation
- **Food is consistently the single largest expense category** across all three currency groups, followed by Rent — both essential, recurring costs. Travel and Utilities round out the top four in every group, a pattern that holds steady and appears genuine (unlike the income figures)
- **A sharp Income/Expense swing occurs in October** (Expense drops, Income spikes) — verified via a Year slicer to be a **one-off anomaly**, not a recurring seasonal pattern, since it does not repeat consistently across the multi-year data

## Business Recommendations
- Treat Unknown-currency transactions as lower-confidence data; prioritize resolving/verifying the currency before using these records in financial decision-making
- Since Food and Rent dominate expenses across the board, these are the highest-leverage categories for any cost-reduction or budgeting initiative
- Flag the cross-currency income uniformity as a data-quality concern if this analysis were to inform real financial reporting — the surplus figures should not be presented as confirmed real-world findings without validating the income data source
- Investigate the October anomaly directly (e.g., a one-time bonus payout or a data entry batch) rather than planning around it as a repeating pattern

## Dashboard
Built in Power BI, across three pages:

1. **Overview** — KPI cards (Total Income/Expense per currency) and an Income-to-Expense ratio comparison across currencies
2. **Category Spending** — Top expense categories and income by payment mode, broken out by currency
3. **Trends Over Time** — Monthly Income/Expense trend lines (USD), with Year and Quarter filters

## Dashboard Images

### Overview
![Overview](images/Overview.png)

### Category Spending
![Category Spending](images/Category_Spending.png)

### Trends Over Time
![Trends Over Time](images/Trends_Over_Time.png)

## Tools Used
- **Python** (pandas) — data cleaning and exploratory analysis, including fuzzy-matching-based typo correction
- **Power BI** (Power Query, DAX, Time Intelligence) — data modeling and dashboard visualization

## Files in This Repository
- `BudgetWise_Finace_clean.csv` — cleaned dataset, exported from Python
- `Finance_Analysis.ipynb` — full analysis notebook (exploration, cleaning, EDA, findings)
- `Budget_Finance_Analysis_Dashboard.pbix` — Power BI dashboard file
