---
layout: default
title: Olist Data Warehouse
---

[← Back to Portfolio](../../)

# 📦 Olist E-Commerce Data Warehouse & Power BI Pipeline

[add: one-sentence tagline, e.g. "An end-to-end analytics engineering project turning raw marketplace data into executive-ready insights."]

---

## 1. Background & Overview

[add: 2–3 sentences on why you chose this dataset/project — e.g. "This project simulates a real analytics engineering engagement: taking a messy, multi-table e-commerce dataset and turning it into a governed, business-ready reporting layer."]

**Dataset:** Olist Brazilian E-Commerce public dataset — [add link to source, e.g. Kaggle]
**Time period covered:** [add date range, e.g. Sep 2016 – Oct 2018]
**Tools used:** PostgreSQL (data warehouse), Power BI (semantic model & dashboards)

**Business goals this project set out to answer:**
- How healthy is customer acquisition and retention?
- Which sellers are driving growth, and which need support?
- Where is the business losing money or satisfaction to logistics?
- Which customer segments and revenue patterns matter most?

[add: link to live Power BI dashboard / screenshots, if available]

---

## 2. Data Structure Overview

The warehouse follows a **Medallion Architecture** — four schemas, each with a distinct job:

| Layer | Schema | Purpose |
|---|---|---|
| Raw | `bronze` | Ingests raw, untransformed source data (CSV/API) |
| Cleansed | `silver` | Conforms & validates: null handling, deduplication, chronological integrity checks |
| Curated | `gold` | Business-ready **Star Schema** — dimensions, facts, resolved IDs, base metrics |
| Presentation | `reporting` | Denormalized, pre-aggregated views built specifically for Power BI |

**Gold layer (Star Schema):**

| Type | Table | Grain / Contents |
|---|---|---|
| Dimension | `dim_customers` | Unified customer records with latest known location |
| Dimension | `dim_sellers` | Standardized seller profiles with geographic enrichment |
| Dimension | `dim_products` | Product catalog, volumetric data, English category names |
| Fact | `fact_sales` | **Order ID + item** — price, freight, delivery milestones |
| Fact | `fact_payments` | Payment splits and installment metrics |
| Fact | `fact_feedback` | Review scores and comments (chronologically validated) |

**Reporting layer (used directly by Power BI):**

| View | What it answers |
|---|---|
| `customer_360` | RFM segmentation — Champions, Loyal, At Risk, Hibernating, etc. |
| `seller_performance` | MoM revenue momentum vs. lifetime average, by seller health tier |
| `seller_volume_feedback_segmentation` | Order volume × review score quadrant matrix |
| `payment_type_per_state_performance` | Payment method mix by customer state |
| `freight_to_price_30d_rolling_avg` | Daily shipping-cost-to-price ratio, smoothed over 30 days |
| `top_categories_performance` | MoM revenue growth for the top 5 product categories |
| `customer_acquisition_rate` | Daily new customers and cumulative growth |

📄 *Full column-level definitions, DDL scripts, and the ER diagram live in [add link/path, e.g. `/docs` or a wiki page] — this README keeps the technical detail to a minimum on purpose.*

---

## 3. Executive Summary

> **The one-paragraph version:** [add: 2–3 sentences summarizing the overall state of the business in your own words — e.g. "Olist's marketplace is growing steadily, but revenue is concentrated in a specific customer/logistics profile, and both the customer base and the seller base show a 'long tail' of accounts at risk of disengaging."]

**Headline numbers:**

| Metric | Finding |
|---|---|
| Customer growth | Steady, organic — no major spikes or drops [add: total customers / avg growth rate] |
| Seasonal peaks | Category revenue accelerates **May–Jul** and **Nov–Jan** |
| Logistics cost | Freight-to-price ratio rises every **summer**, driven by bulky categories |
| Payment mix | **Credit card** dominates nationally; Boleto is runner-up |
| Seller health | Half of all sellers sit in lower-performing tiers — the biggest growth lever |
| Customer health | New customers are the largest group, but **At Risk / Lost / Cannot-Lose-Them** together outnumber loyal repeat buyers |
| Revenue geography | **70%** of revenue comes from cross-state sales |
| Satisfaction driver | Delivery delays hurt review scores **very strongly** — the #1 lever |
| Top revenue profile | Early-delivered, cross-state orders from **Cannot Lose Them** customers paying in 7+ installments |

---

## 4. Insights Deep Dive

### 📈 Growth & Seasonality

#### Insight 1: Steady, Organic Customer Growth
**So what?** The business has a stable foundation to scale marketing and retention on — growth isn't a fluke or a one-off spike.

| | |
|---|---|
| **Quantified Value** | [add: total new customers / avg daily or MoM growth %] |
| **Business Metric** | Customer Acquisition Rate (daily new customers + cumulative growth) |
| **The Story** | Across the full dataset period, new customer sign-ups climbed consistently without dramatic spikes or crashes — a sign of organic, sustainable growth rather than short-lived marketing bursts. |

#### Insight 2: Two Seasonal Growth Windows
**So what?** These windows are the best times to plan inventory, seller capacity, and marketing spend.

| | |
|---|---|
| **Quantified Value** | [add: MoM growth % in each window for the top 5 categories] |
| **Business Metric** | Month-over-Month revenue growth, top 5 historical categories |
| **The Story** | Twice a year, revenue growth for the leading categories accelerates: once mid-year (**May–Jul**) and once around the holidays (**Nov–Jan**) — a repeatable seasonal pattern worth building the calendar around. |

### 🚚 Logistics & Delivery

#### Insight 3: Summer Shipping Costs Spike, and It's Category-Driven
**So what?** A handful of bulky/heavy categories are quietly eating into margins every summer.

| | |
|---|---|
| **Quantified Value** | Freight-to-price ratio **> 0.18** flagged as high; [add: % of orders/items above this threshold in summer vs. other seasons] |
| **Business Metric** | Freight-to-Price ratio, 30-day rolling average |
| **The Story** | Every summer, shipping costs climb relative to item price, and it's disproportionately driven by `bed_bath_table`, `health_beauty`, and `furniture_decor` — categories that are either heavy, bulky, or both. |

#### Insight 9: Delivery Delays Are the Biggest Threat to Satisfaction
**So what?** On-time delivery is likely the single highest-leverage fix for review scores — bigger than product or price.

| | |
|---|---|
| **Quantified Value** | [add: avg review score for on-time vs. delayed orders, or correlation coefficient] |
| **Business Metric** | Review score vs. delivery delay (actual vs. estimated delivery date) |
| **The Story** | When an order arrives later than promised, satisfaction doesn't just dip — it drops sharply. Delivery reliability outweighs almost every other factor tested against review scores. |

### 💳 Payments

#### Insight 4: Credit Card Wins Nationally — With One Curious Regional Exception
**So what?** The RR pattern is interesting but too small to act on — flagging it for visibility, not for a business decision.

| | |
|---|---|
| **Quantified Value** | [add: national payment type % split]. RR (Roraima) is boleto-led but represents only **0.05% of all orders** |
| **Business Metric** | Payment type share by customer state |
| **The Story** | Credit card is the dominant payment method across almost every state. RR stands out with boleto as its top method and zero debit-card or voucher usage — but with so few orders, this is a curiosity, not a trend. |

### 🏪 Sellers

#### Insight 5: More Volume, More "Needs Improvement" — But Fewer "Bad" Reviews
**So what?** Scale seems to trade perfection for consistency — worth investigating *why* before drawing conclusions.

| | |
|---|---|
| **Quantified Value** | [add: % needs-improvement and % bad reviews by volume tier] |
| **Business Metric** | Seller Volume × Review Score quadrant (High/Medium/Low volume × Excellent/Needs Improvement/Bad) |
| **The Story** | As sellers handle more orders, the share of merely "needs improvement" reviews rises — but, surprisingly, the share of outright "bad" reviews falls. **Open question: why?** (See Caveats.) |

#### Insight 6: Half the Seller Base Is a Growth Opportunity Waiting to Happen
**So what?** "Cooling Off" sellers are the fastest path to more Leaders; "Recovering/Underperforming" sellers are the biggest lever for overall platform revenue.

| | |
|---|---|
| **Quantified Value** | Cooling Off sellers are **2x** as numerous as Leaders (with slightly higher avg sales); Recovering + Underperforming make up **~50%** of sellers with **60%+ lower** average sales |
| **Business Metric** | Seller Performance tiers — Avg Sales (AS) by tier (Growth Leader, Cooling Off, Recovering, Underperforming) |
| **The Story** | Leader sellers give the platform a solid revenue base. Cooling Off sellers already perform nearly as well and are numerous — a natural pipeline into the Leader tier. Recovering and Underperforming sellers make up half the base by count but contribute far less revenue each — the single biggest opportunity to lift overall platform sales. |

### 👥 Customers

#### Insight 7: The Customer Base Is Skewed Toward Risk
**So what?** Retention — not just acquisition — needs to be the priority, because most of the "next tier" of customers are disengaged, not loyal.

| | |
|---|---|
| **Quantified Value** | [add: % of customers in each RFM segment] |
| **Business Metric** | RFM Segmentation (Recency, Frequency, Monetary) |
| **The Story** | New customers form the largest group — but the next three largest are **Cannot Lose Them**, **Lost**, and **At Risk**, not repeat loyal buyers. The base is fragile, and losing these segments would hurt more than the new-customer pipeline can offset. |

<details>
<summary>RFM segment reference (click to expand)</summary>

| Segment | Criteria | Profile | Recommended Action |
|---|---|---|---|
| Recent / New | r_score ≥ 4 | Freshly acquired, high repeat-purchase potential | Onboarding emails, second-purchase coupons, recommendations |
| Cannot Lose Them | m_score = 5 (dormant) | High-value "whale" accounts gone quiet | Aggressive reactivation, premium win-back offers, direct outreach |
| Lost / Hibernating | r_score ≤ 2, m_score ≤ 2 | Churned, low-spend, long ago | Exclude from expensive channels; low-cost quarterly re-engagement only |
| At Risk | r_score ≤ 2, m_score ≥ 3 | Previously solid spenders, now disengaged | Personalized win-back surveys, targeted bundles, feedback requests |

</details>

### 🌎 Revenue & Geography

#### Insight 8: Cross-State Sales Are the Real Revenue Engine
**So what?** Olist's core value proposition is connecting customers with sellers *outside* their home state — logistics for this needs to stay a strength, not a weakness.

| | |
|---|---|
| **Quantified Value** | **70%** of total revenue comes from cross-state orders |
| **Business Metric** | Revenue split — cross-state vs. in-state (customer state vs. seller state) |
| **The Story** | Most purchases aren't local — customers are routinely buying from sellers in a different state, making cross-state logistics a core (not peripheral) part of the business model. |

#### Insight 10: The Highest-Value Order Profile Is Very Specific
**So what?** This is a small but extremely valuable customer profile worth understanding and replicating — not a coincidence.

| | |
|---|---|
| **Quantified Value** | [add: % of total net revenue this profile represents] |
| **Business Metric** | Total Net Revenue by delivery-speed × cross-state × customer segment × installment count |
| **The Story** | The single biggest source of net revenue is orders that arrive **6+ days early**, ship **cross-state**, are placed by **Cannot Lose Them** customers, and are paid in **7+ installments** — a specific, high-value combination worth targeting directly. |

---

## 5. Recommendations

**Sellers**
- Build a "graduation" program to help **Cooling Off** sellers cross into the **Leader** tier — they're already close on average sales *(→ Insight 6)*.
- Prioritize support (training, promotion tools, logistics help) for **Recovering** and **Underperforming** sellers, since they represent half the seller base but a fraction of the revenue *(→ Insight 6)*.
- Investigate *why* high-volume sellers get more "needs improvement" but fewer "bad" reviews — e.g. via review-text analysis — before designing a fix *(→ Insight 5)*.

**Customers**
- Launch reactivation campaigns targeted specifically at **Cannot Lose Them** and **At Risk** segments — they represent disproportionate historical value at real risk of full churn *(→ Insight 7)*.
- Study and replicate the **Cannot Lose Them + cross-state + early delivery + 7+ installments** profile in marketing and offer design, since it's the single highest-revenue combination found *(→ Insight 10)*.
- Keep new-customer onboarding strong, but treat it as only half the retention story *(→ Insight 7)*.

**Logistics**
- Treat on-time delivery as the top lever for customer satisfaction — prioritize it above other satisfaction initiatives *(→ Insight 9)*.
- Review packaging/shipping logistics specifically for `bed_bath_table`, `health_beauty`, and `furniture_decor` ahead of summer, when freight ratios spike *(→ Insight 3)*.
- Protect and invest in cross-state fulfillment reliability, given it drives 70% of revenue *(→ Insight 8)*.

**Planning & Operations**
- Align inventory and seller-capacity planning with the two known seasonal growth windows (**May–Jul**, **Nov–Jan**) *(→ Insight 2)*.
- Treat the RR/boleto pattern as a monitoring note, not an action item, until order volume there grows *(→ Insight 4)*.

[add: any additional recommendations specific to your presentation audience]

---

## 6. Caveats & Assumptions

- **Dataset scope:** Olist's data reflects the Brazilian marketplace only, over [add date range]; findings may not generalize to other markets, countries, or time periods.
- **Currency:** All monetary figures are in Brazilian Real (BRL) unless stated otherwise. [add: note any conversions applied, if none, remove this line]
- **Small-sample caveat:** The RR/boleto finding (Insight 4) is based on just **0.05% of all orders** — interesting, but not statistically robust enough to act on.
- **Freight-to-price threshold:** The `> 0.18` cutoff used to flag "high" freight ratio orders was chosen [add: rationale — e.g. top quartile of the distribution / a round business threshold]; it's a judgment call, not an industry standard.
- **RFM scoring:** Recency and Monetary scores are quintile-based (1–5), calculated **relative to this dataset's own population** — they are not universal benchmarks and would shift if applied to a different customer base or time window.
- **Seller tier thresholds:** Volume tiers (High/Medium/Low) and rating tiers (Excellent/Needs Improvement/Bad) use fixed cutoffs: [add: restate or confirm the exact thresholds, e.g. High Volume >200 orders, Excellent ≥4.0, etc.]
- **Open question:** Why high-volume sellers see more "needs improvement" but fewer "bad" reviews (Insight 5) is unresolved in this analysis — it would benefit from qualitative follow-up, e.g. review-text sentiment analysis.
- **Delivery delay definition:** "Delayed" is calculated as [add: actual delivery date vs. estimated delivery date — confirm exact field logic used].
- **Grain of `fact_sales`:** Order ID + item — meaning multi-item orders contribute multiple rows, which affects any "per order" counts unless explicitly deduplicated.
- **Project purpose:** This is a portfolio/learning project. Insights are demonstrative of analytics capability and are not verified, board-level business decisions.


