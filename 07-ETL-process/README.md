# ETL Process

## Introduction

> How to design dimensional models and how to bring data from source to DWH is the ETL process itself at a high level

<table>
    <tr>
        <td>
            <img src="https://github.com/user-attachments/assets/258a4473-4769-4f71-96af-62ac5ccc149a">
        </td>
        <td>
            <img src="https://github.com/user-attachments/assets/ac9eb1d2-b129-4f5f-8a78-db534455c02d">
        </td>
    </tr>
</table>

## ETL Process

ETL process is done by ETL tools (set of built-in tools to):

- **connect to different data sources**
  - extract data from different sources
- **transform/clean data**
  - many tools to transform data, change data types, to add additional columns, model , clean data.
- **load data into data warehouse**
  - writing back data into databases - data warehouse
- **etc.**

#### ETL setup

**Building workflows**

> - **staging** workflow
> - **core**/transformation workflow
> - **data mart** workflow

**Jobs**

> - **run the workflows**
> - **are scheduled based on defined rules**

#### _Hierarchy in ETL Execution_

- **Task**: The smallest executable unit (e.g., "Extract data from a table").
- **Job**: A group of related tasks performing a complete ETL function (e.g., "Extract Sales Data").
- **Workflow**: A sequence of jobs orchestrating an end-to-end ETL process (e.g., "Daily Data Pipeline").

## Extract data

Extract data from different sources to **Staging** area where:

- data becomes part of DWH
- all data is stored in tables
- from here data is transformed
- most common staging layer is **transient** (temporary, all data copied and then deleted)

there are mainly 2 extracting types:

- **Initial Load**: Load all data from source to DWH
  - first real run
  - all data
- **Delta Load**: Load only new data from source to DWH
  - subsequent runs
  - only additional data

### Initial Load

- first initial extraction from source data, _done after discussion with business users and IT responsibles_
  - discuss **what data is needed?**
  - dicuss **when is the good time to load the data? (night, weekends, etc.)**
  - will be increasing load on source systems
  - make smaller extractions to test (process, time needed, etc.)

> after data is in staging layer we also have some initial load to core with transformations after all the transformation steps have been designed. This makes all the data done from the Staging (no filtering), so all the data is in the core layer.

### Delta Load

- incremental periodic extraction/load
- delta column for every table which typically is a timestamp, date (like transaction_date, create_date) from which we can identify the new data.

  > if we dont have some timestamp, date column we can use some other column to identify the new data like PK (autoincremented column)
  > if we have natural key then it could be bit more complex to identify the new data.

<img src="https://github.com/user-attachments/assets/e52d4fdc-0f24-48bb-943f-b68fd5d36e77" width="75%">

#### What if there is no Delta column?

Mostly, Delta column is present. Mostly for dimension tables delta column might be missing.

- there are some tools that can capture automatically which data has been already loaded by the metadata.
- just full load everytime and compare the data that is already loaded 
- depending on the data volumnes keep performance in mind
