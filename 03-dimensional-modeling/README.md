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
