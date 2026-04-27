# Stage 3 AAPL – Technical Specification

**Created by:** Kyson Murasaki
**Updated by:** Kyson Murasaki
**Date Created:** April 22, 2026
**Date Updated:** April 26, 2026
**Version:** 2.0
**LLM Used:** None

**Role:** Financial Analyst / FP&A Analyst
**Audience:** CFO or Director of FP&A

**Purpose:** Provide a professional, quantitative specification documenting your Excel model's analytical structure for computing and interpreting accounting and performance ratios from a public company's financial statements. This post-build spec captures what you built, what you learned, and how the model should be refined.

---

## 1. Problem Statement

Apple is a publicly traded technology company. This specification outlines the analytical framework for computing 25+ accounting and performance ratios from the company's FY 2025 and 2024 financial statements, enabling management to assess financial health, operational efficiency, leverage, and value creation. The decision context would be for a CFO briefing or presentation for the Director of FP&A.

---

## 2. Inputs (Known Variables)

### Balance Sheet Items (Current Year and Prior Year)

| Variable | Description | Named Range | Year | Value |
|----------|-------------|-------------|------|---------|
| Cash & marketable securities | Liquid assets | `BAL_cash_marketable_securities_2025` | 2025 | 53,454 |
| Cash & marketable securities | Liquid assets | `BAL_cash_marketable_securities_2024` | 2024 | 65,171 |
| Receivables | Accounts receivable | `BAL_receivables_2025` | 2025 | 33,625 |
| Receivables | Accounts receivable | `BAL_receivables_2024` | 2024 | 33,410 |
| Inventories | Inventory balance | `BAL_inventories_2025` | 2025 | 7,230 |
| Inventories | Inventory balance | `BAL_inventories_2024` | 2024 | 7,286 |
| Total current assets | Sum of current assets | `BAL_assets_current_2025` | 2025 | 137,852 |
| Total current assets | Sum of current assets | `BAL_assets_current_2024` | 2024 | 152,987 |
| Net tangible fixed assets | PP&E less depreciation | `BAL_fixed_assets_net_2025` | 2025 | 45,446 |
| Total assets | All assets | `BAL_assets_total_2025` | 2025 | 370,722 |
| Total assets | All assets | `BAL_assets_total_2024` | 2024 | 364,980 |
| Total current liabilities | Short-term obligations | `BAL_liabilities_current_2025` | 2025 | 184,452 |
| Long-term debt | Non-current borrowings | `BAL_debt_long_term_2025` | 2025 | 84,566 |
| Long-term debt | Non-current borrowings | `BAL_debt_long_term_2024` | 2024 | 85,750 |
| Total liabilities | All liabilities | `BAL_liabilities_total_2025` | 2025 | 316,201 |
| Shareholders' equity | Book value of equity | `BAL_equity_shareholders_2025` | 2025 | 54,521 |
| Shareholders' equity | Book value of equity | `BAL_equity_shareholders_2024` | 2024 | 56,950 |

### Income Statement Items

| Variable | Description | Named Range | Value |
|----------|-------------|-------------|---------|
| Net sales | Total revenue | `INC_sales` | 400,736 |
| Cost of goods sold | Direct costs | `INC_cost_goods_sold` | 214,179 |
| SG&A expenses | Operating expenses | `INC_sga` | 27,012 |
| Depreciation | Non-cash expense | `INC_depreciation` | 11,861 |
| EBIT | Operating income | `INC_ebit` | 147,684 |
| Other income | Non-operating income | `INC_other_income` | 3,718 |
| Interest expense | Cost of debt | `INC_interest_expense` | 3,614 |
| Taxes | Income tax expense | `INC_taxes` | 29,984 |
| Net income | Bottom line | `INC_net` | 117,804 |
| Dividends | Shareholder distributions | `INC_dividends` | 16,430 |

### Cash Flow Statement Items

| Variable | Description | Named Range | Value |
|----------|-------------|-------------|---------|
| Cash from operations | Operating cash flow | `CASH_operating` | 137,205 |
| Cash from investments | Investing cash flow | `CASH_investments` | -4,077 |

### Market / Analyst Inputs

| Variable | Description | Named Range | Value |
|----------|-------------|-------------|---------|
| Share price | Market price per share | `share_price` | 213.49 |
| Shares outstanding | Total shares (millions) | `shares_outstanding` | 15,115 |
| Cost of capital | WACC or required return | `cost_capital` | 9.0% |
| Tax rate | Statutory or effective rate | `tax_rate` | 24.1% |

---

## 3. Named Range Conventions

All named ranges are in the inputs section tables above.

---

## 4. Assumptions & Constraints

- All figures reported in millions unless otherwise noted.
- Tax rate assumed at 24.1%.
- Cost of capital estimated at 9.0%.
- Interest rates quoted on a simple annual basis.
- Depreciation figure taken from Income Statement.
- Start-of-year values use prior fiscal year's Balance Sheet.
- No off-balance-sheet items or contingent liabilities included.

---

## 5. Calculation Flow

### Step 1: Derived Inputs
1. Compute `market_capitalization` = `share_price` x `shares_outstanding`
2. Compute `currentYear_after_tax_operating_income` = `INC_net` + (1 - `tax_rate`) x `INC_interest_expense`
3. Compute daily figures: `currentYear_daily_sales_average` = `INC_sales` / 365
4. Compute averages: `avg_equity` = AVERAGE(`startYear_equity`, `currentYear_equity`)
5. Compute `currentYear_working_capital_net` = `currentYear_assets_current` - `currentYear_liabilities_current`

### Step 2: Performance Ratios
- MVA = `market_capitalization` - `currentYear_equity`
- Market-to-Book = `market_capitalization` / `currentYear_equity`
- EVA = `currentYear_after_tax_operating_income` - (`cost_capital` x `startYear_total_capitalization`)

### Step 3: Profitability Ratios
- ROA = `currentYear_after_tax_operating_income` / `startYear_total_assets`
- ROC = `currentYear_after_tax_operating_income` / `startYear_total_capitalization`
- ROE = `INC_net` / `startYear_equity`
- (Repeat with average denominators)

### Step 4: Efficiency Ratios
- Asset Turnover = `INC_sales` / `startYear_total_assets`
- Receivables Turnover, Inventory Turnover, margins, etc.

### Step 5: Leverage Ratios
- Long-term Debt Ratio, Debt-Equity, Total Debt Ratio
- Times Interest Earned, Cash Coverage, Debt Burden, Leverage Ratio

### Step 6: Liquidity Ratios
- NWC-to-Assets, Current Ratio, Quick Ratio, Cash Ratio

### Step 7: Du Pont Decomposition
- Du Pont ROA = Asset Turnover x Operating Profit Margin
- Du Pont ROE = Leverage x Asset Turnover x Operating Profit Margin x Debt Burden

---

## 6. Outputs

| Output | Description | Format | Purpose |
|--------|-------------|--------|---------|
| Ratio summary table | All 25+ ratios organized by category | Table | Core analytical output |
| Du Pont decomposition | ROA and ROE breakdown | Table | Identifies return drivers |
| Formula documentation | Named-range formula for each ratio | Column | Auditability |

---

## 7. Model Review — What Worked & What to Improve

- **What worked well?** Which formulas, layouts, or named ranges operated as intended?

The ROA (start-of-year) and ROA (average) were relatively close to each other (33.0% and 32.8% respectively). The Du Pont ROA (33.0%) matched my directly computed ROA (33.0%). The Du Pont ROE (219.5%) was somewhat similar to my directly computed ROE (206.9%). Lastly,  the current ratio was 0.7x (less than 1), and the NWC was -12.6% (negative), which matches up.

- **What would you change?** Were there workarounds, naming inconsistencies, or layout issues worth fixing?

There was a rounding difference between the actual share price (213.49) and the model (213). There were also inconsistencies in the cost of capital and tax rate in the model (ex: actual cost of capital was 9.0%, but model showed 0%; actual tax rate was 24.1%, but model showed 0%). In terms of formatting/layout issues, random cells had no fill color when they should have been filled, and the auto generated cell widths caused some words and numbers to be unreadable. Also, some of the named ranges had 2023 for the year for some reason, despite me originally asking Claude to use FY2025 and 2024 data. 

- **What would make the model more auditable?** Consider formula documentation, color coding, or structural changes.

Having borders around each cell, and also having alternating fill colors for every other row would make the model easier to read. Currently, it is just highlighted text on a white background.

- **What additional analysis is worth including?** Industry comparisons, trend data, additional ratios?

I think including some analysis from other companies in the tech industry would help determine if Apple is doing well in comparison to those other companies.

---

## 8. Limitations & Next Steps

This specification does not incorporate comparisons from other companies in the tech industry, as well as an analysis from previous years to show possible trends. The next phase will involve writing a structured AI prompt and final analysis interpreting the ratio results.
