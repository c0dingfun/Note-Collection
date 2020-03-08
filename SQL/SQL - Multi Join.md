# [SQL Multi Join](https://academy.vertabelo.com/blog/illustrated-guide-multiple-join/)

## What is Multiple Join?

- Each query may comprise zero, one, or more joins.
- A multiple join is a use of more than one join in a single query.
- The joins used may be all of the same type, or their types can differ.

## Tables with Data

- "person" table

|id|lastname|
|--|--|
|1|Watson|
|2|Miller|
|3|Smith|
|4|Brown|

- "color" table

|id|name|
|--|--|
|1|green|
|2|yellow|
|3|blue|

- "vehicle" table

|id|name|color_id|person_id|
|--|--|--|--|
|1|car|1|4|
|2|bicycle|2|NULL|
|3|motorcycle|NULL|1|
|4|scooter|1|3|

## Examples

1. List each vehicle's color and its owner's lastname

```sql
    SELECT v.name, c.name, p.lastname
    FROM vehicle
    INNER JOIN color c ON v.color_id = c.id -- only vehicle with assigned color
    INNER JOIN person p ON v.person_id = p.id; -- only vehicle with assigned owner
```

- Results:

|name|name|lastname|
|--|--|--|
|car|green|Brown|
|scooter|green|Smith|

- Explanations:

    * Basically, 1st INNER Join returns us the following:

        |name|name|lastname|
        |--|--|--|
        |car|green|Brown|
        |bicycle|yellow|NULL|   -- bicycle has no owner
        |scooter|green|Smith|

    * Then, 2nd INNER Join (vehicles with a owner) joins with the result of the 1st INNER Join; thus we have:

        |name|name|lastname|
        |--|--|--|
        |car|green|Brown|
        |scooter|green|Smith|

    * Note that all JOIN operations are performed from left to right. In step one, the tables in the first JOIN are matched (tables vehicle and color). As a result, an intermediate table is created. In step two, this intermediate table (treated as the left table) is joined with another table (table person) using the second JOIN.

    * Remember that a single JOIN of any type produces a single intermediate table (commonly called a derived table) during a multi-join query.

    * So basically, (n)th Join result is used in (n+1)th Join to get then (n+1)th results.


## Mixed Left and Right Join with Inner Join

- It is also possible to combine different types of joins in a multi-join query.

## Example of an INNER JOIN with LEFT JOIN

- Intuitively, we would start with the person table and join it with the vehicle table using a LEFT JOIN. In that case, the LEFT JOIN would match each record from the person table with a record from the vehicle table, and for any person for whom a matching record was not found, it would fill missing values with NULLs. This join will produce a list of all people in the database with any associated vehicle data, even if they do not own one. But we're interested in seeing only vehicles with colors assigned. This means we must use an INNER JOIN on tables vehicle and color.

```sql
SELECT v.name vehicle_name, c.name color_name,  p.lastname
FROM person p
LEFT JOIN vehicle v ON v.person_id = p.id
INNER JOIN color c ON v.color_id = c.id
```

- Results:

    |name|name|lastname|
    |--|--|--|
    |car|green|Brown|
    |scooter|green|Smith|

- It's the same result as before (1st multi join query using ONLY INNER Joins)

- This is because the INNER JOIN skipped those results which did not match in both tables, i.e. in the derived table (created by joining tables person and vehicle) and the color table.

- If we want to list a person without vehicle, what can we do?

- The following query presents one of a few possible solutions. Here the derived table returns vehicles with colors only, and is then RIGHT JOINed with the person table in order to obtain all of the people.

```sql
    SELECT p.lastname, v.name, c.name
    FROM vehicle v
    INNER JOIN color c ON  v.color_id = c.id
    RIGHT JOIN person p ON v.person_id = p.id;
```

- Results:

|lastname|name|name|
|---|---|---|
|Smith|scooter|green|
|Brown|car|green|
|Miller|||
|Watson|||

- Explanation

    * Now we have a list of all the people: those with colored vehicles and those without vehicles. We started with an INNER JOIN of tables vehicle and color. Each vehicle included in the derived table must have a color assigned, which is why this join type is appropriate. Having selected the colored vehicles, we could now use a RIGHT JOIN on the derived table with the person table, which is how we obtained people who were not vehicle owners alongside those (from the derived table) who owned a colored vehicle.

## Anther Way, using Subquery

- Another method to solve this problem is to use a LEFT JOIN on the person table and a subquery in which we used an INNER JOIN on tables vehicle and color.

```sql
    SELECT p.lastname, o.vehicle_name, o.color_name
    FROM person p LEFT JOIN
    (  SELECT v.name vehicle_name, c.name color_name, v.person_id
       FROM vehicle v
       INNER JOIN color c ON v.color_id=c.id
    ) o ON o.person_id = p.id;
```

## Mixed JOINs with Full JOIN

```sql
    SELECT p.lastname, v.name, c.name
    FROM vehicle v
    FULL JOIN color c ON  v.color_id = c.id
    FULL JOIN person p ON v.person_id = p.id ;
```

- The query above matches the records from three tables: person, vehicle and color in such a way that even records without a match in the other two tables will appear in the result table.
- Empty columns will be filled with NULL values.
- That is why the query returns

    1. all people regardless of whether they have a vehicle,
    2. all vehicles regardless of whether they have a color assigned, and
    3. all colors regardless of whether they are assigned to any vehicle.

- Result

|lastname|name|name|
|--|--|--|
|Smith|scooter|green|
|Brown|car|green|
||bicycle|yellow|
|||blue|
|Watson|motorcycle||
|Miller|||

## Full Join mixed with INNER Join

- FULL JOIN can also appear in a query with another join type, thus creating a multiple-join with mixed types. The query below makes use of a FULL JOIN with an INNER JOIN.

```sql
    SELECT p.lastname, v.name, c.name
    FROM vehicle v
    INNER  JOIN color c ON  v.color_id = c.id
    FULL  JOIN person p ON v.person_id = p.id ;
```

- This query enables us to retrieve a list of all people, whether or not they are vehicle owners, and all vehicles that have a color assigned.

- First, tables vehicle and color are combined using an INNER JOIN. Next the derived table is combined with the person table using a FULL JOIN.

- Result

|lastname|name|name|
|--|--|--|
|Smith|scooter|green|
|Brown|car|green|
||bicycle|yellow|
|Watson|||
|Miller|||

## Summary

- A single SQL query can join two or more tables.
- When there are three or more tables involved, queries can use a single join type more than once, or they can use multiple join types.
- When using multiple join types we must carefully consider the join sequence in order to produce the desired result.
- The examples presented in this article clearly demonstrate how a minor change in the type of join, (or, in the case of multiple joins, the order in which they appear in the query) can completely change the query result, making or breaking the success of the query.

- NOTE:

- To which join combinations should we pay particular attention to? INNER JOINs with OUTER JOINs, and OUTER JOINs with OUTER JOINs. Each of these combinations can produce erroneous query results when used inappropriately.