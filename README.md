--section 1: creating and managing the database 
--1
create database store;
--2
exec sp_databases 
--3
use store;
--4
create database temp_test;
--5
drop  database temp_test;
--6
select db_name() 
--7 database consists of number of ralated tabes
--section 2:creating tables and choosing data types
--8
create table department (
department_id int identity(1,1) primary key ,
department_name varchar(20) not null);
--9
create table employee(
employee_id int identity(1,1) primary key,
first_name varchar(15) ,
last_name varchar(15),
email varchar(30),
hire_date date ,
salary decimal(15,5),
department_id int foreign key references departments(department_id)); 
--10
create table project(
project_id int identity(1,1) primary key,
project_name varchar(15),
start_date date,
end_date date,
department_id int foreign key references departments(department_id));
--11
create table customer(
customer_id int identity(1,1) primary key,
full_name varchar(25) ,
email varchar(30),
city varchar(10),
join_date date);
--12
create table product(
product_id int identity(1,1) primary key,
product_name varchar(20),
category varchar(20),
price decimal(10,3),
stock_quantity int);
--13
create table orders(
order_id int identity(1,1) primary key,
customer_id int foreign key references customers(customer_id),
order_date date,
order_status varchar(15));
--14
create table order_item(
order_item_id int identity(1,1) primary key,
order_id int foreign key references orders(order_id),
product_id int foreign key references products(product_id),
quantity int,
unit_price decimal(10,2));
--15
exec sp_tables;
--16
select*from employees;
--17 each employee works at a spisific department
--section 3:constraints and relationships
--18
alter table employees 
add unique (email);
--19
alter table employees
alter column  hire_date date NOT NULL;
--20
alter table employees
add constraint salary_check check (salary>0);
--21 already added
--22 already added
--23 already added
--24 already added
--25
alter table orders
add constraint status_check default 'pending' for order_status; 
--26
alter table customers
add constraint un_email unique(email);
--27 fk exists in two tables to make them related
--section 4: altering tables
--28
alter table employees 
add phone_number varchar(10);
--29
alter table employees
add is_manager bit not null default 0;
--30
exec sp_rename 'customers.full_name','customer_name','column';
--31
alter table employees
alter column phone_number varchar(20);
--32
alter table order_items 
add discount decimal(10,2) default 0;
--33
alter table employees 
drop constraint DF__employees__is_ma__5EBF139D; 
alter table employees 
drop column is_manager;
--34
exec sp_rename 'order_items','order_line_items';
exec sp_rename 'order_line_items','order_items';
--35
alter table projects
add budget decimal(10,2);
--36
alter table employees
drop constraint salary_check;
--37
create database temp_test;
create table test(name int);
truncate table test;
--section 5: inserting data
--38
insert into departments(department_name)
values
('HR'),
('Sales'),
('IT'),
('Marketing');
--39 added by hand(edit first 200 raws)
select *from employees;
--40
insert into employees(first_name,last_name,email,hire_date,salary,department_id,phone_number)
values
('sama','fars','sama@gmail.com','3/4/2020',10000,4,010023455);
select *from employees
--41 added by hand(edit top 200 raws)
select*from projects
--42 added by hand(edit top 200 raws)
select*from customers 
--43 added by hand(edit top 200 raws)
select*from products
--44 added by hand(edit top 200 raws)
select*from orders
update orders
set order_status ='shipped' where order_id= 2;
update orders
set order_status ='canceld' where order_id=1;
select*from orders
update orders
set order_status='delivered' where order_id=5;
update orders
set order_status='delivered' where order_id=6;
update orders
set order_date='12/1/2022' where order_id=4;
select*from orders-- fixing some  wrong input
--45 added by hand(edit top 200 raws)
select*from order_items
--46
insert into employees(employee_id,email)
values(12,'asmaa@gmail.com');--rejected
--47
insert into departments(department_name)
values('finance'),('legal');
select*from departments
--section 6 :updating data
--48
update employees
set salary= salary +(salary*0.12) where department_id=3;
select*from employees
--49
update employees
set email='hady@gmail.com' where employee_id=6;
select*from employees
--50
update orders
set order_status='comleted' where order_date< '1/1/2022';
select*from orders
--51
update products
set stock_quantity=233
where product_id=3;
update products
set stock_quantity= stock_quantity+50 
where product_id=3;
select*from products
--52
update products
set price=price+(price*0.15) where category='school';
select*from products
--53
update employees
set phone_number=0122345676 where employee_id=13;
select*from employees
--54
update employees
set department_id=3 where employee_id=4;
select*from employees
--55
select*from order_items
update order_items
set discount= discount+(discount*0.05) where quantity>3;
--56
update employees
set email='test@gmail.com';
--i did not excute this comand because it will change the whole email column and this is dangerous
--57
update projects
set end_date='3/4/2027' where project_id=6;
select*from projects
--section 7:deleting data
--58
delete from customers where customer_id=8;
select*from customers
--59
delete from products where stock_quantity=0;
select*from products
--60
select*from departments
delete from departments where department_id=1;
--(error)The DELETE statement conflicted with the REFERENCE constraint "FK__employees__
--depar__4CA06362". The conflict occurred in database "store", table "dbo.employees", column 'department_id'.
--61
select*from orders
update orders
set order_status='dilevered' where order_status='comleted';
delete from order_items where order_id=(
select top 1 order_id from orders where order_status='cancelled');
--62
delete from orders where order_id=(
select top 1 order_id from orders where order_status='cancelled');
select*from orders
--63
delete from projects where end_date<'1/1/2020';
select*from projects
--section 8: basic select queries
--64
select*from employees
--65
select first_name ,last_name , salary from employees;
--66
select distinct city from customers;
--67
select distinct category from products;
--68
select product_name , price as unit_cost from products;
--69
select top 5 *from orders;
--70
select first_name +' '+last_name as full_name from employees;
--71
select* ,price*0.9  as new_discount from products;
--72
alter table employees
add annual_salary as (salary*12 );
select *from employees;
--73
select*from departments order by department_name asc;
--section 9:filtering with where
--74
select*from employees where salary>5000;
--75
select*from employees where department_id=2;
--76
select* from products where price between 20 and 100; 
--77
select *from customers where city='giza';
--78
select *from orders where order_status in ('pending','shipped');
--79
select* from employees where first_name like'a%';
--80
select *from  products where product_name like'%pro%';
--81
select*from employees where department_id!=3;
--82
select* from employees where phone_number is null;
--83
select * from employees where phone_number is not null;
--84
select * from orders where order_date>'7/30/2026';
--85
select *from products where price>50 and stock_quantity>10;
--86
select * from customers where year(join_date) =2023 or city='alex';
--section 10: sorting and limiting results
--87
select * from employees order by salary desc;
--88
select * from products order by category , price asc;
--89
select top 3* from employees order by hire_date desc ;
--90
select top 5* from products order by price asc;
--91
select *from customers order by join_date desc OFFSET 5 rows fetch next 5 rows only;
--section 10B: ranking with top /rank / row_number
--92
select top 3* from employees order by salary desc;
--93
select top 5* from products order by price desc;
--94
select employee_id ,row_number() over ( order by salary desc) as highest_salary from employees;
--95
select employee_id,first_name,department_id , 
rank() over(partition by department_id order by salary desc) as highest_sal from employees;
--96
select employee_id,first_name,department_id , 
dense_rank() over(partition by department_id order by salary desc) as highest_sal from employees;
--dense_rannk does not leave a gap when similar number appers
--97
select employee_id,first_name, department_id,salary from(
select employee_id ,first_name,department_id ,salary, DENSE_RANK() 
over (partition by department_id order by salary desc)
as highest_emplo from employees)as ranked where highest_emplo=1;
--98
select product_id,category,price from(
select product_id,category,price,row_number() over (partition by category order by price asc) 
as cheapest_items from products) as rankedc where cheapest_items in(1,2);
--section 11:aggregate functions and group by
--99
select count(employee_id) as total_emp from employees;
--100
select department_id, count(employee_id)
as total_emp_dep from employees group by department_id;
--101
select AVG(salary) as averge_sal from employees;
--102
select department_id , avg(salary)
as avg_per_dep from employees group by department_id;
--103
select max(salary) as highest_sal , min(salary) as lowest_sal from employees;
--104
select sum(price* stock_quantity) as total_value from products;
--105
select customer_id , count(order_id) 
as order_per_customer from orders group by customer_id;
--106
select product_id , sum(quantity*unit_price)
as total_revenue from order_items group by product_id;
--107
select category , count(product_id) as product_num
from products group by category having count(product_id)>2;
--108
select department_id , avg(salary) avg_per_dep
from employees group by department_id having avg(salary)>6000;
--109
select order_status, count(order_id) 
as total_orders from orders group by order_status;
--110
select min(hire_date) as earlist_emp , max(hire_date) as latest_emp from employees;
-- section 12:joins
--111
select first_name,department_name from employees e 
join departments d on e.department_id=d.department_id;
--112
select department_name,employee_id from departments d 
left join employees e on d.department_id=e.department_id;
--113
select c.customer_id,customer_name,order_id from customers c 
left join orders o on c.customer_id=o.customer_id;
--114
select order_id ,customer_name from orders o 
inner join customers c on o.customer_id=c.customer_id;
--115
select o.order_id,product_name ,quantity from orders o 
inner join order_items ot on o.order_id=ot.order_id
inner join products p on p.product_id=ot.product_id;
--116
select first_name, department_name,project_name 
from employees e inner join departments d on e.department_id=d.department_id
inner join projects p on d.department_id=p.department_id;
--117
select customer_name,
sum((quantity*unit_price)-discount ) as total_paid 
from customers c join orders o on c.customer_id=o.customer_id
join order_items ot on ot.order_id=o.order_id
group by customer_name;
--118
select product_name ,p.product_id from products p 
left join order_items ot on p.product_id=ot.product_id 
where ot.product_id is  null;
--119
select employee_id , first_name ,d.department_id,department_name ,project_name
from departments d left join projects p on d.department_id= p.department_id
join employees e on d.department_id= e.department_id
where project_name is null;
--120
select first_name,highest_sal, department_name from employees e 
join departments d on e.department_id=d.department_id
join( select department_id ,max(salary) as highest_sal from employees 
group by department_id) m on e.department_id=m.department_id;
--section 13:subqueries
--121
select first_name ,salary from (
select first_name,salary,avg(salary) over()  as avergesal from employees )as higsal where salary>avergesal;
--122
select product_name,price,category from( 
select product_name,price,category, avg(price) over () as avergprice from products  
) as avgcatprice where price> avergprice;
--123
select c.customer_name,c.customer_id from customers c 
left join orders o on c.customer_id=o.customer_id  where o.order_id is null;
--124
select department_name,sum(salary) as tot_sal_cost from departments d join employees e 
on d.department_id=e.department_id
group by department_name order by tot_sal_cost desc;
--125
select top 1
product_name , sum(quantity)as totqnt from 
products p join order_items ot on p.product_id=ot.product_id 
group by product_name order by totqnt desc;
--section 14:views,indexes and a final challenge 
--126
CREATE VIEW 
employee_department_view AS 
select
first_name,salary,department_name from employees e 
join departments d on e.department_id=d.department_id;
--127
select first_name,salary from employee_department_view where salary>5000;
--128
drop view employee_department_view;
--129
create index email_customer on customers(email);
--130 indexex speed the proccess of retriving data as it 
--jumps to the matching records insted of checking raw by raw
--131
select c.customer_name,count(distinct o.order_id) as no_orders,
sum((quantity*unit_price)-discount) as tot_spent,
max(order_date) as most_recent 
from customers c join orders o on c.customer_id=o.customer_id
join order_items ot on o.order_id=ot.order_id
group by customer_name
order by tot_spent desc;
--132
select top 1
d.department_id,sum(salary) as total_value from departments d 
join employees e on d.department_id=e.department_id
where employee_id in (
select project_id from projects where end_date >GETDATE())
group by d.department_id,department_name order by total_value desc;
--133
create table suppliers(
suppliers_id int primary key,
suppliers_name varchar(20) ,
product_id int foreign key references products(product_id),
product_price int );
insert into suppliers(suppliers_id,suppliers_name,product_id,product_price) 
values
(1,'sameh',3,15),
(2,'saad',5,5),
(3,'hany',2,2);
select*from suppliers;
--qurey to find supplier with best profit 
select top 1 suppliers_name,sum(price-s.product_price) as  highprofit 
from suppliers s join products p on s.product_id=p.product_id 
group by suppliers_name order by highprofit desc;


