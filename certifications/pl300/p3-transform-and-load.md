## 3. Transform and Load the Data

1. Group and aggregate rows
2. Pivot, unpivot and transpose data
3. Convert semi-structured data into a table
4. Create fact and dimension tables
5. Reference and duplicated query use
6. Merge and append queries
7. Identify and create adequate keys for relationships
8. Configure data loading for queries

### Column data types

- Switch to appropriate data type as soon as possible in PQ
- Column type selection is based on column usage and resource optimization

#### list of column data types

- *text* : can be strings, dates or numbers represented in a text format
- *any* : given to a column that does not have a deliberate data type definition (ABC123 type k)
- *true/false* : bool
- *date* : represents a date without time, when converted into the model, date is the same as Date/Time with a zero for the time fraction
- *time* : corresponds to time, without the date. when converted, a time value is the same as Date/Time with no digits in the time portion
- *date/time* : represents both date and time
- *decimal number* : 64-bit float, most common number data type; can also handle integers/whole numbers
- *fixed decimal number* : currency type, has a fixed location for the decimal separator, which always has 4 digits to its right

|     **date/time**      | **quantity** | **category** | **price** |      |
| :--------------------: | ------------ | :----------: | --------- | ---- |
| 2022-12-31 12:00:00 AM | 1            |   *type A*   | 99.99     |      |
| 2023-01-05 12:00:00 AM | 2            |   *type B*   | 84.09     |      |
| 2023-01-05 12:00:00 AM | 1            |   *type A*   | 84.99     |      |
| 2023-01-07 12:00:00 AM | 1            |   *type C*   | 24.99     |      |

- **category** → text
- **date/time** → *divide* or *convert* as it is not a best practice, as the time values do not provide information, converting into a *date* type is optimal
- **quantity** → whole number
- **price** → fixed decimal number 

> **convert multiple columns at the same time →**  `double-click on columns > Transform tab > Data type`

> **date/time → ** if *both date and time information from a column is important*, select column, `Add column tab > Date > Date (or time)`, etc.

> **every column is abc123 (any) →** `Transform tab > Detect data type`

### Create and transform columns

- two important tabs in power query, `Transform tab` and `Add Column tab`, both with similar features.

> **create column**
>
> - *from examples*
> - *custom*
> - *conditional*
> - *merge*
> - *number*
> - *date/time*

> **transform column** 
>
> - *replace values*
> - *format*
> - *extract*

- **from examples →**  adding a suffix, such as `@domain.com` to a text-based column → `select col > Add Column tab > Column from examples > Selection`, *suggests a M-language transformation based on the examples given on 'Merged'*
- **custom column →** `Full name = [First Name] & " " & [Last Name]`
- **conditional column →** *can be used to replace multiple values*
  - **if** `Column Name = Code` and `value` = `OS` **then** `Office supplies` *(clause 1)*
  - **if** `Column Name = Code` and `value` = `BS` **then** `Bathroom supplies` *(clause 2)*
- **merge columns →** `select First Name and Last Name` > `Add/Transform tab` > `Merge Columns` > `Space separator`
- **number columns →** `select Unit price and quantity` > `Add Column tab` > `standard math op` > `Multiply`
- **replace values →** `select column` > `Transform tab` > `Replace values`
- **format column →** `select column` > `Transform tab` > `Format` > `Trim`, `Capitalize Each Word`, etc.
- **extract →** *length, first characters, last characters, range*; *text before delimiter, text after delimiter, text between delimiters*

### Group and aggregate rows

