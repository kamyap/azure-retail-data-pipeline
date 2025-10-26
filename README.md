**Retail Data Engineering on Azure**

This project implements an end-to-end data pipeline on Azure using the Medallion Architecture (Bronze → Silver → Gold → Consumption).


🔹 **Databricks Notebooks – Bronze / Silver / Gold**

Bronze: Ingest raw CSVs into ADLS Bronze container.

Notebook: 01-bronze/bronze.ipynb

Silver: Clean and transform data (remove nulls/duplicates, format columns).

Notebook: 02-silver/silver.ipynb

Gold: Aggregate sales data by country .

Notebook: 03-gold/gold.ipynb

 -->Notebooks are provided so the pipeline can be rerun directly in Databricks.

 

🔹 **ADF Pipeline – Bronze → Silver → Gold → Synapse**

   Single pipeline orchestrates the entire flow:
   
   Raw CSV → Bronze → Silver → Gold → Synapse SQL Pool
   
   Includes linked services for ADLS and Synapse.
   
   Pipeline JSON included for redeployment: pipelines/adf_pipeline.json

   

Pipeline Canvas Screenshot:

<img width="1268" height="397" alt="ADF_Pipeline_Canvas" src="https://github.com/user-attachments/assets/caa3ec7d-0e6b-49ec-8aaa-5ae8feeccd87" />

Figure 2: End-to-end pipeline in ADF



🔹 **Consumption & Analytics**
   Data copied from Gold ADLS → Synapse Dedicated SQL Pool using ADF.
   
   <img width="413" height="557" alt="Screenshot 2025-10-03 211339" src="https://github.com/user-attachments/assets/07f2e99b-8cc9-435d-9823-ee3cc5fce043" />
   
   Analytics performed in Synapse Studio.
   
   Sales by Country Chart:
   
   Queryed directly from Gold table (dbo.Sales_Cleaned)
   
     **SELECT Country, Total_Sales
     FROM dbo.Sales_Cleaned
     ORDER BY Total_Sales DESC;**
   
   
   Visual: Column / Bar chart
   
   Screenshot: 04-consumption/synapse_chart.jpeg
   
   ⚠ Note: Only the Sales by Country chart is included in this project because the Gold table already contains aggregated country-level data. 
   
   🔹 **Storage**
   🔹 **Tools & Services Used**
   
   1.Data Ingestion & Orchestration: Azure Data Factory
   
   2.Storage: ADLS Gen2 (Bronze, Silver, Gold)
   
   3.Transformation: Azure Databricks (PySpark)
   
   4.Data Warehouse: Azure Synapse Dedicated SQL Pool
   
   5.Visualization: Synapse Studio (Charts)



🔹 **Key Learnings**

   1.Implemented Medallion Architecture for scalable pipelines
   
   2.Orchestrated end-to-end pipeline using a single ADF pipeline
   
   3.Learned cost-saving strategies (pausing SQL pool)
   
   4.Created basic analytics directly on curated data in Synapse Studio

   

🔹 Enhancements & Future Work
  
   **Enhancements Implemented:**
  
  1. **Parameterized Gold Dataset Folder**
     - Added **dataset-level parameter `folder_name`** in the Gold dataset.
     - Allows dynamic selection of the folder inside the Gold container for Copy Data activity.
       
  2. **Pipeline-Level Parameter `goldFolder`**
     - Added **pipeline-level parameter** for flexible folder naming at pipeline run time.
     - Can be used to dynamically feed the dataset parameter (`folder_name`).
       
  3. **Log Analytics Integration for Monitoring**
     - ADF pipeline diagnostic logs are sent to Log Analytics.
     - Users can track **pipeline runs, activity status, start/end times**, and troubleshoot failures.
    
  4. **Documentation of Logs & Queries**
     - KQL queries saved in `/Log_Analytics/KQL_Queries.txt`.
     - Pipeline run screenshot saved in `/Log_Analytics` to showcase result of the KQLquery.
  
  **Potential Future Enhancements:**
  - Add automated **alerting for failed activities** in ADF via Log Analytics.
  - Parameterize **Silver dataset folder** for additional flexibility.
  - Extend **Databricks visualizations** to include monthly trends, top-selling products, and more KPIs.

