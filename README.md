# Marketing Performance Analysis & Growth Strategy

A freelance analytics project: given six months of multi-channel marketing and sales data for an e-commerce brand, the goal was to diagnose what was working, identify specific waste, and build a data-driven plan to grow sales by 25% **without increasing the marketing budget**.

> **Note on data:** all figures in this repo are anonymized/illustrative. Client identity, raw datasets, and exact business figures have been withheld to respect client confidentiality. The analytical process, methodology, and code structure are fully representative of the original work.

---

## Objective

Increase sales by 25% at flat marketing spend, for a brand running paid campaigns across two major ad platforms and selling through an e-commerce storefront.

## Tools Used

- **Python (pandas, numpy)** — data cleaning, metric calculation, cohort/trend analysis
- **Matplotlib** — trend and diagnostic visualizations
- **Power BI (DAX)** — interactive dashboard with a custom data model and calculated measures
- **Jupyter Notebook** — end-to-end reproducible analysis

## Data

Three raw sources covering the same 6-month window:
- Paid search advertising data (spend, clicks, impressions, conversions)
- Paid social advertising data (spend, funnel events, purchases — at campaign / ad-set / ad-creative grain)
- E-commerce sales data (orders, revenue, customer type, product-level detail)

## Process

1. **Data cleaning & integration** — profiled all three sources, resolved missing values with logic-driven rules (not blind fills), and identified a shared geographic field to join spend and sales data across sources.
2. **Performance analysis** — compared platform-level efficiency (ROAS, CTR, CPC), tracked efficiency trends over time, and identified a shared decline pattern across both ad platforms.
3. **Campaign / ad-set / ad-level audit** — drilled below account-level averages to find specific winners and losers, using consistent, stated ROAS thresholds. Detected a concrete creative-fatigue signature (falling CTR + spiking CPC on the same ad over time).
4. **Budget reallocation modeling** — built a fully budget-neutral reallocation plan: named which campaigns to scale, hold, cut, or test, with a rationale for each move (not just "give more money to the highest ROAS").
5. **Growth feasibility modeling** — calculated the exact efficiency improvement required to hit the growth target at flat spend, and gave an evidence-based (not optimistic-by-default) verdict on whether the target was realistic.
6. **Dashboard build** — designed a 7-page interactive Power BI report (executive overview, channel comparison, campaign performance, ad-set/ad deep dive, funnel diagnostics, budget plan, and a management summary) backed by a clean star-schema data model and reusable DAX measures.

## Key Findings

- One ad platform was substantially more capital-efficient than the other, but received a small fraction of total budget — a clear, provable case of spend not matching proven returns.
- Roughly **one in three** ad-level line items with meaningful spend were returning less than they cost — a direct source of recoverable budget.
- Identified a severe funnel drop-off between cart and checkout on one platform, and explicitly separated the proven data (the drop itself) from the unproven hypothesis (its root cause) — flagged for further investigation rather than assumed.
- Found that both ad platforms showed a synchronized efficiency decline in the second half of the period, pointing to a shared cause (creative fatigue / audience saturation) rather than a platform-specific problem.
- Delivered an honest, math-backed verdict on the growth target: reallocation alone could close roughly a third to half of the required efficiency gap — the rest was conditional on fixing the funnel issue and reversing the efficiency decline, and this was stated plainly rather than glossed over.

## Repository Structure

```
marketing-analytics-growth-strategy/
├── README.md
├── notebooks/
│   └── analysis.ipynb          # Full analysis, cleaning to modeling (anonymized)
├── dashboard/
│   ├── dax_measures.md         # All DAX formulas used in the Power BI model
│   └── screenshots/            # Dashboard page previews
├── reports/
│   └── executive_summary.md    # Sanitized findings & recommendations summary
└── charts/
    └── *.png                   # Key diagnostic visualizations
```

## Deliverables (original engagement)

- Full written analysis and recommendation report
- Interactive Power BI dashboard (data model + DAX measures)
- Budget-neutral reallocation plan with named line items
- Growth feasibility assessment
- Executive summary for stakeholder review

---

*This project was completed as a freelance data analysis engagement. Client details and raw data are withheld; methodology, code, and dashboard design are original work and fully reproducible with any similarly structured dataset.*
