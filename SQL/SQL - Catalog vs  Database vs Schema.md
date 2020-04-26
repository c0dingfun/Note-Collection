Catalog vs Database vs Schema
====

- Cluster > Catalog > Schema > Table > Columns & Rows

> A database server is a cluster.
> A cluster has catalogs. ( Catalog == Database )
> Catalogs have schemas. (Schema == namespace of tables, and security boundary)
> Schemas have tables.
> Tables have rows.
> Rows have values, defined by columns.
> Those values are the business data your apps and users care about such as person's name, invoice due date, product price, gamer’s high score. The column defines the data type of the values (text, date, number, and so on).

- The four-part-sql object-name
- (InstanceName.DatabaseName.SchemaName.ObjectName)
- The default schema is "dbo"
- Example of Schema

![What is a Schema](_images/SQL-What-is-Schema.png)

- Complete example

![Complete Picture](_images/SQL-Cluster.png)

- [Ref](https://stackoverflow.com/questions/7022755/whats-the-difference-between-a-catalog-and-a-schema-in-a-relational-database)