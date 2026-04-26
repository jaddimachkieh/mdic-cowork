---
name: telecom-actuals-vs-budget
description: Use when asked to create, generate, or build the Actual vs Budget telecom dashboard artifact. Generates a self-contained HTML artifact with EBITDA, Revenue, COS, and OPEX monthly charts plus YTD totals and category breakdowns for Apr–Nov 2025.
---

# Telecom — Actual vs Budget Dashboard (GHS)

## Trigger
User asks to generate the Actual vs Budget dashboard, the budget comparison page, or Dashboard 2.

## Output
Generate a **single self-contained HTML artifact** using Chart.js loaded from CDN.
All data is embedded — no external files needed. Period: April–November 2025.

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
- Center: white badge — "USD/GHS Rate (Avg.)  **11.64**"
- Right: "Month" dropdown showing "All"

### ROW 1 — Four YTD total cards

Each card has two pill rows (Actual = crimson bg, Budget = dark gray bg):

| Total Revenue | Total COS | Total OPEX | EBITDA |
|---|---|---|---|
| **Actual** 2,006,392,025 | **Actual** -527,540,814 | **Actual** -828,578,132 | **Actual** 650,273,172 |
| Budget 1,907,348,059 | Budget -515,665,774 | Budget -1,092,172,219 | Budget 299,510,066 |

### ROW 2 — Two charts side by side

**LEFT — EBITDA by Month** (line chart, two lines)
```
Month:   Apr    May     Jun    Jul    Aug    Sep    Oct    Nov
Actual:  89.35  232.46  58.03  57.68  13.22  44.13  102.16  53.24
Budget:  29.22   41.25  38.74  38.25  39.31  31.63   41.63  39.47
```
Show data labels on both lines. Legend: "● Actual  ● Budget"

**RIGHT — Revenue Breakdown** (grouped horizontal bar, Actual red / Budget gray)
```
Category              Actual_M   Budget_M
Data                  1308.8     1283.15
MFS                    223.0      198.1
Voice Outgoing         186.8      146.2
Content                 77.6       59.6
Voice Incoming          68.7       62.8
IoT                     42.5       38.7
SMS                     37.0       35.7
Non-service Revenue     35.4       59.1
Inbound Roaming         22.0       19.4
Outbound Roaming         4.6        4.6
```

### ROW 3 — Two charts side by side

**LEFT — Revenue by Month** (line chart, two lines)
```
Month:   Apr     May     Jun     Jul     Aug     Sep     Oct     Nov
Actual:  243.3   248.5   251.0   251.4   242.15  252.6   266.4   251.04
Budget:  222.4   234.6   242.3   241.2   237.0   235.9   247.0   247.45
```
Label October actual peak: "266.4M"

**RIGHT — COS Breakdown** (grouped vertical bar, negative values, bars point downward)
```
Category                    Actual_M   Budget_M
VBA - Direct cost             -2.8       -4.1
Interconnect SMS              -2.8       -5.0
Interconnect Roaming          -8.4       -6.3
Bandwidth                    -14.8      -26.3
Content                      -33.3      -13.2
Regulatory & Microwave fees  -41.1      -29.1
Bad debt                     -46.8      -46.8
Commissions                  -72.6      -42.6
Interconnect Voice           -108.5     -75.1
Terminal Cost                -196.8    -111.87
```

### ROW 4 — Two charts side by side

**LEFT — COS by Month** (grouped vertical bar, negative values)
```
Month:   Apr-25  May-25  Jun-25  Jul-25  Aug-25   Sep-25  Oct-25  Nov-25
Actual:  -50.1   -39.8   -54.7   -62.1   -68.9   -118.6   -71.8   -61.54
Budget:  -59.58  -63.75  -72.46  -64.85  -28.39   -65.17  -80.74  -80.74
```
Note: September actual (-118.6M) is a notable spike — make it visually prominent.

**RIGHT — OPEX Breakdown** (grouped horizontal bar, mixed pos/neg)
```
Category                           Actual_M   Budget_M
Other Overheads                     +82.2      +59.5
Net Foreign Exchange Gain           +59.5      +19.7
VBA Operating expenses               -8.1        0.0
Contractors                          -0.8       -2.1
Consulting fees                      -2.1       -3.4
Intercomp charges                    -2.1       -3.4
Office Accommodation                -17.4      -17.5
Outsourced Operations               -32.5      -35.0
Travel & Accommodation              -39.8      -40.0
Publicity                           -39.8      -42.0
Salaries & Related Benefits        -146.4     -152.0
S16 Lease Costs                    -173.7     -180.0
Network / IT Maintenance expenses  -507.6     -696.47
```

### ROW 5 — OPEX by Month (full-width grouped bar chart)
```
Month:   Apr      May     Jun      Jul      Aug      Sep      Oct      Nov
Actual: -103.85  +23.76  -138.27  -131.62  -160.03  -89.87   -92.44  -136.26
Budget: -133.6   -129.6  -131.1   -138.1   -169.3   -139.1  -124.63  -127.07
```
⚠️ May actual is **positive (+23.76M)** due to FOREX gain — bar renders above zero line. Label it "+23.76M".

---

## Embedded Data (paste as JS constants)

```js
const MONTHS = ['April','May','June','July','August','September','October','November'];
const MONTHS_SHORT = ['Apr-25','May-25','Jun-25','Jul-25','Aug-25','Sep-25','Oct-25','Nov-25'];
const USD_GHS_RATE = 11.64;

const TOTALS = {
  revenue:  { actual: 2006392025, budget: 1907348059 },
  cos:      { actual:  -527540814, budget:  -515665774 },
  opex:     { actual:  -828578132, budget: -1092172219 },
  ebitda:   { actual:   650273172, budget:   299510066 },
};

const EBITDA_M = {
  actual: [89.35, 232.46, 58.03, 57.68, 13.22, 44.13, 102.16, 53.24],
  budget: [29.22,  41.25, 38.74, 38.25, 39.31, 31.63,  41.63, 39.47],
};
const REVENUE_M = {
  actual: [243.3, 248.5, 251.0, 251.4, 242.15, 252.6, 266.4, 251.04],
  budget: [222.4, 234.6, 242.3, 241.2, 237.0,  235.9, 247.0, 247.45],
};
const COS_M = {
  actual: [-50.1, -39.8, -54.7, -62.1, -68.9, -118.6, -71.8, -61.54],
  budget: [-59.58,-63.75,-72.46,-64.85,-28.39, -65.17,-80.74,-80.74],
};
const OPEX_M = {
  actual: [-103.85, 23.76, -138.27, -131.62, -160.03, -89.87, -92.44, -136.26],
  budget: [-133.6, -129.6,  -131.1,  -138.1,  -169.3, -139.1,-124.63,-127.07],
};

const REV_BREAKDOWN = [
  { cat:'Data',                actual:1308.8, budget:1283.15 },
  { cat:'MFS',                 actual: 223.0, budget:  198.1 },
  { cat:'Voice Outgoing',      actual: 186.8, budget:  146.2 },
  { cat:'Content',             actual:  77.6, budget:   59.6 },
  { cat:'Voice Incoming',      actual:  68.7, budget:   62.8 },
  { cat:'IoT',                 actual:  42.5, budget:   38.7 },
  { cat:'SMS',                 actual:  37.0, budget:   35.7 },
  { cat:'Non-service Revenue', actual:  35.4, budget:   59.1 },
  { cat:'Inbound Roaming',     actual:  22.0, budget:   19.4 },
  { cat:'Outbound Roaming',    actual:   4.6, budget:    4.6 },
];

const COS_BREAKDOWN = [
  { cat:'VBA - Direct cost',           actual:  -2.8, budget:  -4.1  },
  { cat:'Interconnect SMS',            actual:  -2.8, budget:  -5.0  },
  { cat:'Interconnect Roaming',        actual:  -8.4, budget:  -6.3  },
  { cat:'Bandwidth',                   actual: -14.8, budget: -26.3  },
  { cat:'Content',                     actual: -33.3, budget: -13.2  },
  { cat:'Regulatory & Microwave fees', actual: -41.1, budget: -29.1  },
  { cat:'Bad debt',                    actual: -46.8, budget: -46.8  },
  { cat:'Commissions',                 actual: -72.6, budget: -42.6  },
  { cat:'Interconnect Voice',          actual:-108.5, budget: -75.1  },
  { cat:'Terminal Cost',               actual:-196.8, budget:-111.87 },
];

const OPEX_BREAKDOWN = [
  { cat:'Other Overheads',                   actual:  82.2, budget:   59.5  },
  { cat:'Net Foreign Exchange Gain',         actual:  59.5, budget:   19.7  },
  { cat:'VBA Operating expenses',            actual:  -8.1, budget:    0.0  },
  { cat:'Contractors',                       actual:  -0.8, budget:   -2.1  },
  { cat:'Consulting fees',                   actual:  -2.1, budget:   -3.4  },
  { cat:'Intercomp charges',                 actual:  -2.1, budget:   -3.4  },
  { cat:'Office Accommodation',              actual: -17.4, budget:  -17.5  },
  { cat:'Outsourced Operations',             actual: -32.5, budget:  -35.0  },
  { cat:'Travel & Accommodation',            actual: -39.8, budget:  -40.0  },
  { cat:'Publicity',                         actual: -39.8, budget:  -42.0  },
  { cat:'Salaries & Related Benefits',       actual:-146.4, budget: -152.0  },
  { cat:'S16 Lease Costs',                   actual:-173.7, budget: -180.0  },
  { cat:'Network / IT Maintenance expenses', actual:-507.6, budget: -696.47 },
];
```

---

## Rules
- Actual always in **#e8335a**, Budget always in **#8888aa**
- YTD totals row: format with commas (2,006,392,025)
- Chart values: "M" suffix (89.35M)
- May OPEX actual is positive — bar above zero, label "+23.76M"
- Month dropdown: "All" = YTD totals; selecting a month filters all charts to that month's data
