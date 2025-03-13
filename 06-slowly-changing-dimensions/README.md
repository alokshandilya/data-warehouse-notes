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
