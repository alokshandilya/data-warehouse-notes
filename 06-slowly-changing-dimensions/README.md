# Slowly Changing Dimensions

### Introduction

- till now we have pretended dimensions never change
  - indeed, they are usually static
  - but, sometimes in real world, they do change

> Business users mostly don't know about changes in dimensions.
>
> - **be proactive**: ask about potential changes with Business Users + IT and then develop a strategy for each changing attribute.

> **Ralph Kimball** introduced SCD in 1995 (book, "The Data Warehouse Toolkit" published 1996) and distinguished between different types (1, 2, 3, ....).

SCDs are changes to dimensions that arrive unexpectedly, sporadically, and less frequently than fact table measurements. Kimball identified three main types of SCD responses: Type 1 (overwrite), Type 2 (add a new row), and Type 3 (add a column).

### Type 0: Retain Original

- there won't be any changes
- data table (holidays etc. can have some changes)
- very simple and easy to maintain

### Type 1: Overwrite

- old attributes are just overwritten
- no history is kept, only current state is reflected
- very simple to implement
- no fact table needs to be modified

#### Problems

- history is lost
- might affect/break existing queries

<img src="https://github.com/user-attachments/assets/b1dee340-9fc0-4c9e-812c-f7012f0d9540" width="75%">

### Type 2: Add New Row :star2:

- **Problem with Type 1**: No history of dimensions!
- only current state is reflected
- with type 2 we can **perfectly partition and segment history**

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/2b99698f-c184-4492-9e00-1e3c33e2191b">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/cf4d35bf-350f-48ba-884e-f2c7e6264de4">
        </td>
    </tr>
</table>

<img src="https://github.com/user-attachments/assets/09463757-50c6-40d7-9b3e-4b9d68a9ae2e" width="75%">

- we just add a new row in the dimension table

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/6f617875-9561-4b9f-80b2-4364944c4f4c">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/f69e6b1a-9b17-4d20-9587-f4444585b78d">
        </td>
    </tr>
</table>

- we can count the number of products by counting distinct `product_id` (natural key) in the dimension table.
- how to find all the current products?
  - we can't distinguish b/w the current and old product with same natural key `product_id`
  - **Solution**: add `start_date` and `end_date` columns to the dimension table, optionally `is_current` column (recommended by Kimball)

<img src="https://github.com/user-attachments/assets/af87126b-97ce-443b-9296-c7f4d4413fea" width="75%">

## Administerate Type 2 SCD

- include `start_date` and `end_date` columns in the dimension table, optionally `is_current` column
  - in `end_date`, instead of `NULL`, we can use a future date (e.g. 9999-12-31) so that sql functions like `BETWEEN` can be used.

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/46aa941d-5443-4423-a1af-035056d08ac1">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/dea6ceea-d734-46d9-8d39-fc9b09577343">
        </td>
    </tr>
</table>

> in some cases, like for product `name`, type 2 might not be necessary. It depends on the business requirements.

- can we use mixing of type 1 and type 2?

#### Mixing Type 1 and Type 2

Yes, we can mix, some attributes can be in type 1 and some type 2.

- change old `product_name` to new value (type 1)
- add new row for new `product_category` (type 2)

<img src="https://github.com/user-attachments/assets/1462d322-d6f4-4ebe-81b5-a9c68936e378" width="75%">

### Type 3: Additonal Attributes (Add a Column)

It's the least frequently used type.

- **Type 1**: static, no history
- **Type 2**: default strategy to maintain reflected history
- **Type 3**: in between, switching back and forth between versions
  - instead of adding a row, we add a column

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/ae23f058-b655-4fe2-aeaa-12efae7a9b67">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/">
        </td>
    </tr>
</table>

> - typically use for significant changes at a time (eg. restructurings in organization) so that we can compare the old and new values.
> - enables switching between historic/current view

> If you are a manager and you want to analyze sales by region, you can use type 3 and after restructuring, you can compare how would the sales be if the restructuring was not done (from West to South region in below example)

| Reg_PK  | Region      | Prev_Region |
| :-----: | ----------- | ----------- |
|    1    | North       | North       |
| _**2**_ | _**West**_  | _**West**_  |
| _**3**_ | _**South**_ | _**West**_  |

- if there are new attributes, we can simply add a new row

| Reg_PK  | Region     | Prev_Region           |
| :-----: | ---------- | --------------------- |
|    1    | North      | North                 |
|    2    | West       | West                  |
|    3    | South      | West                  |
| _**4**_ | _**East**_ | _**Not Appliacable**_ |

- also, we can add multiple historical columns (above uses only one `Prev_Region`), but we should not add too many historical columns.

#### Limitations

- type 3, is mainly used when there is a big predictable change happening, in one moment values are changing.
- _not suitable for frequent or unpredictable changes_ **(use type 2)**
- _minor changes and no body cares much about the history_ **(use type 1)**
