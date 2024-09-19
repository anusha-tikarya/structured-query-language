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
