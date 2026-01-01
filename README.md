# Feature Store and Data Versioning with Delta Lake

## Overview
This project demonstrates how to design and implement a simple, production-style **feature store** using **Delta Lake** on top of **Apache Spark**. It focuses on reliable data ingestion, consistent feature computation, and robust feature versioning to support reproducible machine learning workflows.

The project follows modern data engineering best practices by separating raw data storage from derived feature storage and leveraging Delta Lake’s transaction log for version control and time travel.

---

## Technologies Used
- **PySpark** – Distributed data processing and scalable feature computation  
- **Delta Lake** – ACID transactions, schema enforcement, data versioning, and time travel  
- **Python** – Pipeline orchestration and feature engineering logic  
- **Google Colab** – Interactive execution environment  

---

## Project Description
Raw event or transactional data is first ingested into an **immutable Delta table**, ensuring data reliability, auditability, and schema consistency. This raw layer acts as the single source of truth and is never modified once written.

Using Spark transformations and aggregations, meaningful **user-level features** are computed from the raw data. These engineered features are stored in a dedicated Delta table that acts as the **feature store**.

Each update to the feature logic automatically creates a new version of the feature table. Delta Lake preserves all previous versions, enabling historical feature access through **time travel**. This is critical for reproducible model training, feature backfills, and debugging.

---

## Architecture

### `data/raw`
Contains raw CSV files that simulate source system data such as user events or transactions. This represents the original input data before any processing.

### `delta/raw_table`
Stores raw data in Delta format as an immutable table. This layer enforces schema consistency, supports ACID transactions, and serves as the authoritative source for downstream feature computation.

### `delta/features`
Acts as the feature store. Derived features are written to this Delta table. Each overwrite or update creates a new version, allowing safe feature evolution over time.

---

## Key Features

### Explicit Schema Ingestion
Raw CSV data is ingested using a predefined schema. This avoids schema inference issues, ensures consistent data types, and guarantees reproducible pipelines.

### ACID-Compliant Storage
Delta Lake provides full ACID guarantees, ensuring reliable reads and writes even when multiple updates or feature changes occur.

### Feature Versioning and Evolution
When feature logic changes, the feature table can be safely overwritten. Delta Lake automatically versions the table while preserving all historical states.

### Time Travel Support
Historical versions of the feature store can be queried using version numbers. This enables reproducible experiments, auditing, debugging, and feature backfills.

---

## Time Travel Example
```python
spark.read.format("delta") \
    .option("versionAsOf", 0) \
    .load("delta/features")
