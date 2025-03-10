## Data Warehouse Architecture

## Layers of Data Warehouse

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/f1c96757-f938-4a96-8e62-132146527ab0">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/10fb54fb-637b-4cb6-b8ae-729fae98a435">
        </td>
    </tr>
</table>

- **Source Layer:**
  - This is where the raw data originates. It includes various sources like transactional databases, CRM systems, ERP systems, and external data feeds.
  - The data in this layer is typically in its original format.
- **Staging Layer:**
  - This layer acts as an intermediary area where data is extracted from the source systems.
  - It's used for data cleansing, validation, and basic transformations before the data is loaded into the core data warehouse.
  - This layer helps to ensure data quality and consistency.
- **Storage Layer:**
  - This is the core of the data warehouse, where the transformed and integrated data is stored.
  - Data is typically organized into data marts or a central data warehouse, optimized for analytical queries.
  - This layer focuses on providing a stable and efficient environment for data storage and retrieval.
- **Presentation Layer:**
  - This is the layer where users access the data for reporting and analysis.
  - It includes business intelligence (BI) tools, reporting software, and other applications that allow users to query and visualize the data.
  - The goal is to provide users with easy access to the information they need to make informed decisions.

#### Key points to remember:

- The flow of data through these layers is often managed by **ETL (Extract, Transform, Load)** or **ELT (Extract, Load, Transform)** processes.
- _Modern cloud-based data warehouses have increased flexibility in how these layers are implemented._
- _Data governance is very important within all of the layers._

## Staging Layer

#### Why need a Staging Layer?

- short time on the source systems
- quickly extract data from multiple sources
- move data(csv, NoSQL etc.) to relational database
- start transformations from there

> In a data warehouse, the **staging layer** is an intermediate storage area where raw data is temporarily stored before processing and loading into the data warehouse. There are mainly two types of staging layers:

1. **Transient (or Temporary) Staging Layer (TSL)**
   - Stores data only for a short period, usually until the ETL job completes.
   - Data is removed after processing (append or truncate-and-load strategy).
   - Reduces storage costs and complexity.
   - Best for real-time or batch processing where historical raw data retention is unnecessary.
   - _Example_: A table where daily transaction logs are loaded and processed before moving to the warehouse.
2. **Persistent Staging Layer (PSL)**
   - Stores data for a longer duration (days, weeks, or permanently).
   - Allows historical tracking and reprocessing of data in case of failures or changes in business rules.
   - Useful for compliance and auditing.
   - Requires more storage and maintenance.
   - _Example_: A table that holds all raw incoming data before ETL transformations.
