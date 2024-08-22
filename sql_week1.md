
# WEEK 1 SQL ASSIGNMENT : : 

## 1. DATE FUNCTIONS
--2
select datediff(month, '2002-06-22',getdate());

use university
select * from orders;
--3 therla
select datediff(orderdate,-30,getdate()) from orders;

--4
select year(getdate()) as y,month(getdate()) as m,day(getdate()) as d;

--5
select datediff(year,'2002-06-22','2024-06-22') as diff;

--6 

## 2. STRING FUNCTIONS
--10
select * from employee;

select upper(name) from employee;

--11
select substring(productname,1,5) as shortword from products;

--12
select concat(productname, '-' , category) as word from products;

--13
select replace ( productname,'phone', 'device') as replaced from products;

--14
select charindex('e',name) as position from employee;

## 3. AGGREGATE FUNCTIONS
--17
select * from orders;

select sum(totalamount) as total from orders;

--18
select * from products;

select category,avg(price) from products
group by category;

--19
select month(orderdate) as monthss, count(orderid) as ordersInMonth from orders
group by month(orderdate);

--20
select max(quantity) as maxi , min(quantity) as mini from orders;

--21
select category, sum(stockquantity) as total from products
group by category;

## 4. JOINS
--24
select * from tblPerson
--25
select * from products;
select* from orders;

select productname,quantity from products
inner join orders on
products.productid= orders.productid;

--26
select distinct productname from products
left join orders on
products.ProductID=orders.ProductID;

--27
select * from employee;
select * from Departments;

select name,departmentname from employee
join Departments on 
Employee.DepartmentId =Departments.id;

--28
select e1.name as empnam1 , e2.name as empname2 from employee e1
join Employee e2 on
 e1.DepartmentId=e2.DepartmentId
where e1.id<>e2.id;

## 5. SUBQUERY
--32
select productname from products 
where price > (
select avg(price) from products);

--33
--customer

--34
select productname,price from products 
where price in (select price from products
order by price desc
offset 0 rows
fetch next 3 rows only);


--35
select id,name,salary,DepartmentId from employee e
where salary> (select avg(salary) from employee
where DepartmentId=e.departmentid
);

--36(doubt)
select distinct Departmentid , id,name from Employee e
where DepartmentId=(select DepartmentId from employee
where salary > (select avg(salary) from employee)
group by DepartmentId)
;

## 6. GROUPING
--40
--customer

--41
select* from Products;
select* from orders;


select category, avg(price) as average from Products
group by category;

--42
select month(orderdate) as months ,sum(quantity) as sales from orders
group by month(orderdate);

--43
select category ,count(category) as number from products
group by Category;

--44
--customer

## 7. SET OPERATIONS

--48
--customer

--49
select productname from products 
where category = 'Electronics'
intersect 
select productname from products 
where category = 'Accessories';

--50

select productname from products 
where category = 'Electronics'
except

select productname from products 
where category = 'Furniture';
