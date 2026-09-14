# DAX Measures Reference

Measures used to power the interactive Power BI dashboard for this project. Grouped by table, with a short note on intent where the logic isn't self-evident from the formula alone.

---

## On the Paid Search Ads table

**Search Spend**
```
Search Spend = SUM(SearchAds[Cost (INR)])
```

**Search Revenue**
```
Search Revenue = SUM(SearchAds[Conv. Value (INR)])
```

**Search ROAS**
```
Search ROAS = DIVIDE([Search Revenue], [Search Spend], 0)
```

**Search CTR %**
```
Search CTR % = DIVIDE(SUM(SearchAds[Clicks]), SUM(SearchAds[Impressions]), 0)
```

**Search CPC**
```
Search CPC = DIVIDE([Search Spend], SUM(SearchAds[Clicks]), 0)
```

---

## On the Paid Social Ads table

**Social Spend**
```
Social Spend = SUM(SocialAds[Amount Spent (INR)])
```

**Social Revenue**
```
Social Revenue = SUM(SocialAds[Purchases Conversion Value (INR)])
```

**Social ROAS**
```
Social ROAS = DIVIDE([Social Revenue], [Social Spend], 0)
```

**Social CTR %**
```
Social CTR % = DIVIDE(SUM(SocialAds[Clicks (all)]), SUM(SocialAds[Impressions]), 0)
```

**Cart to Checkout Rate**
```
Cart to Checkout Rate = DIVIDE(SUM(SocialAds[Checkouts Initiated]), SUM(SocialAds[Adds to Cart]), 0)
```
*Purpose: surfaces funnel drop-off between cart and checkout as a single callout metric — useful for flagging tracking gaps or real friction points at a glance.*

---

## Cross-Platform Measures (dedicated "Measures" table)

**Total Ad Spend**
```
Total Ad Spend = [Search Spend] + [Social Spend]
```

**Total Attributed Revenue**
```
Total Attributed Revenue = [Search Revenue] + [Social Revenue]
```

**Blended ROAS**
```
Blended ROAS = DIVIDE([Total Attributed Revenue], [Total Ad Spend], 0)
```

**Total Sales**
```
Total Sales = SUM(Sales[Total Sales (INR)])
```

**Account ROAS (Sales/Spend)**
```
Account ROAS (Sales/Spend) = DIVIDE([Total Sales], [Total Ad Spend], 0)
```
*Purpose: distinct from Blended ROAS — this uses total business sales (including organic/direct traffic), not just ad-attributed revenue. This is the ROAS figure that belongs in any "growth target" math, since the growth target is about total sales, not just paid-channel performance.*

**Target Sales (parameterized growth %)**
```
Target Sales = [Total Sales] * (1 + [Growth Target %])
```
*Assumes a `Growth Target %` parameter measure or disconnected table exists, so the target percentage can be changed via a slicer rather than hardcoded.*

**Required ROAS**
```
Required ROAS = DIVIDE([Target Sales], [Total Ad Spend], 0)
```

**Required Efficiency Gain %**
```
Required Efficiency Gain % = DIVIDE([Required ROAS], [Account ROAS (Sales/Spend)], 0) - 1
```

**Unique Orders**
```
Unique Orders = DISTINCTCOUNT(Sales[Order ID])
```

**AOV (Average Order Value)**
```
AOV = DIVIDE([Total Sales], [Unique Orders], 0)
```
*Purpose: uses DISTINCTCOUNT on Order ID rather than summing a pre-aggregated "Orders" column directly — necessary whenever the underlying sales export is at line-item grain (one row per product per order), where summing a per-line "Orders" flag would overcount.*

---

## Design Notes

- Every ratio measure (`ROAS`, `CTR %`, `CPC`, etc.) uses `DIVIDE()` rather than the `/` operator, with an explicit `0` fallback — this avoids divide-by-zero errors when a filter context returns no spend or no clicks, which happens often once the dashboard has slicers applied.
- Platform-specific measures (Search, Social) are kept on their own fact tables; cross-platform measures live in a separate blank "Measures" table. This keeps the field list organized and makes it obvious which measures are safe to use with which visuals.
- Where a metric could be read two ways (e.g., "ROAS" from ad-attributed revenue vs. from total business sales), both are kept as separate, clearly-named measures rather than collapsed into one — the distinction matters for correctly answering different stakeholder questions.
