---
name: telecom-kpis-dashboard
description: Use when asked to create, generate, or build the Monthly KPIs telecom dashboard artifact. Reads CSV files from the working folder's data/ directory and generates a self-contained HTML artifact with financials cards, subscriber KPIs, revenue breakdown donut, and inflows/outflows.
---

# Telecom — Monthly KPIs Dashboard

## Trigger
User asks to generate the Monthly KPIs dashboard, the KPI page, or Dashboard 1.

## Step 1 — Read CSV Files
Read the following files from the current working directory (/Users/jaddimachkieh/Documents/mdic_test/data):

| File | Purpose |
|---|---|
| `monthly_financials.csv` | KPI cards, CAPEX, profit/loss, network availability, inflows/outflows |
| `subscribers_monthly.csv` | Subscriber KPI cards |
| `nov_revenue_breakdown_pie.csv` | Revenue donut chart |

**`monthly_financials.csv` key columns:**
`Month`, `Revenue_Actual_M`, `OPEX_Actual_M`, `Gross_Margin_Pct`, `OPEX_Intensity_Pct`, `EBITDA_Pct`, `CAPEX_M`, `CAPEX_Intensity_Pct`, `Profit_Loss_M`, `Network_Availability_Pct`, `Inflows`, `Outflows`

**`subscribers_monthly.csv` key columns:**
`Month`, `Total_Subs_000`, `Gross_Adds_000`, `Active_Data_Subs_000`, `Mobile_RGS_000`, `Active_Cash_Subs_000`, `Cash_RGS_000`, `Fixed_RGS_000`, `Churn_Pct`

**`nov_revenue_breakdown_pie.csv` key columns:**
`Category`, `Pct`, `Amount_M`

Default to the **latest month** in the data.

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
2. Call `readFile()` for all 3 CSVs in parallel using `Promise.all`
3. Parse and aggregate the CSV data in-browser using JS
4. Render all cards, charts, and KPIs from the parsed data — default to latest month
5. Show an error box if any file fails to load

Register `mcp_tools: ["mcp__filesystem__read_file"]` on the artifact.

---

## Color Scheme
```
page bg:        #1a1a28
card bg:        #2d2d3f
section header: #5c1028   (dark maroon strip)
active tab:     #8b1535
accent red:     #e8335a   (charts, highlights)
over-budget:    #ff3b5c   (CAPEX and CAPEX Intensity cards)
budget gray:    #8888aa
text:           #ffffff
subtext:        #ccccdd
border:         #3a3a55
```

---

## Layout

### TOP BAR
- Left: bold white "Monthly KPIs" + small white rectangle accent
- Center: pill-button month tabs — one per month found in the CSV, latest month active
- Right: year selector driven by years present in the data

### ROW 1 — Three panels side by side

**LEFT — "Financials (GHS)"** (dark maroon header strip)
2×4 grid of KPI cards — populated from selected month's CSV row:
```
| Revenue_Actual_M     | OPEX_Actual_M        |
| Gross_Margin_Pct %   | OPEX_Intensity_Pct % |
| EBITDA_Pct %         | CAPEX_M          🔴  |
| Profit_Loss_M        | CAPEX_Intensity_Pct 🔴|
```
🔴 = render in #ff3b5c (CAPEX values are over-budget, always highlight red)

**CENTER** — two stacked cards
- Top: label "FTEs", value shown as "(BL..." in gray/blurred style
- Bottom: label "Network Availability", value from `Network_Availability_Pct`

**RIGHT — "Subscribers (000)"** (dark maroon header strip)
2×4 grid — populated from selected month's subscribers CSV row:
```
| Total_Subs_000       | Gross_Adds_000       |
| Active_Data_Subs_000 | Mobile_RGS_000       |
| Active_Cash_Subs_000 | Cash_RGS_000         |
| Fixed_RGS_000        | Churn_Pct %          |
```

### ROW 2 — Two panels side by side

**LEFT — "Revenue Breakdown"** — Donut chart (cutout 70%)
Data from `nov_revenue_breakdown_pie.csv`. Assign colors:
```
Data    → #e8335a
Voice   → #2a1a20
MFS     → #4a4a5a
Others  → #5c1028
```
Legend: Category, Pct%, Amount_M on the left of the donut.

**RIGHT — "Inflows / Outflows"**
Values from `Inflows` and `Outflows` columns of the selected month:
- INFLOWS: formatted integer with commas, white
- OUTFLOWS: formatted integer with commas, red (#e8335a)

---

## Rules
- All `_M` values display with "M" suffix: "251.04M"
- Inflows/Outflows display as full integers with comma formatting
- CAPEX_M and CAPEX_Intensity_Pct always render in **#ff3b5c**
- Month tabs are driven by actual months in the CSV — not hardcoded
- Clicking a tab reloads all cards with that month's data
- If a CSV file is missing, show a clear inline error message
- System sans-serif font only
