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

**Cardinality** corresponds to the number of *distinct values* relative to the *number of rows* in a table; *how unique a specific column is*

