# Azure Data Factory (ADF) Assets Description

This document explains the key components used in the Azure Data Factory (ADF) pipeline for ingesting Blinkit CSV data into the Bronze layer of the data lake.

---

## Linked Services

1. DataLakeLS
- Type: AzureBlobFS
- Purpose: Connects ADF to Azure Data Lake Storage Gen2
- Authentication: Secure string (account key via parameter)
- URL: `https://blinkitdl.dfs.core.windows.net/` *(redact if needed)*

### 2. GitLS
- Type: HTTP Server
- Purpose: Fetches data and parameter JSON from a public GitHub repository
- Authentication: Anonymous
- URL: `https://raw.githubusercontent.com` (parameterized)

---

## Datasets

1. paramsgit
- **Type**: JSON
- **Source**: GitHub
- **Purpose**: Provides a list of files (with relative URLs) to be ingested by the pipeline

2. rawdatagit
- Type: Delimited Text (CSV)
- Source: GitHub (dynamic via parameters)
- Purpose: Reads individual CSV files listed in `paramsgit`

3. BronzeDataLake
- Type: Parquet
- Sink: Azure Data Lake Gen2 (`bronze` file system)
- Purpose: Stores transformed CSV data in Parquet format using dynamic folder and file parameters

---

## Pipeline: `GitToBronze`

### Overview:
This pipeline performs raw data ingestion from GitHub into the Bronze layer of ADLS using Parquet format.

### Activities:
1. Lookup1
   - Reads the `paramsgit` JSON file from GitHub to get the list of CSV files and their metadata.

2. ForEachCSV
   - Iterates over each file from `Lookup1`.
   - Uses a nested **Copy Activity** (`GitToBronzeCopy`) to:
     - Source: Read CSV file from GitHub.
     - Sink: Write to ADLS in Parquet format using the `BronzeDataLake` dataset.
     - Features: Tabular translation, format conversion, schema-flexible ingestion.

---

## Parameters

| Parameter Name                             | Description                             |
|--------------------------------------------|-----------------------------------------|
| `DataLakeLS_accountKey`                    | Secure string for ADLS authentication   |
| `DataLakeLS_properties_typeProperties_url` | ADLS URL for Bronze file system         |
| `GitLS_properties_typeProperties_url`      | Base GitHub URL for source files        |

---

## Output
- All files are written in Parquet format under the `bronze/` folder of Azure Data Lake Storage.

---

*Note: All secure information has been parameterized for safe public sharing.*
