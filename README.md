# Azure End-to-End Data Engineering Pipeline
## Medallion Architecture on Azure Synapse Analytics

## Project Overview
This project demonstrates a production-grade data engineering pipeline built 
on Microsoft Azure, implementing the Medallion Architecture pattern to process, 
transform, and serve data across three layers — Bronze, Silver, and Gold.

The pipeline ingests raw data from GitHub, applies progressive transformations 
through each layer, and exposes the final Gold layer as queryable SQL tables 
accessible to business users via SSMS and Power BI.

---

## Architecture
![Architecture Diagram](architecture/architecture_diagram.png)

---

## Tech Stack
| Tool | Purpose |
|---|---|
| Azure Data Factory (ADF) | Data ingestion and orchestration |
| ADF Data Flows | Data transformation (Bronze → Silver) |
| Azure Data Lake Storage Gen2 | Storage for all three layers |
| Azure Synapse Analytics | Unified analytics platform |
| Synapse Notebooks (PySpark) | Data transformation (Silver → Gold) |
| Delta Lake | Storage format for Gold layer |
| Serverless SQL Pool | Exposing Gold layer as SQL tables |
| Microsoft Entra ID | Authentication and access control |
| Azure RBAC | Role-based access management |

---

## Pipeline Layers

### Bronze Layer — Raw Ingestion
- **Source:** GitHub dataset
- **Tool:** Azure Data Factory Pipeline
- **Process:** Raw data ingested as-is with no transformations
- **Storage:** ADLS Gen2 raw container
- **Format:** CSV / original source format

### Silver Layer — Cleaned and Transformed
- **Tool:** ADF Data Flows
- **Transformations applied:**
  - Null value handling
  - Data type casting
  - Column renaming and standardization
  - Duplicate removal
- **Storage:** ADLS Gen2 silver container
- **Format:** Parquet

### Gold Layer — Business Ready
- **Tool:** Synapse Notebooks (PySpark)
- **Transformations applied:**
  - Business-level aggregations
  - Final data modeling
  - KPI calculations
- **Storage:** ADLS Gen2 gold container
- **Format:** Delta tables (ACID compliant)

---

## Data Access Layer
- Gold Delta tables exposed via **Serverless SQL Pool**
- External tables created over Delta files — zero data movement
- Users query via standard SQL — no knowledge of underlying storage needed
- Access managed through **Microsoft Entra ID guest accounts**
- **Group-based RBAC** — single group controls access for all users
- Compatible with SSMS, Power BI, Azure Data Studio, Excel

---

## Key Concepts Demonstrated
- Medallion Architecture (Bronze / Silver / Gold)
- Delta Lake for ACID-compliant storage
- Serverless SQL for cost-effective querying (pay per query)
- Managed Identity for secure, passwordless authentication
- External tables vs Views in Serverless SQL Pool
- Azure AD B2B guest access for external users
- Schema-level permission management

---

## Project Structure
pipeline/          → ADF ingestion pipelines
dataflow/          → ADF data flow transformations  
notebook/          → Synapse PySpark notebooks
sqlscript/         → SQL scripts for database setup and access
architecture/      → Architecture diagrams

---

## Results
- Raw GitHub data successfully ingested into Bronze layer
- Data cleaned and standardized in Silver layer
- Business-ready Delta tables created in Gold layer
- External users (Gmail) able to query Gold tables via SSMS
- Group-based access control implemented for scalable user management
