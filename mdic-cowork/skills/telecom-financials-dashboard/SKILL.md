---
name: telecom-financials-dashboard
description: Use when asked to create, generate, or build the Financials telecom dashboard artifact. Reads CSV files from the working folder and generates a self-contained HTML artifact with GHS and USD side-by-side panels showing EBITDA, Revenue, COS, and OPEX sparklines, totals, and category breakdowns.
---

# Telecom — Financials Dashboard (GHS & USD)

## Trigger
User asks to generate the Financials dashboard, the GHS/USD page, or Dashboard 3.

## Step 1 — Read CSV Files
Read the following files from the current working directory (/Users/jaddimachkieh/Documents/mdic_test/data):

| File | Purpose |
|---|---|
| `financial_long.csv` | GHS monthly P&L by category (Actual only for this dashboard) |
| `usd_financials_monthly.csv` | USD monthly totals and EBITDA margin |

**`financial_long.csv`** — filter `Type=Actual`, then:
- Group by `Month` + `PL_Line` (sum) → monthly sparklines
- Group by `Category` + `PL_Line` (sum across months) → breakdown bars
- Sum all → YTD totals

**`usd_financials_monthly.csv` key columns:**
`Month`, `Revenue_USD_M`, `COS_USD_M`, `OPEX_USD_M`, `EBITDA_USD_M`, `EBITDA_Margin_Pct`, `USD_GHS_Rate`

Parse both CSVs and embed all aggregated data as JS arrays in the artifact.

## Step 2 — Generate HTML Artifact
Generate a **single self-contained HTML artifact** using Chart.js (CDN).
Two-column layout: GHS on the left, USD on the right.

---

## Color Scheme
```
page bg:      #1a1a28
card bg:      #2d2d3f
header strip: #5c1028
actual line:  #e8335a   (all charts — crimson red)
text:         #ffffff
subtext:      #ccccdd
border:       #3a3a55
```

---

## Layout

### TOP BAR
- Left: bold white "Financials  |"
- Center: white badge — "USD/GHS Rate  **11.64**"
- Right: two dropdowns — "Type: Actual" and "Month: Multiple selections"

### TWO-COLUMN LAYOUT — GHS (left) | USD (right)

Bold **"GHS"** / **"USD"** labels at top of each column.
Each column has the same 8-section structure listed below.

---

## GHS Column
All values computed from `financial_long.csv` where `Type=Actual`.

### 1. EBITDA by Month (sparkline — compact line chart ~100px tall)
Source: `PL_Line=EBITDA`, group by `Month`, sum `Amount_M_GHS`.
Show data labels on each point. No Y-axis grid.

### 2. Summary KPI Cards (2 rows of 3)
```
Row 1: Total Revenue (sum)  |  Total COS (sum)  |  Total OPEX (sum)
Row 2: EBITDA (sum)         |  Total Interest -121.26M (hardcoded) | FOREX 646.59M (hardcoded)
```
Display Revenue as "Xbn" if ≥ 1000M, else "XM".

### 3. Revenue by Month — `PL_Line=Revenue`, group by `Month`, sum
Label the peak month value.

### 4. Revenue Breakdown — `PL_Line=Revenue`, group by `Category`, sum across months
Render as compact horizontal proportional bars with value labels (not a full Chart.js chart), sorted descending.

### 5. COS by Month — `PL_Line=COS`, group by `Month`, sum (negative, bars downward)

### 6. COS Breakdown — `PL_Line=COS`, group by `Category`, sum. Sort by absolute value ascending.

### 7. OPEX by Month — `PL_Line=OPEX`, group by `Month`, sum. Positive months bar above zero.

### 8. OPEX Breakdown — `PL_Line=OPEX`, group by `Category`, sum. Positives first.

---

## USD Column
Same 8-section structure, all data from `usd_financials_monthly.csv`.

### 1. EBITDA by Month — `EBITDA_USD_M` column by `Month`

### 2. Summary KPI Cards
```
Row 1: sum(Revenue_USD_M) | sum(COS_USD_M) | sum(OPEX_USD_M)
Row 2: sum(EBITDA_USD_M)  | sum(Interest_USD_M) | avg(EBITDA_Margin_Pct) — labeled "EBITDA Margin"
```
Note: USD column shows **EBITDA Margin** instead of FOREX.

### 3–8. Revenue/COS/OPEX by Month
Use `Revenue_USD_M`, `COS_USD_M`, `OPEX_USD_M` columns directly by month.
USD has no category breakdown available — show monthly charts only; omit breakdown bars for USD.

---

## Rules
- GHS Revenue YTD ≥ 1000M → display as "Xbn"
- USD column shows EBITDA Margin in place of FOREX
- Sparkline charts: ~100px height, no Y-axis grid, data labels on points
- Revenue breakdown bars: proportional width, value label, sorted descending
- All chart lines/bars in **#e8335a** (Actual only — no budget on this dashboard)
- If any OPEX month is positive — bar above zero line
- If a CSV is missing, show a clear inline error
