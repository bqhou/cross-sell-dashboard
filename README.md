# Insurance Cross-Sell Dashboards

Interactive analytics dashboards for an insurance book of business, built from a SQLite database and rendered as standalone HTML pages with Chart.js.

**🔗 Live demo:** https://bqhou.github.io/cross-sell-dashboard/

## Dashboards

| Dashboard | What it answers |
|---|---|
| [Cross-Sell](cross_sell_dashboard.html) | How do cross-sell offers move through the funnel (Impression → Click → Quote Started → Converted), and where do customers drop off by agent, channel and product? |
| [Customers](customers_dashboard.html) | How fast is the customer base growing, and how does it break down by product mix, age and agent? |
| [Policies](policies_dashboard.html) | How many policies are written over time, what is their status, and how are premiums distributed by policy type? |

All three share a navigation bar and interactive filters, and update every chart and KPI on filter change.

## Data

- **Source:** `insurance_sample.db` (SQLite), with tables `customers`, `policies` and `cross_sell_events`. All data is synthetic.
- **Metric definitions:** see [data/metrics_definitions.md](data/metrics_definitions.md) for KPI formulas, funnel stages and column mappings.
- Data is pre-aggregated with SQL and embedded in each page, so the dashboards run with no server or database connection.

## Tech stack

SQL (SQLite) · HTML / CSS / JavaScript · Chart.js 4.4

## Run locally

Open any `.html` file in a browser. No build step or install is needed.
