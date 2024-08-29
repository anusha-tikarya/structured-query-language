### Syntax
```sql
<function_name>(expression) 
OVER (
  [PARTITION BY partition_expression, ...] 
  [ORDER BY sort_expression, ...] 
  [ROWS or RANGE frame_clause]
)
```
### Example Usages
**Using ROW_NUMBER()** to Assign Row Numbers:
```sql
SELECT 
    employee_id,
    employee_name,
    ROW_NUMBER() OVER (ORDER BY hire_date) AS row_num
FROM 
    employees;
```
This query assigns a unique row number to each employee based on their hire_date.
 
**Running Total Using SUM() with a Window Function**:
```sql
SELECT 
    order_id,
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM 
    orders;
This query computes a running total (running_total) of the amount column ordered by order_date
```

**Frame Clauses**
You can define the window frame to limit the set of rows that the window function considers:

ROWS BETWEEN specifies the starting and ending point of the window frame.
RANGE BETWEEN specifies a range of values instead of rows.
Example with a frame clause:
```sql
SUM(amount) OVER (ORDER BY order_date ROWS BETWEEN 1 PRECEDING AND CURRENT ROW)
```
This calculates the sum for the current and previous rows.

Let's explore the `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` window functions in MSSQL. These functions are used to assign a unique number to rows within a specified window (or partition) of a dataset.

### Differences Between `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()`

1. **`ROW_NUMBER()`**: Assigns a unique sequential integer to rows within a partition of a result set. If two rows have the same values, they will still get unique numbers.
2. **`RANK()`**: Provides a ranking of rows within a partition. If two rows have the same values, they will receive the same rank, but the next rank will be skipped (leaving a gap).
3. **`DENSE_RANK()`**: Similar to `RANK()`, but without gaps. Rows with the same values will receive the same rank, and the next rank will continue sequentially.

### Example Scenario

Suppose we have an `employees` table as follows:

| employee_id | employee_name | department | salary |
|-------------|---------------|------------|--------|
| 1           | Alice         | Sales      | 5000   |
| 2           | Bob           | Sales      | 7000   |
| 3           | Charlie       | HR         | 5000   |
| 4           | David         | HR         | 6000   |
| 5           | Eve           | Sales      | 7000   |

### 1. **Using `ROW_NUMBER()`**

Let's assign a unique row number to each employee ordered by their `salary` within each `department`:

```sql
SELECT 
    employee_id,
    employee_name,
    department,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_number
FROM 
    employees;
```

**Output:**

| employee_id | employee_name | department | salary | row_number |
|-------------|---------------|------------|--------|------------|
| 2           | Bob           | Sales      | 7000   | 1          |
| 5           | Eve           | Sales      | 7000   | 2          |
| 1           | Alice         | Sales      | 5000   | 3          |
| 4           | David         | HR         | 6000   | 1          |
| 3           | Charlie       | HR         | 5000   | 2          |

- **Explanation:** Each employee is assigned a unique row number within their department, even if they have the same salary.

### 2. **Using `RANK()`**

Let's rank the employees by their `salary` within each `department`:

```sql
SELECT 
    employee_id,
    employee_name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank
FROM 
    employees;
```

**Output:**

| employee_id | employee_name | department | salary | rank |
|-------------|---------------|------------|--------|------|
| 2           | Bob           | Sales      | 7000   | 1    |
| 5           | Eve           | Sales      | 7000   | 1    |
| 1           | Alice         | Sales      | 5000   | 3    |
| 4           | David         | HR         | 6000   | 1    |
| 3           | Charlie       | HR         | 5000   | 2    |

- **Explanation:** Employees with the same salary receive the same rank, but the next rank is skipped (creating a gap).

### 3. **Using `DENSE_RANK()`**

Now, let's rank the employees by their `salary` using `DENSE_RANK()`:

```sql
SELECT 
    employee_id,
    employee_name,
    department,
    salary,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank
FROM 
    employees;
```

**Output:**

| employee_id | employee_name | department | salary | dense_rank |
|-------------|---------------|------------|--------|------------|
| 2           | Bob           | Sales      | 7000   | 1          |
| 5           | Eve           | Sales      | 7000   | 1          |
| 1           | Alice         | Sales      | 5000   | 2          |
| 4           | David         | HR         | 6000   | 1          |
| 3           | Charlie       | HR         | 5000   | 2          |

- **Explanation:** Similar to `RANK()`, employees with the same salary receive the same rank, but there are no gaps in the subsequent ranks.

### Summary of Differences

- **`ROW_NUMBER()`**: Generates a unique row number for each row in the result set, regardless of duplicates.
- **`RANK()`**: Assigns the same rank to duplicate values, but leaves gaps in the ranking sequence.
- **`DENSE_RANK()`**: Assigns the same rank to duplicate values without any gaps in the ranking sequence.

