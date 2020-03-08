# Major SQL Concepts and Examples

## [SQL - Constraints](https://www.tutorialspoint.com/sql/sql-constraints.htm)

Constraints are the rules enforced on the data columns of a table.

These are used to limit the type of data that can go into a table. 

This ensures the accuracy and reliability of the data in the database

1. **NOT NULL** - ensures a column can not have NULL value
2. **DEFAULT** - provides a default value for a column when none is specified

```sql
alter table tblPerson
Add Constraint DefaultValue
Default 1 for GenderId

insert into tblPerson (Id, Name, Email)
values(6, 'h', 'h@h.com')`  -- no gender provided, so default is used
```

3. **UNIQUE** - ensures that all values in the column are different, and **can one NULL value, and can have multiple Unique Columns in a table**--which are different from the Primary Key column. Unique column can have non-cluster index.

4. **PRIMARY KEY** - uniquely identifies each row/record in a table--**one table can only have one Primary key column and can't take NULL value**. Primary Key column has cluster index on them automatically.

Note: **Identity Column** allows SQL Server to automatically generate value so that when we insert something into the table with Identity Column, we don't need to provide the value. Normally, Primary Key adn Identity column are used together.

```sql
Create Table tblPerson
(
    PersonId int Identity(1,1) Primary Key
    Name nvarchar(20)
)
```

5. **FOREIGN Key** - uniquely identifies a row/record in any of the given table.
6. **CHECK** - ensures that all the values in a column satisfies certain conditions; such as:

```sql
ALTER TABLE tblPerson
ADD CONSTRAINT CK_tbl_Person_Age
CHECK (Age > 0 AND Age < 150)
```

7. **INdEX** - used to create index so that retrieve data from database quickly.

Drop Contraint

```sql
ALTER Table tblPerson
DROP CONSTRAINT CK_tbl_Person_Age
```

## Data Integrity

- **Entity Integrity** − There are no duplicate rows in a table.
- **Domain Integrity** − Enforces valid entries for a given column by restricting the type, the format, or the range of values.
- **Referential integrity** − Rows cannot be deleted, which are used by other records.
- **User-Defined Integrity** − Enforces some specific business rules that do not fall into entity, domain or referential integrity.

## Database Normalization

Database normalization is the process of efficiently organizing data in a database.

There are two reasons of this normalization process −

- **Eliminating redundant data**, for example, storing the same data in more than one table.
- **Ensuring data dependencies** make sense.
- Reducing the amount of space a database consumes

Without normalization, in addition to larger database because of repetition of data, it causes problems when inserting--inserting duplicate data, updating--all duplicated data must be updated, and deleting data--if all data is deleted info used to be in duplicated data are missing-- from database tables.

Normalization is to minimize duplications of data, not to eliminate duplications--because some duplications are needed.

We want to achieve a Database with:

- Logical
- Independent, but
- Related data

There are 1st Norm Form, 2nd, 3rd and 4th, 5th.
Normally, goes to 3rd would sufficient.

### 1NF (1st Norm Form)

The First normal form (1NF) sets basic rules for an organized database −

Rules:

1. Each column should contain atomic values, can't have multiple values in a column.
2. A column should contain values of the same type, no mixing of different types in a single column.
3. Each column should have a unique name, no duplicate column names!
4. Order in which data is saved does not matter.

---

1. Place the related data items in a table.
2. Ensure that there are no repeating groups of data.
3. Ensure that there is a primary key.

### 2NF (2nd Norm Form)

1NF + No Partial dependencies of any of the columns on the primary key.

### 3NF (3rd Norm Form)

2NF + All non Primary fields are depends on the Primary Key

### FAQ Items


#### [Trigger](https://www.sqlservertutorial.net/sql-server-triggers/sql-server-create-trigger/)

The CREATE TRIGGER statement allows you to create a new trigger that is fired automatically whenever an event such as INSERT, DELETE, or UPDATE occurs against a table.

```sql
CREATE TRIGGER [schema_name.]trigger_name
ON table_name
AFTER  {[INSERT],[UPDATE],[DELETE]}
[NOT FOR REPLICATION]
AS
{sql_statements}
```

The schema_name is the name of the schema to which the new trigger belongs. The schema name is optional.
The trigger_name is the user-defined name for the new trigger.
The table_name is the table to which the trigger applies.
The event is listed in the AFTER clause. The event could be INSERT, UPDATE, or DELETE. A single trigger can fire in response to one or more actions against the table.
The NOT FOR REPLICATION option instructs SQL Server not to fire the trigger when data modification is made as part of a replication process.
The sql_statements is one or more Transact-SQL used to carry out actions once an event occurs.

```sql
CREATE TABLE production.product_audits(
    change_id INT IDENTITY PRIMARY KEY,
    product_id INT NOT NULL,
    product_name VARCHAR(255) NOT NULL,
    brand_id INT NOT NULL,
    category_id INT NOT NULL,
    model_year SMALLINT NOT NULL,
    list_price DEC(10,2) NOT NULL,
    updated_at DATETIME NOT NULL,
    operation CHAR(3) NOT NULL,
    CHECK(operation = 'INS' or operation='DEL')
);

CREATE Trigger production.trg_product_audit
ON product.products
AFTER INSERT, DELETE
AS
BEGIN
SET COUNT ON;
INSERT INTO product.product_audits 
(product_id, product_name, brand_id, category_id, model_year, list_price, updated_at, operation)
SELECT
    i.product_id,
    product_name,
    brand_id,
    category_id,
    model_year,
    i.list_price,
    GETDATE(),
    'INS'
FROM
    inserted AS i
UNION ALL
    SELECT
        d.product_id,
        product_name,
        brand_id,
        category_id,
        model_year,
        d.list_price,
        getdate(),
        'DEL'
    FROM
        deleted AS d;
END;
```

#### [Stored Procedure](https://www.sqlservertutorial.net/sql-server-stored-procedures/basic-sql-server-stored-procedures/)

```sql
CREATE PROCEDURE uspProductList
AS
BEGIN
    SELECT 
        product_name, 
        list_price
    FROM 
        production.products
    ORDER BY 
        product_name;
END;
```
