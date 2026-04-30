---
name: telecom-financial-analysis
description: Use when the user asks to analyze, deep dive, or explain the telecom dashboard they just generated. Asks the user to select a focus area, then produces a focused CFO-level text commentary for that area only.
---

# Telecom — Financial Deep Dive Analysis

## Trigger
User says: "analyze this", "give me the deep dive", "what does this tell us?", "explain the results", "financial analysis", or similar after generating a dashboard.

## Step 1 — Identify Which Dashboard Was Just Run
Check conversation context:
- **Dashboard 1 (Monthly KPIs)** → offer Menu A
- **Dashboard 2 (Actuals vs Budget)** → offer Menu B
- **Dashboard 3 (Financials GHS & USD)** → offer Menu C

If unclear, ask: *"Which dashboard would you like me to analyze — KPIs, Actuals vs Budget, or Financials?"*

## Step 2 — Present the Deep Dive Menu

Only offer options that are supported by the data available for that dashboard. Do not offer options where data is insufficient.

### Menu A — Dashboard 1 (Monthly KPIs)
```
What would you like to deep dive into?

1. 📊 Financials Scorecard — Revenue, OPEX, Gross Margin, EBITDA, CAPEX, Profit/Loss
2. 🔴 CAPEX Overrun — Overrun amount, % over budget, cash impact
3. 📱 Subscriber Health — Total subs, gross adds, churn, active data, RGS
4. 💰 Cash Position — Inflows vs outflows, net surplus or deficit
5. 📡 Network Performance — Availability vs industry benchmark
```

### Menu B — Dashboard 2 (Actuals vs Budget)
```
What would you like to deep dive into?

1. 📋 P&L Variance Summary — Full Actual vs Budget table across Revenue, COS, OPEX, EBITDA
2. 📈 Revenue Analysis — Budget gap, top category drivers, MoM trend
3. ⚠️ Cost Escalation — COS and OPEX categories over budget, cost ratio vs revenue
4. 📉 EBITDA Margin — Margin compression or expansion vs budget, plain-language explanation
5. 🔢 Key Ratios — Gross margin %, OPEX intensity %, EBITDA %, COS/Revenue %
```

### Menu C — Dashboard 3 (Financials GHS & USD)
```
What would you like to deep dive into?

1. 📈 Revenue Trend — MoM growth, peak/trough months, annualized run-rate, category concentration
2. 📉 EBITDA Margin Trajectory — Monthly margin trend, best/worst months, full-year implied target
3. 💸 Cost Structure — COS and OPEX as % of revenue, MoM cost growth vs revenue growth
4. 💱 FX & Currency Impact — GHS vs USD delta, FX sensitivity, impact of GHS weakening
5. 🏦 Interest & FOREX — Interest burden coverage, FOREX gain sustainability, underlying EBITDA
6. 🔢 Key Ratios — Gross margin %, EBITDA %, OPEX intensity %, Interest/Revenue %, FOREX/EBITDA %
```

## Step 3 — Deliver the Selected Deep Dive

Wait for the user's selection, then output only that section as a focused analysis block. Use markdown. Always lead with a one-line executive summary.

---

## Deep Dive Templates

### A1 — Financials Scorecard
```
## Financials Scorecard | [Month] [Year]

**Executive Summary:** [One sentence on overall financial health.]

| KPI | Value | Status |
|-----|-------|--------|
| Revenue | XM | 🟢/🟡/🔴 |
| OPEX | XM | 🟢/🟡/🔴 |
| Gross Margin | X% | 🟢/🟡/🔴 |
| OPEX Intensity | X% | 🟢/🟡/🔴 |
| EBITDA % | X% | 🟢/🟡/🔴 |
| CAPEX | XM | 🔴 Over budget |
| CAPEX Intensity | X% | 🔴 Over budget |
| Profit / (Loss) | XM | 🟢/🟡/🔴 |

[2-3 sentences on what the scorecard tells us and what to act on.]

**Risks & Opportunities**
- 🔴 [Top risk]
- 🟡 [Watch item]
- 🟢 [Positive signal]
```

### A2 — CAPEX Overrun
```
## CAPEX Overrun | [Month] [Year]

**Executive Summary:** [One sentence on severity of overrun.]

- Actual CAPEX: XM | Budget: XM | Overrun: XM (X% over)
- CAPEX Intensity: X% vs budget X%
- Cash impact: [what this means for net cash position]
- Annualized CAPEX run-rate: XM — implied full-year overrun: XM

[2-3 sentences on root cause if inferable, and recommended action.]

**Risks & Opportunities**
- 🔴 [Risk]
- 🟡 [Watch]
- 🟢 [Opportunity if any]
```

### A3 — Subscriber Health
```
## Subscriber Health | [Month] [Year]

**Executive Summary:** [One sentence on subscriber base direction.]

| KPI | Value | Signal |
|-----|-------|--------|
| Total Subscribers | X,XXX | 🟢/🟡/🔴 |
| Gross Adds | XXX | 🟢/🟡/🔴 |
| Churn % | X.X% | 🟢/🟡/🔴 |
| Active Data Subs | X,XXX | 🟢/🟡/🔴 |
| Mobile RGS | X,XXX | 🟢/🟡/🔴 |
| Active Cash Subs | X,XXX | 🟢/🟡/🔴 |
| Fixed RGS | XX.X | 🟢/🟡/🔴 |

[2-3 sentences: is the base growing or eroding, data penetration rate, churn concern.]

**Risks & Opportunities**
- 🔴 [Risk]
- 🟡 [Watch]
- 🟢 [Strength]
```

### A4 — Cash Position
```
## Cash Position | [Month] [Year]

**Executive Summary:** [One sentence: surplus or deficit and magnitude.]

- Inflows: X,XXX,XXX
- Outflows: X,XXX,XXX
- Net position: X,XXX,XXX (surplus / deficit)
- Annualized burn rate (if deficit): XM/year

[2-3 sentences on sustainability and what is driving the gap.]

**Risks & Opportunities**
- 🔴 [Risk]
- 🟡 [Watch]
- 🟢 [Positive]
```

### A5 — Network Performance
```
## Network Performance | [Month] [Year]

**Executive Summary:** [One sentence on availability vs benchmark.]

- Network Availability: X.XX% | Industry benchmark: 99.5%
- Status: 🟢 Above / 🟡 At / 🔴 Below benchmark
- Implied downtime this month: X hours X minutes

[2-3 sentences on what the availability level means operationally and commercially.]

**Risks & Opportunities**
- 🔴 [Risk if below benchmark]
- 🟢 [Strength if above]
```

### B1 — P&L Variance Summary
```
## P&L Variance — Actuals vs Budget | YTD [Year]

**Executive Summary:** [One sentence on overall budget performance.]

| Line | Actual | Budget | Variance | Var % | Signal |
|------|--------|--------|----------|-------|--------|
| Revenue | | | | | 🟢/🟡/🔴 |
| COS | | | | | 🟢/🟡/🔴 |
| OPEX | | | | | 🟢/🟡/🔴 |
| EBITDA | | | | | 🟢/🟡/🔴 |

Note: For costs, under budget = favorable = 🟢. For revenue/EBITDA, over budget = 🟢.

[3-4 sentences explaining the overall picture and biggest single driver.]

**Risks & Opportunities**
- 🔴 [Critical]
- 🟡 [Monitor]
- 🟢 [Outperforming]
```

### B2 — Revenue Analysis
```
## Revenue Analysis — Actuals vs Budget | YTD [Year]

**Executive Summary:** [One sentence on revenue vs budget.]

- YTD Revenue Actual: XM | Budget: XM | Gap: XM (X%)
- Top category over budget: [Category] +XM
- Top category under budget: [Category] -XM
- MoM trend: gap [widening / narrowing] — last 3 months: X, X, X

[2-3 sentences on what is driving the gap and outlook.]

**Risks & Opportunities**
- 🔴 [Risk]
- 🟡 [Watch]
- 🟢 [Opportunity]
```

### B3 — Cost Escalation
```
## Cost Escalation | YTD [Year]

**Executive Summary:** [One sentence on cost discipline vs budget.]

**COS Over Budget**
| Category | Actual | Budget | Overrun | Overrun % |
|----------|--------|--------|---------|-----------|
| [Category] | | | | |

**OPEX Over Budget**
| Category | Actual | Budget | Overrun | Overrun % |
|----------|--------|--------|---------|-----------|
| [Category] | | | | |

- Combined cost ratio (COS+OPEX)/Revenue: Actual X% vs Budget X%
- Revenue growth vs cost growth: Revenue +X%, Total Costs +X% — [costs growing faster/slower than revenue]

**Risks & Opportunities**
- 🔴 [Critical cost item]
- 🟡 [Developing pressure]
- 🟢 [Cost line under control]
```

### B4 — EBITDA Margin
```
## EBITDA Margin Analysis | YTD [Year]

**Executive Summary:** [One sentence on margin vs budget.]

- EBITDA Actual: XM (X%) | Budget: XM (X%) | Variance: XM (X pp)
- Margin is [compressing / expanding] by X percentage points vs budget
- Primary driver of compression/expansion: [revenue shortfall / cost overrun / mix]

[3-4 sentences explaining what is behind the margin movement in plain language.]

**Risks & Opportunities**
- 🔴 [Risk to margin]
- 🟡 [Watch]
- 🟢 [Margin strength]
```

### B5 — Key Ratios (Dashboard 2)
```
## Key Ratios — Actuals vs Budget | YTD [Year]

**Executive Summary:** [One sentence on ratio health overall.]

| Ratio | Actual | Budget | Signal |
|-------|--------|--------|--------|
| Gross Margin % | | | 🟢/🟡/🔴 |
| OPEX Intensity % | | | 🟢/🟡/🔴 |
| EBITDA Margin % | | | 🟢/🟡/🔴 |
| COS / Revenue % | | | 🟢/🟡/🔴 |

[2-3 sentences on which ratios are most off track and why it matters.]

**Risks & Opportunities**
- 🔴 [Ratio most off track]
- 🟡 [Watch]
- 🟢 [Ratio performing well]
```

### C1 — Revenue Trend
```
## Revenue Trend (GHS) | YTD [Year]

**Executive Summary:** [One sentence on revenue trajectory.]

| Month | Revenue (M) | MoM Growth % |
|-------|-------------|--------------|
| [Month] | | |

- Peak month: [Month] at XM | Trough month: [Month] at XM
- YTD total: XM | Annualized run-rate: XM
- Top 2 categories: [Cat1] X% of revenue, [Cat2] X% of revenue

[2-3 sentences on trend direction and concentration risk.]

**Risks & Opportunities**
- 🔴 [Risk]
- 🟡 [Watch]
- 🟢 [Strength]
```

### C2 — EBITDA Margin Trajectory
```
## EBITDA Margin Trajectory | YTD [Year]

**Executive Summary:** [One sentence on margin direction.]

| Month | EBITDA (M) | Margin % |
|-------|------------|----------|
| [Month] | | |

- Best month: [Month] at X% | Worst month: [Month] at X%
- Trend: [expanding / compressing / stable]
- Implied full-year EBITDA at current run-rate: XM

[2-3 sentences on what is driving margin movement month to month.]

**Risks & Opportunities**
- 🔴 [Risk]
- 🟡 [Watch]
- 🟢 [Strength]
```

### C3 — Cost Structure
```
## Cost Structure Analysis | YTD [Year]

**Executive Summary:** [One sentence on cost efficiency.]

| Cost Line | GHS Amount | % of Revenue |
|-----------|------------|--------------|
| COS | XM | X% |
| OPEX | XM | X% |
| Total Costs | XM | X% |

- Revenue MoM growth (avg): X% | Total cost MoM growth (avg): X%
- Costs growing [faster / slower] than revenue — margin is [under pressure / improving]
- Highest cost category: [Category] at XM

[2-3 sentences on cost structure health and biggest risk line.]

**Risks & Opportunities**
- 🔴 [Cost risk]
- 🟡 [Watch]
- 🟢 [Efficiency strength]
```

### C4 — FX & Currency Impact
```
## FX & Currency Impact | YTD [Year]

**Executive Summary:** [One sentence on FX exposure and direction.]

- USD/GHS rate: X.XX
- YTD Revenue in GHS: XM | In USD: $XM
- FX sensitivity: 5% GHS depreciation → USD Revenue impact: -$XM, USD EBITDA impact: -$XM
- GHS has [strengthened / weakened] vs USD — impact on USD-reported results: [positive / negative]

[2-3 sentences on FX risk management implication.]

**Risks & Opportunities**
- 🔴 [FX risk]
- 🟡 [Watch]
- 🟢 [FX tailwind if applicable]
```

### C5 — Interest & FOREX
```
## Interest & FOREX Analysis | YTD [Year]

**Executive Summary:** [One sentence on below-the-line items impact.]

- Interest expense: -121.26M | Annualized: -XM | Interest/Revenue: X%
- FOREX gain: +646.59M | FOREX/EBITDA: X% — this is [recurring / likely one-off]
- Underlying EBITDA (ex-FOREX): XM vs reported EBITDA XM — difference: XM

[3-4 sentences: interest coverage, whether FOREX gain is structural or exceptional, quality of EBITDA.]

**Risks & Opportunities**
- 🔴 [Risk — e.g. FOREX gain not recurring]
- 🟡 [Watch]
- 🟢 [Strength]
```

### C6 — Key Ratios (Dashboard 3)
```
## Key Ratios | YTD [Year]

**Executive Summary:** [One sentence on overall ratio health.]

| Ratio | Value | Signal |
|-------|-------|--------|
| Gross Margin % | | 🟢/🟡/🔴 |
| EBITDA Margin % | | 🟢/🟡/🔴 |
| OPEX Intensity % | | 🟢/🟡/🔴 |
| Interest / Revenue % | | 🟢/🟡/🔴 |
| FOREX / EBITDA % | | 🟢/🟡/🔴 |

[2-3 sentences on which ratios stand out and what they signal.]

**Risks & Opportunities**
- 🔴 [Ratio most concerning]
- 🟡 [Watch]
- 🟢 [Strongest ratio]
```

---

## Rules
- **Always present the menu first** — never output analysis before the user selects a focus area
- **Only show menu options available for the active dashboard** — never offer FX analysis for Dashboard 1, never offer subscriber health for Dashboard 2 or 3
- After selection, output **only that section** — do not add unrequested sections
- After delivering the analysis, ask: *"Would you like to explore another area?"* — list remaining options
- Traffic lights: 🟢 on/ahead of target | 🟡 within 5% of threshold | 🔴 over threshold or behind budget
- For costs: under budget = 🟢 | over budget = 🔴
- For revenue/EBITDA: over budget = 🟢 | under budget = 🔴
- GHS values: "M" suffix (e.g. 251.04M) | use "bn" if ≥ 1000M
- USD values: "$" prefix + "M" suffix
- Variances: always show both absolute (M) and percentage (%)
- End every analysis with **Risks & Opportunities** — always 3 bullets: 🔴 🟡 🟢
