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
