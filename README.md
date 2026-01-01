# Feature Store and Data Versioning with Delta Lake

## Technologies
- PySpark
- Delta Lake
- Python
- Google Colab

## Project Description
This project demonstrates how to build a simple feature store using Delta Lake.
Raw data is ingested into an immutable Delta table, features are computed using Spark,
and feature tables are versioned using Delta Lake’s transaction log.

## Architecture
data/raw        → Source CSV data  
delta/raw_table → Immutable raw Delta table  
delta/features  → Versioned feature store  

## Key Features
- Explicit schema ingestion
- ACID-compliant Delta tables
- Feature evolution with versioning
- Delta Lake time travel for historical feature access

## Time Travel Example
```python
spark.read.format("delta").option("versionAsOf", 0).load("delta/features")
