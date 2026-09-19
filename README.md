# Retail Analytics — Power BI

A Power BI retail analytics project focused on revenue, profitability, customer behaviour, promotions, returns, delivery performance, and marketing data.

The analysis uses the `Orders`, `Campaigns`, and `Calendar` tables from the Power BI data model and covers **January 2022 to December 2024**.

## Key Metrics

| Metric              |       Result |
| ------------------- | -----------: |
| Net Revenue         | **€951,405** |
| Gross Profit        | **€518,937** |
| Gross Margin        |    **54.5%** |
| Order Lines         |    **9,740** |
| Sales               |    **8,171** |
| Returns             |    **1,569** |
| Return Rate         |    **19.2%** |
| On-Time Delivery    |    **61.9%** |
| NPS                 |    **-44.5** |
| 2024 Revenue Change |    **-7.7%** |

---

## What I Found

### 1. Clothing and Shoes have a much higher return rate

Clothing has a **31.0%** return rate and Shoes **26.2%**. Most other categories are around 10–14%.

This makes Clothing and Shoes the clearest categories to investigate first, particularly because returns in these categories can often be related to sizing and fit.

**Business recommendation:**
Analyze return reasons at SKU level. If sizing is the main reason, improve size guides and consider adding fit-related information or a size recommendation feature to product pages.

---

### 2. Deeper discounts are reducing margin without clearly increasing volume

| Promotion | Orders | Revenue |    Margin |
| --------- | -----: | ------: | --------: |
| FLASH10   |    708 | €80,187 |     55.9% |
| WINTER15  |    684 | €84,562 |     52.4% |
| SUMMER20  |    668 | €69,512 |     49.9% |
| NEWUSER25 |    659 | €65,298 |     46.1% |
| VIP30     |    670 | €61,103 |     42.7% |
| BFCM40    |    729 | €53,271 | **32.9%** |

The number of orders stays fairly similar across the different discount levels, while margin falls significantly as discounts increase.

For example, BFCM40 generated 729 orders compared with 708 for FLASH10, but margin was much lower.

**Business recommendation:**
Test whether 15–20% discounts can generate similar order volumes to deeper promotions. Reserve 30–40% discounts mainly for clearance or products that need to be moved.

---

### 3. Revenue declined across the business in 2024

| Year | Net Revenue | YoY Change |
| ---- | ----------: | ---------: |
| 2022 |    €323,898 |          — |
| 2023 |    €326,307 |      +0.7% |
| 2024 |    €301,200 |  **-7.7%** |

Revenue was almost flat between 2022 and 2023 before falling in 2024.

The decline was not limited to one quarter. All four quarters of 2024 were below their corresponding 2023 quarters.

The main sales channels also show broadly similar margins and average order values, so the data does not point to one specific channel as the main cause.

**Business recommendation:**
Look at active customers, orders per customer, and purchase frequency to determine whether the decline came from customer acquisition, retention, or lower purchasing frequency.

---

### 4. Customer segments do not clearly reflect customer value

| Segment  | Customers | Avg. LTV | Avg. Orders |
| -------- | --------: | -------: | ----------: |
| Gold     |       270 | **€727** |         6.2 |
| Platinum |        85 |     €690 |         5.7 |
| Bronze   |       522 |     €675 |         5.7 |
| Silver   |       532 |     €646 |         5.7 |

The current segment structure does not create a clear value hierarchy. Platinum customers, for example, have lower average LTV than Gold customers, while Bronze customers are relatively close to Platinum.

The difference between the highest and lowest average LTV is also less than €81.

**Business recommendation:**
Rebuild the segmentation using **RFM analysis**:

* **Recency:** How recently did the customer purchase?
* **Frequency:** How often do they purchase?
* **Monetary:** How much do they spend?

This would create segments based on actual customer behaviour rather than the existing segment labels.

---

### 5. Delivery performance suggests a broader process issue

Only **61.9%** of deliveries are on time.

The issue is also fairly consistent across carriers and warehouses:

* Carriers: approximately **60.9–63.7%**
* Warehouses: approximately **59.8–63.7%**

There is no obvious single carrier or warehouse responsible for the problem.

**Business recommendation:**
Review the complete fulfillment process, including:

* Promised delivery-date calculation
* Pick-and-pack processing time
* Warehouse cutoff times
* Carrier handoff
* Delivery-time estimates

Because the performance is similar across different carriers and warehouses, the problem may be related to a shared process rather than one specific partner.

---

## Marketing Data Quality Flag

The marketing data needs to be treated separately from the actual order data.

The `Campaigns` table reports:

* **€106.1M** campaign-reported revenue
* **€12.2M** campaign spend

The `Orders` table contains only **€951K** in net revenue.

`Orders` and `Campaigns` are separate fact tables connected only through the `Calendar` table. There is no order-level or customer-level relationship connecting campaigns to actual purchases.

Because of this, campaign metrics such as **ROAS, CAC, and LTV:CAC should not be interpreted as verified sales attribution**.

They represent the figures reported within the Campaigns table.

Within the Campaigns data:

| Metric                    | Result |
| ------------------------- | -----: |
| Email CAC                 | €47.68 |
| Google Ads CAC            | €81.28 |
| Meta reported ROAS        |  11.2x |
| Display reported ROAS     |   7.5x |
| Retargeting reported ROAS |   6.6x |
| Influencer reported ROAS  | ~10.3x |
| Performance reported ROAS | ~10.4x |

These figures can still be used for directional comparison, but they should be labelled **campaign-reported performance**.

---

## Priority Actions

| Priority | Action                               | Reason                                                         |
| -------- | ------------------------------------ | -------------------------------------------------------------- |
| 1        | Investigate Clothing & Shoes returns | Return rates are significantly higher than other categories    |
| 2        | Test shallower promotional discounts | Deep discounts reduce margin without a clear volume benefit    |
| 3        | Rebuild customer segments using RFM  | Current segments do not clearly separate customer value        |
| 4        | Audit the delivery process           | On-time performance is weak across carriers and warehouses     |
| 5        | Diagnose the 2024 revenue decline    | Need to separate acquisition, retention, and frequency effects |

---

## Tools & Skills

**Power BI**

* Data modeling
* DAX
* Power Query
* Interactive dashboards
* KPI development
* Business analysis

**Analytics**

* Revenue analysis
* Profitability analysis
* Customer segmentation
* RFM analysis
* Promotion analysis
* Return-rate analysis
* Delivery performance analysis
* Marketing data validation

**Business Skills**

* Turning data into business recommendations
* Identifying operational problems
* Evaluating promotional effectiveness
* Data-quality assessment
* Translating dashboard findings into actionable decisions

---

## Project Structure

```text
Retail-Analytics-PowerBI/
│
├── Power Bi Analysis and dashboards.pbix
├── README.md
└── screenshots/
    └── dashboard.png
```

## Key Takeaway

The main lesson from this project is that a dashboard is only useful when it leads to better business questions.

The analysis goes beyond reporting revenue and KPIs. It looks at **why performance is changing, where margin is being lost, which customer data can actually support segmentation, and where the underlying data has limitations**.

The next step would be to validate the findings with more granular data, particularly SKU-level return reasons, customer-level purchase history, and campaign-to-order attribution.
