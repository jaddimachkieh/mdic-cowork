---
name: telecom-financials-dashboard
description: Use when asked to create, generate, or build the Financials telecom dashboard artifact. Generates a self-contained HTML artifact with GHS and USD side-by-side panels showing EBITDA, Revenue, COS, and OPEX sparklines, totals, and category breakdowns for Apr–Nov 2025.
---

# Telecom — Financials Dashboard (GHS & USD)

## Trigger
User asks to generate the Financials dashboard, the GHS/USD page, or Dashboard 3.

## Output
Generate a **single self-contained HTML artifact** using Chart.js loaded from CDN.
Two-column layout: GHS on the left, USD on the right. All data embedded.

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

### 1. EBITDA by Month (sparkline — compact line chart ~100px tall)
```
Apr: 89.35   May: 232.46  Jun: 58.03  Jul: 57.68
Aug: 13.22   Sep: 44.13   Oct: 102.16 Nov: 53.24
```
Show data labels on each point. No Y-axis grid.

### 2. Summary KPI Cards (2 rows of 3)
```
Row 1: Total Revenue 2.01bn  |  Total COS -528M  |  Total OPEX -828.58M
Row 2: EBITDA 650.27M        |  Total Interest -121.26M  |  FOREX 646.59M
```

### 3. Revenue by Month (small line chart)
```
Apr:243.3  May:248.5  Jun:251.0  Jul:251.4
Aug:242.15 Sep:252.6  Oct:266.4  Nov:251.04
```
Label October peak: "266.4M"

### 4. Revenue Breakdown (horizontal bar list, sorted descending by actual)
Render as compact bars with value labels — not a full chart:
```
Data                  1,308.8M ████████████████████
MFS                     223.0M ███
Voice Outgoing          186.8M ██
Content                  77.6M █
Voice Incoming           68.7M █
IoT                      42.5M
SMS                      37.0M
Non-service Revenue      35.4M
Inbound Roaming          22.0M
Outbound Roaming          4.6M
```

### 5. COS by Month (small bar chart, bars downward)
```
Apr:-50.1  May:-39.8   Jun:-54.7   Jul:-62.1
Aug:-68.9  Sep:-118.6  Oct:-71.8   Nov:-61.54
```
Sep spike (-118.6M) should be visually prominent.

### 6. COS Breakdown (horizontal bar list, sorted by absolute value)
```
Interconnect SMS          -2.8M
VBA - Direct cost         -2.8M
Interconnect Roaming      -8.4M
Bandwidth                -14.8M
Content                  -33.3M
Regulatory & Microwave   -41.1M
Bad debt                 -46.8M
Commissions              -72.6M
Interconnect Voice      -108.5M
Terminal Cost           -196.8M
```

### 7. OPEX by Month (small bar chart, mixed pos/neg)
```
Apr:-103.85  May:+23.76  Jun:-138.27  Jul:-131.62
Aug:-160.03  Sep: -89.87 Oct: -92.44  Nov:-136.26
```
May is positive — bar above zero.

### 8. OPEX Breakdown (horizontal bar list, positives first)
```
Other Overheads              +82.2M
Net Foreign Exchange Gain    +59.5M
VBA Operating expenses        -8.1M
Contractors                   -0.8M
Consulting fees               -2.1M
Intercomp charges             -2.1M
Office Accommodation         -17.4M
Outsourced Operations        -32.5M
Travel & Accommodation       -39.8M
Publicity                    -39.8M
Salaries & Related Benefits -146.4M
S16 Lease Costs             -173.7M
Network / IT Maintenance    -507.6M
```

---

## USD Column (same 8-section structure)

### 1. EBITDA by Month (sparkline)
```
Apr:6.13  May:22.17  Jun:5.51  Jul:5.36
Aug:1.12  Sep:3.49   Oct:9.34  Nov:4.69
```

### 2. Summary KPI Cards (2 rows of 3)
```
Row 1: Total Revenue 174.41M  |  Total COS -45M   |  Total OPEX -71.22M
Row 2: EBITDA 57.81M          |  Total Interest -10.35M  |  EBITDA Margin 32.47%
```
Note: USD column shows **EBITDA Margin %** instead of FOREX.

### 3. Revenue by Month
```
Apr:23.7  May:23.0  Jun:23.3  Jul:21.3
Aug:19.07 Sep:24.4  Oct:22.1  Nov:21.57
```

### 4. Revenue Breakdown
```
Data                113.8M
MFS                  19.4M
Voice Outgoing       16.2M
Content               6.8M
Voice Incoming        6.0M
IoT                   3.7M
SMS                   3.2M
Non-service Revenue   3.1M
Inbound Roaming       1.9M
Outbound Roaming      0.4M
```

### 5. COS by Month
```
Apr:-3.4  May:-3.8  Jun:-5.2  Jul:-5.8
Aug:-5.8  Sep:-9.44 Oct:-6.9  Nov:-4.64
```

### 6. COS Breakdown
```
VBA - Direct cost      -0.24M
Interconnect SMS       -0.24M
Interconnect Roaming   -0.72M
Bandwidth              -1.15M
Content                -2.88M
Regulatory & Microwave -3.57M
Bad debt               -3.78M
Commissions            -6.34M
Interconnect Voice     -9.33M
Terminal Cost         -17.12M
```

### 7. OPEX by Month
```
Apr:+2.3   May:+2.0   Jun:-7.1   Jul:-7.1
Aug:-8.5   Sep:-11.4  Oct:-5.9   Nov:-5.7
```

### 8. OPEX Breakdown
```
Other Overheads              +7.2M
Net Foreign Exchange Gain    +5.7M
VBA Operating expenses        0.0M
Contractors                  -0.1M
Consulting fees              -0.2M
Intercomp charges            -0.2M
Office Accommodation         -1.5M
Outsourced Operations        -2.8M
Travel & Accommodation       -3.4M
Publicity                    -3.5M
Salaries & Related Benefits -12.7M
S16 Lease Costs             -15.1M
Network / IT Maintenance    -43.6M
```

---

## Embedded Data (paste as JS constants)

```js
const MONTHS = ['April','May','June','July','August','September','October','November'];
const USD_GHS_RATE = 11.64;

const GHS = {
  ebitda:   [89.35, 232.46, 58.03, 57.68, 13.22, 44.13, 102.16, 53.24],
  revenue:  [243.3, 248.5, 251.0, 251.4, 242.15, 252.6, 266.4, 251.04],
  cos:      [-50.1, -39.8, -54.7, -62.1, -68.9, -118.6, -71.8, -61.54],
  opex:     [-103.85, 23.76, -138.27, -131.62, -160.03, -89.87, -92.44, -136.26],
  totals:   { revenue:'2.01bn', cos:'-528M', opex:'-828.58M',
              ebitda:'650.27M', interest:'-121.26M', forex:'646.59M' },
  rev_breakdown: [
    ['Data',1308.8],['MFS',223.0],['Voice Outgoing',186.8],
    ['Content',77.6],['Voice Incoming',68.7],['IoT',42.5],
    ['SMS',37.0],['Non-service Revenue',35.4],
    ['Inbound Roaming',22.0],['Outbound Roaming',4.6],
  ],
  cos_breakdown: [
    ['Interconnect SMS',-2.8],['VBA - Direct cost',-2.8],
    ['Interconnect Roaming',-8.4],['Bandwidth',-14.8],
    ['Content',-33.3],['Regulatory & Microwave fees',-41.1],
    ['Bad debt',-46.8],['Commissions',-72.6],
    ['Interconnect Voice',-108.5],['Terminal Cost',-196.8],
  ],
  opex_breakdown: [
    ['Other Overheads',82.2],['Net Foreign Exchange Gain',59.5],
    ['VBA Operating expenses',-8.1],['Contractors',-0.8],
    ['Consulting fees',-2.1],['Intercomp charges',-2.1],
    ['Office Accommodation',-17.4],['Outsourced Operations',-32.5],
    ['Travel & Accommodation',-39.8],['Publicity',-39.8],
    ['Salaries & Related Benefits',-146.4],['S16 Lease Costs',-173.7],
    ['Network / IT Maintenance expenses',-507.6],
  ],
};

const USD = {
  ebitda:   [6.13, 22.17, 5.51, 5.36, 1.12, 3.49, 9.34, 4.69],
  revenue:  [23.7, 23.0, 23.3, 21.3, 19.07, 24.4, 22.1, 21.57],
  cos:      [-3.4, -3.8, -5.2, -5.8, -5.8, -9.44, -6.9, -4.64],
  opex:     [2.3, 2.0, -7.1, -7.1, -8.5, -11.4, -5.9, -5.7],
  totals:   { revenue:'174.41M', cos:'-45M', opex:'-71.22M',
              ebitda:'57.81M', interest:'-10.35M', ebitda_margin:'32.47%' },
  rev_breakdown: [
    ['Data',113.8],['MFS',19.4],['Voice Outgoing',16.2],
    ['Content',6.8],['Voice Incoming',6.0],['IoT',3.7],
    ['SMS',3.2],['Non-service Revenue',3.1],
    ['Inbound Roaming',1.9],['Outbound Roaming',0.4],
  ],
  cos_breakdown: [
    ['VBA - Direct cost',-0.24],['Interconnect SMS',-0.24],
    ['Interconnect Roaming',-0.72],['Bandwidth',-1.15],
    ['Content',-2.88],['Regulatory & Microwave fees',-3.57],
    ['Bad debt',-3.78],['Commissions',-6.34],
    ['Interconnect Voice',-9.33],['Terminal Cost',-17.12],
  ],
  opex_breakdown: [
    ['Other Overheads',7.2],['Net Foreign Exchange Gain',5.7],
    ['VBA Operating expenses',0.0],['Contractors',-0.1],
    ['Consulting fees',-0.2],['Intercomp charges',-0.2],
    ['Office Accommodation',-1.5],['Outsourced Operations',-2.8],
    ['Travel & Accommodation',-3.4],['Publicity',-3.5],
    ['Salaries & Related Benefits',-12.7],['S16 Lease Costs',-15.1],
    ['Network / IT Maintenance expenses',-43.6],
  ],
};
```

---

## Rules
- GHS revenue total displays as "2.01bn" (not "2010M")
- USD column shows EBITDA Margin (32.47%) in place of FOREX
- Sparkline charts: compact (~100px height), no Y-axis grid, data labels on points
- Revenue breakdown: horizontal proportional bars (not a Chart.js chart) with value labels
- All chart lines in **#e8335a** (single color — this dashboard shows Actual only)
- May OPEX (both GHS and USD) is positive — bar renders above zero
