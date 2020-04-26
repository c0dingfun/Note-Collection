SQL - Local Variables and Table Variable
====

Local Variable
----

- Declaration

```sql
DECLARE { @LOCAL_VARIABLE data_type [ = value ] }
```

- Example:

```sql
DECLARE @TestVar AS NVARCHAR(100)='Save Our Planet'
PRINT @TestVar
```

Assigning a Value to SQL Variable
----

- Use "SET"

```sql
DECLARE @TestVariable AS VARCHAR(100)
SET @TestVariable = 'One Planet One Life'
PRINT @TestVariable
```

- Use "SELECT"

```sql
DECLARE @TestVariable AS VARCHAR(100)
SELECT @TestVariable = 'Save the Nature'
PRINT @TestVariable
```

- Assigning a Value from a table, view or scalar-valued, to a Variable

- Example: from a table

```sql
DECLARE @PurchaseName AS NVARCHAR(50)
SELECT @PurchaseName = [Name]
FROM [Purchasing].[Vendor]
WHERE BusinessEntityID = 1492
PRINT @PurchaseName
```

- Example: from a scalar function

```sql
DECLARE @StockVal AS INT
SELECT @StockVal=dbo.ufnGetStock(1)
SELECT @StockVal AS [VariableVal]
```

Declare Multiple Variables
----

```sql
DECLARE @Variable1 AS VARCHAR(100)
DECLARE @Variable2 AS UNIQUEIDENTIFIER
SET @Variable1 = 'Save Water Save Life'
SET @Variable2= '6D8446DE-68DA-4169-A2C5-4C0995C00CC1'
PRINT @Variable1
PRINT @Variable2
```

- shorthand

```sql
DECLARE @Variable1 AS VARCHAR(100), @Variable2 AS UNIQUEIDENTIFIER
SET @Variable1 = 'Save Water Save Life', @Variable2= '6D8446DE-68DA-4169-A2C5-4C0995C00CC1'
PRINT @Variable1
PRINT @Variable2
```

- using SELECT to assign to multiple variables

```sql
DECLARE @VarAccountNumber AS NVARCHAR(15), @VariableName AS NVARCHAR(50)
SELECT @VarAccountNumber=AccountNumber, @VariableName=Name
FROM [Purchasing].[Vendor]
WHERE BusinessEntityID = 1492
PRINT @VarAccountNumber
PRINT @VariableName
```

- using TempTable to store values
- Note: in TempDb, there is no "Schema"; that is why '..' to 'skip' the schema part of the 4-part sql server object reference

```sql
IF OBJECT_ID('tempdb..#TempTbl') IS NOT NULL DROP TABLE #TempTbl
DECLARE @TestVariable AS VARCHAR(100)
SET @TestVariable = 'Hello World'
SELECT @TestVariable AS VarVal INTO #TempTbl
GO
DECLARE @TestVariable AS VARCHAR(100)
SELECT @TestVariable = VarVal FROM #TempTbl
PRINT @TestVariable
```

Table Variable
----

- It is like a temp table

- Declaration

```sql
DECLARE @LOCAL_TABLEVARIABLE TABLE
(column_1 DATATYPE, column_2 DATATYPE, column_N DATATYPE)
```

- Usage

```sql
DECLARE @ListOWeekDays TABLE(DyNumber INT,DayAbb VARCHAR(40) , WeekName VARCHAR(40))

INSERT INTO @ListOWeekDays
VALUES 
(1,'Mon','Monday')  ,
(2,'Tue','Tuesday') ,
(3,'Wed','Wednesday') ,
(4,'Thu','Thursday'),
(5,'Fri','Friday'),
(6,'Sat','Saturday'),
(7,'Sun','Sunday')	
SELECT * FROM @ListOWeekDays
```







- [Ref](https://www.sqlshack.com/sql-variables-basics-and-usage/) 
- [SSMS Shortcuts](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-keyboard-shortcuts?view=sql-server-2017)