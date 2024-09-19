
### What is `CASE WHEN`?

The `CASE WHEN` statement evaluates conditions and returns a specific result based on the condition that is met. It can be used in `SELECT`, `UPDATE`, and other SQL statements.

### Syntax

There are two main forms of the `CASE` statement:

1. **Simple `CASE` Statement**
2. **Searched `CASE` Statement**

#### 1. Simple `CASE` Statement

Used when you want to compare one expression to multiple possible values.

**Syntax:**

```sql
CASE expression
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ...
    ELSE default_result
END
```

**Example:**

Suppose you have a table of employees and you want to categorize them based on their department codes.

```sql
SELECT
    EmployeeID,
    DepartmentCode,
    CASE DepartmentCode
        WHEN 'HR' THEN 'Human Resources'
        WHEN 'IT' THEN 'Information Technology'
        WHEN 'FIN' THEN 'Finance'
        ELSE 'Other'
    END AS DepartmentName
FROM Employees;
```

#### 2. Searched `CASE` Statement

Used when you need to evaluate multiple conditions that are not just comparisons against one expression.

**Syntax:**

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    ELSE default_result
END
```

**Example:**

Suppose you want to categorize sales amounts into different performance tiers.

```sql
SELECT
    SaleID,
    SaleAmount,
    CASE
        WHEN SaleAmount >= 1000 THEN 'High'
        WHEN SaleAmount >= 500 THEN 'Medium'
        ELSE 'Low'
    END AS PerformanceTier
FROM Sales;
```


### Key Points

- **Evaluation Order**: Conditions are evaluated sequentially. The first condition that evaluates to `TRUE` will be executed, and the rest are ignored.
- **`ELSE` Clause**: Optional but useful for handling cases that do not match any of the specified conditions.
- **Flexibility**: Can be used in `SELECT`, `WHERE`, `ORDER BY`, and other clauses to control data presentation.

### Examples in Different Contexts

1. **In `SELECT` Clause**: Customize the output of your query.
   
   ```sql
   SELECT
       EmployeeID,
       Salary,
       CASE
           WHEN Salary > 70000 THEN 'High Salary'
           WHEN Salary BETWEEN 50000 AND 70000 THEN 'Medium Salary'
           ELSE 'Low Salary'
       END AS SalaryRange
   FROM Employees;
   ```

2. **In `WHERE` Clause**: Apply conditional logic to filter results.

   ```sql
   SELECT *
   FROM Orders
   WHERE CASE
       WHEN OrderDate > '2023-01-01' THEN Status
       ELSE 'Old'
   END = 'Shipped';
   ```

3. **In `ORDER BY` Clause**: Order results based on conditional logic.

   ```sql
   SELECT
       ProductName,
       Sales
   FROM Products
   ORDER BY
       CASE
           WHEN Sales > 10000 THEN 1
           ELSE 2
       END;
   ```

In this example, products with sales greater than 10,000 are listed first.

```sql
--------------- CASE WHEN

--You need to categorize employees based on their years of experience and calculate their bonus accordingly. Employees with less than 2 years of experience get a 5% bonus, those with 2 to 5 years get a 10% bonus, and those with more than 5 years get a 15% bonus.


CREATE TABLE Employee (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(50),
    YearsOfExperience INT,
    Salary DECIMAL(10,2)
);

INSERT INTO Employ(EmployeeID, Name, YearsOfExperience, Salary)
VALUES 
    (1, 'Alice', 1, 50000),
    (2, 'Bob', 3, 60000),
    (3, 'Charlie', 6, 70000),
    (4, 'David', 4, 80000);

select employeeId , Name , yearsofexperience , salary,
case 
 when yearsofexperience < 2 then salary*0.05
 when yearsofexperience <5 then  salary*0.1
 when yearsofexperience >5 then salary*0.15
 end as bonus, 
 Salary + 
    CASE 
        WHEN YearsOfExperience < 2 THEN Salary * 0.05
        WHEN YearsOfExperience BETWEEN 2 AND 5 THEN Salary * 0.10
        WHEN YearsOfExperience > 5 THEN Salary * 0.15
        ELSE 0
    END AS TotalCompensation
 from employ
```

