```sql
use Aub
--1
--Scenario: You have a table of products and their sales amounts. You want to rank the products within each category based on their sales amounts.
-- Create table for products
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    Category VARCHAR(50),
    SalesAmount DECIMAL(10, 2)
);

-- Insert data into Products
INSERT INTO Products (ProductID, Category, SalesAmount)
VALUES 
    (1, 'Electronics', 1500.00),
    (2, 'Electronics', 1200.00),
    (3, 'Furniture', 800.00),
    (4, 'Furniture', 950.00),
    (5, 'Electronics', 1800.00);

select productid, salesAmount , category,
 rank() over( partition by category order by salesamount desc) as rank
 from Products;

 --2
 --Scenario: You have a table of daily sales data. You want to calculate the running total of sales for each day.

 -- Create table for daily sales
CREATE TABLE DailySales (
    SaleDate DATE PRIMARY KEY,
    SalesAmount DECIMAL(10, 2)
);

-- Insert data into DailySales
INSERT INTO DailySales (SaleDate, SalesAmount)
VALUES 
    ('2024-09-01', 200.00),
    ('2024-09-02', 300.00),
    ('2024-09-03', 250.00),
    ('2024-09-04', 400.00);

select saleDate, salesAmount,
sum(SalesAmount) over (partition by saledate) as totalsales from dailysales;


--3
--Scenario: You have a table of monthly sales data. You want to calculate the moving average of sales for each month, considering the sales of the current month and the previous two months.

-- Create table for monthly sales
CREATE TABLE MonthlySales (
    SalesMonth CHAR(7) PRIMARY KEY,  -- Format: YYYY-MM
    SalesAmount DECIMAL(10, 2)
);

-- Insert data into MonthlySales
INSERT INTO MonthlySales (SalesMonth, SalesAmount)
VALUES 
    ('2024-06', 5000.00),
    ('2024-07', 5500.00),
    ('2024-08', 6000.00),
    ('2024-09', 6500.00);



SELECT 
    SalesMonth,
    SalesAmount,
    AVG(SalesAmount) OVER (ORDER BY SalesMonth ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS MovingAverage
FROM MonthlySales;


--4
--- You have a table of employee salaries. You want to compare each employee's salary with the salary of the previous and next employees.
-- Create table for employee salaries
CREATE TABLE EmployeeSalaries (
    EmployeeID INT PRIMARY KEY,
    Salary DECIMAL(10, 2)
);

-- Insert data into EmployeeSalaries
INSERT INTO EmployeeSalaries (EmployeeID, Salary)
VALUES 
    (1, 50000.00),
    (2, 55000.00),
    (3, 60000.00),
    (4, 65000.00);

	select employeeid , salary,
	lag(salary, 1) over ( order by employeeid) as privious_sal,
	lead(salary, 1) over  (order by employeeid) as next_sal
	from EmployeeSalaries;


--5
-- You have a table of sales figures and want to calculate the cumulative distribution of sales amounts.
-- Create table for sales data
CREATE TABLE SalesData (
    SaleID INT PRIMARY KEY,
    SaleAmount DECIMAL(10, 2)
);

-- Insert data into SalesData
INSERT INTO SalesData (SaleID, SaleAmount)
VALUES 
    (1, 100.00),
    (2, 200.00),
    (3, 300.00),
    (4, 400.00);

select saleId ,saleAmount,
cume_dist() over (order by saleamount) as cumulative_distribution
from  salesdata;

```

| **Function**        | **Purpose**                                            | **Syntax**                                                  | **Working**                                                                                                            | **Why Use It?**                                        | **Exceptions/Mandatory**                      |
|---------------------|--------------------------------------------------------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------------------------------|
| **ROW_NUMBER()**    | Assigns a unique sequential integer to rows in a result set | `ROW_NUMBER() OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Numbers rows sequentially within each partition based on the `ORDER BY` clause.                                        | Useful for unique row identification and pagination. | Requires `ORDER BY` clause.                   |
| **RANK()**          | Assigns a rank to rows within a partition, with gaps for ties | `RANK() OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Ranks rows in order, with the same rank for tied values and gaps in ranking.                                            | Useful for ranking with ties, where gaps are acceptable. | Requires `ORDER BY` clause.                   |
| **DENSE_RANK()**    | Assigns a rank to rows within a partition, without gaps for ties | `DENSE_RANK() OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Ranks rows in order, with the same rank for tied values but no gaps in ranking.                                         | Useful for ranking where gaps in ranks are not desirable. | Requires `ORDER BY` clause.                   |
| **NTILE()**         | Divides rows into a specified number of approximately equal parts | `NTILE(number_of_buckets) OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Divides the result set into `number_of_buckets` groups and assigns a bucket number to each row.                         | Useful for splitting data into quantiles or percentiles. | Requires `ORDER BY` clause.                   |
| **SUM()**           | Calculates the cumulative sum of values | `SUM(expression) OVER (PARTITION BY partition_expression ORDER BY order_expression ROWS window_spec)` | Computes the sum of values within the specified window.                                                                | Useful for calculating running totals or cumulative sums. | Optional `PARTITION BY`, `ORDER BY`, and `ROWS`. |
| **AVG()**           | Calculates the average of values | `AVG(expression) OVER (PARTITION BY partition_expression ORDER BY order_expression ROWS window_spec)` | Computes the average of values within the specified window.                                                             | Useful for calculating moving averages or average values. | Optional `PARTITION BY`, `ORDER BY`, and `ROWS`. |
| **MIN()**           | Returns the minimum value within a window | `MIN(expression) OVER (PARTITION BY partition_expression ORDER BY order_expression ROWS window_spec)` | Finds the smallest value within the specified window.                                                                    | Useful for identifying minimum values within partitions. | Optional `PARTITION BY`, `ORDER BY`, and `ROWS`. |
| **MAX()**           | Returns the maximum value within a window | `MAX(expression) OVER (PARTITION BY partition_expression ORDER BY order_expression ROWS window_spec)` | Finds the largest value within the specified window.                                                                     | Useful for identifying maximum values within partitions. | Optional `PARTITION BY`, `ORDER BY`, and `ROWS`. |
| **CUME_DIST()**     | Calculates the cumulative distribution of a value | `CUME_DIST() OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Computes the proportion of rows with values less than or equal to the current row’s value.                               | Useful for ranking data points within a dataset.      | Requires `ORDER BY` clause.                   |
| **PERCENT_RANK()**  | Calculates the percentile rank of a value | `PERCENT_RANK() OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Computes the relative rank of a row as a percentage of the total number of rows.                                         | Useful for finding percentile ranks within a dataset.  | Requires `ORDER BY` clause.                   |
| **LEAD()**          | Provides access to a row at a specified physical offset from the current row | `LEAD(expression, offset, default) OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Retrieves the value of a specified column from a subsequent row.                                                         | Useful for comparing current row values with future rows. | Optional `PARTITION BY`, `ORDER BY`, and `ROWS`. |
| **LAG()**           | Provides access to a row at a specified physical offset from the current row | `LAG(expression, offset, default) OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Retrieves the value of a specified column from a previous row.                                                           | Useful for comparing current row values with previous rows. | Optional `PARTITION BY`, `ORDER BY`, and `ROWS`. |
| **FIRST_VALUE()**   | Returns the first value in the ordered partition | `FIRST_VALUE(expression) OVER (PARTITION BY partition_expression ORDER BY order_expression)` | Retrieves the first value in the result set or partition as ordered by the `ORDER BY` clause.                           | Useful for finding the initial value in a sorted list. | Requires `ORDER BY` clause.                   |
| **LAST_VALUE()**    | Returns the last value in the ordered partition | `LAST_VALUE(expression) OVER (PARTITION BY partition_expression ORDER BY order_expression ROWS window_spec)` | Retrieves the last value in the result set or partition as ordered by the `ORDER BY` clause.                            | Useful for finding the final value in a sorted list.  | Requires `ORDER BY` and `ROWS` clause.        |

### Summary

- **Purpose**: Window functions perform calculations across a set of table rows related to the current row.
- **Syntax**: Generally follows `function_name() OVER (PARTITION BY ... ORDER BY ... ROWS ...)`.
- **Working**: Operates over a window of rows defined by `PARTITION BY`, `ORDER BY`, and `ROWS`.
- **Why Use It?**: To perform advanced analytics and calculations without needing subqueries or complex joins.
- **Exceptions/Mandatory**: Most require an `ORDER BY` clause to define the row order, and some may need `PARTITION BY` or `ROWS` clauses for specific calculations.

