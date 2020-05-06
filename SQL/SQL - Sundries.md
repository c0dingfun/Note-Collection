SQL - Sundries
====

ANSI_NULLS, ON|OFF
----

> Tell how to treat NULLs when doing = or !=
> NOT AFFECTED "IS NULL" or "IS NOT NULL", though

```sql
SET ANSI_NULLS ON
GO

CREATE PROCEDURE ANSI_NULL_1
AS
IF NULL = NULL
    PRINT OBJECT_NAME(@@PROCID) + ': Equal'
ELSE
    PRINT OBJECT_NAME(@@PROCID) + ': Not Equal' -- not equal
GO

SET ANSI_NULLS OFF
GO
CREATE PROCEDURE ANSI_NULL_2
AS
IF NULL = NULL
    PRINT OBJECT_NAME(@@PROCID) + ': Equal' -- equal
ELSE
    PRINT OBJECT_NAME(@@PROCID) + ': Not Equal'
GO

exec ANSI_NULL_1
exec ANSI_NULL_2

-- output
-- ANSI_NULL_1: Not Equal
-- ANSI_NULL_2: Equal
```

@@PROCID
----

> mostly used in Stored Procedure, user function, triggers to get to provide the object-id of the currently running procedure.

> we can use OBJECT_NAME(@@PROCID) to get the procedure name.

```sql
DECLARE @this_proc_name sysname =
        QUOTENAME(OBJECT_SCHEMA_NAME(@@PROCID)) +'.'+ QUOTENAME(OBJECT_NAME(@@PROCID))
```

- Example:

```sql
CREATE PROC demo_an_error_message
AS
DECLARE @this_proc_name sysname =
    QUOTENAME(OBJECT_SCHEMA_NAME(@@PROCID)) +'.'+QUOTENAME(OBJECT_NAME(@@PROCID))

DECLARE @msg varchar(255)

SET @msg = 'ERROR in ' + @this_proc_name + ' whatever the message should say'
RAISERROR (@msg, 16, 1)
GO

exec demo_an_error_message
GO

-- output
-- Msg 50000, Level 16, State 1, Procedure demo_an_error_message, Line 9
-- ERROR in [dbo].[demo_an_error_message] whatever the message should say
--          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

What is **sysname**?
----

> sysname is a system-supplied user-defined data type that is functionally equivalent to nvarchar(128), except that it is not nullable. sysname is used to reference database object names.

How to find out SQL Server Version info?
----

- Use windows Properties windows

1. In SSMS, properties of the instance name
2. Properties on sqlservr.exe

- Programmatically

1. select @@version

2. exec xp_msver

3. use SererProperty() function

```sql
SELECT
  CASE
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '8%' THEN 'SQL2000'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '9%' THEN 'SQL2005'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '10.0%' THEN 'SQL2008'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '10.5%' THEN 'SQL2008 R2'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '11%' THEN 'SQL2012'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '12%' THEN 'SQL2014'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '13%' THEN 'SQL2016'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '14%' THEN 'SQL2017'
     WHEN CONVERT(VARCHAR(128), SERVERPROPERTY ('productversion')) like '15%' THEN 'SQL2019'
     ELSE 'unknown'
  END AS MajorVersion,
  SERVERPROPERTY('ProductLevel') AS ProductLevel,
  SERVERPROPERTY('Edition') AS Edition,
  SERVERPROPERTY('ProductVersion') AS ProductVersion
```

What do these SQL Server Version Numbers Mean?
----

- So now that you have this number such as 9.00.1399.06 or 8.00.760 what do these even mean? The first digits refer to the version of SQL Server such as:

```
8.0 for SQL Server 2000
9.0 for SQL Server 2005
10.0 for SQL Server 2008
10.5 for SQL Server 2008 R2
11.0 for SQL Server 2012
12.0 for SQL Server 2014
13.0 for SQL Server 2016
14.0 for SQL Server 2017
15.0 for SQL Server 2019
```


SELECT CASE Statement
----

- Format

```dotnetcli
SELECT CASE Expression
    When expression1 Then Result1
    When expression2 Then Result2
    ...
    ELSE Result
END
```

- Example1:

```sql
DECLARE @ProdId int
SET @ProdId = 1
SELECT
Case @ProdId
    When 1 THEN 'Bread and Biscuits'
    When 2 THEN 'Confectioneries'
    When 3 THEN 'Fruits and Vegetables'
    ELSE 'No such a product'
END
```

- Example2:

```sql
SELECT EmployeeName,
CASE
    WHEN Salary >=80000 AND Salary <=100000 THEN 'Director'
    WHEN Salary >=50000 AND Salary <80000 THEN 'Senior Consultant'
    Else 'Director'
END AS Designation
FROM Employee
```

- Example3: [Order By] + CASE

> We can use Case statement with order by clause as well.
> In SQL, we use Order By clause to sort results in ascending or descending order.
> Example: we want to sort result in the following method.
> For Female employee, employee salaries should come in descending order
> For Male employee, we should get employee salaries in ascending order

```sql
SeLECT EmployeeName,Gender,Salary
FROM Employee
ORDER BY
CASE Gender
    WHEN 'F' THEN Salary
End DESC,
Case
    WHEN Gender='M' THEN Salary
END
```

- Example 4: [Group By] + CASE

> We can use a Case statement with Group By clause.
> This example groups employees based on their salary.
> Then further to calculate the minimum and maximum salary for a particular range of employees.

> In the following query, you can see that we have Group By clause and it contains i with the condition to get the required output.

```sql
SELECT
CASE
WHEN Salary >=80000 AND Salary <=100000 THEN 'Director'
WHEN Salary >=50000 AND Salary <80000 THEN 'Senior Consultant'
ELSE 'Director'
END AS Designation,
Min(salary) AS MinimumSalary,
Max(Salary) AS MaximumSalary
FROM Employee

Group By
CASE
WHEN Salary >=80000 AND Salary <=100000 THEN 'Director'
WHEN Salary >=50000 AND Salary <80000 THEN 'Senior Consultant'
ELSE 'Director'
END
```

- Example 5: [Update] + CASE

> Update can use Case statement in SQL DML as well.
> Suppose we want to update State code of employees based on Case statement conditions.
> In the following code, we are updating state code with the following condition.
> If employee state code is AR, then update to FL
> If employee state code is GE, then update to AL
> For all other state codes update value to IN

```sql
UPDATE employee
SET StateCode  = CASE StateCode
WHEN 'Ar' THEN 'FL'
WHEN 'GE' THEN 'AL'
ELSE  'IN'
END
```

- Example 6: CASE in [INSERT VALUES]

> We can insert data into SQL tables as well with the help of Case statement in SQL.
> Suppose we have an application that inserts data into Employees table.
> We do not want to insert value 0 and 1 for Male and Female employees.
> We need to insert the required values M and F for employee gender.

> In the following query, we specified variables to store column values.
> In the insert statement, you can we are using a Case statement to define corresponding value to insert in the employee table.
> In the Case statement, it checks for the required values and inserts values from THEN expression in the table.

```sql
DECLARE @EmployeeName varchar(100), @Gender int, @Statecode char(2),  @salary money
SET @EmployeeName='Raj', @Gender=0, @Statecode='FL',  @salary=52000
INSERT INTO employee
VALUES
(
    @EmployeeName,
    CASE @Gender
        WHEN 0 THEN 'M'
        WHEN 1 THEN 'F'
    END,
    @Statecode,
    @salary
)
```

- CASE Statement LIMITATIONS

> We CANNOT control the execution flow of STORED PROCEDURES, FUNCTIONS using a CASE statement in SQL
> We can have multiple conditions in CASE statement; however, it works in a SEQUENTIAL model. If one condition is satisfied, it STOPS CHECKING further conditions
> We CANNOT use a CASE statement for checking NULL values in a table

- [Ref](https://www.sqlshack.com/case-statement-in-sql/)

SQL Server: sys.<*> tables
----

- [Ref](https://renenyffenegger.ch/notes/development/databases/SQL-Server/administration/schemas/sys/index)

Server Server Instance
----

> A computer (or generally an SQL Server installation) can have zero, one or more instances of SQL Server installed.
> An instance consists of one or more databases.
> A database contains one or more logical file groups.
> Each file group has one or more data files.
> The data files physically store the data that is in entrusted to SQL Server.

> For a given machine, we you want multiple instance of sql servers, you need to install it multiple times.
> For a given machine, it can have ONE DEFAULT instance of sql server, which the computer is the name of the instance-id.
> For a given machine, it can MULTIPLE NAMED instance of sqlservers, which mean you MUST provide unique instance-id

- [Ref](https://renenyffenegger.ch/notes/development/databases/SQL-Server/architecture/instance)

SP_HELP
----

> On Transact SQL language the sp_help is part of Database Engine Stored Procedures and reports information about a database object or a data type.
> Sp_help syntax:
> sp_help [ @objname = 'Object name.' ] ;

- Example:

```sql
sp_help @objname='person.address'
sp_help 'person.address'
sp_help int
```

Collation
----

> Collation is a set of rules that tell database engine how to compare and sort the character data in SQL Server.
> Database
> Table
> Column

> Server Configuration

> SELECT SERVERPROPERTY('collation')
> sp_help Product <<< table

> select name, description
> from sys.fn_helpcollations();

Start and Stop SQL Server Services
----

> The state of a service might be changed with the SQL Server configuration manager (SQLServerManager….msc).
> In a console (cmd.exe, PowerShell), a service can be started or stopped with net.exe.
Startup options can be added at the end of the command (with slashes (/) rather than hyphens (-):

```powershell
net start "SQL Server (MSSQLSERVER)" /f /m
net start MSSQLSERVER /f /m
net stop  MSSQLSERVER
```

Use SQL functions HOST_NAME and HOST_ID for SSMS
----

- the host id is the process id in the Windows Task Manager

```dotnetcli
SELECT host_name(), host_id()
```

Catalog Views (important ones per Database)
----

```dotnetcli

sys.objects
> Contains a row for each user-defined, schema-scoped object that is created within a database, including natively compiled scalar user-defined function.
sys.tables
sys.parameters
sys.procedures
sys.triggers	
sys.views	
sys.columns	

sys.sequences
sys.events	
sys.sql_modules
sys.stats
sys.synonyms
```

OBJECT_ID()
----

- Returns the database object identification number of a schema-scoped object.

```sql
--format
OBJECT_ID ('[database_name.[ schema_name ].|schema_name.]object_name' [,'object_type'])
```

- Example1: Returning the object id for a specified object

```sql
USE master;
GO
SELECT OBJECT_ID(N'AdventureWorks2012.Production.WorkOrder') AS 'Object ID';
GO
```

- Example2 (very often used): Verifying that an object exists

```sql
USE AdventureWorks2012;
GO
IF OBJECT_ID (N'dbo.AWBuildVersion', N'U') IS NOT NULL
    DROP TABLE dbo.AWBuildVersion;
GO
```

- Example3: Using OBJECT_ID to specify the value of a system function parameter

```sql
DECLARE @db_id int;
DECLARE @object_id int;
SET @db_id = DB_ID(N'AdventureWorks2012');
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');
IF @db_id IS NULL
  BEGIN;
    PRINT N'Invalid database';
  END;
ELSE IF @object_id IS NULL
  BEGIN;
    PRINT N'Invalid object';
  END;
ELSE
  BEGIN;
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);
  END;
GO
```

OBJECT_NAME
----

- Returns the database object name for schema-scoped objects.

```sql
-- format
OBJECT_NAME ( object_id [, database_id ] )
-- return sysname
```

- Example:


```sql
USE AdventureWorks2012;
GO
    SELECT DISTINCT OBJECT_NAME(object_id)
    FROM master.sys.objects;
GO
```

QUOTED_IDEENTIFIER
----

- QUOTED_IDENTIFIER controls the behavior of SQL Server handling DOUBLE-QUOTES.

```sql
SET QUOTED_IDENTIFIER OFF
GO
SELECT "Rajendra" -- OK
GO

SET QUOTED_IDENTIFIER ON
GO
SELECT "Rajendra"   -- NOT OK
GO

```
- SET QUOTED_IDENTIFIER OFF:
    > If this setting is off, SQL Server treats the value inside the double quotes as a STRING.
    > We can use any string in the double quotes, and SQL Server does not check for rules such as reserved keyword

- SET QUOTED_IDENTIFIER ON:
    > With this option, SQL Server treats values inside double-quotes as an IDENTIFIER.
    > It is the default setting in SQL Server.

Table-Valued Function
---

- TODO

- [Ref](https://www.sqlservertutorial.net/sql-server-user-defined-functions/sql-server-table-valued-functions/)


CROSS APPLY and OUTER APPLY
---

- TODO

- [Ref](https://www.sqlshack.com/the-difference-between-cross-apply-and-outer-apply-in-sql-server/)


ANSI_PADDING ON (default) |OFF
---

- It is all about the STORAGE!!!! of CHAR, VARCHAR, etc., NOT AFFECTING other operations.

- An option that controls how CHAR, VARCHAR and BINARY, VARBINARY values are STORED.
- If ANSI_PADDING is turned ON, then SQL Server will NOT trim the trailing spaces when it inserts into a VARCHAR field.
- Similarly, it will NOT trim trailing nulls when it inserts into a VARBINARY field.

- Setting the ANSI_PADDING only affects the spaces on inserts.
- It does NOT affect comparisons!!!!!

- NOTE: The SET ANSI_PADDING option is always ON for the columns with NCHAR, NVARCHAR, NTEXT, TEXT, IMAGE data types.

```sql
SET	ANSI_PADDING ON
GO

INSERT
INTO Animal (AnimalName)
VALUES ('Dog    ')  -- will not trimmed!
GO

SET	ANSI_PADDING OFF
GO
```

Drop Table, Many Checks before Dropping
---

- TODO
- [Ref](https://www.sqlshack.com/how-to-drop-temp-tables-in-sql-server/)

Database.BeginTransaction vs Transactions.TransactionScope
---

- TODO

Ambient Transaction
---

- TODO

MERGE
---

- used to synchronize two tables by inserting, deleting, and updating the target table rows based on the join condition with the source table.

- syntax:

```sql
    MERGE TOP (value) <target_table>
    USING <table_source>
    ON <merge_search_condition>
        [ WHEN MATCHED [ AND <clause_search_condition> ]
            THEN <merge_matched> ]
        [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]
            THEN <merge_not_matched> ]
        [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]
            THEN <merge_matched> ]
        [ <output_clause> ]
        [ OPTION ( <query_hint> ) ]
    ;
```

- Example (Setup)

```sql
    -- Target Table
    IF EXISTS (SELECT 1 FROM SYS.TABLES where name ='Locations')
    BEGIN
    DROP TABLE LOCATIONS
    END

    -- Source Table
    IF EXISTS (SELECT 1 FROM SYS.TABLES where name ='Locations_stage')
    BEGIN
    DROP TABLE LOCATIONS_STAGE
    END

    CREATE TABLE [dbo].[LOCATIONS](
        [LocationID] [int] NULL,
        [LocationName] [varchar](100) NULL)
    GO

    CREATE TABLE [dbo].[LOCATIONS_STAGE](
        [LocationID] [int] NULL,
        [LocationName] [varchar](100) NULL)
    GO

    INSERT INTO LOCATIONS values (1,'Richmond Road'),(2,'Brigade Road') ,(3,'Houston Street')

    INSERT INTO LOCATIONS_STAGE values (1,'Richmond Cross') ,(3,'Houston Street'), (4,'Canal Street')
```

- Example (Using Merge to Update Matched Rows)

- WHEN MATCHED clause in SQL Server MERGE statement is used to UPDATE, DELETE the rows in the target table when the rows are matched with the source table based on the join condition.

- In this case, LOCATIONS is the target table, LOCATIONS_STAGE is the source table and the column LocationID is used in the join condition.

```sql
    MERGE LOCATIONS T
    USING LOCATIONS_STAGE S ON T.LocationID = S.LocationID
    WHEN MATCHED THEN
    UPDATE SET LocationName=S.LocationName;
```

- Explained

- Rows with LocationID 1 and 3 are matched in the TARGET and SOURCE table as per the join condition and the value of LocationName in the TARGET was updated with the value of LocationName in the SOURCE table for both rows.

- Additional condition added to "WHEN MATCHED" clause

```sql
    MERGE LOCATIONS T
    USING LOCATIONS_STAGE S ON T.LocationID=S.LocationID
    WHEN MATCHED AND T.LocationID = 3 THEN
    UPDATE SET LocationName=S.LocationName;
```

- NOTE:

- At most, we can specify only (TWO) WHEN MATCHED clauses in the MERGE statement.
- If (TWO) WHEN MATCHED clauses are specified, one clause must have an UPDATE operation and the other one must use DELETE operation.

```sql
    MERGE LOCATIONS T
    USING LOCATIONS_STAGE S ON T.LocationID=S.LocationID
    WHEN MATCHED AND T.LocationID =3 THEN
    DELETE
    WHEN MATCHED AND T.LocationID =1 THEN
    UPDATE SET LocationName=S.LocationName;
```

- NOTE:

- When there is more than one row in the SOURCE table that matches the join condition, the UPDATE in SQL Server MERGE statement fails and returns error “The MERGE statement attempted to UPDATE or DELETE the same row more than once.

- This happens when a TARGET row matches more than one SOURCE row.

-A MERGE statement cannot UPDATE/DELETE the same row of the target table multiple times.

- Use ON clause to ensure a target row matches at most one source row,
- OR use the GROUP BY clause to group the source rows.”

- Using MERGE to INSERT rows in TARGET table

- WHEN NOT MATCHED BY TARGET clause is used to insert rows into TARGET table that does not match join condition with a SOURCE table.
- WHEN NOT MATCHED BY TARGET clause can be SPECIFIED ONLY ONCE in the SQL Server MERGE statement.

- For example, the row with LocationID = 4 in the table Locations_stage does not match join condition and is present in the source table only.

- Now when we use WHEN NOT MATCHED BY TARGET clause in the MERGE statement to insert the additional row from LOCATIONS_STAGE into LOCATIONS.

```sql
    MERGE LOCATIONS T
    USING LOCATIONS_STAGE S ON T.LocationID=S.LocationID
    WHEN NOT MATCHED BY TARGET
    THEN
    INSERT (LocationID,LocationName) VALUES (S.LocationID, S.LocationName);
```

- Using MERGE to DELETE the rows in the target

- We can use WHEN NOT MATCHED BY SOURCE clause in SQL Server MERGE statement to DELETE the rows in the TARGET table that does not match join condition with a SOURCE table.

- For example, the row with locationID = 2 in the TARGET table does not match the join condition and the row is present only in the TARGET table.

- So, when we use WHEN NOT MATCHED BY SOURCE and can either DELETE the row or UPDATE it.

- Please refer to the below T-SQL script to delete the row in the target table using WHEN NOT MATCHED BY SOURCE clause.

```sql
    MERGE LOCATIONS T
    USING LOCATIONS_STAGE S ON T.LocationID=S.LocationID
    WHEN NOT MATCHED BY SOURCE
    THEN
    DELETE;
```

- NOTE:

- WHEN NOT MATCHED BY SOURCE clause can be specified two times, BUT one MUST use an UPDATE operation and another one MUST use DELETE operation.

```sql
    MERGE LOCATIONS T
    USING LOCATIONS_STAGE S ON T.LocationID=S.LocationID
    WHEN NOT MATCHED BY SOURCE AND LocationID =2
    THEN
    DELETE
    WHEN NOT MATCHED BY SOURCE AND LocationID =6
    THEN
    UPDATE SET LocationName ='Test';
```

- NOTE:

- We can use all the three clauses in the single MERGE statement to synchronize the TARGET table with the SOURCE table.

```sql

    MERGE LOCATIONS T
    USING LOCATIONS_STAGE S ON T.LocationID=S.LocationID
    
    WHEN MATCHED THEN
    UPDATE SET LocationName=S.LocationName
    
    WHEN NOT MATCHED BY TARGET 
    THEN 
    INSERT (LocationID,LocationName)
    VALUES (S.LocationID,S.LocationName)
    
    WHEN NOT MATCHED BY SOURCE 
    THEN 
    DELETE;

```

![example illustration](_images/Merge-Example.png)
