# LoadBookingFact Pipeline

The **LoadBookingFact** pipeline performs several data transformations to update the **Booking Fact Table** in **Azure Synapse**. It uses data from **CosmosDB** and **Synapse SQL Pool** for enriching and updating booking data.

![LoadBookingFact](../assets/images/load_booking_fact_pipeline.png)

## **Pipeline Activities**

### **1. Data Flow Activity** (DatasetBookingFactTransformation)

This activity uses **Azure Data Factory Data Flow** to transform the raw booking data. It performs several transformations and enrichment on the data before it is written to the **Booking Fact Table** in **Azure Synapse**.

#### **Transformations**

![BookingFactDataflow](../assets/images/booking_data_dataflow.png)

1. **Data Quality Check**: Ensures that `check_out_date` is later than `check_in_date`.
2. **Derived Column**: Calculates `stay_duration`, extracts `booking_year`, `booking_month`, and forms a `full_address`.
3. **Join with Synapse**: Performs a lookup between incoming data and existing records in Synapse to identify new or updated bookings.
4. **Alter Row Policies**: Determines whether to insert or update a row based on whether `booking_id` already exists in Synapse.
5. **Cast Columns**: Ensures that the data types match the target schema in **Synapse SQL Pool**.
6. **Final Columns**: Selects the final set of columns to be written to the **Booking Fact Table** in **Synapse SQL Pool**.

### **2. Stored Procedure Activity** (SPAggregateData)

After the data flow activity completes successfully, the **SPAggregateData** activity calls a stored procedure to aggregate the transformed booking data. This stored procedure updates the **BookingCustomerAggregation** table, which contains aggregated data at the customer level.

#### **Stored Procedure Execution**:

- **Stored Procedure Name**: `[airbnb].[BookingAggregation]`
- **Linked Service**: Azure Synapse Analytics
- **Dependency**: This activity depends on the successful completion of the previous data flow activity.
- **Timeout and Retry**: The activity has a timeout of 12 hours and is set to retry 0 times if it fails.

This activity ensures that the aggregated data in the **BookingCustomerAggregation** table is updated after the detailed booking data has been processed.

### **Pipeline Configuration**

The configuration for this pipeline is located in the **[LoadBookingFact.json](./pipelines/LoadBookingFact.json)**.
