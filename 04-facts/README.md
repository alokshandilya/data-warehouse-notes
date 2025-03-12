# Facts

## Additivity

Think of it as: "Does it make logical sense to add these numbers together?"
If the answer is yes, the facts are additive. If the answer is sometimes then they are semi additive, and if the answer is no, they are non-additive.

### 1. Additive Facts (Summing Always Works)

<p>
    <img src="https://github.com/user-attachments/assets/2486a0d9-b19f-4773-a2b6-53781f81d191" width="75%">
</p>

_**Another Example**: Sale Revenue_
| Date | Product | Region | Sales Revenue |
| :--------: | :-----: | :----: | :-----------: |
| 2023-01-01 | Shirts | East | $100 |
| 2023-01-02 | Pants | West | $150 |
| 2023-01-02 | Shirts | East | $120 |

- These are measures where adding them together across any dimension gives you a meaningful total.
- Think of things that accumulate.
- _Imagine you have a store_
  - If you sell $100 worth of goods on Monday and $150 on Tuesday, the total sales for those two days is $250. This makes sense.
  - If you sell $50 worth of shirts and $75 worth of pants, the total sales is $125. Again, this makes sense.
  - If your East region sold $300 and your West region sold $500, then the total sales is $800.
- You can add up:
  - Sales Revenue by Date: $100 + $150 + $120 = $370 (total sales for the period)
  - Sales Revenue by Product: $100 + $120 = $220 (total shirt sales) and $150 (total pant sales)
  - Sales Revenue by Region: $220 (East Sales) $150 (West Sales)

### 2. Semi-Additive Facts (Summing Works Sometimes)

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/b7778703-be06-4b35-a802-881063ffdf06">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/a53a44c6-a31c-4113-bd22-a5dcd694d5e3">
        </td>
    </tr>
</table>

_**Another Example**: Bank Account Balance_

|    Date    | Account | Balance |
| :--------: | :-----: | :-----: |
| 2023-01-01 |    A    |  $100   |
| 2023-01-01 |    B    |  $150   |
| 2023-01-02 |    A    |  $120   |

- These are measures where adding them together works for some dimensions, but not others, especially time.
- Think of things that represent a snapshot at a specific point in time.
- **Why it doesn't always work:**
  - If your balance is $100 on Monday and $120 on Tuesday, adding them ($220) doesn't tell you anything meaningful. It doesn't represent your total balance over those two days. It's just two separate snapshots.
- **Why it does work sometimes:**
  - If you have two different accounts, you can add their balances to find a total balance accross accounts, at a specific point in time.
- **You can add up**:
  - Balance by Account (on a specific date): $100 + $150 = $250 (total balance of all accounts on 2023-01-01)
- **You cannot add up**:
  - Balance by Date: $100 + $150 + $120 = $370 (meaningless)

### 3. Non-Additive Facts (Summing Never Works)

<p>
    <img src="https://github.com/user-attachments/assets/debb11ff-a272-4752-ad85-4ad568f71fa0" width="75%">
</p>

_**Another Example**: Average Customer Rating_

|    Date    | Product | Avg. Rating |
| :--------: | :-----: | :---------: |
| 2023-01-01 |    A    |      4      |
| 2023-01-02 |    A    |      5      |

- These are measures where adding them together never gives a meaningful total.
- Think of things that are calculated values, like averages or ratios.
- **Why it never works:**
  - If your average rating is 4 stars on Monday and 5 stars on Tuesday, adding them (9 stars) is meaningless. You can't say your "total average rating" is 9.
  - To find the average customer rating over the two days, you would need to calculate a new average based on the total number of ratings, and the total of all of the rating values.
- **You cannot add up:**
  - Avg. Rating by Date: 4 + 5 = 9 (meaningless)

> - Additivity is about whether summing the numbers makes sense in the real-world context of the data.
> - If the sum represents a meaningful total, it's additive.
> - If the sum is just a random number, it's not additive.
> - Semi additive facts are additive across all dimensions except time related dimensions.

<p>
    <img src="https://github.com/user-attachments/assets/154fcaf0-8b8c-4eac-bd45-bedfaf765af2" width="75%">
</p>

## Nulls in Facts

- If you have a null in a fact table, it will not be included in the aggregation.
  - but, should be handled if needed (eg, by replacing with 0).

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/0abfcf37-8868-43ed-86e7-90b9331bd157">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/b7daf52d-a56b-4cda-94ff-de35155f948e">
        </td>
    </tr>
</table>

- Nulls should not be in FK columns, as it can lead to missing data, conflicts etc if we want to connect it to some dimension table.
  - you can create some dummy value in the dimension table to handle this (eg, "-1", "Unknown", "Not Available" etc).

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/bb318b6e-8c3d-4a6b-bbbf-37cde41e3812">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/38421b8d-cfdb-4361-8b59-631e77f37258">
        </td>
    </tr>
</table>

## Year-to-Date Facts

it's often requested by business users and we are tempted to create a YTD, MTD, QTD etc. fact column in the fact table. But, it's not a good idea to store YTD facts in the fact table.

<p align="center">
    <img src="https://github.com/user-attachments/assets/a868933c-f20e-4186-8179-7fac5d3c72d5" width="75%">
</p>

> the problem is that these calculations are not in the defined grain of the fact table.
>
> :star2: better store the underlying values (eg. revenue) in defined grain (eg. daily grain) and calculate all the to-Date variations in BI tool, also OLAP cubes can handle these calculations.

## Types of Fact Tables

- Transactional Fact Table
- Periodic Snapshot Fact Table
- Accumulating Snapshot Fact Table
- Factless Fact Table

### Transactional Fact Table

<p>
    <img src="https://github.com/user-attachments/assets/7a970771-81ea-4434-8764-1f4b1e25bf0c" width="75%">
</p>

- This is the **most common** and **flexible** because we can analyze it in many different ways with many dimensions.
- typically additive
- tend to have a lot of dimensions associated (FKs) with it.
- can be enormous in size and may have rapid growth.

### Periodic Snapshot Fact Table

<p>
    <img src="https://github.com/user-attachments/assets/46b8c6aa-adca-4651-a809-36c598e7ec48" width="75%">
</p>

- tends to be not as enormous in size.
- typically additive
- tend to have a lot of facts and fewer dimensions associated.
- No events = null or 0. (null won't be included in aggregation eg. AVG)

### Accumulating Snapshot Fact Table

<p>
    <img src="https://github.com/user-attachments/assets/32e4ea9f-4b64-4e30-b1f4-7a891c5aa6fd" width="75%">
</p>

- least common
- workflow or process analysis
- multiple date/time FKs (for each process step)
- date/time keys associated with **role-playing dimensions**

> **Key Differences**  
> key difference b/w these types of Fact tables
>
> <img src="https://github.com/user-attachments/assets/bf1bd130-17dc-4fc1-b272-eaf4b1d93682">

### Factless Fact Table

In a Fact table there can be multiple facts, but in a Factless Fact table there are no facts, only FKs.

- **fact**: numeric measurement used to track the perforamance of a certain business process.
- **fact table**: table that contains facts, FKs etc.
- **factless fact table**: table that contains only FKs, no facts.

Sometimes only dimensional aspects of an event are recorded, and no measures are associated with the event. In such cases, a factless fact table is used.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/d5b5340e-b129-47ab-bc78-671bec07eea4">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/8e07320d-3e09-4e16-a1d9-7e6eceb097d8">
        <p><b>Occurance of events like:</b> <i>Employee Promotion</i>
        </p>
        </td>
    </tr>
</table>

We can answer questions like:

> - How many employees have been registered last month?
> - How many employees have been registered in a certain region?

## Steps to create a Fact Table

There are 4 key decisions to make **considering the business needs** to create our tables and columns.

<img src="https://github.com/user-attachments/assets/23a5d8ff-26e7-420b-8ad3-9f4faa85522e" width="75%">

- **Step 1**: Identify the business process to analyze.
- **Step 2**: Decide the grain of the fact table.
  > **Grain**: the level of detail in the fact table (_what is one row in the fact table representing?_). It's _recommended to have the finest grain possible_ (not pre-aggregated, leave us open for broader analysis).
- **Step 3**: Choose the relevant dimensions.
- **Step 4**: Identify the facts.
