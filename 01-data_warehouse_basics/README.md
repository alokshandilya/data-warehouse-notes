# Data Warehouse Basics

## Why do we need a Data Warehouse?

Data mainly used in company for 2 purposes:

1. **Operational**: Used for day-to-day operations, such as transactional data.
   - **OLTP (Online Transaction Processing)**
   - eg. _receive orders_, _react to complaints_, _fill up stock_, etc.
   - in short, _keep the wheel turning_
2. **Analytical**: Used for analysis, such as reporting, data mining, and machine learning.
   - **OLAP (Online Analytical Processing)**
   - eg. _find trends_, _predict future_, etc.
   - answer questions like _"What's the best category?"_, _"How many sales compared to last year?"_, _"What can be improved?"_, etc.
   - in short, _evaluate performance and make decisions, how to turn the wheel better_

> _Lack of Data Warehouse_ :ghost: in a company can lead to statements like:
>
> - _Yes, we have a lot of data, but we don't use it._
> - _Our data is very complicated and difficult to analyze._
> - _It's spread all over different systems and difficult to access._
> - _I just want to see what's relevant._
> - _We need to access data quick and easily._
> - _We want to make fact based decisions._

1. **OLTP**
   - usually, one record at a time
   - data input
   - usually, no long history
2. **OLAP**
   - thousands of records at a time
   - faster query performance
   - historical context
   - usability
   - **Data Warehouse** is there to address these analytical needs :star2:

## What is Data Warehouse?

> **A database used and optimized for analytical purposes.**
>
> Also, should be:
>
> - _user friendly_
> - _fast query performance_
> - _enabling data analysis_

<p>
   <img src="https://github.com/user-attachments/assets/97262f7e-4e71-4f49-bdc5-80a06a7f925d" width="75%">
</p>

#### Goal of Data Warehouse:

- _centralized and consistent location for data_
- _data must be accessible fast (query performance)_
- _data must be usable (user friendly)_
- _must load data consistently and repeatedly **(ETL)**_
- _reporting and data visualization on top_

## What is Business Intelligence?

> **we create data warehouse for Business Intelligence (BI)**

<p>
    <img src="https://github.com/user-attachments/assets/0bb217a6-4e95-467b-828b-ab1121f71187" width="75%">
</p>

**Business Intelligence (BI)** refers to the process of collecting, analyzing, and transforming raw data into actionable insights to support business decisions. It involves tools (e.g., dashboards, reports) and practices (e.g., data mining, visualization) to identify trends, optimize operations, and improve strategic outcomes.

**Key elements**:

- **Data collection** (sales, customer behavior, etc.).
- **Analysis** (spotting patterns, forecasting).
- **Visualization** (charts, graphs, dashboards).

**Goal**: Turn data into **actionable insights** to improve efficiency, profitability, and competitiveness.

**Example**: A retail company uses BI to track sales trends, manage inventory, and personalize marketing campaigns.

## Data Warehouse vs. Data Lake

> both used as centralized data storage

<p>
    <img src="https://github.com/user-attachments/assets/189ea474-2717-49a1-b111-031cbd5657e6" width="75%">
</p>

| **Aspect**     | **Data Warehouse**                             | **Data Lake**                                      |
| -------------- | ---------------------------------------------- | -------------------------------------------------- |
| **Structure**  | _Structured_ (tables, schemas).                | _Raw_ (structured, semi-structured, unstructured). |
| **Data Type**  | Cleaned, processed, ready for analysis.        | Raw, unprocessed (logs, JSON, videos, etc.).       |
| **Processing** | _Schema-on-write_ (structured before storage). | _Schema-on-read_ (structure applied when used).    |
| **Storage**    | Optimized for _query performance_.             | Optimized for _scalability and cost_.              |
| **Use Case**   | Business intelligence, reporting, dashboards.  | Machine learning, exploratory analysis, big data.  |
| **Tools**      | Redshift, Snowflake, BigQuery.                 | Hadoop, AWS S3, Databricks.                        |

- **Warehouse**: _Structured data_ for _fast queries_ (e.g., sales reports).
- **Lake**: _Raw data_ for _flexibility_ (e.g., AI experiments).

#### When to use which?

- **Warehouse**: Structured data → actionable insights.
- **Lake**: Raw data → future innovation (but risks becoming a "data swamp" without governance).

> - They are not mutually exclusive, _can be used together_
> - Modern systems often combine both (**data lakehouse**, eg: Databricks).
