## 2. profile and clean the data

2.1. Evaluate data, including data statistics and column properties

2.2. Resolve inconsistencies, unexpected or null values and data quality issues

2.3. Resolve data import errors

### 2.1. evaluate data

three options for column profiling:

- `Column distribution` for anomalies
- `Column quality` and `Column profile` for statistics

#### Column distribution 

- expands the area below column names to feature a *distribution visual*, along with the *number of unique and distinct values*
- can be used to detect anomalies

```
powerQuery > View tab > Column distribution
```

#### *distinct values*

corresponds to the number of different values within a column

#### *unique values*

corresponds to the distinct values that show up only once

> **if** *distinct values* **>** *unique values* **, some values within that column are** *duplicates*. 

#### Column quality

- displays the *percentage* of *valid*, *errors* and *empty values* within a column
- also expands area below column names

```
powerQuery > View tab > Column quality
```

#### Column profile

- displays some descriptive statistics for a *specific* column, including
  - `count`, `average`, `min`, `max`
  - `distinct`, `unique`
  - `empty`, `errors`

```
powerQuery > View tab > Column profile
```

- detailed information about duplicates can be found via column profiling
- by default, column profiling is based on top 1000 rows
  - can be changed to evaluate entire dataset

### Resolve inconsistencies

- transforming an uppercase text column
  - `right-click on column > Transform > Capitalize Each Word`
- removing whitespace at start and end of strings 
  - `right-click on column > Transform > Trim`
  - `Transform tab > Format > Trim`

- removing duplicates in a column

  - `Home tab > Reduce rows > Remove rows > Remove duplicates`

    > *distinct values* **will be now equal to** *unique values* **after duplication, allowing for using column as key***

- dividing a column into two or more columns

  - `Home tab > Split column > By delimiter`
  - usually followed by a trim

- replacing null values

  - `right-click on one or more selected columns > Replace values > null`

### Resolve data import errors

- most common import errors within PBI:
  - timeout/expiration
  - source data/system missing
  - un-usable data formats
- with a column error, the data will not load into the data model
  - *replace*, *clean* and *transform* to remove errors