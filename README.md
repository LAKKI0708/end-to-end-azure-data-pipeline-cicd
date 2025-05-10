# Azure Data Engineering Project - Blinkit Sales Pipeline

#Overview:

This project demonstrates an end-to-end data engineering pipeline on Azure using Blinkit sales data (6 CSV files). The goal was to ingest, transform, and visualize data across Bronze, Silver, and Gold layers using the following tools:

- Azure DevOps
- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure Databricks (Unity Catalog + Data Transformation + Delta Live Tables)
- Power BI
---

Architecture:

<p align="center">
  <img src="https://github.com/user-attachments/assets/0186f171-67da-435d-a3cd-95d39fbf1eef" width="65%">
</p>

---

#Layer-wise Breakdown:

Bronze Layer (Raw Ingestion):
- Tool: Azure Data Factory (ADF)
- Description: Ingested raw CSV files from source into the Bronze folder in ADLS in **Parquet format**
- Deployment: Used Azure DevOps to generate and deploy ARM templates to ADF
- Output: Raw Parquet files stored in `bronze/` folder of ADLS

Silver Layer (Transformed Data):
- Tool: Azure Databricks + Unity Catalog
- Description:
  - Created Unity Catalog & metastore connected to ADLS folder using Azure AccessConnector for Databricks
  - Used PySpark notebooks to clean and transform Bronze data
  - Stored schema in Unity Catalog, and data as Delta Tables in `silver/` folder
- Output: Cleaned, well-structured tables in Delta format

Gold Layer (Aggregated Data):
-Tool: Delta Live Tables (DLT)
- Description:
  - Created a DLT pipeline to build business-level summary and insightful tables from Silver data
  - Output tables saved in Unity Catalog and `gold/` folder in ADLS
- Output: Insightful Delta tables

Visualization Layer:
- Tool: Power BI Desktop
- Description:
  - Connected to Unity Catalog using Power BI configuration file (.pbids)
  - Created dashboards and visualizations using Gold layer data

---

#Tech Stack:

| Tool               | Purpose                                      |
|--------------------|----------------------------------------------|
| Azure DevOps       | CI/CD & ARM template publishing              |
| Azure Data Factory | Ingest data into Bronze layer using pipelines|
| ADLS Gen2          | Data Lake storage for all 3 layers           |
| Databricks         | Data transformation,Delta Lake,Unity Catalog |
| Delta Live Tables  | Pipelines for Gold layer                     |
| Power BI           | Business intelligence & reporting            |

---

#Repository Contents:

| Folder                 | Contents                                      |
|------------------------|-----------------------------------------------|
| `bronze_layer_adf/`    | ARM templates and ADF pipeline descriptions   |
| `silver_layer`         | PySpark notebooks for transformation,catalog info |
| `gold_layer_dlt/`      | DLT pipeline notebooks and config             |
| `power_bi/`            | Power BI config and screenshots               |
| `data source/`         | Sample CSV or data snippet                    |

---








