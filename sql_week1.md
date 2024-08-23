
# WEEK 1 SQL ASSIGNMENT : : 

## 1. DATE FUNCTIONS
1. Calculate the number of months between your birthday and the current date.

> select datediff(month, '2002-06-22',getdate());

2. Retrieve all orders that were placed in the last 30 days.


>SELECT * FROM Orders
WHERE orderdate >= DATEADD(DAY, -30, GETDATE());

3. Write a query to extract the year, month, and day from the current date.

> select year(getdate()) as y,month(getdate()) as m,day(getdate()) as d;

4. Calculate the difference in years between two given dates.


> select datediff(year,'2002-06-22','2024-06-22') as diff;

5. Retrieve the last day of the month for a given date.
> select day(EOMONTH('2020-05-01'));


## 2. STRING FUNCTIONS

1. Convert all customer names to uppercase.

> select upper(name) from employee;

2. Extract the first 5 characters of each product name.
>select substring(productname,1,5) as shortword from products;

3. Concatenate the product name and category with a hyphen in between.
> select concat(productname, '-' , category) as word from products;

4. Replace the word 'Phone' with 'Device' in all product names.


>select replace ( productname,'phone', 'device') as replaced from products;

5. Find the position of the letter 'a' in customer names.

>select charindex('a',name) as position from employee;

## 3. AGGREGATE FUNCTIONS
1. Calculate the total sales amount for all orders.
> select sum(totalamount) as total from orders;


2. Find the average price of products in each category.

>select category,avg(price) from products
group by category;

3. Count the number of orders placed in each month of the year.

>select month(orderdate) as monthss, count(orderid) as ordersInMonth from orders
group by month(orderdate);

4. Find the maximum and minimum order quantities
> select max(quantity) as maxi , min(quantity) as mini from orders;

5. Calculate the sum of stock quantities grouped by product category.

>select category, sum(stockquantity) as total from products
group by category;

## 4. JOINS
1. Write a query to join the Customers and Orders tables to display customer names and their order details.

> since theres no customer table..i have created one and inserted values

```
create table Customer(
customerid int not null primary key,
customername varchar(50),
orderid int,
foreign key(orderid) references orders(orderid)
);
```

```
insert into Customer values
(10,'jammu',1),
(11,'ramya',2),
(12,'dhanu',1),
(13,'somu',1),
(14,'sathya',1),
(15,'ravi',4),
(16,'kavya',1),
(17,'jay',1)
```

>select *from customer
left join orders on 
customer.orderid = orders.orderid;

2. Perform an inner join between Products and Orders to retrieve product names and quantities sold.

> select productname,quantity from products
inner join orders on
products.productid= orders.productid;


3. Use a left join to display all products, including those that have not been ordered.

>select distinct productname from products
left join orders on
products.ProductID=orders.ProductID;

4. Write a query to join Employees with Departments and list employee names and their respective department names.

>select name,departmentname from employee
join Departments on 
Employee.DepartmentId =Departments.id;

5. Perform a self-join on an Employees table to show pairs of employees who work in the same department.

>select e1.name as empnam1 , e2.name as empname2 
from employee e1
join Employee e2 on
 e1.DepartmentId=e2.DepartmentId
where e1.id<>e2.id;

## 5. SUBQUERY

1. Write a query to find products whose price is higher than the average price of all products.

>select productname from products 
where price > (
select avg(price) from products);

2. Retrieve customer names who have placed at least one order by using a subquery.
>select customername from customer
where orderid in (select orderid from orders);

3. Find the top 3 most expensive products using a subquery.

>select productname,price from products 
where price in (select price from products
order by price desc
offset 0 rows
fetch next 3 rows only);

4. Write a query to list all employees whose salary is higher than the average salary of their department.
>select id,name,salary,DepartmentId from employee e
where salary> (select avg(salary) from employee
where DepartmentId=e.departmentid
);

5. Use a correlated subquery to find employees who earn more than the average salary of all employees in their department.

>select distinct Departmentid , id,name from Employee e
where DepartmentId=(select DepartmentId from employee
where salary > (select avg(salary) from employee)
group by DepartmentId)
;

## 6. GROUPING
1. Group orders by customer and calculate the total amount spent by each customer.

>select customername,sum(totalamount) as total
from customer
left join orders on
customer.orderid=orders.orderid
group by customername;


2. Group products by category and calculate the average price for each category.

>select category, avg(price) as average from Products
group by category;

3. Group orders by month and calculate the total sales for each month.
>select month(orderdate) as months ,sum(quantity) as sales from orders
group by month(orderdate);

4. Write a query to group products by category and calculate the number of products in each category.
>select category ,count(category) as number from products
group by Category;

5. Use the HAVING clause to filter groups of customers who have placed more than 5 orders.

>select customername,orders.orderid as total
from customer 
join orders on 
customer.orderid = orders.orderid
group by orders.orderid,customername
having count(orders.orderid)>5

## 7. SET OPERATIONS

1. Write a query to combine the results of two queries that return the names of customers from different tables using UNION.

>select customername from customer
where orderid=1
union
select customername from orders
join customer on 
customer.orderid=orders.orderid
where quantity>2;

2. Find products that are in both the Electronics and Accessories categories using INTERSECT.
> select productname from products 
where category = 'Electronics'
intersect 
select productname from products 
where category = 'Accessories';

3. Write a query to find products that are in the Electronics category but not in the Furniture category using EXCEPT.

>select productname from products 
where category = 'Electronics'
except
select productname from products 
where category = 'Furniture';
