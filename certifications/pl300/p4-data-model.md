

## 4. the data model

### Configure table and column properties

- as *few* as possible in terms of table and column quantity for ease-of-use
- *adequately* named tables and columns to avoid misuse and misinterpretation
- *organized* into appropriate structures

> 🎯goal → **drag-and-drop, simple** *end-user self-service*

`PBI Desktop` > `Model view` > `select table` > `Properties`

- `General`
  - `Description`
  - `Synonyms` expands and enhances the *Q&A function*; comma separated → "`product key, key`"
  - `Is hidden` removes the table from *Report view*

- `Advanced`
  - `Sort by column`
  - `Data category`
  - `Summarize by`

### Implement role-playing dimensions

A *role-playing dimension* is a dimension table that can have multiple purposes/roles. 

- for example, a `Date` can be multiple things, such as `Order Date`, `Ship Date`, `Due Date`
- or `Time` for airline flight data, such as `Departure time`, `Closed doors time`, `Takeoff time`, `Landing time`, `Arrival time`

> In `Model view`, **solid** lines indicate an *active* relationship between tables, whilst a **dotted** line indicates an *inactive* relationship between tables

> *tables can have **only one active** relationship for each table connected*

> *to use an inactive relationship* → 
>
> **DAX** `USERELATIONSHIP()` function
>
>  
>
> - function returns *no value* and *enables the indicated relationship* only for calculation
> - cannot be used when row-level security (RLS) is defined for the table used by the measure
> - `USERELATIONSHIP(<columnName1>, <columnName2>)`

### Define cardinality and cross-filter direction

**Cardinality** corresponds to the number of *distinct values* relative to the *number of rows* in a table. 

> **Cardinality** choices
>
> - one-to-many  `(1 → *)` 
> - many-to-one `(* → 1)`
> - many-to-many `(* → *)`
> - one-to-one `(1 → 1)`

#### many-to-many relationship example

**product dimension table**

| *product* |
| :-------: |
| Road Bike |
| MTB Bike  |
|  Jersey   |
| Road Bike |
| Road Bike |
|  Jersey   |
| MTB Bike  |

**sales transactions fact table**

| *product* | *quantity* | *amount* |
| :-------: | :--------: | :------: |
| Road Bike |     1      |  $3300   |
| MTB Bike  |     2      |   $999   |
| Road Bike |     1      |  $3300   |
| MTB Bike  |     2      |   $999   |
| MTB Bike  |     2      |   $999   |
|  Jersey   |     2      |   $60    |
|  Jersey   |     1      |   $30    |

- *duplicate entries on **product** table* makes the relationship many-to-many

#### one-to-many relationship example

- ***de-duplicated** dimension table*
- *one-to-many = **reversed** many-to-one*

| *product* |
| :-------: |
| Road Bike |
| MTB Bike  |
|  Jersey   |

| *product* | *quantity* | *amount* |
| :-------: | :--------: | :------: |
| Road Bike |     1      |  $3300   |
| MTB Bike  |     2      |   $999   |
| Road Bike |     1      |  $3300   |
| MTB Bike  |     2      |   $999   |
| MTB Bike  |     2      |   $999   |
|  Jersey   |     2      |   $60    |
|  Jersey   |     1      |   $30    |

#### one-to-one relationship example

| *product* |
| :-------: |
| Road Bike |
| MTB Bike  |
|  Jersey   |

| *product* | *quantity* | *amount* |
| :-------: | :--------: | :------: |
| Road Bike |     1      |  $3300   |
| MTB Bike  |     2      |   $999   |
|  Jersey   |     1      |   $30    |

### Relationship direction

- the relationship's direction determines *how Power BI filters data*, data can be filtered by *one* or *both* tables in a relationship

> *single **cross-filter***
>
> - only one table from the relationship is used to filter data
> - commonly in one-to-many/many-to-one relationships
> - filter direction comes from de-duplicated table *('one' side)*
> - from fact table perspective:
>   - **cardinality → many-to-one **
>   - **cross-filter → single**

> *bidirectional **cross-filter***
>
> - either table can be used to filter the other
> - used by default in one-to-one relationships
> - may lead to performance issues and/or ambiguity when used with other relationships

#### DAX `CROSSFILTER()`

- used to override and cross-filter direction for the indicated relationship *(for the duration of the query)*
- function itself returns *no value*

> `CROSSFILTER(<columnName1>, <columnName2>, <direction>)`

### Common date table

- *dimension* tables that allow end-users to analyze data by date attributes
- **necessary** for *DAX time intelligence functions* 

> options for **date table creation**
>
> - Extract from a source system from the org
> - Create using *Power Query*
> - Create in the data model using *DAX*

> some **DAX functions** for date tables
>
> - `CALENDAR(<start_date>, <end_date>)`
> - `CALENDARAUTO([fiscal_year_end_month])` creates a range of dates from the earliest to latest in data model *(no parameters for default)*
>   - in case the first date available is a `birth date`, for example, the date table probably has excess data;
>   - `CALENDARAUTO(<int>)` is used to offset fiscal year end months

```DAX
CALENDAR DYNAMIC = 
# takes the min and max of the fact table to create a table
CALENDAR(
	MINX('FactTable', [OrderDate]), 
	MAXX('FactTable', [OrderDate])
) 
```

[SQLBI - Creating a simple date table in DAX](https://www.sqlbi.com/articles/creating-a-simple-date-table-in-dax/)

```
Date =
VAR MinYear = YEAR ( MIN ( Sales[Order Date] ) )
VAR MaxYear = YEAR ( MAX ( Sales[Order Date] ) )
RETURN
ADDCOLUMNS (
    FILTER (
        CALENDARAUTO( ),
        AND ( YEAR ( [Date] ) >= MinYear, YEAR ( [Date] ) <= MaxYear )
    ),
    "Calendar Year", "CY " & YEAR ( [Date] ),
    "Month Name", FORMAT ( [Date], "mmmm" ),
    "Month Number", MONTH ( [Date] )
)
```

> *after the date table is created, it is a good idea to mark it as a **date table*** (where?)

#### Auto Date/Time

- *data load option* available in PBI Desktop
  - *convenient time intelligence reporting* based on *date columns loaded into a model*
  - allows *filtering, grouping and drilldown* by using **calendar time periods**, such as years, quarters, months and days
- when **enabled**, PBI Desktop creates a *hidden auto date/time table* for each date column, providing all conditions are true:
  - `Storage mode` **=** `Import`
  - `Column data type` **=** `Date` / `DateTime`
  - `Column` isn't in the *many side* of a relationship

> In the case a data model has a lot of `Date` and `DateTime`, *Auto Date/Time* will create **redundant** date tables, beyond necessary.

> *if a column has* `Date Hierarchy`, *a date table is built behind the scenes for it*

- `On/Off` → `File` → `Options and settings` → `Options` → `Data Load` → `check/uncheck Auto Date/Time`

#### Marking a table as a date table

- Allows for the control of the *date hierarchies created in the model*
- When specified, the following validations are performed by PBI:
  - contains **unique, non-null values**
  - contains **contiguous date values**
  - if `type` = `Date/Time`, values should have **the same timestamp across each value**
- `Table view` → `Table tools tab` → `Mark as date table` → *pick column with* `Type` *=* `Date`
  - if validated successfully, all requirements are ok

### Calculated columns and tables

- Allows for the creation of *additional tables or columns* from the existing data within the **data model**:

  - **Data transforms**, such as creating an aggregation or filtering certain rows
  - **Derived data**, to create new tables based on calculations that combine or manipulate columns in other tables
  - **Custom relationships**, if necessary to set up relationships between tables that *are not directly sourced from your data*
  - **Static tables**, to define tables with fixed values, such as *lookup tables for categories*

  

| **Regular tables**                                           | **Calculated tables**                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| imported directly from external data sources, such as databases, spreadsheets and online services | created within *PBI Desktop using DAX*, with its data derived from calculations based on other tables/columns in the data model |
| regular tables *reflect* raw data and are refreshed whenever source data is updated | calculated tables are static until their *DAX expression is recalculated*, which happens when **data model is refreshed** or code modifications |
| *holds primary dataset, **providing foundation** for analysis* | *extend the model with additional data that **doesn't exist in the source**, such as summarized, filtered or aggregated data* |
| *typically more efficient*, since they rely directly on imported data | *can impact performance*, especially if DAX expressions are complex |

#### Use cases - Calculated Columns

> useful when necessary to create *new data fields* based on existing data within the **data model**
>
> - **row-by-row calculations**: to perform calculations on each row of a table, such as combining text fields and difference between dates
> - **custom viz fields**: to create fields that can be used in `axes`, `legends` or `groupings` in visuals
> - **static values**: calculated cols are a good choice if *calculated values do not change dynamically based on filters or slices*
> - **simplifying data models**: they may also help reduce complexity by precomputing values that are frequently used in reports