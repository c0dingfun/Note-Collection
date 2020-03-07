## [SQL: How to Query - the Basics](https://thoughtbot.com/blog/back-to-basics-sql#junction-table)

This is a very good write-up for learning how to construct SQL queries.

# [SQL Self Join (Illustrated)](https://learnsql.com/blog/illustrated-guide-sql-non-equi-join/)

- Note: It seems Self RIGHT JOIN has no meaning. So, seems nobody is doing it.

Questions to Answer:

- What is Self-Join?

    A: A Self-Join (inner, left, cross) is a join using the same table as both the left and the right table.

- How Self-Join Works?

    A: Must use aliases when performing a self join.

- When should Self-Join be used?

    A: In following scenarios:

    * Hierarchical relationships
        - employee and manager
        - parts and Car-Component (component contains parts/parts belong to a component)
        - parent and child

        for example (Family Table):

        |id|first_name|last_name|birth|mother_id|father_id|
        |--|--|--|--|--|--|
        |1|John|Watson|1945|NULL|NULL|
        |2|Anne|Brown|1950|NULL|NULL|
        |6|Scarlett|Miller|1985|2|1|
        |7|Jacob|Miller|1982|NULL|NULL|
        |8|David|Miller|2015|6|7|

        ```sql
        select
            p.first_name,
            p.last_name,
            p.birth,

            m.last_name,
            f.lsat_name
        from family p
        left join family m on p.mother_id = m.id
        left join family f on p.father_id = f.id
        ```

    * Sequential relationships

        - Example (instruction table): records that describe the consecutive steps required to prepare a dish. All  the steps can be placed in a single table. Their order is determined based on the columns that point to the IDs of the previous and next records in the same table.

        |id|content|previous_id|next_id|
        |--|-------|-------|---|
        |1|Preheat an oven to 220 degrees C.|NULL|2|
        |2|Peel four potatoes.|1|4|
        |3|Toss sliced potatoes with oil.|4|6|
        |4|Cut potatoes into slices.|2|3|
        |5|Season the hot slices with salt and pepper.|6|NULL|
        |6|Bake in the preheated oven for 20 minutes.|3|5|

        ```sql
        ```

    * Graph data

        - relationships needed for graphs. A graph is a structure consisting of nodes connected to each other with edges (relations). One example of a graph is the road network between cities.

        - We’re using two tables to store this data. The city table stores the ID number and the name of each city. The route table contains the route ID number, the starting city (the from_city_id column) and the target city (the to_city_id column).

        - "city" table

            |id|name|
            |--|---|
            |1|Laredo|
            |2|San Antonio|
            |3|Austin|
            |4|Waco|
            |5|Houston

        - "route" table

            |id|from_city|to_city_id|
            |--|--|--|
            |1|4|1|
            |2|4|3|
            |3|4|2|
            |4|1|4| wrong should be 2
            |5|2|3|
            |6|2|5|
            |7|5|3|

        - We can use a SQL self join on the city table, along with an INNER JOIN of the two tables, to find out what routes exist between cities.

        ```sql
        SELECT 
            c1.name AS from_city, 
            c2.name AS to_city
        FROM city c1
        JOIN route r ON c1.id = r.from_city_id
        JOIN city c2 ON c2.id = r.to_city_id ;
        ```

        - The city and route tables were joined using the id column from city and the from_city_id column from route. At this point, we could only retrieve the name of the start city. In order to retrieve the name of the target city, we used a self-join on the city table. This time, we compared the id from the aliased city table with the to_city_id column in the route table.

        - Result of the joins

        |from_city|to_city|
        |--|--|
        |Waco|Laredo|
        |Waco|Austin|
        |Waco|San Antonio|
        |Laredo|Waco|
        |San Antonio|Austin|
        |San Antonio|Houston|
        |Houston|Austin|

## Using SQL Self Join to Find Duplicate Values

- Self-joins can also be used to identify duplicate values in a table.
- Let’s introduce an example table called color:

    |id|name|
    |--|----|
    |1|blue|
    |2|green|
    |3|yellow
    |4|blue|
    |5|yellow|

- Each record in the table is different because of the id column, which must always be unique. But this doesn’t prevent two rows from storing the same color name. We want to identify such cases and find the IDs of the duplicate color names.

- Let’s try this:

```sql
SELECT
    c1.id AS id1,
    c1.name AS color1,

    c2.id AS id2,
    c2.name AS color2
FROM color c1
JOIN color c2
ON c1.name = c2.name AND c1.id < c2.id;
```

- We were able to find duplicate color names because we self-joined records based on the color name. The second condition is used to skip identical records from both tables as well as the same pairs of records in reverse order.

    |id1|color1|id2|color2|
    |--|--|--|--|
    |1|blue|4|blue|
    |3|yellow|5|yellow

- It is now easy to see that there are duplicate values for blue and yellow.
