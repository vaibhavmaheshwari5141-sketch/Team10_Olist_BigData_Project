# Team10_Olist_BigData_Project
Olist E-Commerce Data Transformation using Databricks, PySpark and Delta Lake


## Checkpoint 3: Data Transformation Using Databricks

### Project Overview

This project implements a data engineering pipeline for the Brazilian Olist E-Commerce Dataset using Databricks, PySpark, SQL, Delta Lake, and Unity Catalog.

The goal is to prepare clean and structured datasets to analyze delivery performance, customer satisfaction, and revenue patterns.

### Business Problem

Where is delivery performance hurting customer reviews and revenue, and which sellers, product categories, and customer states should operations prioritize?

Our project aims to answer:

1. How do monthly revenue and orders vary by product category and customer state?
2. Which sellers, seller states, and product categories experience the highest late-delivery rates?
3. How do late deliveries relate to customer review scores and revenue?

### Dataset

**Source:** Brazilian E-Commerce Public Dataset by Olist

Dataset link: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

The dataset contains approximately 100,000 orders from 2016–2018 and nine related CSV files covering customers, orders, order items, payments, reviews, products, sellers, geolocation, and product category translations.

**License:** CC BY-NC-SA 4.0. Used for non-commercial educational purposes with attribution to Olist.

### Technologies Used

- Databricks Free Edition
- Apache Spark / PySpark
- SQL
- Delta Lake
- Unity Catalog
- Python
- Git and GitHub

### Data Pipeline Architecture

Raw Olist CSV Files  
↓  
Bronze Layer – Raw Data Ingestion  
↓  
Silver Layer – Data Cleaning and Transformation  
↓  
Data Quality Validation  
↓  
Gold Layer – Analytical Tables (Planned for Checkpoint 4)  
↓  
Dashboard and Business Recommendations (Planned)

### Databricks Notebooks

| Notebook | Description |
|---|---|
| 00_Setup.ipynb | Databricks catalog and schema configuration |
| 01_Bronze_Ingestion.ipynb | Ingestion of nine CSV datasets into Bronze Delta tables |
| 02_Silver_Transformation.ipynb | Data cleaning, type conversions, duplicate handling, standardization and derived columns |
| 03_Data_Quality_Checks.ipynb | Data validation, relationship checks and quality logging |

### Data Transformation Activities

The Silver pipeline performs the following transformations:

- Converts timestamp and numeric columns to appropriate data types.
- Investigates and handles duplicate records.
- Creates delivery-delay and delivery-status fields.
- Standardizes customer and seller geographical information.
- Converts product measurements into numeric values.
- Translates product categories from Portuguese to English.
- Creates order-level payment summaries.
- Deduplicates geolocation records and produces ZIP-code-level summaries.
- Selects one review per order for order-level review analysis.

### Data Quality Validation

The project includes checks for:

- Missing business identifiers
- Duplicate business keys
- Invalid delivery chronology
- Invalid prices and freight charges
- Review scores outside the expected range
- Missing relationships between orders, customers, products, and sellers

The Databricks validation execution reported **50 passing checks**. Results are stored in a Delta quality-log table.

### Agile Development Approach

The project follows an iterative approach in which ingestion, transformation, and validation are developed as separate tasks.

GitHub is used to maintain and share the notebook code.

A separate Agile write-up documents team responsibilities, sprint planning, challenges, progress, and future work.

### How to Run

1. Set up a Databricks workspace with Unity Catalog.
2. Create the `olist` catalog and the `bronze`, `silver`, and `gold` schemas.
3. Create the `olist.bronze.raw_files` managed Volume.
4. Download the nine source CSV files from the Olist dataset page and upload them to that Volume.
5. Run `00_Setup.ipynb` for the required environment configuration.
6. Run `01_Bronze_Ingestion.ipynb`.
7. Run `02_Silver_Transformation.ipynb`.
8. Run `03_Data_Quality_Checks.ipynb`.
9. Review the resulting Delta tables and quality-check outputs.

Run the notebooks sequentially and inspect any failed checks before using the data for analysis.

### Next Steps – Checkpoint 4

- Build Gold analytical tables and a star schema.
- Create monthly revenue and delivery-performance aggregates.
- Automate the pipeline with Databricks Jobs.
- Develop an analytical dashboard.
- Produce business recommendations based on the findings.

### Limitations

- Data represents the historical Olist marketplace between 2016 and 2018.
- Findings may not represent current e-commerce activity.
- Delivery delays may be associated with review scores without proving causation.
- Missing delivery dates and repeated review records require documented analytical decisions.

### Team

Team 10 – Big Data Project Work

### Project Status

**Checkpoint 3:** Data ingestion, transformation, and data quality validation completed.

**Checkpoint 4:** Gold modelling, workflow automation, and dashboard development planned.
