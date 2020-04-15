ETL - Extract Transfer Load
====

ETL

: The process of extracting, transforming, and loading data into a new host source, such as a data warehouse. It's needed mostly for optimize the data for analytics.

- Why?

- Data Warehousing: OLTP databases are good for reading and update individual rows of data with low latency, but they not great for conducting large-scale analytics across huge databases.

- Cross-Domain Analysis: Joining data from disparate data sources is needed to answer deeper business questions.

ETL Process
----

1) Data **Exaction**: from OLTP (OnLine Transaction Process) (aka transactional) databases into the staging area.

2) Data **Transformation**: Data cleansing and optimizing the data for analysis.

3) Data **Load**: into OLAP (Online Analytical Processing) database so that data can be "mined" for Business Intelligence (BI), for input to Machine Learning Algorithm and other Data Science projects.

![ETL Process](_images\etl-process.png)