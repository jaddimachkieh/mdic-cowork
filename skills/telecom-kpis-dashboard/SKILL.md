---
name: telecom-kpis-dashboard
description: Use when asked to create, generate, or build the Monthly KPIs telecom dashboard artifact. Generates a self-contained HTML artifact with financials cards, subscriber KPIs, revenue breakdown donut chart, and inflows/outflows for November 2025.
---

# Telecom — Monthly KPIs Dashboard

## Trigger
User asks to generate the Monthly KPIs dashboard, the KPI page, or Dashboard 1.

## Output
Generate a **single self-contained HTML artifact** using Chart.js loaded from CDN.
All data is embedded — no external files needed.

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
- Center: pill-button month tabs Jan–Dec, **November active** (dark crimson background)
- Right: "2024 / ● 2025" radio selector (2025 selected with red dot)

### ROW 1 — Three panels side by side

**LEFT — "Financials (GHS)"** (dark maroon header strip)
2×4 grid of KPI cards:
```
| Revenue        | OPEX           |
| 251.04M        | 136.21M        |
|----------------|----------------|
| Gross Margin   | OPEX Intensity |
| 75%            | 54%            |
|----------------|----------------|
| EBITDA %       | CAPEX          |
| 21%            | 242.12M  🔴    |
|----------------|----------------|
| Profit/(Loss)  | CAPEX Intensity|
| -97.52M        | 21%      🔴    |
```
🔴 = render in #ff3b5c (over-budget red)

**CENTER** — two stacked cards
- Top card: label "FTEs", value "(BL..." in gray/blurred style
- Bottom card: label "Network Availability", large value **99.01%**

**RIGHT — "Subscribers (000)"** (dark maroon header strip)
2×4 grid of KPI cards:
```
| Total Subs     | Gross Adds     |
| 8,503          | 284            |
|----------------|----------------|
| Active Data    | Mobile RGS     |
| 2,732          | 4,145          |
|----------------|----------------|
| Active Cash    | Cash RGS       |
| 2,950          | 2,489          |
|----------------|----------------|
| Fixed RGS      | Churn          |
| 63.9           | -1.50%         |
```

### ROW 2 — Two panels side by side

**LEFT — "Revenue Breakdown"** — Donut chart (cutout 70%)
```
Segment    %      Color
Data      64.2%   #e8335a  (bright crimson)
Voice     13.0%   #2a1a20  (very dark)
MFS       11.6%   #4a4a5a  (dark gray)
Others    11.1%   #5c1028  (dark maroon)
```
Legend labels (Category % Amount) on the left side of the donut.

**RIGHT — "Inflows / Outflows"**
Two values displayed with diverging visual (arrow or filled triangle):
- INFLOWS:  **266,815,869** (white, pointing right/up)
- OUTFLOWS: **-314,450,676** (red #e8335a, pointing down)
Label each value at its base.

---

## Embedded Data (paste as JS constants in the artifact)

```js
const NOV = {
  revenue_m:           251.04,
  opex_m:              136.21,
  gross_margin_pct:    75,
  opex_intensity_pct:  54,
  ebitda_pct:          21,
  capex_m:             242.12,   // render red — over budget
  capex_intensity_pct: 21,       // render red — over budget
  profit_loss_m:       -97.52,
  network_avail_pct:   99.01,
  inflows:             266815869,
  outflows:           -314450676,
};

const SUBS = {
  total_subs:       8503,
  gross_adds:       284,
  active_data_subs: 2732,
  mobile_rgs:       4145,
  active_cash_subs: 2950,
  cash_rgs:         2489,
  fixed_rgs:        63.9,
  churn_pct:       -1.50,
};

const PIE = [
  { label: 'Data',   pct: 64.2, amt_m: 161.17, color: '#e8335a' },
  { label: 'Voice',  pct: 13.0, amt_m:  32.64, color: '#2a1a20' },
  { label: 'MFS',    pct: 11.6, amt_m:  29.12, color: '#4a4a5a' },
  { label: 'Others', pct: 11.1, amt_m:  27.87, color: '#5c1028' },
];
```

---

## Rules
- Monetary values use "M" suffix: "251.04M"
- Inflows/Outflows display with comma formatting: 266,815,869
- CAPEX (242.12M) and CAPEX Intensity (21%) must render in **#ff3b5c**
- Month tabs are clickable — non-November tabs show a "No data for this month" placeholder
- System sans-serif font only (no Google Fonts)
- Donut chart hole = 70% cutout
