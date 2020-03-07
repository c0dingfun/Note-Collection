--Create Database Sample2
 
-- Rename Database
--Alter Database Sample2 Modify Name = Sample333
--sp_renamedb 'Sample111', 'Sample222'

-- Drop Database
--drop database Sample333

-- Single user database
-- Alter Database DataBaseName Set Single_User with Rollack Immediate
 

 --Create Table tblGender
 --(
 --ID int NOT NULL Primary Key,
 --Gender nvarchar(50) NOT NULL
 --)
 
-- alter table tblPerson
--add constraint FK_GenderID
--Foreign Key (GenderId) References tblGender(ID)

--select * from tblPerson
--select * from tblGender

--Insert Into tblPerson (Id, Name, Email)
--Values(3, 'c', 'c@c.com')

--select * from tblPerson

-- Creating the default Constraint for a column
-- so that default value is used when no value is
--Provided. (note: NULL is considered a value)

--alter table tblPerson
--Add Constraint DefaultValue
--Default 1 for GenderId

--insert into tblPerson (Id, Name, Email)
--values(6, 'h', 'h@h.com')

-- dropping the constraint
--alter table tblPerson
--drop constraint DefaultValue

-- Cascading Referential integrity
-- defines the actions SQL serer should take
-- when a user attempts to delete or update
-- a key to which  an existing foreign keys points
-- By default, such an attempt will failed and
-- the action will be rollback

-- Options:
--  1) No Actions -- attempt to delete or update is rollaback
--  2) Cascade -- all rows containing the foreign keys also deleted or updated
--  3) Set Null
--  4) Set default <<< control by the Default Constraint

-- Check Constraint
-- intended to limit the value of a column, for example: age
--ALTER TABLE tblPerson
--ADD CONSTRAINT CK_tblPerson_Age 
--CHECK (Age > 0 AND Age < 150)

--General form
--ALTER TABLE { TABLE_NAME }
--ADD CONSTRAINT { CONSTRAINT_NAME } 
--CHECK ( BOOLEAN_EXPRESSION )

-- Identity column
-- Have the SQL server automatically generate value
-- so that when we insert something into the table
-- with Identity column, we don't need to provide value.
--Create Table tblPerson
--(
--PersonId int Identity(1,1) Primary Key,
--Name nvarchar(20)
--)


-- If a column is deleted, there will be gap in the 
-- identity column, because SQL server won't fill the gap.
-- We have to manually fill the gap, by
-- 1) SET Identity_Insert tblPerson ON
-- 2) Insert into tblPerson(PersonId, Name) values(2, 'John')
-- to provide a value for the Identity column.
-- 3) Turn it off!!!!
-- SET Identity_Insert tblPerson ON

-- If data in the whole table is deleted, we need to reset
-- the Identity Column, we need to RESEED it.
--DBCC CHECKIDENT(tblPerson, RESEED, 0)

--https://codingsight.com/sql-server-delete-vs-truncate/
--TRUNCATE:
--TRUNCATE is a DDL command.
--TRUNCATE TABLE always locks the table and page. However, it doesn’t lock each row.
--The WHERE condition can not be used.
--The command removes all the data.
--TRUNCATE TABLE cannot activate a trigger because the operation does not log individual row deletions.
--Faster in performance, because it does not keep logs.
--Rollback is possible.
--DELETE:
--DELETE is a DML Command.
--DELETE statement uses a row lock while executing. Each row in the table is locked for deletion.
--You can specify filters in WHERE clause.
--The command deletes specified data if the WHERE condition exists.
--DELETE activates a trigger because the operations are logged individually.
--DELETE is slower than TRUNCATE because it keeps logs.
--Rollback is possible.

-- Retrieve last Identity Value
--Select SCOPE_IDENTITY() -- same session in the same scope
--Select @@IDENTITY       -- same session and across any scope
--Select IDENT_CURRENT('tblPerson') -- specific table of any session and scope

-- Unique Constraint
-- used to enforce uniqueness of a column, ie: the column shouldn't
-- allow any duplicate values. 

--Alter Table Table_Name
--Add Constraint Constraint_Name Unique(Column_Name)

-- Alter table tblPerson
-- Add Constraint UQ_tblPerson_Email Unique(Email)

-- Unique Key vs Primary Key
-- A table can have multiple unique key
-- A table can only have one primary key
-- an Employee table:
-- has a Employee Id --as a Identity column and Primary Key
-- has an email and ssn column, they both should be unique
-- so those two columns will have unique key constraints

-- Select

-- Distinct row
-- Select distinct city from tblPerson

--Select distinct name, city from tblPerson
-- get all row of distinct combining name and city

--select * from tblPerson where city = 'London'
-- select * from tblPerson where city <> 'London'
-- select * from tblPerson where city != 'London'

--Comparison Operators and Wild Cards
--= , equal to (! ==)
--!= or <> , no equal to
--> , greater than
-->= , greater than or equal to 
--< , less than
--<= , less than or equal to 
--IN , specify a list values
--BETWEEN , specify a range
--LIKE , specify a pattern
--NOT , not in a list range etc
--% , specify zero or more charaters
--_ , specify exactly one charater
--[] , ay character within the brackets
--[^] , not any charactres within the brackets

-- select * from tblPerson where age in (20, 23, 29)
-- select * from tblPerson where age between 20 and 25
-- select * from tblPerson where city like 'L%'
-- select * from tblPerson where city NOT LIKE 'L%'
-- select * from tblPerson where email LIKE '_@_.com'
-- select * from tblPerson where name LIKE '[MST]%'

-- select * from tblPerson 
-- where ( city = 'London' or city = 'Mumabi') and age > 25 

-- select * from tblPerson Order By Name desc
-- select * from tblPerson Order by Age asc << default

-- select * from tblPerson order by Name DESC, Age ASC

-- select top 10 * from tblPerson
-- select top 10 name, age from tblPerson
-- select top 1 percent * from tblPerson

-- **** useful to find the top age
-- select top 1 * from tblPerson order by age desc


-- Group By *****
--In SQL Server we have got lot of aggregate functions. 
--Examples
--1. Count()
--2. Sum()
--3. avg()
--4. Min()
--5. Max()

--Group by clause is used to group a selected set of rows 
--into a set of summary rows by the values of one or more 
--columns or expressions. It is always used in conjunction 
--with one or more aggregate functions.

-- find the each city's total salary
--select city, sum(salary) as TotalSalary from tblPerson Group By city

-- total salaries by city, by gender
--select city, gender, sum(salary) as totalsalary from tblPerson Order by city, gender

-- total salaries and total number of employees by city, by gender

--select city, gender, sum(salary) as totalsalary, count(ID) as totalEmployess
--from tblPerson
--Order by city, gender


--Filtering Groups:
--WHERE clause is used to filter rows before aggregation, 
--where as HAVING clause is used to filter groups after aggregations. 


-- Example:
--Filtering rows using WHERE clause, before aggrgations take place:

--Select City, SUM(Salary) as TotalSalary
--from tblEmployee
--Where City = 'London'
--group by City

--Filtering groups using HAVING clause, after all aggrgations take place:

--Select City, SUM(Salary) as TotalSalary
--from tblEmployee
--group by City
--Having City = 'London'

--It is also possible to combine WHERE and HAVING
--Select City, SUM(Salary) as TotalSalary
--from tblEmployee
--Where Gender = 'Male'
--group by City
--Having City = 'London'

--Difference between WHERE and HAVING clause:
--1. WHERE clause can be used with - Select, Insert, and Update statements, 
--where as HAVING clause can only be used with the Select statement.

--2. WHERE filters rows before aggregation (GROUPING), 
--where as, HAVING filters groups, after the aggregations are performed.

--3. Aggregate functions cannot be used in the WHERE clause, 
--unless it is in a sub query contained in a HAVING clause, 
--whereas, aggregate functions can be used in Having clause.


-- JOINS ***
--Joins in SQL server are used to query (retrieve) data from 2 or more 
--related tables. In general tables are related to each other using 
--foreign key constraints.

--In SQL server, there are different types of JOINS.
--1. CROSS JOIN
--2. INNER JOIN 
--3. OUTER JOIN ---> 3 types Left, Right, Full

--Outer Joins are again divided into 3 types
--1. Left Join or Left Outer Join
--2. Right Join or Right Outer Join
--3. Full Join or Full Outer Join

--Create table tblDepartment
--(
--     ID int primary key,
--     DepartmentName nvarchar(50),
--     Location nvarchar(50),
--     DepartmentHead nvarchar(50)
--)
--Go

--Insert into tblDepartment values (1, 'IT', 'London', 'Rick')
--Insert into tblDepartment values (2, 'Payroll', 'Delhi', 'Ron')
--Insert into tblDepartment values (3, 'HR', 'New York', 'Christie')
--Insert into tblDepartment values (4, 'Other Department', 'Sydney', 'Cindrella')
--Go

--Create table tblEmployee
--(
--     ID int primary key,
--     Name nvarchar(50),
--     Gender nvarchar(50),
--     Salary int,
--     DepartmentId int foreign key references tblDepartment(Id)
--)
--Go

--Insert into tblEmployee values (1, 'Tom', 'Male', 4000, 1)
--Insert into tblEmployee values (2, 'Pam', 'Female', 3000, 3)
--Insert into tblEmployee values (3, 'John', 'Male', 3500, 1)
--Insert into tblEmployee values (4, 'Sam', 'Male', 4500, 2)
--Insert into tblEmployee values (5, 'Todd', 'Male', 2800, 2)
--Insert into tblEmployee values (6, 'Ben', 'Male', 7000, 1)
--Insert into tblEmployee values (7, 'Sara', 'Female', 4800, 3)
--Insert into tblEmployee values (8, 'Valarie', 'Female', 5500, 1)
--Insert into tblEmployee values (9, 'James', 'Male', 6500, NULL)
--Insert into tblEmployee values (10, 'Russell', 'Male', 8800, NULL)
--Go


--General Formula for Joins
--SELECT      ColumnList
--FROM        LeftTableName
--JOIN_TYPE  RightTableName
--ON         JoinCondition

--CROSS JOIN
--CROSS JOIN, produces the cartesian product of the 2 tables 
--involved in the join. For example, in the Employees table we 
--have 10 rows and in the Departments table we have 4 rows. 
--So, a cross join between these 2 tables produces 40 rows. 
--Cross Join shouldn't have ON clause. 

--CROSS JOIN Query:
--SELECT Name, Gender, Salary, DepartmentName
--FROM tblEmployee
--CROSS JOIN tblDepartment

--JOIN or INNER JOIN
--Write a query, to retrieve Name, Gender, Salary and 
--DepartmentName from Employees and Departments table. 
--The output of the query should be as shown below.

--select * from tblEmployee
--select * from tblDepartment

--select Name, Gender, Salary, DepartmentName
--from tblEmployee
--Inner Join tblDepartment
--On tblEmployee.DepartmentId = tblDepartment.Id

-- in summary, INNER JOIN, returns only the matching rows 
-- between both the tables. Non matching rows are eliminated.

--LEFT JOIN or LEFT OUTER JOIN
--Now, let's say, I want all the rows from the Employees table, 
--including JAMES and RUSSELL records. 

--select * from tblEmployee
--select * from tblDepartment

--select name, gender, salary, departmentname
--from tblEmployee
--Left Join tblDepartment
--On tblEmployee.departmentId = tblDepartment.Id
--Where tblDepartment.Id is NULL

--select name, gender, salary
--from tblEmployee
--where departmentId is null


--LEFT JOIN, returns all the matching rows + non matching rows 
--from the left table. 

--In reality, INNER JOIN and LEFT JOIN are extensively used.

--select * from tblDepartment
--SELECT Name, Gender, Salary, DepartmentName
--FROM tblEmployee 
--RIGHT JOIN tblDepartment 
--ON tblEmployee.DepartmentId = tblDepartment.Id
--where tblEmployee.DepartmentId is  null

--LEFT JOIN, returns all the matching rows + non matching rows 
--from the left table. 
--RIGHT JOIN, returns all the matching rows + non matching rows 
--from the right table.

--FULL JOIN, returns all rows from both the left and right tables,
--including the non matching rows.

--select * from tblEmployee
--select * from tblDepartment

--select name, gender, salary, DepartmentName
--from tblEmployee
--Full Join tblDepartment
--On tblEmployee.DepartmentId = tblDepartment.Id
----where tblEmployee.DepartmentId is NULL
--where tblDepartment.Id is Null


--select * from tblEmployee
--select * from tblDepartment
--select name, gender, salary, departmentName
--from tblDepartment
--full join tblEmployee
--on tblDepartment.Id = tblEmployee.DepartmentId


-- Advanced Joins
--1. Advanced or intelligent joins in SQL Server
--2. Retrieve only the non matching rows from the left table
--3. Retrieve only the non matching rows from the right table
--4. Retrieve only the non matching rows from both the left and 
--right table
--select * from tblEmployee
--select * from tblDepartment
--SELECT Name, Gender, Salary, DepartmentName
--FROM tblEmployee E
--LEFT JOIN tblDepartment D
--ON E.DepartmentId = D.Id
----WHERE D.Id IS NULL
--Where E.DepartmentId is null


--select * from tblEmployee
--select * from tblDepartment
--SELECT         Name, Gender, Salary, DepartmentName
--FROM             tblEmployee E
--RIGHT JOIN    tblDepartment D
--ON                   E.DepartmentId = D.Id
----WHERE          E.DepartmentId IS NULL
--where d.id is null

--SELECT         Name, Gender, Salary, DepartmentName
--FROM              tblEmployee E
--FULL JOIN      tblDepartment D
--ON                   E.DepartmentId = D.Id
--WHERE          E.DepartmentId IS NULL
--OR                   D.Id IS NULL

-- Self Join ***
-- Joining a table with itself - is self-join
-- Self-Join can be classified as
-- Inner Self Join
-- Outer Self Join (Left, Right, and Full)
-- Cross Self Join

--Drop table tblEmployee2
--Create table tblEmployee2
--(
----PersonId int Identity(1,1) Primary Key,
--EmployeeId int Identity Primary Key,
--Name nvarchar(50) NOT NULL,
--ManagerId int Null
--)

--insert into tblEmployee2 values('Mike', 3)
--insert into tblEmployee2 values('Rob', 1)
--insert into tblEmployee2 values('Todd', null)
--insert into tblEmployee2 values('Ben', 1)
--insert into tblEmployee2 values('Sam', 1)

--select * from tblEmployee2

--select E.Name as Employee, M.Name as Manager
--from tblEmployee2 E
--left join tblEmployee2 M
--On E.ManagerId = M.EmployeeId

--select E.Name as Employee, M.Name as Manager
--from tblEmployee2 E
--inner join tblEmployee2 M
--On E.ManagerId = M.EmployeeId

--select E.Name as Employee, M.Name as Manager
--from tblEmployee2 E
--cross join tblEmployee2 M

--Different ways to replace NULL in sql server  ***
-- Replacing NULL value using ISNULL() function
-- pssing 2 parameters to ISNULL() function
-- If M.Name returns null, then "No Manager' string
-- is used as the replacement Value

--SELECT E.Name as Employee, ISNULL(M.Name,'No Manager') as Manager
--FROM tblEmployee2 E
--LEFT JOIN tblEmployee2 M
--ON E.ManagerID = M.EmployeeID

---- Replacing NULL value using Coalesce() function returns
---- the first NON null value
--SELECT E.Name as Employee, COALESCE(M.Name, 'No Manager') as Manager
--FROM tblEmployee2 E
--LEFT JOIN tblEmployee2 M
--ON E.ManagerID = M.EmployeeID


-- TESLA SQL ******
create table plays
(
	id int not null,
	title nvarchar(40) not null,
	writer nvarchar(40) not null,
	unique(id)
)

create table reservations
(
	id int not null,
	play_id int not null,
	number_of_tickets int not null,
	theater nvarchar(40) not null,
	unique(id)
)

select * from plays 
select * from reservations

select p.id as 'id of play', p.title as 'title of play', sum(r.number_of_tickets) as 'total number of reserved ticket for play'
from plays p
Inner Join reservations r
On p.id = r.play_id
Group By p.title, p.id
Order By sum(r.number_of_tickets) desc , p.id asc

-- Subquery ****
Create Table tblProducts
(
 [Id] int identity primary key,
 [Name] nvarchar(50),
 [Description] nvarchar(250)
)

Create Table tblProductSales
(
 Id int primary key identity,
 ProductId int foreign key references tblProducts(Id),
 UnitPrice int,
 QuantitySold int
)
Insert into tblProducts values ('TV', '52 inch black color LCD TV')
Insert into tblProducts values ('Laptop', 'Very thin black color acer laptop')
Insert into tblProducts values ('Desktop', 'HP high performance desktop')

Insert into tblProductSales values(3, 450, 5)
Insert into tblProductSales values(2, 250, 7)
Insert into tblProductSales values(3, 450, 4)
Insert into tblProductSales values(3, 450, 9)

select * from tblProducts
select * from tblProductSales

-- using Subquery

select id, name, description
from tblProducts
where id not in (select distinct ProductId from tblProductSales)

-- Most of times subqueries can be very easily replace with Joins
Select tblProducts.[Id], [Name], [Description], [QuantitySold], [ProductId]
from tblProducts
left join tblProductSales
on tblProducts.Id = tblProductSales.ProductId
--where tblProductSales.ProductId IS NULL
where tblProductSales.QuantitySold is NULL

-- Subquery in Select Clause, instead of in Where Clause
Select [Name],
(Select SUM(QuantitySold) from tblProductSales where ProductId = tblProducts.Id) as TotalQuantity
from tblProducts
order by Name

-- Same can be accomplisehd with left Join
select name, sum(quantitysold)
from tblProducts
left join tblProductSales
on tblProducts.id = tblProductSales.ProductId
group by name
order by name

2020-02-25 10:17:40
## Change DB Owner
use AdventureWorks2017 
Exec sp_changedbowner 'sa'
go

2020-02-25 12:01:50
## Find out Default Schema
select schema_name()

normally, it is "dbo"


2020-02-28 11:50:11

## Left Join

Suppose, we want to join two tables:

	A and B. 
	
	SQL left outer join returns all rows in the left table (A) and 
	all the matching rows found in the right table (B). 
	It means the result of the SQL left join always contains the rows in the left table.

    