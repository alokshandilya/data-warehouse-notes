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
