# Business Recommendations — Retail Analytics (Power BI)

Analysis of the `Orders` and `Campaigns` tables behind [`Power Bi Analysis and dashboards.pbix`](./Power%20Bi%20Analysis%20and%20dashboards.pbix), pulled directly from the data model — 9,740 order lines and 300 marketing campaigns, Jan 2022–Dec 2024.

---


| Metric | Value |
|---|---|
| Net Revenue (2022–24) | €951,405 |
| Gross Profit / Margin | €518,937 · 54.5% |
| Order lines | 9,740 (8,171 sales / 1,569 returns) |
| Overall return rate | 19.2% |
| On-time delivery | 61.9% |
| NPS | -44.5 |
| 2024 vs 2023 revenue | -7.7% (€326,307 → €301,200) |

Five findings anchor the recommendations below: a category-specific returns problem, a discount tier that's destroying margin without adding volume, a broad-based 2024 revenue dip, a customer segmentation field that isn't tracking value, and a systemic delivery-reliability gap. A sixth item is a data-quality caveat on the marketing numbers — see [§6](#6-data-quality-flag-marketing-roi).

---

## 1. Returns are a category-specific profit drain

| Category | Sales | Returns | Return Rate |
|---|---|---|---|
| Clothing | 1,749 | 543 | **31.0%** |
| Shoes | 1,783 | 467 | **26.2%** |
| Beauty | 690 | 93 | 13.5% |
| Jewelry | 464 | 62 | 13.4% |
| Sports | 744 | 96 | 12.9% |
| Kids | 789 | 95 | 12.0% |
| Home & Living | 451 | 53 | 11.8% |
| Watches | 523 | 60 | 11.5% |
| Bags & Accessories | 978 | 100 | 10.2% |

Clothing and Shoes — classic size/fit-driven categories — return at ~2.5x the rate of everything else. This gap is too large to be noise.

**Recommendation:** pull SKU-level return reasons if available (wrong size vs. changed mind vs. damaged). If size is confirmed as the driver, prioritize a size-guide / fit-predictor feature on Clothing and Shoes PDPs before spending further on acquisition into those categories — incremental orders there carry a much higher return-and-reverse-logistics cost.

---

## 2. Deep discounting is destroying margin faster than it's driving volume

| Promo Code | Orders | Revenue (€) | Discount (€) | Margin % |
|---|---|---|---|---|
| None | 4,053 | 537,472 | 0 | **59.8%** |
| FLASH10 | 708 | 80,187 | 8,910 | 55.9% |
| WINTER15 | 684 | 84,562 | 14,922 | 52.4% |
| SUMMER20 | 668 | 69,512 | 17,378 | 49.9% |
| NEWUSER25 | 659 | 65,298 | 21,766 | 46.1% |
| VIP30 | 670 | 61,103 | 26,187 | 42.7% |
| BFCM40 | 729 | 53,271 | 35,514 | **32.9%** |

Order volume is nearly flat across every tier (~660–730 orders) regardless of discount depth — BFCM40 (40% off) sells about as many orders as FLASH10 (10% off), for almost half the margin.

**Recommendation:** cap standard promotional depth around 15–20%; reserve 30–40% discounts for genuine clearance/inventory-clearing SKUs rather than blanket sitewide events. A/B test a shallower Black Friday offer against the current BFCM40 to check whether volume actually holds up.

---

## 3. Revenue is drifting down, and it's broad-based

| Year | Net Revenue (€) | YoY Change |
|---|---|---|
| 2022 | 323,898 | — |
| 2023 | 326,307 | +0.7% |
| 2024 | 301,200 | **-7.7%** |

All four quarters of 2024 sit below their 2023 counterparts. Acquisition channels and platforms (Own Website, Mobile App, Marketplace) all carry similar margins (~54–55%) and AOV (~€115–118), so the decline isn't concentrated in one channel — it looks like broad demand softening.

**Recommendation:** before assuming a channel or pricing fix, check whether active-customer counts and order frequency (not just revenue) fell in 2024. That distinguishes an acquisition problem from a retention problem from a wallet-share problem — each needs a different fix.

---

## 4. Customer segmentation isn't capturing value tiers

| Segment | Customers | Avg. LTV (€) | Avg. Orders |
|---|---|---|---|
| Gold | 270 | **727** | 6.2 |
| Platinum | 85 | 690 | 5.7 |
| Bronze | 522 | 675 | 5.7 |
| Silver | 532 | 646 | 5.7 |

A working tier system should show a clear LTV staircase from Bronze to Platinum. Here, the top tier (Platinum) trails Gold, and the full spread across four tiers is under €81 — `CustomerSegment` isn't tracking realized value.

**Recommendation:** rebuild segmentation on an RFM basis (recency, frequency, monetary) computed directly from `orders`, rather than relying on the current `CustomerSegment` assignment. Re-target retention/loyalty spend once tiers actually separate by value.

---

## 5. Delivery reliability is a systemic ~38% miss rate

On-time delivery sits at **61.9%** overall — and critically, it's nearly identical across every carrier (60.9–63.7%) and every warehouse (59.8–63.7%). No single vendor or fulfillment center is the outlier.

**Recommendation:** audit the promised-delivery-date logic before renegotiating with carriers. A number this uniform across every carrier and warehouse points to a shared process step (e.g. pick/pack SLA, carrier handoff cutoff, or the delivery-estimate calculation itself) rather than one underperforming partner — tightening a single carrier's contract won't move it.

---

## 6. Data quality flag: marketing ROI

> **Campaign-reported revenue (€106.1M) is ~112x actual net order revenue (€951K), and total campaign spend (€12.2M) alone is ~13x total order revenue.**

`Orders` and `Campaigns` are independent fact tables sharing only the `Calendar` dimension, with no order- or customer-level relationship (by design — see the data model section above). This means ROAS, CAC, and LTV:CAC figures (e.g. a headline 10.5x LTV:CAC) describe the `Campaigns` table's self-reported numbers in isolation — they are **not reconciled to actual sales**.

Within the campaigns table on its own terms: Email (CAC €47.68) and Meta (ROAS 11.2x) are the strongest platforms; Google Ads (CAC €81.28) and Display (ROAS 7.5x) are weakest. Retargeting is the weakest campaign type by ROAS (6.6x) vs. Influencer/Performance (~10.3–10.4x). Directionally useful for a budget conversation, but should be labeled "campaign-reported" rather than presented as verified sales impact.

---

## Priority Action List

| # | Action | Why |
|---|---|---|
| 1 | Investigate Clothing & Shoes return drivers | 2.5x the return rate of other categories — largest single margin-recovery lever |
| 2 | Cap blanket discounts at ~15–20%; restrict 30–40% tiers to true clearance | BFCM40/VIP30 nearly halve margin for no extra volume vs. shallower promos |
| 3 | Rebuild `CustomerSegment` on RFM logic | Current tiers show a <€81 LTV spread — not usable for targeting |
| 4 | Audit promised-delivery-date calibration | 38% miss rate is uniform across all carriers/warehouses |
| 5 | Diagnose the 2024 revenue dip (acquisition vs. retention vs. frequency) | Broad-based decline across every channel and platform |

---

*Data pulled from the `.pbix` data model (`Orders`, `Campaigns`, `Calendar` tables) rather than any external file, to keep figures tied to the exact schema used in the report's DAX measures.*
