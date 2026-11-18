# Olist Delivery Performance Pipeline (Google Colab version)

**Team 9 | Big Data Project Work | Checkpoint 4**
Aarshi Singh (BA05001), Gorikapudi Anne (BA05061), Harsh Parashar (BA05022), Rajeshwary Shil (BA05044), Vaibhav Maheshwari (BA05057)

Posting: Accenture, Junior Azure Data Engineer with Databricks (AI&Data), Job No. R00317827.

## Business question

Where is delivery performance hurting customer reviews and revenue, and which sellers, product categories and customer states should operations fix first?

* **Q1** How do monthly revenue and orders split by category and customer state?
* **Q2** Which seller states, sellers and categories have the highest late delivery rate (actual against estimated date)?
* **Q3** How much do late deliveries lower review scores, and how much revenue sits in the worst groups?

## Data

Brazilian E-Commerce Public Dataset by Olist (Kaggle): https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
About 100k anonymised orders, 2016 to 2018, 9 CSV files. Licence CC BY-NC-SA 4.0: credit Olist, non-commercial use only, share derivatives under the same licence.

## What is in this submission

| File | Content |
|---|---|
| `Olist_Pipeline_Colab.ipynb` | The whole pipeline in one notebook: setup, bronze, silver, quality checks, gold star schema, dashboard queries, charts, evidence export |
| `T09_CP4_evidence.zip` | Chart images (PNG), the quality log (`dq_log.csv`) and the gold tables as CSV, created by the last cell |
| `README_Colab.md` | This file |
| `Governance_Note_Colab.md` | One-page data governance note |

## Pipeline

```
9 CSV files (/content/raw_files)
  -> Section 1 Bronze:   load as-is + ingestion_timestamp + source_file          (database bronze)
  -> Section 2 Silver:   dedupe, cast types, translate categories, delivery_status (database silver)
  -> Section 3 Quality:  rows in/out, nulls, keys, orphans, business rules -> silver.dq_log
  -> Section 4 Gold:     dimensions, facts, aggregates, reconciliation checks      (database gold)
  -> Section 5 Queries:  one SQL query per dashboard chart
  -> Section 6 Charts:   matplotlib dashboard for Q1 to Q3
  -> Section 7 Export:   evidence zip
```

### Gold data model (star schema)

| Table | Grain | Used for |
|---|---|---|
| `fact_order_items` | one row per order item | revenue by category, seller, state, month |
| `fact_orders` | one row per order | late rate and review score without double counting |
| `dim_date`, `dim_customer`, `dim_seller`, `dim_product` | day / customer / seller / product | slicing |
| `agg_monthly_revenue` | month x category x customer state | Q1 |
| `agg_delivery_by_seller_state`, `_by_seller`, `_by_category`, `_by_customer_state` | one row per group | Q2, Q3 |
| `agg_monthly_delivery` | month | late-rate trend |
| `agg_review_impact` | delay bucket | Q3 |

Surrogate keys are `xxhash64` of the source id, so reruns give identical keys.

### Business definitions

* Revenue = sum of item `price` (freight separate). Valid sale = status not `canceled` or `unavailable`.
* Delivered order = status `delivered` with a delivery date. Late = delivered after the estimated date. Late rate = late / delivered orders.
* Partial months (before 2017-01, after 2018-08) are flagged and left out of time charts.
* Rankings only include groups with at least 30 delivered orders.

## How to rerun (about 15 to 20 minutes)

1. Open `Olist_Pipeline_Colab.ipynb` in Google Colab (File -> Upload notebook).
2. Runtime -> Run all.
3. When asked, upload the Kaggle `archive.zip` (it contains the 9 CSV files).
4. Wait for the last cell, which downloads `T09_CP4_evidence.zip`.
5. Check `silver.dq_log`: every row is PASS, or WARN with a note that explains it.

Use one notebook and one Colab session. Tables live in memory only: if the runtime restarts, run again from the top.

## Differences from the plan in Checkpoint 2 (honest note)

Our plan used Databricks with Unity Catalog. For this checkpoint we ran the pipeline on Google Colab with open-source PySpark, so these planned parts were **not done**:

| Planned | What we did instead |
|---|---|
| Unity Catalog catalog/schemas, role-based GRANTs | Plain Spark databases `bronze`, `silver`, `gold`; no access control, single user |
| Delta tables | Parquet tables |
| Scheduled Databricks Job | One notebook run top to bottom ("Run all") |
| Databricks dashboard | matplotlib charts in the notebook, saved as PNG |
| Lineage screenshot in Unity Catalog | Not available; lineage is documented in the pipeline diagram above |

The notebooks for Databricks (01 to 06) are written and kept in the repository, but have not been used for this submission. The gaps Unity Catalog, Databricks pipelines and Azure remain open and are covered in the learning log and the Azure mapping note.

## Data limits

2016 to 2018 only; one marketplace; incomplete months excluded from time charts; late rate uses dates, not time of day; orders with several sellers or categories count once in each group; results show association, not proof that lateness causes low reviews.

## AI assistant use

An AI assistant (Claude) helped write the quality-check, gold, charting and documentation code. The team reviewed the work and is ready to explain every step.
