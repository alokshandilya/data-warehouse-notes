# Dimensions

## Dimension Tables

- for slicing and dicing data ("group" and "filter" data)
- always have a primary key
- use surrogate keys (integers) as primary keys, optionally we can keep the natural keys.
  - create a **lookup table** (table with only the natural key and the surrogate key).
  - _distinct values of the natural key are stored in the lookup table, with an auto-incremented integer as the surrogate key._
- create relation between fact table and dimension table using the surrogate key.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/162a1341-9614-4695-a22f-32dbd580250c">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/1a8e01cb-b577-4a43-a000-cebbac3c5850">
        </td>
    </tr>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/66341c04-92d3-402d-a351-feecd49e0d8d">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/d30e02dc-ad75-4e88-86da-199baf44e8db">
        </td>
    </tr>
</table>

## Date Dimension

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/a7ef5f8a-e2f8-4f65-9a5d-2f29ed95b56d">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/b4d5fb21-5943-4433-bb84-8a0446cfa0f3">
        </td>
    </tr>
</table>

## Nulls in Dimension Tables

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/8271270a-1670-4d4c-9229-f88ab32b900a">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/ecc7e68d-68d3-4aed-8e11-a6936ae1e0de">
        </td>
    </tr>
</table>

<p>
    <img src="https://github.com/user-attachments/assets/e578c555-7675-4898-9c10-2373d1d01445" width="75%">
</p>

## Hierarchies in Dimensions

Often times, dimensions have hierarchies (source data is in a normalized form, resulting in snowflake schema, which should be avoided due to bad performance and usability), which can be flattened in the data warehouse.

> normalized data saves disk space and good for write operations, but bad for read operations (which is needed in data warehousing, analytical data processing).

#### Flatten the hierarchy in the data warehouse. (Denormalize the data)

  <img src="https://github.com/user-attachments/assets/b7fb0ac2-fc1e-4402-b1d3-f33a6b1e9b93" width="75%">

  <img src="https://github.com/user-attachments/assets/10ecb8b1-c91e-4c85-86ec-31f9aa5d327f" width="75%">

> we can also combine attributes of different levels of hierarchy into a single column, if it's needed (user wants the combined data).
>
> <img src="https://github.com/user-attachments/assets/fcfc559b-232d-4d38-99d9-a8eac518ca9d" width="75%">

## Conformed Dimensions

A **Conformed Dimension** is a dimension that is **shared across multiple fact tables** in a data warehouse. It has the **same meaning, structure, and values** across different business processes.

- Dimension that is **shared by multiple fact tables/stars**.
  - conformed dimension is/are shared attributes across multiple fact tables.
- **used to compare facts across different fact tables in a report or analysis.**

<img src="https://github.com/user-attachments/assets/26548832-ff56-4464-9f91-96266137593b" width="75%">

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/8a4d8f13-60a7-4fed-b011-57b6cf78f8d5">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/fbb0b627-f886-47a2-b74f-e0e9cc35c36d">
        </td>
    </tr>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/e71c2010-46c8-4d49-b489-f014c9462713">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/bf63f44f-9a0a-4d85-a130-10014cb0ab75">
        </td>
    </tr>
</table>

**Why It’s Important?**

- Ensures **consistency** in reporting across different data marts.
- Enables **seamless integration** of data from multiple sources.
- Supports **Drill Across** analysis.

**Example**

Consider a retail business with two fact tables:

1. **Sales Fact Table** (tracks transactions)
2. **Inventory Fact Table** (tracks stock levels)

Both these fact tables share a **"Product" dimension** with the same attributes (Product ID, Name, Category, etc.). Since the "Product" dimension is structured the same way across both tables, it is a **Conformed Dimension**.

#### Drill Across

**Drill Across** is a technique used to **query multiple fact tables** simultaneously using a common conformed dimension. It allows users to analyze **different business metrics together** by joining fact tables based on shared dimensions.

**Why It’s Important?**

- Helps in analyzing **cross-functional** business performance.
- Allows users to correlate different metrics that belong to separate fact tables.

**Example**  
Suppose we have:

- **Sales Fact Table** (measures revenue, units sold)
- **Inventory Fact Table** (measures stock levels, reorder quantities)
- **Common Conformed Dimension: "Product"**

A Drill Across query could analyze:

- **Revenue vs. Stock Levels** → Identify products with high sales but low inventory.
- **Sales vs. Reorder Quantity** → Understand if reorder policies align with sales trends.

**How It Works in SQL?**

```sql
SELECT
    p.product_name,
    SUM(s.sales_amount) AS total_sales,
    SUM(i.stock_quantity) AS total_stock
FROM sales_fact s
JOIN product_dim p ON s.product_id = p.product_id
JOIN inventory_fact i ON i.product_id = p.product_id
GROUP BY p.product_name;
```

This retrieves **both sales and inventory data** using the shared **Product** dimension.

| Feature       | Conformed Dimension                                   | Drill Across                                                     |
| ------------- | ----------------------------------------------------- | ---------------------------------------------------------------- |
| Definition    | A dimension used across multiple fact tables          | Querying multiple fact tables using shared dimensions            |
| Purpose       | Ensures data consistency                              | Combines metrics from different business processes               |
| Example       | "Customer" used in both Sales and Support fact tables | Comparing Revenue (Sales Fact) and Stock Levels (Inventory Fact) |
| SQL Technique | Joins with dimension tables                           | Joins across multiple fact tables                                |

> - **Conformed Dimensions** ensure **data consistency** across fact tables.
> - **Drill Across** leverages conformed dimensions to **combine insights from multiple fact tables**.

## Degenerate Dimension

A **Degenerate Dimension (DD)** is a dimension that **exists in the fact table but does not have its own separate dimension table**. Instead, it is stored as an attribute in the fact table itself.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/4c6a54b9-6d0e-46a5-a532-bb601d6fdeb5">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/d1a1056f-1791-4482-8165-d0dd03cb16e3">
        </td>
    </tr>
</table>

## Junk Dimension

A **Junk Dimension** is a dimension table that combines **low-cardinality, miscellaneous attributes** that don’t fit into other dimensions. These attributes are usually flags, indicators, or descriptive fields that would otherwise clutter the fact table.

> we call it **Junk Dimension** usually only internally. Talking to business users we can refer to as _**"transactional indicator dimension"**_.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/d26c4ebf-e17d-4317-8b3e-d8b2e4be398f">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/dd678c24-8c22-4799-bd30-73a3de00ffcb">
        </td>
    </tr>
</table>

> we can also do grouping if needed.
>
> <img src="https://github.com/user-attachments/assets/5b6172c7-0787-4594-b31b-57db0cc1a197" width="50%">

> note that, **Junk Dimension** can have huge number of combinations, so it's better to keep it small.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/3605ad5e-afd7-45c9-882f-990e14fffb1a">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/760e4434-e03a-427b-b051-a30e6143433a">
        </td>
    </tr>
</table>

## Role-playing Dimension

A **Role-Playing Dimension** is a single dimension that is used multiple times in a fact table, playing different roles depending on the business context. This is common when the same type of information (e.g., Date, Employee, Location) is needed for different purposes.

- **Avoids Data Duplication:** Instead of creating multiple similar dimension tables, we reuse a single table with different aliases.

#### Example: Order Management System

Consider a **Sales Fact Table**, where we need to track different dates:

1. **Order Date** – When the order was placed.
2. **Shipping Date** – When the order was shipped.
3. **Delivery Date** – When the customer received the order.

Instead of creating three separate date dimension tables, we use **one Date Dimension** multiple times in the fact table.

**Date Dimension Table (`dim_date`)**  
| Date_ID | Date | Day_Name | Month | Year |  
|---------|----------|---------|-------|------|  
| 1 | 2025-03-10 | Monday | March | 2025 |  
| 2 | 2025-03-11 | Tuesday | March | 2025 |  
| 3 | 2025-03-12 | Wednesday | March | 2025 |

**Sales Fact Table (`fact_sales`)**  
| Order_ID | Order_Date_ID | Ship_Date_ID | Delivery_Date_ID | Sales_Amount |  
|----------|--------------|--------------|------------------|-------------|  
| 1001 | 1 | 2 | 3 | 200 |  
| 1002 | 2 | 3 | 3 | 350 |

Here, the **Date Dimension (dim_date)** is playing three different roles:

- **Order Date** (when the order was placed)
- **Shipping Date** (when the order was shipped)
- **Delivery Date** (when the order was received)

In SQL queries, we can **alias the Date Dimension** to differentiate its roles:

```sql
SELECT
    o.order_id,
    od.date AS order_date,
    sd.date AS ship_date,
    dd.date AS delivery_date,
    o.sales_amount
FROM fact_sales o
JOIN dim_date od ON o.order_date_id = od.date_id
JOIN dim_date sd ON o.ship_date_id = sd.date_id
JOIN dim_date dd ON o.delivery_date_id = dd.date_id;
```

#### Other Common Role-Playing Dimensions

- **Customer Dimension** → "Billing Customer" vs. "Shipping Customer"
- **Employee Dimension** → "Salesperson" vs. "Manager"
- **Location Dimension** → "Store Location" vs. "Warehouse Location"

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/72ae7224-a02d-4924-84ed-42932c782b92">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/deb9ff17-a546-4e41-aa6c-8567897453f0">
        </td>
    </tr>
</table>

- for analysis in SQL, we can use `JOIN` with alias.
  - can create additional views for each role.
- no duplication data but still it appears like a different dimension.
