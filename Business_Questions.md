# Business Questions — Adventure Works Dataset

> **Project context:** Retail bicycle company. Dataset includes transactions, returns, products, customers, and territories.
> **Analyst:** Zelly Irigon
> **Last updated:** April 2026

---

## Project Context

**Simulated stakeholder:** Head of Commercial, Adventure Works Cycles
**Central decision:** Where to adjust product mix, return policy, and territory focus to maximise net margin
**Delivery format:** Executive dashboard + insights document
**Portfolio audience:** Recruiters and hiring managers at mid-size companies in New Zealand

---

## Business Objective

Provide the Head of Commercial with an executive view of sales performance
across products, regions, and categories, combining a weekly pulse with
YTD trend analysis, enabling faster decisions on where to protect margin
and where to redirect sales effort.

**The dashboard answers three questions:**
- Are we on track to hit our revenue and margin targets this period?
- Which products and territories are driving performance, and which are dragging it down?
- Where are return rates creating a net margin risk that gross revenue is hiding?

---

## How to read this document

Each question follows a standard structure used in professional BI work:

- **Question** — what the business is actually asking
- **Why it matters** — the decision this question supports
- **Key metrics** — what to calculate
- **Dimensions** — how to slice the data
- **Watch out for** — data quality or logic traps specific to this question

---

## BQ-01 — Revenue performance: YTD vs prior year

**Question:** Is the business growing? How does revenue this year compare to the same period last year?

**Why it matters:** The single most common executive ask. Before any other analysis, leadership needs to know if the top line is moving in the right direction. This is the baseline for every other conversation.

**Key metrics:**
- Revenue YTD (current year)
- Revenue YTD (prior year, same cutoff date)
- Absolute variance (R$ / NZD)
- Percentage variance (%)

**Dimensions:** Month, quarter, product category, territory

**Watch out for:** Using total revenue without accounting for returns inflates the number. Always present **gross revenue** and **net revenue (after returns)** side by side so the audience understands what they're looking at.

---

## BQ-02 — Top 10 products by profit margin

**Question:** Which products actually make money, not just generate sales volume?

**Why it matters:** High-revenue products with thin margins can destroy profitability. A product selling $500k at 5% margin is less valuable than one selling $200k at 40% margin. This question guides inventory, pricing, and marketing investment decisions.

**Key metrics:**
- Gross margin per product (`Product Price - Product Cost`)
- Margin % (`Gross Margin / Revenue`)
- Total margin contribution (margin % × quantity sold)

**Dimensions:** Product name, category, subcategory

**Watch out for:** Margin % alone can be misleading for low-volume products. Rank by **total margin contribution** (not just %) to surface products that move the needle.

---

## BQ-03 — Return rate and net revenue impact by product

**Question:** Which products are being returned most, and what is the true revenue cost of those returns?

**Why it matters:** This is the question most junior analysts miss. Returns are not just a logistics problem — they directly reduce net revenue and signal product quality issues, misleading descriptions, or fulfillment errors. In the Maven dataset this is a competitive differentiator: most public datasets don't include returns.

**Key metrics:**
- Return rate per product (`Returns / Orders`)
- Gross revenue lost to returns
- Net revenue (`Gross Revenue - Returned Revenue`)
- Return rate by category and subcategory

**Dimensions:** Product, category, month (trend), territory (are returns concentrated in one region?)

**Watch out for:** A product with 2% return rate and $800k in sales causes more damage than one with 10% return rate and $5k in sales. Always present **absolute revenue impact**, not just the rate.

---

## BQ-04 — Sales performance by region and continent

**Question:** Where is the business winning, and where is it losing?

**Why it matters:** Geographic analysis informs where to expand, where to invest in sales teams, and where pricing or product mix may need adjustment. In New Zealand BI roles, territory analysis is a very common deliverable because businesses often span AU/NZ and sometimes Asia-Pacific.

**Key metrics:**
- Total orders, revenue, and net revenue by territory
- Average order value by region
- Return rate by region

**Dimensions:** Country, continent, sales territory

**Watch out for:** Different regions may have different product mixes, which distorts direct revenue comparisons. When comparing regions, also look at **which categories** are driving the numbers in each place.

---

## BQ-05 — Monthly trend and seasonality

**Question:** Does the business have predictable peaks and troughs across the calendar year?

**Why it matters:** Seasonality drives inventory planning, staffing, marketing spend timing, and cash flow forecasting. Identifying it turns a "gut feel" into a data-backed operational plan.

**Key metrics:**
- Monthly revenue (2+ years to spot patterns)
- Month-over-month growth rate
- Same month prior year comparison
- Rolling 3-month average (to smooth noise)

**Dimensions:** Month, year, category (do bikes and accessories peak in different months?)

**Watch out for:** One-off events (promotions, stockouts, COVID years) can masquerade as seasonality. Always annotate anomalies before calling something a seasonal pattern.

---

## BQ-06 — Customer Pareto: do 20% of customers generate 80% of revenue?

**Question:** What share of customers are responsible for the majority of revenue?

**Why it matters:** If 20% of customers drive 80% of revenue, the retention strategy for those customers is completely different from mass-market acquisition. This is one of the most actionable segmentation analyses in retail.

**Key metrics:**
- Cumulative revenue by customer (ranked descending)
- % of customers needed to reach 80% of total revenue
- Average order value and order frequency by customer tier

**Dimensions:** Customer ID, tier (top 10%, next 20%, rest)

**Watch out for:** Run this on **net revenue** (after returns), not gross. A customer who buys $10k and returns $8k is not a top customer.

---

## BQ-07 — Demographic profile of top buyers

**Question:** Who are the best customers? What do their age, income, education, and occupation look like?

**Why it matters:** Demographics explain the "why" behind purchase behaviour and inform marketing targeting. If top buyers are consistently aged 35-50, professional, with mid-to-high income, that shapes channel selection, messaging tone, and sponsorship decisions.

**Key metrics:**
- Distribution of age, annual income, education level, occupation for top 20% of customers by revenue
- Compare top tier vs bottom tier profiles

**Dimensions:** Age band, income band, education, occupation, marital status, number of children

**Watch out for:** Correlation is not causation. A demographic skew in your top buyers may reflect who your marketing reaches, not who would buy if targeted differently. Flag this assumption in your report.

---

## BQ-08 — Categories with worst margin adjusted for returns

**Question:** After accounting for returns, which product categories are the least profitable?

**Why it matters:** This is the "net margin by category" question and it's the most honest view of category profitability. A category with decent gross margin can become a loss leader once return costs are baked in.

**Key metrics:**
- Gross margin by category
- Revenue lost to returns by category
- **Return-adjusted margin** (`(Gross Revenue - Returned Revenue - COGS) / (Gross Revenue - Returned Revenue)`)
- Rank change: which categories drop in rank when returns are included?

**Dimensions:** Category, subcategory

**Watch out for:** This question requires joining the returns table carefully. A return should remove the revenue AND the cost of goods from the margin calculation. Validate your join logic before presenting results.

---

## BQ-09 — Customer retention and repeat purchase rate

**Question:** Are customers coming back, or is the business dependent on constant acquisition?

**Why it matters:** In retail, acquisition is expensive. A business with high repeat purchase rates is far more capital-efficient. This question also surfaces loyalty opportunities.

**Key metrics:**
- % of customers with 2+ orders (repeat rate)
- Average time between first and second purchase
- Revenue from repeat vs first-time buyers

**Dimensions:** Year, territory, customer demographic tier

**Watch out for:** The dataset time range matters. If you only have 6 months of data, repeat rates will look artificially low. Always state the observation window in your report.

---

## BQ-10 — Average order value trend over time

**Question:** Are customers spending more or less per transaction over time?

**Why it matters:** Average Order Value (AOV) is a leading indicator of pricing power and product mix shifts. A declining AOV might mean customers are trading down to cheaper products, or that discounts are being overused.

**Key metrics:**
- AOV by month (`Total Revenue / Total Orders`)
- AOV by category
- AOV by customer tier

**Dimensions:** Month, category, territory, customer segment

**Watch out for:** Returns affect AOV if you're calculating on net revenue. Be explicit about whether your AOV is gross or net — and be consistent throughout the report.

---

## Summary table

| ID    | Question                               | Decision it supports              | Requires returns data |
|-------|----------------------------------------|-----------------------------------|-----------------------|
| BQ-01 | YTD vs prior year revenue              | Executive reporting               | Yes                   |
| BQ-02 | Top 10 products by margin              | Inventory & pricing               | No (but recommended)  |
| BQ-03 | Return rate & net revenue impact       | Product quality & net P&L         | Yes                   |
| BQ-04 | Sales by region and continent          | Territory strategy                | No                    |
| BQ-05 | Monthly trend and seasonality          | Planning & forecasting            | No                    |
| BQ-06 | Customer Pareto 80/20                  | Retention investment              | Yes                   |
| BQ-07 | Demographic profile of top buyers      | Marketing targeting               | No                    |
| BQ-08 | Category margin adjusted for returns   | Category management               | Yes                   |
| BQ-09 | Repeat purchase rate                   | Loyalty & acquisition strategy    | No                    |
| BQ-10 | Average order value trend              | Pricing power & mix shift         | Recommended           |
