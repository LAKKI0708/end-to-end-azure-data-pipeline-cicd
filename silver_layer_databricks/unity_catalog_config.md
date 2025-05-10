# Unity Catalog Configuration

This document outlines how Unity Catalog was configured and used to securely manage data access and metadata for the Azure Databricks environment in this project.

---

## Unity Catalog Setup

- Workspace: Azure Databricks
- Metastore Name: `blinkitmetastore`
- Storage Root Path: `abfss://metastore@<your-storage-account>.dfs.core.windows.net/`
- Region: <your-region> (e.g., East US)
- Assigned to Workspace: Yes, through the Azure Portal

---

## 🔐 Secure Data Access via Azure Access Connector

- Access Method: Used Azure Access Connector for Azure Databricks** to securely connect to **Azure Data Lake Storage Gen2 (ADLS Gen2).
- Why: Avoids storing credentials or using access keys manually.
- How:
  - Access Connector was enabled under the Databricks workspace in the Azure Portal.
  - The connector was assigned the `Storage Blob Data Contributor` role at the storage account level.
  - Unity Catalog used this identity to read/write data in `bronze/`, `silver/`, and `gold/` folders.

---


###Catalog: `blinkit`

#### Schema: `bronze`
- Contains raw ingested data in **Parquet format**
- Written by ADF pipeline

#### Schema: `silver`
- Contains cleaned and transformed tables
- Processed using PySpark notebooks in Databricks

#### Schema: `gold`
- Contains aggregated and summary tables
- Generated using **Delta Live Tables (DLT)**

---

## Configuration Steps

1. Enable Unity Catalog
   - Set up from Azure Portal and assigned to Databricks workspace

2. Create Metastore
   - Backed by an ADLS Gen2 container (used as the root path)

3. Assign Metastore to Workspace
   - Applied via workspace settings in the Azure Portal

4. Create Catalog and Schemas
   - `blinkit_catalog` → `bronze`, `silver`, `gold`

5. Grant Permissions
   - Schema/table-level permissions were granted via Unity Catalog ACLs to notebooks and users

---

