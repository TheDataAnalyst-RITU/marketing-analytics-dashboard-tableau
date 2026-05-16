# 📊 Cross-Channel Marketing Analytics Dashboard

> An end-to-end marketing analytics project built in **Tableau Public** — from synthetic data generation in Python to a fully interactive executive dashboard with multi-level drill-downs, period-over-period deltas, and analyst-grade insights.

---

## 🔗 Quick Links

- 🎨 **Live Dashboard:** [Marketing Insights — Executive Summary](https://public.tableau.com/app/profile/ritu.chaudhary5589/viz/MarketingInsights-ExecutiveSummary/ExecutiveSummary)
- 🎥 **Video Walkthrough (~1.5 min):** [YouTube](https://youtu.be/uVjC4VnLRE4)
- 💼 **LinkedIn:** [linkedin.com/in/ritu-c-27b333202](https://www.linkedin.com/in/ritu-c-27b333202/)

---

## 🎯 Business Problem

Marketing teams running paid media across **Paid Search, Paid Social, Programmatic, and Organic** struggle with a recurring question: 
> *"a number moved — what's going on, and what do we do?"*
Most dashboards show aggregate metrics, but real budget decisions get made at the **data source level** (Facebook vs. LinkedIn, DV360 vs. SA360) and the **campaign level**. Without a drill-capable view, analysts spend hours stitching together reports from each platform.
**This dashboard solves that** with a single view that answers three questions in under 30 seconds:

1. **Are we on track?** — KPI strip with deltas + sparklines
2. **Is it a trend or a blip?** — Time-series chart by channel
3. **Which channel → platform → campaign do I act on?** — Multi-level performance tables

---

## 🚀 Project Overview

This is an **end-to-end analytics project** covering the full lifecycle:

| Phase | What I Did |
|---|---|
| 🧠 **Problem framing** | Identified the gap between aggregate channel reporting and platform/campaign-level decision-making |
| 🐍 **Data generation** | Built a synthetic 2023 marketing dataset in **Python** (numpy, pandas) with realistic seasonality, fatigue patterns, and platform mix shifts |
| 🏗️ **Data modeling** | Designed a **single fact table** at `date × ad_set` grain with three dimension tables (Channel, Data Source, Campaign) |
| 📈 **Dashboard build** | Built **19 worksheets** in Tableau combined into one executive dashboard with cross-filtering |
| 🧪 **Calculated fields** | Created **~30 calculated fields** including LOD-style aggregations, period-over-period deltas, conditional formatting logic, and rate metrics |
| 🎨 **UX design** | Designed a tiered layout (KPI strip → time series → drill tables) matching real analyst workflow |
| 📝 **Insight generation** | Surfaced **3 actionable insights** with clear next-step recommendations |

---

## 📦 Data Source

The dataset is **fully synthetic**, generated in Python with a fixed seed for reproducibility. It mirrors how real cross-channel paid media data looks at a media agency or in-house marketing team.

**Scale:**
- 🔢 **~11,680 rows** at the lowest practical grain (one row per `date × ad_set`)
- 📅 **Full-year 2023** daily data
- 📡 **4 channels** — Paid Search, Paid Social, Programmatic, Organic
- 🌐 **8 data sources** — StackAdapt, Amazon Ad Server, Google DV360, Google Search Ads 360, Facebook, LinkedIn, Bing Ads, and more
- 🎯 **16 campaigns** across the channel mix
- 📍 **32 ad sets** (campaign-level child rows)

**Schema design choice:**
The fact table stores **only raw counts** — impressions, clicks, spend, conversions, video views. Every **rate metric** (CTR, CPC, CPM, Conversion Rate) is computed inside Tableau using `SUM(numerator) / SUM(denominator)`. This is the **correct way** to model rate metrics for any BI tool, because storing pre-computed rates and averaging them at a rollup level produces mathematically wrong numbers at the channel or account level. This is a subtle but important design choice that comes up in real BI work.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| 🎨 **Tableau Public** | Dashboard build, calculated fields, interactive filtering |
| 🐍 **Python (numpy, pandas)** | Synthetic data generation with seasonality and growth multipliers |
| 📄 **CSV** | Fact and dimension table storage |
| 🔁 **Reproducible seed** | Fixed random seed (`seed=42`) ensures identical data on every regeneration |

---

## 🧪 Advanced Tableau Techniques Used

This project was an opportunity to go deeper than standard Tableau builds. Specific techniques applied:

### 1. ⚙️ Calculated Fields (~30 total)
- **Period-over-period delta fields** — `IF [Date] is in Q4 2023 THEN [Metric]` paired with a Q3 version, then delta = SUM(Current) - SUM(Prior). Built for all 8 KPI metrics.
- **Rate metric calculations** — CTR, CPC, CPM, Conversion Rate all computed as ratios of summed numerators and denominators (not averages of pre-computed rates).
- **Conditional formatting calc fields** — Separate `{Metric} Delta Positive` and `{Metric} Delta Negative` fields that return NULL when the sign doesn't match, enabling green/red text formatting on the same row.

### 2. 🔼🔽 Conditional Arrow Indicators
Each KPI card shows a ▲ green or ▼ red arrow based on whether the delta is positive or negative — built using `IF [Delta] > 0 THEN "▲" ELSE "▼" END` with color formatting tied to the delta value.

### 3. 📉 Sparklines Inside KPI Cells
Daily-rate calculated fields (`CTR Daily`, `CPM Daily`, etc.) feed sparkline worksheets that sit underneath each KPI's big number. All axes, gridlines, and titles hidden to create a minimal trend indicator.

### 4. 🗂️ Performance Tables with Measure Names / Measure Values
The three drill tables (Channel, Data Source, Campaign) use **Measure Names on Columns** + **Measure Values on Marks** to display Impressions, Impressions Delta %, CTR, and CTR Delta % in a single compact table per dimension.

### 5. 🎯 Top-N Filtering with Worksheet-Scoped Filters
The Campaign Performance table is filtered to the **Top 10 by Impressions Sum**, with the filter scoped to *only this worksheet* — preventing the Top-N from incorrectly excluding low-volume channels (like Organic) on other sheets.

### 6. 🔗 Cross-Filtering Across Worksheets
All four filters (Channel, Data Source, Campaign, Date Range) apply to **all worksheets using this data source**, so a single click cascades through the entire dashboard.

### 7. 🎨 Custom Number Formatting
Spend formatted in **Billions** ("$1.11B") to fit inside KPI cards. Rate metrics formatted as percentages with 2 decimals via **Default Properties → Number Format**.

### 8. 🏗️ Dashboard Composition with Tiled Layout
19 worksheets composed into one executive dashboard via tiled layout (containers proved finicky for the KPI grid). Each KPI cell pairs a big-number worksheet with its corresponding sparkline below.

---

## 💡 Insights Surfaced

The synthetic data was designed with three real-world patterns baked in — patterns I've observed in actual marketing accounts. The dashboard surfaces all three.

### 1. 🔄 Channel Rotation Across the Year
No single channel dominates every quarter. **Paid Search leads Q1** (tax season retail), **Programmatic owns mid-year** (brand pushes and product launches), **Paid Social closes Q4 strong** (holiday window). This challenges the common practice of locking annual budget splits — dynamic reallocation captures more value.

### 2. 📉 The Efficiency Paradox on Workhorse Channels
**Programmatic CTR -4.4%** and **Paid Search CTR -3.8%** vs prior period, *while spend holds steady or grows*. This is **the single most common pattern in mature paid media accounts** — the channels we rely on most quietly degrade as creative fatigues and audiences saturate. The dashboard surfaces this through period-over-period CTR deltas. **Recommendation:** creative refresh on anything older than 90 days before considering budget cuts.

### 3. 🔀 Platform Mix Shifts Hidden Inside Channel Totals
The Channel-level chart shows Programmatic and Paid Social as relatively stable. But the **Data Source table reveals the truth**: Google DV360 -16%, Google Search Ads 360 -11%, while **Facebook +45% and LinkedIn +32%** absorb that volume. This is invisible without a separate platform-level view. **This is exactly why the Data Source table exists in the dashboard.**

---

## 🧠 What I Learned

This project taught me — or reinforced — several lessons that apply directly to real BI/analytics work:

- 🧮 **Store raw counts, compute rates in the BI tool.** Aggregation correctness matters more than dashboard prettiness. A wrong number is worse than no number.
- 🏗️ **Single fact table at lowest grain > multiple pre-aggregated tables.** Aggregations live in the BI layer; the warehouse stays clean and flexible.
- 🎯 **One question per widget.** If a single chart is trying to answer two questions, it answers neither well. Dashboard density should mirror analyst workflow, not show off the data.
- 📐 **Layout follows the investigation path.** Filters and tables should flow left-to-right / top-to-bottom in the same order an analyst would actually narrow down a question (Channel → Data Source → Campaign).
- ⚠️ **Top-N filters need careful scoping.** A worksheet-level filter applied globally can silently exclude entire dimensions from other views (this is how I lost Organic from the time series chart at one point).
- 🎨 **Visual signals (color, arrows, sparklines) do the executive's mental math.** A bare number is harder to act on than a colored delta with a trend line. Good dashboards reduce cognitive load.
- 📊 **Synthetic data is a skill.** Designing data with realistic patterns — seasonality, fatigue curves, platform shifts — is itself an analytics exercise. Random data tells no story.

---

## 📷 Dashboard Snapshots

### 📊 Marketing Analytics Dashboard -
![Dashboard Preview](https://github.com/TheDataAnalyst-RITU/Tableau-Project---Marketing-Analytics-Dashboard/blob/main/Snapshot%20of%20Marketing%20Insights%20Dashboard.png)
