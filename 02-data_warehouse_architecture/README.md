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

## Data Marts

<p>
    <img src="https://github.com/user-attachments/assets/0fca9965-2d05-4bf8-a14d-b05c52fb9ac4" height=275>
</p>

> A **Data Mart** is a **subset** of a data warehouse that is focused on a specific business function, department, or subject area. It provides **filtered and structured** data tailored for a particular group of users, improving query performance and ease of access.

- subset of data warhouse
- dimensional model (fact table in the center, surrounded by dimension tables)
- can be further aggregated
- increases usability and acceptance
- can increase performance (_in-memory database like Power BI, in some cases dimensional cubes_)
- use cases:
  - tools, departments, regions, products, customers, etc.

## Relational Database vs. In-Memory Database

### 1. Relational Database (RDBMS)

> A **Relational Database Management System (RDBMS)** stores data in tables with predefined schemas, enforcing relationships using primary and foreign keys. Data is typically stored **on disk** and retrieved using SQL queries.

#### Characteristics:

- Data is stored **persistently** on disk.
- Uses **indexes and caching** to optimize query performance.
- ACID (**Atomicity, Consistency, Isolation, Durability**) compliant.
- Supports **large-scale transactional processing**.
- Can handle **OLTP (Online Transaction Processing) and OLAP (Online Analytical Processing)** workloads.
- examples:
  - PostgreSQL, MySQL, SQL Server, Oracle Database etc

#### Use Cases:

- Banking and finance (transactional consistency).
- E-commerce applications.
- Enterprise resource planning (ERP) systems.

### 2. In-Memory Database (IMDB)

> An **In-Memory Database (IMDB)** stores **all** data in **RAM (memory)** instead of disk, leading to significantly faster read/write speeds. Some IMDBs persist data to disk asynchronously to ensure durability.

#### Characteristics:

- Data is stored **entirely in RAM**, making it much **faster** than traditional RDBMS.
- Can still be **ACID-compliant**, but with trade-offs for performance.
- Some IMDBs offer **hybrid storage** (memory + disk persistence).
- Best for **high-speed transactions** and **real-time analytics**.
- examples:
  - Redis, Memcached, SAP HANA, Apache Ignite, Oracle in-memory, Amazon MemoryDB

#### Use Cases:

- Real-time analytics & big data processing.
- Low-latency applications (e.g., gaming, ad tech, financial trading).
- Session management & caching for web applications.
