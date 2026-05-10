---
name: telecom-benchmarking
description: Use when the user asks to benchmark, compare to competitors, or see how the telecom performance stacks up against industry. Reads dashboard CSVs and a synthetic benchmark CSV to produce a competitive positioning analysis.
---

# Telecom — Competitive Benchmarking

## Trigger
User says: "benchmark this", "how do we compare to competitors", "industry comparison", "competitive positioning", or similar.

## What This Skill Benchmarks

Only metrics that have external industry comparables are included.
Dashboard 2 (Actuals vs Budget) is excluded — budget variances are internal and not externally benchmarkable.

| Metric | Category | Derived From |
|---|---|---|
| EBITDA Margin % | Profitability | monthly_financials.csv |
| Gross Margin % | Profitability | monthly_financials.csv |
| OPEX Intensity % | Efficiency | monthly_financials.csv |
| CAPEX Intensity % | Investment | monthly_financials.csv |
| Network Availability % | Quality | monthly_financials.csv |
| Churn % | Subscribers | subscribers_monthly.csv |
| Active Data Penetration % | Subscribers | subscribers_monthly.csv |
| Data Revenue % | Revenue Mix | nov_revenue_breakdown_pie.csv |
| ARPU (GHS) | Revenue | monthly_financials.csv + subscribers_monthly.csv |
| Revenue Growth MoM Avg % | Growth | monthly_financials.csv |

## Step 1 — Read Files

Read the following files:

**Company data** from `/Users/jaddimachkieh/Documents/mdic_test/data/`:
- `monthly_financials.csv`
- `subscribers_monthly.csv`
- `nov_revenue_breakdown_pie.csv`

**Benchmark data** from `/Users/jaddimachkieh/Documents/Playground/telecom-dashboard-data/output/`:
- `benchmark_telecom.csv`

Use `window.cowork.callMcpTool('mcp__filesystem__read_file', { path })` to read all 4 files.

## Step 2 — Derive Company Metrics

Compute YTD/average values from the company CSVs:

```
EBITDA_Margin_Pct      = sum(EBITDA_Actual_M) / sum(Revenue_Actual_M) * 100
Gross_Margin_Pct       = avg(Gross_Margin_Pct) across all months
OPEX_Intensity_Pct     = avg(OPEX_Intensity_Pct) across all months
CAPEX_Intensity_Pct    = sum(CAPEX_M) / sum(Revenue_Actual_M) * 100
Network_Availability   = avg(Network_Availability_Pct) across all months
Churn_Pct              = avg(abs(Churn_Pct)) across all months
Active_Data_Penetration= avg(Active_Data_Subs_000 / Total_Subs_000) * 100
Data_Revenue_Pct       = Data row Amount_M / sum(all Amount_M) * 100
ARPU_GHS               = avg(Revenue_Actual_M * 1000 / Total_Subs_000) across all months
Revenue_Growth_MoM_Avg = avg of MoM % changes in Revenue_Actual_M
```

## Step 3 — Generate HTML Artifact

Generate a single self-contained HTML artifact using Chart.js (CDN).

Register `mcp_tools: ["mcp__filesystem__read_file"]` on the artifact.

On `DOMContentLoaded`: load all 4 files in parallel via `Promise.all`, compute company metrics, render the dashboard. Show loading spinner and error box if any file fails.

---

## Color Scheme
```
page bg:        #1a1a28
card bg:        #2d2d3f
header strip:   #5c1028
our company:    #e8335a   (crimson red)
competitor a:   #4a90d9   (blue)
industry avg:   #8888aa   (gray)
best in class:  #2ecc71   (green)
text:           #ffffff
border:         #3a3a55
```

---

## Layout

### TOP BAR
- Left: bold white "Competitive Benchmarking"
- Center: badge — "Synthetic Benchmark  |  Ghana Telecom Market"
- Right: "vs" selector — "Competitor A" | "Industry Average" | "Best in Class" (default: Industry Average)

### ROW 1 — Benchmark Score Card
A single prominent card showing an overall **Competitive Score** out of 10.

Score = count of metrics where company beats selected benchmark / total metrics * 10, rounded to 1dp.

Display: large number, color-coded (≥7 green, 5–6.9 amber, <5 red), with label "out of 10 metrics".

### ROW 2 — Metric Comparison Table
Full comparison table — one row per metric:

| Metric | Category | Our Value | Benchmark | Gap | Signal |
|---|---|---|---|---|---|

- Gap = Our Value minus Benchmark (positive = ahead, negative = behind)
- For metrics where Higher_is_Better=FALSE, flip the signal: gap < 0 means we're ahead
- Signal: 🟢 ahead | 🟡 within 5% of benchmark | 🔴 behind

### ROW 3 — Radar Chart
Spider/radar chart with 6 key metrics (one per axis):
`EBITDA Margin`, `Gross Margin`, `OPEX Intensity` (inverted), `Network Availability`, `Churn` (inverted), `Data Revenue %`

Plot two lines: **Our Company** (crimson) vs selected **Benchmark** (blue/gray/green).
Normalise each axis to 0–100 based on range [0, Best_in_Class value].

### ROW 4 — Category Deep Dives (4 cards side by side)

**Profitability** — EBITDA Margin % + Gross Margin %
**Efficiency** — OPEX Intensity % + CAPEX Intensity %
**Subscribers** — Churn % + Active Data Penetration %
**Growth & Mix** — Revenue Growth MoM % + Data Revenue % + ARPU

Each card shows a small grouped bar (Our Company vs Benchmark) per metric in that category.
Below each bar: benchmark note from `benchmark_telecom.csv`.

### ROW 5 — Positioning Summary (text block)
Auto-generated narrative with this structure:

```
**Competitive Positioning Summary**

Strengths (beating benchmark):
- [Metric]: Our X% vs benchmark Y% — [one-line interpretation]

Gaps (behind benchmark):
- [Metric]: Our X% vs benchmark Y% — [one-line interpretation]

Overall: [2-3 sentences on competitive position, biggest opportunity, biggest risk]
```

---

## Rules
- Default benchmark comparison: **Industry Average**
- Switching the "vs" selector rerenders all charts and the summary — no page reload
- For cost metrics (OPEX Intensity, CAPEX Intensity, Churn): lower is better — flip gap color accordingly
- Never show absolute GHS/subscriber count comparisons — benchmark CSV only contains ratios and rates
- CAPEX Intensity caveat: always add footnote — "CAPEX intensity varies by network build phase — interpret in context"
- If `benchmark_telecom.csv` is missing, show inline error: "Benchmark file not found at /Users/jaddimachkieh/Documents/Playground/telecom-dashboard-data/output/benchmark_telecom.csv"
