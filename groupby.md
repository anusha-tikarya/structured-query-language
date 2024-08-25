```sql
create database salesDB
use salesDB;
create table customers(
cID int primary key,
CName varchar(50) Not Null,
country varchar(30),
city varchar(30),
Age Int);

create table orders(
OrderID INT Primary Key,
CustomerID INT Foreign Key references Customers(cID),
OrderDate DATE,
TotalAmount DECIMAL)

create table products(
pID Int Primary key,
pNAme Varchar(50),
category Varchar(50),
price decimal)

create table  OrderDetails(
ODID INT Primary Key,
OrderID INT Foreign Key references Orders(OrderID),
ProductID INT Foreign Key references Products(PID),
Quantity INT)

-- Insert Data into Customers Table
INSERT INTO Customers (cID, CName, Country, City, Age) VALUES
(1, 'John Doe', 'USA', 'New York', 28),
(2, 'Jane Smith', 'Canada', 'Toronto', 34),
(3, 'Sam Johnson', 'USA', 'Chicago', 45),
(4, 'Emily Davis', 'UK', 'London', 30),
(5, 'Michael Brown', 'Australia', 'Sydney', 40);

-- Insert Data into Orders Table
INSERT INTO Orders (OrderID, CustomerID, OrderDate, TotalAmount) VALUES
(1, 1, '2024-08-01', 250.50),
(2, 1, '2024-08-15', 150.75),
(3, 2, '2024-08-10', 350.00),
(4, 3, '2024-08-18', 450.50),
(5, 4, '2024-08-21', 200.00);

-- Insert Data into Products Table
INSERT INTO Products (pID, pName, Category, Price) VALUES
(1, 'Laptop', 'Electronics', 1000.00),
(2, 'Smartphone', 'Electronics', 600.00),
(3, 'Desk', 'Furniture', 150.00),
(4, 'Chair', 'Furniture', 85.00),
(5, 'Headphones', 'Electronics', 100.00);

-- Insert Data into OrderDetails Table
INSERT INTO OrderDetails (ODID, OrderID, ProductID, Quantity) VALUES
(1, 1, 1, 1),
(2, 1, 5, 2),
(3, 2, 2, 1),
(4, 3, 3, 1),
(5, 4, 4, 3),
(6, 4, 1, 1),
(7, 5, 5, 2);

--Question: List the number of customers from each country.

select count(*) as Total_Customer  , country from customers
group by country

--Question: Find the total number of orders placed each day.
select count(*) , orderdate from orders
group by OrderDate

--Question: Calculate the total quantity ordered for each product.
select count(*) as TotalOrder , ProductID
from OrderDetails 
group by ProductID

--Question: Show the total sales amount for each customer.
select sum(totalAmount) as TotalAmount , customerId from orders
group by CustomerID

--Question: Find the number of orders placed by each customer.
select  count(orderID) as TotalOrders, customerID from orders
group by CustomerID;


--Question: Find the total sales for each country, but only include countries with total sales above 10,000.
select  sum(o.TotalAmount)as TotalAmount , c.country 
from orders o inner join customers c on o.CustomerID= c.cID
group by country
having sum(o.TotalAmount) > 10000;


--Question: List the number of customers in each city, but only for cities with more than 5 customers.
select city, count(cid) AS City from customers
group by city
having count(cid)>5

--Question: Find the average order amount for each customer and show only those customers who have an average order amount greater than 500
select avg(o.totalamount) as avgAmount, c.cid  from orders as o join  customers as c on o.CustomerID = c.cID
group by c.cID 
having avg(o.totalamount) > 500;

--Question: List the total quantity ordered for each product category, but only include categories with a total quantity ordered greater than 100.
select count(odid) as total  , productID from orderdetails
group by ProductID
having  count(odid) >100;

--Question: Calculate the total number of products ordered by each customer and show only those customers who ordered more than 20 products in total.
select count(quantity) as NUMoFproducts , o.customerID
from OrderDetails as od inner join orders as o 
on od.OrderID = o. OrderID
group by o.CustomerID
having count(Quantity)>20

--Question: Find the top 3 customers with the highest total sales.
select top 3 customerID , sum(totalamount) as totalsales
from orders
group by CustomerID
ORDER BY totalsales desc

--Q1: Write a query to list all the customers and their corresponding orders. If a customer has not placed any orders, still include their information in the result set.
select c.cName , o.orderID from customers  c left join orders  o
on  c.cID = o.CustomerID;
```
