# azure-databricks-adventureworks-pipeline
# Project Overview
This project showcases a complete **End-to-End Azure Data Engineering Pipeline** using the **AdventureWorks** dataset.
It involves **data ingestion from GitHub, transformation using Azure Databricks, layered storage architecture (Bronze, Silver, Gold), SQL Serverless Synapse queries**, and **Power BI dashboarding** for business insights.

# Technologies Used
Azure Data Lake Storage Gen2, 
Azure Data Factory (ADF), 
Azure Databricks (PySpark), 
Azure Synapse Analytics (SQL Serverless), 
Power BI, 
GitHub (for hosting source CSVs)

# Dataset
The AdventureWorks dataset includes:
**Calendar, Customers, Products, Sales (2015–2017), Returns, Territories, Product Categories, Subcategories**. All CSVs are uploaded under the Dataset folder.

# Project Architecture
Flowchart LR  
A --> [GitHub CSV Files] -->[Ingest with ADF]   
B --> [Azure Data Lake Storage - Bronze Layer]  
C --> |Transform with Databricks| [Silver Layer]  
D --> |Views in Synapse| [Gold Layer]  
E --> |Query with Power BI| [Interactive Dashboards]

# Pipeline Steps
**1. Data Ingestion**  
--> Created Azure Storage Account with Hierarchical Namespace enabled (Data Lake Gen2).  
--> Built two Linked Services in ADF:  
1. HTTP (GitHub Source)
2. ADLS Gen2 (Destination)

--> Assigned Storage Blob Data Contributor role to the Data Factory Managed Identity.  
Why? — This role allows ADF to read/write blobs and files inside Azure Storage securely.  

Created:  
--> Static pipelines (manual copy for POC — Proof of Concept)  
--> Dynamic pipeline (using ForEach, Lookup, and parameters)  
--> Used a git.json control file to dynamically pull multiple datasets.

**2. Bronze Layer (Raw Data)**  
--> Loaded raw CSV files from GitHub into the Bronze container.  
--> Previewed the data inside Azure Data Lake Bronze layer.  

**3. Silver Layer (Transformed Data)**  
--> Created a Databricks cluster and connected to Azure Data Lake using Service Principal.  
--> Assigned Storage Blob Data Contributor role to the Databricks App Registration.  
Why? — This role enables Databricks notebooks to access files inside Azure Data Lake for reading and writing transformed data.  

**Cleaned and transformed data:**  
--> Created new columns (fullName, formatted dates, etc.)  
--> Converted data to Parquet format for optimized storage and performance.  
--> Stored processed Parquet files into the Silver container.  

**4. Gold Layer (Business-Ready Data)**  
--> Connected Azure Synapse Analytics to Azure Data Lake (Silver Layer).  
--> Assigned Storage Blob Data Contributor role to the Synapse Managed Identity.  
Why? — Allows Synapse SQL serverless pools to query data directly from Azure Storage (ADLS) using OPENROWSET.  
--> Created views over Silver Parquet files inside Synapse (Gold Layer).  
--> Created External Tables using CETAS (CREATE EXTERNAL TABLE AS SELECT) for persistent storage in the Gold Layer.  

**5. Reporting and Visualization**  
--> Connected Power BI to Synapse SQL Serverless endpoint.  
--> Built an interactive Sales and Customer Insights Dashboard based on Gold Layer tables.  

# Final Dashboard 
Here is the final Power BI Dashboard created from the Gold Layer:  


# Key Learnings  
--> Building scalable, layered architectures on Azure.  
--> Automating ingestion and transformation pipelines dynamically.  
--> Leveraging PySpark on Databricks for large-scale transformations.  
--> Integrating Synapse Analytics with Power BI.  
--> Handling real-world datasets with structured and unstructured storage.

# Future Improvements  
--> Implement CI/CD using Azure DevOps Pipelines.  
--> Automate parameterized deployments across multiple environments.  
--> Introduce Data Quality checks and Monitoring.  
--> Add delta tables and optimize transformations further.  

🎉 Thank you for visiting this project repository!
