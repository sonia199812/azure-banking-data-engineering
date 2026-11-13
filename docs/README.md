# Azure Banking Data Engineering | End-to-End Lakehouse Pipeline

**Azure Data Factory · ADLS Gen2 · Azure Databricks · PySpark · Delta Lake · Unity Catalog · GitHub Actions**

An end-to-end banking data engineering portfolio project demonstrating incremental ingestion, data validation, Delta Lake upserts, dimensional modeling, workflow orchestration, and CI/CD practices on Azure.

## 1. Project Overview

This project implements a Medallion Architecture (Bronze, Silver, Gold) to process synthetic banking datasets covering customers, accounts, and financial transactions.

The solution focuses on building reliable, incremental data pipelines using Azure Databricks, with Azure Data Factory for ingestion and orchestration.

**Key engineering objectives:**
- Ingest banking datasets from Azure Data Lake Storage.
- Process newly arriving files incrementally using Databricks Auto Loader.
- Apply data quality checks and isolate invalid records.
- Implement incremental Delta Lake MERGE operations.
- Build curated analytical tables using dimensional modeling.
- Orchestrate dependent notebook tasks using Databricks Lakeflow Jobs.
- Validate failure scenarios, schema changes, and duplicate handling.
- Introduce GitHub-based CI/CD for version control and deployment.

## 2. Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| Cloud platform | Microsoft Azure | Infrastructure |
| Data ingestion | Azure Data Factory | Source ingestion and workflow integration |
| Data lake | ADLS Gen2 | Cloud object storage |
| Processing | Azure Databricks, PySpark | Distributed transformations |
| Storage format | Delta Lake | ACID transactions and incremental updates |
| Governance | Unity Catalog | Catalog and schema organization |
| Incremental ingestion | Auto Loader | New-file processing |
| Incremental processing | Delta Change Data Feed | Process source changes |
| Orchestration | Lakeflow Jobs | Task dependencies and execution |
| Version control | GitHub | Source management |
| CI/CD | GitHub Actions, Databricks CLI | Validation and notebook deployment |

## 3. Architecture

```text
Synthetic Banking Data
(Transactions, Customers, Accounts)
                |
                v
       Azure Data Factory
                |
                v
          ADLS Gen2
         (Raw CSV Files)
                |
                v
     Azure Databricks Auto Loader
                |
                v
       BRONZE — Delta Tables
         Raw Ingestion
                |
                v
      SILVER — Delta Tables
     Validation / Deduplication
       CDC / MERGE / Quarantine
                |
                v
       GOLD — Delta Tables
       Facts / Dimensions
       Business Aggregations
                |
                v
        Databricks SQL
      Analytical Query Layer
```

**Orchestration:** Databricks Lakeflow Jobs coordinates Bronze, Silver, and Gold processing. Azure Data Factory has also been validated for triggering the Databricks job through its REST API.

## 4. Data Processing Design

### Bronze Layer — Incremental Ingestion

The Bronze layer ingests source CSV files using Databricks Auto Loader.

Implemented capabilities:
- Incremental file discovery and processing
- Schema tracking
- Checkpoint-based progress tracking
- Delta Lake storage
- Ingestion metadata
- Change Data Feed enabled on source tables

Auto Loader is used to avoid repeatedly processing previously ingested files.

### Silver Layer — Data Quality and Incremental Upserts

The Silver layer transforms raw data into validated, deduplicated datasets.

Implemented capabilities:
- Data type conversion and validation
- Invalid transaction quarantine
- Duplicate handling
- Change Data Feed-based incremental processing
- `foreachBatch` processing
- Delta Lake `MERGE` for incremental upserts
- Checkpoint-based recovery state

Invalid records are isolated instead of being silently discarded.

### Gold Layer — Analytical Data Model

The Gold layer provides curated datasets for downstream analytical queries.

**Fact and dimension tables:**
- `fact_transactions`
- `dim_customer`
- `dim_account`

**Analytical summaries:**
- `gold_daily_transactions`
- `gold_fraud_summary`

The model supports transaction analysis, daily financial summaries, and fraud-related reporting.

The transaction source does not contain an account identifier, so the project does not assume a direct transaction-to-account relationship.

## 5. Pipeline Orchestration

A Databricks Lakeflow Job coordinates seven processing tasks:

- Three Bronze ingestion tasks
- Three Silver transformation tasks
- One Gold processing task

Task dependencies ensure that downstream processing starts after its required upstream tasks complete.

Azure Data Factory integration was separately tested using a Web activity that successfully triggered the Databricks job through the Jobs API.

The complete ADF Copy Data → Databricks Web activity sequence has not been validated as a single end-to-end run.

## 7. Reliability and Troubleshooting

Isolated practice exercises were used to explore data engineering failure scenarios without modifying the main banking pipeline.

| Scenario | Validation |
|---|---|
| Schema drift | Demonstrated a renamed source column causing a schema mismatch and normalized the incoming schema |
| Duplicate records | Demonstrated duplicate ingestion and deduplication |
| Delta MERGE | Tested existing-key matching and new-record insertion |
| Data skew | Created a skewed dataset to investigate uneven key distribution |
| Failure recovery | Explored failure handling separately from the production-style workflow |

Additional recovery, performance, and repeated-run idempotency testing remain opportunities for improvement.
