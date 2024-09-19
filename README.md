# structured-query-language

## Windows function:

| **Function**      | **Purpose**                                        | **Syntax**                                | **Key Points**                                  | **Exceptions/Mandatory**       |
|-------------------|----------------------------------------------------|-------------------------------------------|------------------------------------------------|--------------------------------|
| **ROW_NUMBER()**  | Sequential integer to rows                        | `ROW_NUMBER() OVER (PARTITION BY ... ORDER BY ...)`                    | Assigns unique row numbers within each partition. | Requires `ORDER BY`; results can be arbitrary without it. |
| **RANK()**        | Rank with gaps for ties                           | `RANK() OVER (PARTITION BY ... ORDER BY ...)`                          | Ranks rows with gaps for ties.                | Requires `ORDER BY`; gaps can appear in ranks.                |
| **DENSE_RANK()**  | Rank without gaps for ties                        | `DENSE_RANK() OVER (PARTITION BY ... ORDER BY ...)`                    | Ranks rows without gaps.                      | Requires `ORDER BY`; no gaps in ranks.                |
| **NTILE()**       | Divides rows into specified number of buckets     | `NTILE(n) OVER (PARTITION BY ... ORDER BY ...)`                        | Divides rows into `n` groups.                 | Requires `ORDER BY`; divides into roughly equal parts.           |
| **SUM()**         | Cumulative sum of values                          | `SUM(expression) OVER (PARTITION BY ... ORDER BY ...)`                 | Computes running total.                       | Optional `PARTITION BY`, `ORDER BY` for specific windows.|
| **AVG()**         | Average of values                                 | `AVG(expression) OVER (PARTITION BY ... ORDER BY ...)`                 | Computes moving average.                      | Optional `PARTITION BY`, `ORDER BY` for specific windows.|
| **MIN()**         | Minimum value within window                       | `MIN(expression) OVER (PARTITION BY ... ORDER BY ...)`                 | Finds minimum value.                         | Optional `PARTITION BY`, `ORDER BY` for specific windows.  |
| **MAX()**         | Maximum value within window                       | `MAX(expression) OVER (PARTITION BY ... ORDER BY ...)`                 | Finds maximum value.                         | Optional `PARTITION BY`, `ORDER BY` for specific windows.  |
| **LEAD()**        | Value from the next row                           | `LEAD(expression, offset) OVER (PARTITION BY ... ORDER BY ...)`        | Retrieves value from a subsequent row.        | Optional `PARTITION BY`, `ORDER BY` for specific windows.     |
| **LAG()**         | Value from the previous row                       | `LAG(expression, offset) OVER (PARTITION BY ... ORDER BY ...)`         | Retrieves value from a previous row.         | Optional `PARTITION BY`, `ORDER BY` for specific windows. |

