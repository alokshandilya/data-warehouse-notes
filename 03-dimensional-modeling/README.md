# Dimensional Modeling

## What is Dimensional Modeling?

- method of organizing data (in a data warehouse)
- all data is organized in either **facts** or **dimensions**
- **facts**: numeric values that can be aggregated
  - are the _measurements of the business_
  - eg. sales, cost, quantity, profit
- **dimensions**: descriptive information related to the facts
  - _additional context for those measurements_
  - eg. time, location, category, period
- with dimesion we can give additional context to the facts
- we can create meaningful insight like:
  - **Profit by year** _(word `by` usually indicates year is a dimension)_
  - **Profit by category** _(category is a dimension)_

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/83076a3d-7502-4e60-a39c-844abc9ef4e6">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/80a70346-6c2c-4a6f-b67a-b4f7f18bc1b9">
        </td>
    </tr>
</table>

## Why Dimensional Modeling?

<p>
    <img src="https://github.com/user-attachments/assets/96185355-b185-4700-8fcb-0c9c4db52529" width="75%">
</p>

## Facts

- **Facts** are the _measurements_ or _metrics_ of the business
- **Grain**: level of detail in a fact table
  - The grain of a fact table defines the most granular level of detail captured by the facts. It determines what each row in the fact table represents in terms of the business process being modeled.
  - It's about the level of atomicity of the data.
  - Defining the grain is crucial for designing an effective dimensional model. It ensures consistency and clarity in the data.
- A well-defined grain enables accurate analysis and reporting.
  - It helps prevent "mixed granularity" issues, where a fact table contains records representing different levels of detail, leading to inaccurate results.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/dbee8b29-0b01-4dbe-9222-95178040165d">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/843258ba-3034-4e56-a0ad-3a6234d253a7">
        </td>
    </tr>
</table>

> ### How to identify a fact?
>
> knowing fact and dimension sometimes may not be straightforward.
>
> usually facts are:
>
> - aggregatable (numeric values)
> - measurable (unlike dimension which is mostly descriptive)
> - event or transactional data
> - date/time in a fact table

## Dimensions

- **Dimensions** are the _descriptive information_ related to the facts
- categorizes facts
- filtering, grouping, and labeling facts
- are usually:
  - non-aggregatable
  - descriptive (facts are measurable)
  - (more) static eg. product name, product category etc.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/b13cce12-5adc-407c-8a8d-f65929a0e00c">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/0e615c2d-2244-4e15-b22e-7b33fdcce6da">
        </td>
    </tr>
</table>

## Star Schema

Both **Star Schema** and **Snowflake Schema** are used in data warehouses for organizing data efficiently for reporting and analytics. However, they differ in structure, performance, and complexity.

### **1. Star Schema ⭐**

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/ffdd55d4-3998-4aab-8ea6-cc496f154774">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/9cbafd05-663e-4cf7-be3b-c319d2fb9c4a">
        </td>
    </tr>
</table>

A **Star Schema** is a **denormalized structure** where the fact table is connected to **directly related dimension tables**. The schema forms a **star-like shape** with a central fact table surrounded by dimension tables.

#### **Example Structure**

```
                      +------------------+
                      |  Date Dimension  |
                      +------------------+
                               |
                               |
+------------------+    +-----------------+    +------------------+
| Product Dim     |----|  Fact Table      |----| Customer Dim     |
+------------------+    +-----------------+    +------------------+
                               |
                               |
                      +------------------+
                      | Region Dimension  |
                      +------------------+
```

#### **Characteristics**

- **Denormalized Dimensions** – Data is stored **without normalization** (repeating data).
- **Simple & Faster Queries** – Fewer joins lead to better **performance** for OLAP queries.
- **More Storage Required** – Since data is duplicated across dimensions.
- **Best for BI & Reporting** – Works well with **Tableau, Power BI, Looker**, etc.

### 2. Snowflake Schema ❄️

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/86de3d62-311d-4b73-916b-0d115d596283">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/862e1655-a804-4f75-881d-0918ab36b2a8">
        </td>
    </tr>
</table>

A **Snowflake Schema** is a **normalized** version of the Star Schema where **dimension tables are further divided** into multiple related tables. This removes redundancy and improves **storage efficiency**.

<p>
    <img src="https://github.com/user-attachments/assets/3dcc5c47-dcae-468b-93bd-0d0022814b7b" width="75%">
</p>

#### **Example Structure**

```
                      +------------------+
                      |   Date Dimension |
                      +------------------+
                               |
                               |
+------------------+    +-----------------+    +------------------+
| Product Dim     |----|  Fact Table      |----| Customer Dim     |
+------------------+    +-----------------+    +------------------+
       |                                      |
       |                                      |
+------------------+                  +------------------+
| Category Dim     |                  | Region Dim       |
+------------------+                  +------------------+
```

#### **Characteristics**

- **Normalized Dimensions** – Reduces data redundancy by **splitting dimension tables**.
- **Efficient Storage** – Saves space by **avoiding duplicate data**.
- **Complex Queries** – More **joins** required, making queries **slower**.
- **Better for Large Warehouses** – Used when handling **huge datasets** with frequent updates.

### **Key Differences: Star Schema vs. Snowflake Schema**

| Feature               | Star Schema ⭐        | Snowflake Schema ❄️  |
| --------------------- | --------------------- | -------------------- |
| **Complexity**        | Simple                | Complex              |
| **Normalization**     | Denormalized          | Normalized           |
| **Query Performance** | Faster (Fewer Joins)  | Slower (More Joins)  |
| **Storage Usage**     | More (Redundant Data) | Less (Optimized)     |
| **Maintenance**       | Easier (Fewer Tables) | Harder (More Tables) |
| **Use Case**          | BI, Reporting         | Large Warehouses     |

### **Which One to Use?**

- Use **Star Schema** when **query performance is a priority** and storage is not a concern.
- Use **Snowflake Schema** when you need **efficient storage and frequent data updates**.
