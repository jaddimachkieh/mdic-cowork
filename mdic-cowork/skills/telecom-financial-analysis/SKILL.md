---
name: telecom-financial-analysis
description: Use when the user asks to analyze, deep dive, or explain the telecom dashboard they just generated. Produces a structured CFO-level financial commentary as a text block — not embedded in the dashboard artifact.
---

# Telecom — Financial Deep Dive Analysis

## Trigger
User says: "analyze this", "give me the deep dive", "what does this tell us?", "explain the results", "financial analysis", or similar after generating a dashboard.

## Step 1 — Identify Which Dashboard Was Just Run
Check the conversation context:
- **Dashboard 1 (Monthly KPIs)** → run KPI Analysis
- **Dashboard 2 (Actuals vs Budget)** → run Variance Analysis
- **Dashboard 3 (Financials GHS & USD)** → run Trend & FX Analysis

If unclear, ask: *"Which dashboard would you like me to analyze — KPIs, Actuals vs Budget, or Financials?"*

## Step 2 — Re-use Already-Loaded Data
The CSV data was already read during the dashboard generation step. Re-use it directly — do not re-read the files unless the data is no longer in context.

## Step 3 — Generate Analysis Block

Output a clean text block using the structure below for the relevant dashboard. Use markdown formatting. Start with a one-line executive summary.

---

## Dashboard 1 — Monthly KPIs Analysis

### Structure
```
## Financial Deep Dive — Monthly KPIs | [Month] [Year]

**Executive Summary**
One sentence: overall performance direction, biggest highlight, biggest risk.

### 1. Financials Scorecard
| KPI | Value | Status |
|-----|-------|--------|
| Revenue | XM | 🟢 / 🟡 / 🔴 |
| OPEX | XM | ... |
| Gross Margin | X% | ... |
| OPEX Intensity | X% | ... |
| EBITDA % | X% | ... |
| CAPEX | XM | 🔴 Over budget |
| CAPEX Intensity | X% | 🔴 Over budget |
| Profit / (Loss) | XM | ... |

### 2. CAPEX Overrun
Flag amount over budget, % overrun, implication on cash.

### 3. Subscriber Health
Commentary on Total Subs, Gross Adds trend, Churn %, Active Data vs Mobile RGS.
Flag if churn is accelerating or gross adds are declining.

### 4. Cash Position
Inflows vs Outflows — net surplus or deficit. Annualized burn rate if in deficit.

### 5. Network
Network availability vs 99.5% industry benchmark.

### 6. Risks & Opportunities
- 🔴 [Top risk]
- 🟡 [Watch item]
- 🟢 [Positive signal]
```

---

## Dashboard 2 — Actuals vs Budget Variance Analysis

### Structure
```
## Financial Deep Dive — Actuals vs Budget | YTD [Year]

**Executive Summary**
One sentence: are we ahead or behind budget overall, and what is driving it.

### 1. P&L Variance Summary
| Line | Actual | Budget | Variance | Var % | Signal |
|------|--------|--------|----------|-------|--------|
| Revenue | | | | | 🟢/🟡/🔴 |
| COS | | | | | |
| OPEX | | | | | |
| EBITDA | | | | | |

Variance = Actual minus Budget. For costs, favorable = under budget = 🟢.

### 2. Revenue Analysis
- Over or under budget by how much and %
- Which categories are driving the gap (top 2-3)
- MoM trend: is the gap widening or narrowing?

### 3. Cost Escalation Flags
- COS: which categories are over budget and by how much
- OPEX: same — flag any category growing faster than revenue
- Combined cost ratio: (COS + OPEX) / Revenue vs budget

### 4. EBITDA Margin
Actual EBITDA % vs Budget EBITDA %. Explain the compression or expansion in plain language.

### 5. Key Ratios
| Ratio | Actual | Budget |
|-------|--------|--------|
| Gross Margin % | | |
| OPEX Intensity % | | |
| EBITDA Margin % | | |
| COS / Revenue % | | |

### 6. Top 3 Variances Explained
Narrative: what happened, why it matters, what to watch.

### 7. Risks & Opportunities
- 🔴 [Critical — needs action]
- 🟡 [Developing — monitor closely]
- 🟢 [Outperforming — protect and amplify]
```

---

## Dashboard 3 — Financials GHS & USD Trend Analysis

### Structure
```
## Financial Deep Dive — Financials GHS & USD | YTD [Year]

**Executive Summary**
One sentence: revenue trajectory, EBITDA margin direction, FX impact summary.

### 1. Revenue Trend (GHS)
- MoM growth rates — highlight peak and trough months
- YTD total vs run-rate annualized projection
- Revenue category concentration (top 2 categories as % of total)

### 2. EBITDA Margin Trajectory
- Monthly EBITDA margin % — is it expanding or compressing?
- Best and worst months — what drove the difference?
- YTD EBITDA vs implied full-year target

### 3. Cost Structure Analysis
| Cost Line | GHS Amount | % of Revenue |
|-----------|------------|--------------|
| COS | | |
| OPEX | | |
| Total Costs | | |

Flag if any cost line is growing faster than revenue MoM.

### 4. FX Impact (GHS vs USD)
- USD/GHS rate used: X.XX
- Revenue in USD vs GHS — implied FX sensitivity
- If GHS weakens by 5%, impact on USD-reported EBITDA

### 5. Interest & FOREX
- Interest burden: -121.26M — annualized cost, coverage ratio
- FOREX gain: 646.59M — is this recurring or one-off? Impact on underlying EBITDA

### 6. Key Ratios
| Ratio | Value |
|-------|-------|
| Gross Margin % | |
| EBITDA Margin % | |
| OPEX Intensity % | |
| Interest / Revenue % | |
| FOREX / EBITDA % | |

### 7. Risks & Opportunities
- 🔴 [Critical]
- 🟡 [Watch]
- 🟢 [Strength]
```

---

## Rules
- Always start with the one-line executive summary — this is the headline
- Use 🟢 (on/ahead of target), 🟡 (within 5% of threshold), 🔴 (over threshold or behind budget)
- For costs: under budget = 🟢, over budget = 🔴
- For revenue/EBITDA: over budget = 🟢, under budget = 🔴
- Format all GHS values with "M" suffix (e.g. 251.04M); use "bn" if ≥ 1000M
- Format USD values with "$" and "M" suffix
- Variances: always show both absolute (M) and percentage (%)
- Keep commentary concise — one paragraph per section max
- End every analysis with the **Risks & Opportunities** section — this is the most actionable part
