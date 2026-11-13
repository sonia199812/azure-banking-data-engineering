# Azure Banking Data Engineering Pipeline

### End-to-End Data Engineering with Azure Databricks

An end-to-end banking data engineering project built using **Azure Data Factory, Azure Data Lake Storage Gen2, Azure Databricks, PySpark, and Delta Lake**.

The project demonstrates the design and implementation of a scalable-style data processing architecture using the **Medallion Architecture (Bronze, Silver, Gold)**, with a focus on incremental data processing, data quality, orchestration, and automated code deployment.

## Architecture Overview

```text
Banking Data Sources
(Customers, Accounts, Transactions)
             |
             v
     Azure Data Factory
             |
             v
        ADLS Gen2
       (Raw Storage)
             |
             v
     Azure Databricks
             |
    +--------+--------+
    |        |        |
  Bronze   Silver    Gold
 Ingestion  Quality  Analytics
    |        |        |
    +--------+--------+
             |
             v
     Curated Delta Tables
```

The pipeline follows a layered architecture that separates raw ingestion, data transformation, and analytical processing.

## Technology Stack

| Technology | Purpose |
|---|---|
| Azure Data Factory | Data ingestion and orchestration integration |
| ADLS Gen2 | Cloud data lake storage |
| Azure Databricks | Distributed data processing |
| PySpark / Spark SQL | Data transformation |
| Delta Lake | Reliable storage and incremental updates |
| Unity Catalog | Data organization and governance |
| Lakeflow Jobs | Workflow orchestration |
| GitHub Actions | CI/CD automation |

## Solution Design

**Bronze — Data Ingestion**

Banking datasets are incrementally ingested from cloud storage using Databricks Auto Loader and persisted as Delta tables. Schema tracking and checkpoints support reliable processing.

**Silver — Data Transformation**

Raw data is validated, standardized, and deduplicated. Delta Change Data Feed and MERGE operations support incremental processing, while invalid records are isolated for investigation.

**Gold — Analytical Modeling**

Curated fact, dimension, and aggregated tables support transaction analysis, fraud reporting, and other banking analytics use cases.

## Orchestration and CI/CD

Databricks Lakeflow Jobs coordinates the processing pipeline across Bronze, Silver, and Gold layers.

GitHub Actions provides basic CI validation and a manually triggered CD workflow. Notebook deployment to Azure Databricks is authenticated through a service principal and performed using the Databricks CLI.

## Engineering Highlights

- Incremental ingestion and processing using Auto Loader and Delta Change Data Feed.
- Data quality validation, deduplication, and quarantine handling.
- Delta Lake MERGE operations for incremental upserts.
- Analytical data modeling with fact and dimension tables.
- Workflow orchestration with task dependencies.
- Version-controlled development and automated notebook deployment.

## Project Scope

This project uses synthetic banking datasets to demonstrate practical Azure data engineering patterns.

The core Bronze-to-Gold pipeline and GitHub Actions notebook deployment have been successfully tested. Further enhancements could include automated notebook-level tests, expanded monitoring, and production environment promotion.

## Repository

**Source Code:** https://github.com/sonia199812/azure-banking-data-engineering

**Project Focus:** Azure Data Engineering | Databricks | PySpark | Delta Lake | Incremental ETL | Data Quality | CI/CD
