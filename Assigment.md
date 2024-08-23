# SQL Function and Query Exercises

## Date Function Exercises:
### Table Structure
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total DECIMAL(10, 2)
);
```

### 1. **Calculate the number of months between your birthday and the current date:**


```sql
--In MSSQL, you can use the `DATEDIFF` function combined with `MONTH`.

SELECT DATEDIFF(MONTH, '2000-01-01', GETDATE()) AS months_difference;
```

### 2. **Retrieve all orders that were placed in the last 30 days:**
```sql
--Assuming you have an `orders` table with an `order_date` column:

SELECT * 
FROM orders 
WHERE order_date >= DATEADD(DAY, -30, GETDATE());
```

### 3. **Extract the year, month, and day from the current date:**
```sql
-- You can use the `YEAR`, `MONTH`, and `DAY` functions in MSSQL.

SELECT YEAR(GETDATE()) AS year, MONTH(GETDATE()) AS month, DAY(GETDATE()) AS day;
```

### 4. **Calculate the difference in years between two given dates:**
```sql
-- In MSSQL, use the `DATEDIFF` function with the `YEAR` interval.

SELECT DATEDIFF(YEAR, '2000-01-01', '2024-08-23') AS years_difference;
```

### 5. **Retrieve the last day of the month for a given date:**


```sql
-- MSSQL has the `EOMONTH` function to get the last day of the month.

SELECT EOMONTH('2024-08-23') AS last_day_of_month;
```

## String Function Exercises:

#### `customers` Table:
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(20)
);
```

#### `products` Table:
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2)
);
```

### 1. **Convert all customer names to uppercase:**

```sql
SELECT UPPER(customer_name) AS customer_name_uppercase
FROM customers;
```

### 2. **Extract the first 5 characters of each product name:**

```sql
SELECT LEFT(product_name, 5) AS product_name_short
FROM products;
```

### 3. **Concatenate the product name and category with a hyphen in between:**

```sql
SELECT CONCAT(product_name, '-', category) AS product_category_combined
FROM products;
```

### 4. **Replace the word 'Phone' with 'Device' in all product names:**

```sql
SELECT REPLACE(product_name, 'Phone', 'Device') AS product_name_replaced
FROM products;
```

### 5. **Find the position of the letter 'a' in customer names:**

```sql
SELECT customer_name, CHARINDEX('a', customer_name) AS position_of_a
FROM customers;
```


## Aggregate Function Exercises :
```sql
ALTER TABLE orders
ADD quantity INT;

ALTER TABLE products
ADD stock_quantity INT;
```

#### 1. **Calculate the Total Sales Amount for All Orders:**

```sql
SELECT SUM(total) AS total_sales_amount
FROM orders;
```

#### 2. **Find the Average Price of Products in Each Category:**

```sql
SELECT category, AVG(price) AS average_price
FROM products
GROUP BY category;
```

#### 3. **Count the Number of Orders Placed in Each Month of the Year:**

```sql
SELECT DATEPART(MONTH, order_date) AS order_month, COUNT(order_id) AS orders_count
FROM orders
GROUP BY DATEPART(MONTH, order_date)
ORDER BY order_month;
```

#### 4. **Find the Maximum and Minimum Order Quantities:**

```sql
SELECT MAX(quantity) AS max_quantity, MIN(quantity) AS min_quantity
FROM orders;
```

#### 5. **Calculate the Sum of Stock Quantities Grouped by Product Category:**

```sql
SELECT category, SUM(stock_quantity) AS total_stock_quantity
FROM products
GROUP BY category;
```

## Join Exercises :

### 1. **Join the Customers and Orders Tables to Display Customer Names and Their Order Details**
```sql
SELECT c.customer_name, o.order_id, o.order_date, o.total
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

### 2. **Perform an Inner Join Between Products and Orders to Retrieve Product Names and Quantities Sold**
```sql
SELECT p.product_name, o.quantity
FROM products p
JOIN orders o ON p.product_id = o.product_id;
```

### 3. **Use a Left Join to Display All Products, Including Those That Have Not Been Ordered**
```sql
SELECT p.product_name, o.order_id, o.quantity
FROM products p
LEFT JOIN orders o ON p.product_id = o.product_id;
```

### 4. **Join Employees with Departments and List Employee Names and Their Respective Department Names**
```sql
SELECT e.employee_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id;
```

### 5. **Perform a Self-Join on an Employees Table to Show Pairs of Employees Who Work in the Same Department**
```sql
SELECT e1.employee_name AS Employee1, e2.employee_name AS Employee2, e1.department_id
FROM employees e1
JOIN employees e2 ON e1.department_id = e2.department_id
WHERE e1.employee_id <> e2.employee_id;
```

## Subquery Exercises :

### 1. **Find Products Whose Price is Higher Than the Average Price of All Products**
```sql
SELECT product_name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);
```

### 2. **Retrieve Customer Names Who Have Placed at Least One Order by Using a Subquery**
```sql
SELECT customer_name
FROM customers
WHERE customer_id IN (SELECT customer_id FROM orders);
```

### 3. **Find the Top 3 Most Expensive Products Using a Subquery**
```sql
SELECT product_name, price
FROM products
WHERE price IN (
    SELECT TOP 3 price 
    FROM products 
    ORDER BY price DESC
)
ORDER BY price DESC;
```

### 4. **List All Employees Whose Salary is Higher Than the Average Salary of Their Department**
```sql
SELECT employee_name, salary, department_id
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department_id = e.department_id
);
```

### 5. **Find Employees Who Earn More Than the Average Salary of All Employees in Their Department Using a Correlated Subquery**
```sql
SELECT employee_name, salary, department_id
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

## Grouping and Summarizing Data Exercises

### 1. **Group Orders by Customer and Calculate the Total Amount Spent by Each Customer**
```sql
SELECT c.customer_name, SUM(o.total) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name;
```

### 2. **Group Products by Category and Calculate the Average Price for Each Category**
```sql
SELECT category, AVG(price) AS average_price
FROM products
GROUP BY category;
```

### 3. **Group Orders by Month and Calculate the Total Sales for Each Month**
```sql
SELECT DATEPART(MONTH, order_date) AS order_month, SUM(total) AS total_sales
FROM orders
GROUP BY DATEPART(MONTH, order_date)
ORDER BY order_month;
```

### 4. **Group Products by Category and Calculate the Number of Products in Each Category**
```sql
SELECT category, COUNT(*) AS number_of_products
FROM products
GROUP BY category;
```

### 5. **Use the HAVING Clause to Filter Groups of Customers Who Have Placed More Than 5 Orders**
```sql
SELECT c.customer_name, COUNT(o.order_id) AS number_of_orders
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name
HAVING COUNT(o.order_id) > 5;
```


## Set Operations (UNION, INTERSECT, EXCEPT):


### 1. **Combine the Results of Two Queries That Return the Names of Customers from Different Tables Using UNION**
```sql
SELECT customer_name FROM customers_online
UNION
SELECT customer_name FROM customers_store;
```

### 2. **Find Products That Are in Both the Electronics and Accessories Categories Using INTERSECT**
```sql
SELECT product_name 
FROM products 
WHERE category = 'Electronics'
INTERSECT
SELECT product_name 
FROM products 
WHERE category = 'Accessories';
```

### 3. **Find Products That Are in the Electronics Category but Not in the Furniture Category Using EXCEPT**
```sql
SELECT product_name 
FROM products 
WHERE category = 'Electronics'
EXCEPT
SELECT product_name 
FROM products 
WHERE category = 'Furniture';
```
...........


