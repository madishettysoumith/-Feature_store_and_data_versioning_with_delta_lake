# Feature Store and Data Versioning with Delta Lake

## Technologies
- **PySpark** – Used for distributed data processing and feature computation.
- **Delta Lake** – Provides ACID transactions, schema enforcement, data versioning, and time travel.
- **Python** – Used for orchestration, data preparation, and feature logic.
- **Google Colab** – Execution environment for running Spark and Delta Lake.

---

## Project Description
This project demonstrates how to build a simple feature store using **Delta Lake** on top of **Apache Spark**.  
Raw event data is ingested into an immutable Delta table to ensure reliability and auditability. Feature engineering is then performed using Spark to compute meaningful, user-level features.

The derived feature tables are stored in Delta Lake and automatically versioned using Delta Lake’s transaction log. This allows safe feature evolution and enables historical feature access through time travel, which is critical for reproducible machine learning workflows.

---

## Architecture
- **data/raw**  
  Contains raw CSV files that simulate source system data such as user events or transactions. This represents the original input data.

- **delta/raw_table**  
  Stores raw data in Delta format as an immutable table. This layer enforces schema consistency, supports ACID transactions, and serves as the single source of truth for downstream processing.

- **delta/features**  
  Acts as the feature store. Derived features are written to this Delta table and automatically versioned whenever feature logic changes.

---

## Key Features

### Explicit Schema Ingestion
Raw CSV data is ingested using a predefined schema. This avoids schema inference issues, ensures consistent data types, and makes the pipeline reproducible.

### ACID-Compliant Delta Tables
Delta Lake provides atomicity, consistency, isolation, and durability (ACID). This guarantees reliable reads and writes even as feature definitions evolve over time.

### Feature Evolution with Versioning
When feature logic changes, the feature table is overwritten. Delta Lake automatically creates a new version while preserving older versions, enabling safe and controlled feature evolution.

### Delta Lake Time Travel
Delta Lake allows querying historical versions of the feature store using version numbers. This enables reproducible model training, feature backfills, and debugging.

---

## Time Travel Example
```python
spark.read.format("delta") \
    .option("versionAsOf", 0) \
    .load("delta/features")
