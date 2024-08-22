![image](https://github.com/user-attachments/assets/80f42c7b-8952-4d5a-b8df-c662af177785)

```sql
-- Drop the existing company database if it exists
---DROP DATABASE if exists company;

-- Create a new company database
CREATE DATABASE company;

-- Create a department table
CREATE TABLE department (
    ID INT PRIMARY KEY,
    DepartmentName VARCHAR(50),
    Location VARCHAR(50),
    DepartmentHead VARCHAR(50)
);

-- Create an employee table
CREATE TABLE employee (
    ID INT PRIMARY KEY,
    Name VARCHAR(50),
    Gender VARCHAR(50),
    Salary INT DEFAULT 20000,
    DepartmentId INT,
    FOREIGN KEY (DepartmentId) REFERENCES department(ID)
);

-- Insert values into the department table
INSERT INTO department VALUES
(1, 'IT', 'LONDON', 'RICK'),
(2, 'Payroll', 'Delhi', 'Ron'),
(3, 'HR', 'New York', 'Christie'),
(4, 'Other Department', 'Sydney', 'Cindrella');

-- Insert values into the employee table
INSERT INTO employee VALUES
(1, 'Tom', 'Male', 4000, 1),
(2, 'Pam', 'Female', 3000, 3),
(3, 'John', 'Male', 3500, 1),
(4, 'Sam', 'Male', 4500, 2),
(5, 'Todd', 'Male', 2800, 2),
(6, 'Ben', 'Male', 7000, 1),
(7, 'Sara', 'Female', 4800, 3),
(8, 'Valarie', 'Female', 5500, 1),
(9, 'James', 'Male', 6500, NULL),
(10, 'Russell', 'Male', 8800, NULL);



![image](https://github.com/user-attachments/assets/d1cf54b7-2c2a-4fc2-9399-ea87ae41bf1c)


```sql
------ 2nd Activity
create  table products(
p_id int primary key ,
P_name varchar(50),
price decimal Not Null);

create table orders(
o_id int primary key,
p_id int Not null foreign key references products(p_id),
quantity int default 1,
o_date date
)

insert into products Values
(1, 'Laptop', 800.00),
(2, 'Smartphone', 500.00),
(3, 'Tablet', 300.00),
(4, 'Headphones', 50.00),
(5, 'Monitor', 150.00);

INSERT INTO Orders (o_id, p_id, quantity, o_date) VALUES
(1, 1, 2, '2024-08-01'),
(2, 2, 1, '2024-08-02'),
(3, 3, 3, '2024-08-03'),
(4, 1, 1, '2024-08-04'),
(5, 4, 4, '2024-08-05'),
(6, 5, 2, '2024-08-06'),
(7, 5, 1, '2024-08-07'); -- inserting 5 as product id bcs product table have no p_id 6
```

### Assigment:
# SQL Function and Query Exercises

## Date Function Exercises

1. **Calculate the number of months between your birthday and the current date.**

2. **Retrieve all orders that were placed in the last 30 days.**

3. **Extract the year, month, and day from the current date.**

4. **Calculate the difference in years between two given dates.**

5. **Retrieve the last day of the month for a given date.**

## String Function Exercises

1. **Convert all customer names to uppercase.**

2. **Extract the first 5 characters of each product name.**

3. **Concatenate the product name and category with a hyphen in between.**

4. **Replace the word 'Phone' with 'Device' in all product names.**

5. **Find the position of the letter 'a' in customer names.**

## Aggregate Function Exercises

1. **Calculate the total sales amount for all orders.**

2. **Find the average price of products in each category.**

3. **Count the number of orders placed in each month of the year.**

4. **Find the maximum and minimum order quantities.**

5. **Calculate the sum of stock quantities grouped by product category.**

## Join Exercises

1. **Write a query to join the Customers and Orders tables to display customer names and their order details.**

2. **Perform an inner join between Products and Orders to retrieve product names and quantities sold.**

3. **Use a left join to display all products, including those that have not been ordered.**

4. **Write a query to join Employees with Departments and list employee names and their respective department names.**

5. **Perform a self-join on an Employees table to show pairs of employees who work in the same department.**

## Subquery Exercises

1. **Write a query to find products whose price is higher than the average price of all products.**

2. **Retrieve customer names who have placed at least one order by using a subquery.**

3. **Find the top 3 most expensive products using a subquery.**

4. **Write a query to list all employees whose salary is higher than the average salary of their department.**

5. **Use a correlated subquery to find employees who earn more than the average salary of all employees in their department.**

## Grouping and Summarizing Data Exercises

1. **Group orders by customer and calculate the total amount spent by each customer.**

2. **Group products by category and calculate the average price for each category.**

3. **Group orders by month and calculate the total sales for each month.**

4. **Write a query to group products by category and calculate the number of products in each category.**

5. **Use the HAVING clause to filter groups of customers who have placed more than 5 orders.**

## Set Operations (UNION, INTERSECT, EXCEPT)

1. **Write a query to combine the results of two queries that return the names of customers from different tables using UNION.**

2. **Find products that are in both the Electronics and Accessories categories using INTERSECT.**

3. **Write a query to find products that are in the Electronics category but not in the Furniture category using EXCEPT.**

