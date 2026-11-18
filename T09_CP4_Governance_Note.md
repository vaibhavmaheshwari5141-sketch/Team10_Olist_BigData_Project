# Data Governance Note (Colab version)

**Team 9 | Olist delivery performance pipeline | Checkpoint 4**

## 1. Source and licence
Brazilian E-Commerce Public Dataset by Olist (Kaggle, https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), licence **CC BY-NC-SA 4.0**. We credit Olist, use the data for non-commercial learning only, and share any derivative under the same licence.

## 2. Privacy
* The source is anonymised: customer and seller ids are random keys, locations are zip-code prefixes. There are no names, emails, phone numbers or street addresses.
* We never try to identify people or link records to outside data. `customer_unique_id` is used only as an anonymous key.
* **Review text is not used.** The comment title and message columns are dropped in silver. A check cell in the notebook lists any gold column that looks like free text or an identifier; the result must be an empty list.
* Seller ids are shortened in the charts. Coordinates are averages per zip prefix, not exact points.

## 3. Access control and lineage
This run was done on Google Colab, so **Unity Catalog access control and automatic lineage were not used**. Data lives in a temporary single-user session and is deleted when the session ends. The evidence zip holds only aggregated or anonymised data. If the pipeline were run in production on Databricks, the design is: bronze and silver restricted to data engineers, `SELECT` on the gold schema only for analysts, and Unity Catalog lineage showing bronze -> silver -> gold. The lineage we do have is documented in the README pipeline diagram and in the `source_file` and `ingestion_timestamp` columns of the bronze and silver tables.

## 4. Data quality
Every check is written to `silver.dq_log` (rows in/out, null rates, duplicate keys, orphan keys, date logic, price and review ranges, revenue and order reconciliation from bronze to gold). Critical failures stop the run. Known warnings and why we accept them:

| Warning | Treatment |
|---|---|
| About 775 orders have no item rows (canceled/unavailable) | Not in the item fact; kept in `fact_orders` with zero revenue |
| 8 orders marked delivered with no delivery date; 6 with a delivery date but another status | Excluded from delivery KPIs |
| Carrier timestamps earlier than purchase for about 0.2% of orders | Carrier dates are not used in any KPI |
| About 610 products without a category | Shown as `Unknown` |
| Geolocation has many exact duplicates | Removed in silver, averaged per zip prefix |

## 5. Bias and limits
* 2016 to 2018 only, one Brazilian marketplace; results may not hold today or elsewhere.
* 2016 and Sep-Oct 2018 are incomplete and are left out of time charts.
* Sellers and customers are concentrated in the Southeast (especially Sao Paulo). Small states and sellers have few orders, so we rank only groups with 30 or more delivered orders.
* Late rate is measured against the estimated date set by the platform, so "late" partly reflects how cautious the estimate was.
* Review scores are self-reported by customers. Results show association, not proven cause.

## 6. Responsible use of AI
An AI assistant helped draft code and documents. The team reviewed and tested the work and can explain every step. AI use is recorded in the learning log.
