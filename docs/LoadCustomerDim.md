# LoadCustomerDim Pipeline (CDC for Customer Data)

The **LoadCustomerDim** pipeline performs Change Data Capture (CDC) on customer data. It reads customer data from **Azure Data Lake Storage (ADLS)** and applies transformations before loading it into the **Customer Dimension Table** in **Azure Synapse**.

![LoadCustomerDim](../assets/images/load_customer_dim_pipeline.png)

## **Pipeline Configuration**

### **Activity 1: Get MetadataOfEachFileInCustomerRawDataContainer**

- Retrieves metadata for each file in the customer raw data container.

### **Activity 2: ForEachFileInCustomerRawContainer**

- Iterates through each file in the raw data container and performs the following steps:
    1. **CopyFileDataToSynapseSQLPool**: Copies the raw customer data from the source (DelimitedText format) to the **SynapseSQLPoolCustomerDimTable** in Synapse using an **Upsert** operation based on the `customer_id` key.
    2. **CopyFileDataToArchiveContainer**: Archives the raw customer data files to a separate storage container after they have been successfully loaded into Synapse.
    3. **DeleteRawDataFile**: Deletes the raw customer data files from the source after they have been archived.

You can find the configuration of this pipeline in the file [LoadCustomerDimDataToSynapse.json](../pipelines/LoadCustomerDimDataToSynapse.json).

## **Conclusion**

The **LoadCustomerDim** pipeline ensures that customer data is accurately updated in the customer dimension table and provides mechanisms to manage raw data files for future reference.
