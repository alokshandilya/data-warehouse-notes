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

> :ghost: Lack of Data Warehouse in a company can create questions like:
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

> A database used and optimized for analytical purposes.
>
> Also, should be:
>
> - user friendly
> - fast query performance
> - enabling data analysis
