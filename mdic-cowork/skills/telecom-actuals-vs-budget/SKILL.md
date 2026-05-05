---
name: telecom-actuals-vs-budget
description: Use when asked to create, generate, or build the Actual vs Budget telecom dashboard artifact. Reads CSV files from the working folder's data/ directory and generates a self-contained HTML artifact with EBITDA, Revenue, COS, and OPEX monthly charts plus YTD totals and category breakdowns.
---

# Telecom — Actual vs Budget Dashboard (GHS)

## Trigger
User asks to generate the Actual vs Budget dashboard, the budget comparison page, or Dashboard 2.

## Step 1 — Read CSV Files
Read the following files from the current working directory (/Users/jaddimachkieh/Documents/mdic_test/data):

| File | Purpose |
|---|---|
| `financial_long.csv` | All monthly P&L data — actuals and budgets by category |
| `usd_financials_monthly.csv` | USD/GHS rate |

**`financial_long.csv` columns:**
`Month`, `Type` (Actual/Budget), `PL_Line` (Revenue/COS/OPEX/EBITDA), `Category`, `Amount_M_GHS`

From this single file you can derive everything:
- **YTD totals**: sum `Amount_M_GHS` grouped by `PL_Line` + `Type`
- **Monthly trends**: group by `Month` + `PL_Line` + `Type`, sum across categories
- **Category breakdowns**: group by `Category` + `Type` where `PL_Line` = target line, sum across months
- **Month filter**: filter rows by `Month` before aggregating

## Step 2 — Generate HTML Artifact
Generate a **single self-contained HTML artifact** using Chart.js (CDN).

The artifact must read CSVs live on every open using `window.cowork.callMcpTool` — do NOT hardcode data as JS arrays.

Use this pattern to read each file:

```js
async function readFile(path) {
  const res = await window.cowork.callMcpTool('mcp__filesystem__read_file', { path });
  if (typeof res === 'string') return res;
  if (Array.isArray(res)) return res.map(r => r.text ?? '').join('');
  if (res?.content) {
    const c = res.content;
    if (typeof c === 'string') return c;
    if (Array.isArray(c)) return c.map(r => r.text ?? '').join('');
  }
  if (res?.text) return res.text;
  return String(res);
}
```

On `DOMContentLoaded`:
1. Show a loading spinner
2. Call `readFile()` for both CSVs in parallel using `Promise.all`
3. Parse and aggregate the CSV data in-browser using JS
4. Render all charts and KPIs from the parsed data
5. Show an error box if either file fails to load

Register `mcp_tools: ["mcp__filesystem__read_file"]` on the artifact.

---

## Color Scheme
```
page bg:      #1a1a28
card bg:      #2d2d3f
header strip: #5c1028
actual:       #e8335a   (crimson-red — ALL actual bars/lines)
budget:       #8888aa   (muted gray-blue — ALL budget bars/lines)
text:         #ffffff
border:       #3a3a55
```

---

## Layout

### TOP BAR
- Left: bold white "Actual vs Budget  |  GHS"
- Center: white badge — "USD/GHS Rate (Avg.)  **{rate from usd_financials_monthly.csv}**"
- Right: "Month" dropdown — "All" = YTD; individual months filter all charts

### ROW 1 — Four YTD total cards
Computed from `financial_long.csv` — sum all months, grouped by PL_Line + Type.
Each card shows two pill rows (Actual = crimson bg, Budget = dark gray bg):

| Total Revenue | Total COS | Total OPEX | EBITDA |
|---|---|---|---|
| Actual {sum} | Actual {sum} | Actual {sum} | Actual {sum} |
| Budget {sum} | Budget {sum} | Budget {sum} | Budget {sum} |

Format totals as full integers with commas (e.g. 2,006,392,025).

### ROW 2 — Two charts side by side

**LEFT — EBITDA by Month** (line chart, two lines)
Aggregate `financial_long.csv`: `PL_Line=EBITDA`, group by `Month` + `Type`, sum `Amount_M_GHS`.
Show data labels on both lines. Legend: "● Actual  ● Budget"

**RIGHT — Revenue Breakdown** (grouped horizontal bar, Actual red / Budget gray)
Aggregate: `PL_Line=Revenue`, group by `Category` + `Type`, sum across all months (or selected month).

### ROW 3 — Two charts side by side

**LEFT — Revenue by Month** (line chart, two lines)
Aggregate: `PL_Line=Revenue`, group by `Month` + `Type`, sum `Amount_M_GHS`.
Label the highest actual month value.

**RIGHT — COS Breakdown** (grouped vertical bar, bars point downward)
Aggregate: `PL_Line=COS`, group by `Category` + `Type`, sum across months.

### ROW 4 — Two charts side by side

**LEFT — COS by Month** (grouped vertical bar, negative)
Aggregate: `PL_Line=COS`, group by `Month` + `Type`, sum `Amount_M_GHS`.

**RIGHT — OPEX Breakdown** (grouped horizontal bar, mixed pos/neg)
Aggregate: `PL_Line=OPEX`, group by `Category` + `Type`, sum across months.

### ROW 5 — OPEX by Month (full-width grouped bar)
Aggregate: `PL_Line=OPEX`, group by `Month` + `Type`, sum `Amount_M_GHS`.
⚠️ If any month's Actual OPEX is positive — bar renders above zero line, label with "+" prefix.

---

## Rules
- Actual always in **#e8335a**, Budget always in **#8888aa**
- YTD totals: format as full integers with commas
- Chart values: "M" suffix rounded to 2dp
- Month dropdown: "All" = all months; selecting a month filters `financial_long.csv` rows by that `Month` value before aggregating
- If CSV is missing, show a clear inline error
