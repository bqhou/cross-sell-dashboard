# Cross-Sell Dashboard — Metric Definitions

## Data Source

**Table:** `cross_sell_events` in `insurance_sample.db`  
**Date range:** 2025-08-31 → 2026-07-01  
**Grain:** One row per (date, agent, channel, policy_type, funnel_stage) combination, with an event count.

**Key columns used:**
| Column | Dashboard field | Description |
|--------|----------------|-------------|
| `event_date` | `d` | Calendar date (YYYY-MM-DD) |
| `agent_name` | `a` | Assigned agent |
| `channel` | `ch` | Customer touchpoint (Web, Mobile App, Agent Call, Branch Visit) |
| `offered_policy_type` | `p` | Product offered (Auto, Home) |
| `funnel_stage` | `s` | Stage name (see below) |
| `event_count` | `n` | Number of events for that combination |

---

## Funnel Stages

Events move through four sequential stages. A customer can appear in multiple stages.

| Stage | `funnel_stage` value | Definition |
|-------|---------------------|------------|
| Impression | `Impression` | Customer was shown a cross-sell offer (ad, banner, agent prompt, etc.) |
| Click | `Click` | Customer engaged with the offer (clicked, expressed interest, agreed to hear more) |
| Quote Started | `Quote_Started` | A formal insurance quote was initiated |
| Conversion | `Converted` | Customer purchased the cross-sell policy |

---

## KPI Metrics

### Impressions
- **Definition:** Total number of cross-sell offer exposures in the selected period and filters.
- **Calculation:** `SUM(event_count) WHERE funnel_stage = 'Impression'`
- **Scope:** Denominator for all rate metrics.

### Clicks
- **Definition:** Total number of customer engagements with the cross-sell offer.
- **Calculation:** `SUM(event_count) WHERE funnel_stage = 'Click'`

### Click-Through Rate (CTR)
- **Definition:** Share of impressions that resulted in a click.
- **Calculation:** `Clicks / Impressions × 100`
- **Interpretation:** Measures offer relevance and initial customer interest.

### Quotes Started
- **Definition:** Total number of formal quotes initiated.
- **Calculation:** `SUM(event_count) WHERE funnel_stage = 'Quote_Started'`

### Quote Rate
- **Definition:** Share of clicks that progressed to a quote.
- **Calculation:** `Quotes Started / Clicks × 100`
- **Interpretation:** Measures how well the conversation converts interest into an active quote.

### Conversions
- **Definition:** Total policies purchased via the cross-sell process.
- **Calculation:** `SUM(event_count) WHERE funnel_stage = 'Converted'`

### Conversion Rate (Conv. Rate)
- **Definition:** Share of impressions that ultimately converted to a sale.
- **Calculation:** `Conversions / Impressions × 100`
- **Interpretation:** The primary end-to-end effectiveness metric. Tracks how much of the audience reached actually purchases.

---

## Dashboard-Calculated Metrics

### Period-over-Period Trend (KPI cards)
- **Method:** The selected date range is split in half by unique dates. The second half is the "current" period; the first half is the "prior" period.
- **Calculation:** `(Current − Prior) / Prior × 100`
- **Display:** ▲ X% (green) for improvement, ▼ X% (red) for decline, shown on each KPI card.
- **Note:** Trend is suppressed when fewer than 4 unique dates are selected (insufficient data for a meaningful split).

### Conv. Rate % (time chart overlay)
- **Definition:** Monthly (or daily/yearly) conv. rate plotted as a line on the secondary Y-axis.
- **Calculation:** `Converted / Impression × 100` for each time bucket.
- **Axis:** Right Y-axis (%). Bar charts use left Y-axis (volume).

### Break by (segmented comparison)
When a "Break by" dimension is selected (Agent / Channel / Policy), all charts re-render to show per-group values:
- **Time chart** → one conv. rate % line per group.
- **Funnel chart** → grouped horizontal bars (each stage broken out by dimension value).
- **Breakdown comparison chart** → bar chart of Impressions + Conversions per group, with conv. rate % overlay line.

---

## Filter Behavior

| Filter | Effect |
|--------|--------|
| **Date Range** | Includes only rows where `event_date` is within [From, To] inclusive. |
| **Date by** | Controls time-bucket aggregation: Day = `YYYY-MM-DD`, Month = `YYYY-MM`, Year = `YYYY`. |
| **Agent** | Includes only rows for the selected agent(s). "All" = no filter applied. |
| **Channel** | Includes only rows for the selected channel(s). "All" = no filter applied. |
| **Policy Type** | Includes only rows for the selected policy type(s). "All" = no filter applied. |
| **Break by** | Adds a dimension-level split to all charts. Does not restrict data — all selected rows are still shown, broken out by the chosen dimension. |

Filters are additive (AND logic): a row must satisfy all active filters to be included.
