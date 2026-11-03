# Agile Approach to the Big Data Project

**Project:** Olist E-Commerce Data Transformation and Analytics  
**Team:** Team 9  
**Course:** Big Data Project Work  
**Checkpoint:** 3 – Data Transformation

## 1. Introduction

Our project focuses on developing a data engineering pipeline for the Brazilian Olist E-Commerce dataset using Databricks, PySpark, SQL, Delta Lake, and Unity Catalog.

The purpose is to prepare reliable and structured data for analyzing delivery delays, customer satisfaction, seller performance, and revenue.

We adopted an iterative Agile approach to organize the project into manageable tasks, validate outputs continuously, and maintain the project code using GitHub.

## 2. Project Objectives

The main objectives are:

- Ingest nine related Olist CSV datasets into Databricks.
- Implement a Bronze and Silver data architecture.
- Perform data cleaning and transformation using PySpark and SQL.
- Validate dataset quality and relationships.
- Maintain version-controlled notebooks in GitHub.
- Prepare the transformed data for future analytical modelling and dashboards.

## 3. Agile Methodology

We organized the project into incremental development stages aligned with the course checkpoints.

Each stage has specific deliverables, and completed work is reviewed before beginning the next stage.

The workflow follows these principles:

- **Iterative development:** Implement and validate one dataset at a time.
- **Incremental delivery:** Build the ingestion, transformation, and quality layers separately.
- **Continuous testing:** Run validation checks after transformations.
- **Collaboration:** Share code, documentation, and results through GitHub.
- **Adaptability:** Investigate errors and revise the pipeline when issues are discovered.

## 4. Sprint Planning

| Sprint | Activities | Deliverables |
|---|---|---|
| Sprint 1 – Planning | Skill-gap analysis, business problem identification, dataset selection | Checkpoints 1 and 2 |
| Sprint 2 – Data Engineering | Databricks setup, Bronze ingestion, Silver transformations, data validation | Checkpoint 3 |
| Sprint 3 – Analytics | Gold analytical tables, automated pipeline, dashboards, recommendations | Planned for Checkpoint 4 |

## 5. Sprint 2: Checkpoint 3 Implementation

### Task 1: Environment Setup

Configured Databricks Free Edition and Unity Catalog with the `olist` catalog and Bronze, Silver, and Gold schemas.

Created a managed Volume to store the Olist CSV files.

### Task 2: Data Ingestion

Uploaded nine Olist CSV files and implemented PySpark-based Bronze ingestion.

Stored the source data in Delta tables with ingestion metadata.

### Task 3: Data Transformation

Implemented Silver-layer transformations including:

- Duplicate record handling.
- Data type conversion.
- Timestamp standardization.
- Delivery-delay calculation.
- Price and freight preparation.
- Customer and seller location standardization.
- Product category translation.
- Payment aggregation.
- Review record selection.
- Geolocation deduplication and aggregation.

### Task 4: Quality Validation

Implemented data-quality checks covering missing identifiers, duplicate keys, invalid prices, delivery chronology, review scores, and relationships between datasets.

The recorded validation output reported **50 passing checks**.

A quality-log Delta table was created to preserve validation results.

### Task 5: Version Control

Exported the Databricks notebooks to Jupyter Notebook format and uploaded the notebooks and HTML exports to GitHub.

The repository also contains project documentation and instructions for reproducing the transformations.

## 6. Challenges and Resolution

**Challenge 1: Incorrect review CSV parsing**

During review-score validation, a timestamp string was incorrectly interpreted as an integer. We investigated the source data and identified a CSV parsing issue.

**Resolution:** Re-read the reviews CSV using multiline-aware parsing, validated the review-score column, corrected the Bronze table, and rebuilt the Silver reviews transformation.

**Challenge 2: Missing Silver Reviews table**

The data-quality notebook initially reported that `olist.silver.order_reviews` was missing.

**Resolution:** Recreated the Silver reviews table and verified its existence before continuing with validation.

**Challenge 3: Multiple related datasets**

The source includes several related tables with different identifier relationships.

**Resolution:** Used appropriate business keys and relationship checks to preserve consistency and avoid accidental duplication.

## 7. Testing and Quality Assurance

Quality validation is performed after transformation to detect missing values, invalid data, duplicate business keys, and broken relationships.

Validation results are saved in a dedicated Delta table to provide evidence of testing.

Checks that fail are investigated before using the affected data for reporting.

## 8. Team Collaboration

The project team consists of:

- Aarshi Singh
- Gorikapudi Anne
- Harsh Parashar
- Rajeshwary Shil
- Vaibhav Maheshwari
- Akhil

The project uses GitHub to share notebooks, document decisions, and maintain version history.

Individual responsibilities and contributions should be recorded according to the work actually completed by each member.

## 9. Future Sprint: Checkpoint 4

The next phase will focus on:

- Creating Gold-layer fact and dimension tables.
- Producing analytical metrics for delivery performance and revenue.
- Automating notebook execution using Databricks Jobs.
- Building dashboards for the three business questions.
- Developing evidence-based operational recommendations.

## 10. Conclusion

The Agile approach supported incremental development of the Olist data engineering pipeline.

By separating ingestion, transformation, and validation into manageable tasks, the team established a structured foundation for analytical modelling.

Checkpoint 3 delivers the code and documentation required for the data transformation stage, while Checkpoint 4 will focus on business insights and automation.
