# AirBnB CDC Ingestion Pipeline

This project implements a near real-time data ingestion and transformation pipeline for AirBnB using Azure Data Factory, Synapse Analytics, CosmosDB, and other Azure technologies. It ensures efficient data processing and automated updates to maintain an up-to-date data warehouse.

## **Tech Stack**

- **Azure Data Factory (ADF)**: Orchestration of pipelines for data movement and transformations.
- **Azure Data Lake Storage (ADLS)**: Storage for raw and intermediate data.
- **Azure Synapse Analytics**: Data warehouse for analytical queries.
- **CosmosDB**: Source of change data for Bookings events.
- **Python**: Custom data generation scripts
- **SQL**: Database and transformation logic.

---

## **Pipeline Features**

1. **Hourly SCD-1 Updates**:
   - Reads customer data from ADLS hourly.
   - Performs Slowly Changing Dimension Type 1 (SCD-1) updates on the `customer_dim` table in Synapse Analytics.

2. **Change Data Capture (CDC)**:
   - Captures incremental booking events from CosmosDB using change feeds.
   - Processes events in Azure Data Factory and upserts transformed data into Synapse.

3. **Automated Workflows**:
   - Configured triggers and dependencies in ADF to enable seamless end-to-end automation.
