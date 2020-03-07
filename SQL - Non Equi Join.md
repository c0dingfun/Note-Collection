# [SQL Non Equi Join](https://learnsql.com/blog/illustrated-guide-sql-non-equi-join/#top)

## What is Non Equi Join

- SQL Queries that use following non-equi join operators:

|Operator|Meaning|
|--------|-------|
|“>”|Greater than|
|“>=”|Greater than or equal to|
|“<”|Less than|
|“<=”|Less than or equal to|
|“!=”|Not equal to|
|”<>”|Not equal to (ANSI Standard)|
|BETWEEN … AND|Values in a range between x and y|

- Note: a SQL non equi join can only be used with one or two tables.

## Data for "person" and "apartment" tables

- "person" table

|id|first_name|last_name|rooms|min_price|max_price|apartment_id|
|--|--|--|--|--|--|--|--|
|1|Anne|Miller|2|40,000|150,000|2|
|2|John|Harris|1|20,000|50,000|2|
|3|Michael|Moore|2|200,000|300,000|6|
|4|Oliver|Watson|4|30,000|100,000|7|

- "apartment" table

|id|rooms|price|city|
|--|--|--|--|
|1|2|30,000|Houston|
|2|2|45,000|Dallas|
|3|3|125,000|Chicago|
|4|5|245,000|Los Angeles|
|5|4|340,000|San Jose|
|6|4|220,000|San Diego|
|7|1|36,000|Cleveland|

## List the apartment chosen by a person (Equi Join)

```sql
    SELECT first_name, last_name, price, city
    FROM person p
    JOIN apartment a ON p.apartment_id = a.id
```

- Results:

|first_name|last_name|price|city|
|--|--|--|--|
|anne|miller|30000.00|houston|
|john|harris|45000.00|dallas|
|michael|moore|220000.00|san diego|
|oliver|watson|36000.00|cleveland|

## List apartments not chosen but within a person's range (Non-Equi Join)

```sql
    SELECT first_name, last_name, min_price, max_price, price, city
    FROM person p
    JOIN apartment a ON price BETWEEN min_price and max_price  AND a.id != p.apartment_id
```

- Results:

|first_name|last_name|min_price|max_price|price|city|
|--|--|--|--|--|--|
|anne|miller|40000.00|150000.00|45000.00|dallas|
|anne|miller|40000.00|150000.00|125000.00|chicago|
|john|harris|20000.00|50000.00|30000.00|houston|
|john|harris|20000.00|50000.00|36000.00|cleveland|
|michael|moore|200000.00|300000.00|245000.00|los angeles|
|oliver|watson|30000.00|100000.00|30000.00|houston|
|oliver|watson|30000.00|100000.00|45000.00|dallas|

## Non-Equi SELF JOIN - very fun

- the "playing_cards" table

- Data

|id|rank|suit|
|--|--|--|
|1|A|Hearts|
|2|A|Spades|
|3|A|Clubs|
|4|K|Spades|
|5|K|Diamonds|
|6|Q|Clubs|
|7|J|Spades|

## Find all card pairs (A of H + A of S, A of C + K of S, etc)  

```sql
    SELECT
        pc1.rank as 'rank 1', pc1.suit as 'suit 1',
        pc2.rank as 'rank 2', pc2.suit as 'suit 2'
    FROM playing_cards pc1
    JOIN playing_cards pc2 ON pc1.id != pc2.i
```

- Results:

|rank 1|suit 1|rank 2|suit 2|
|--|--|--|--|
|A|Spades|A|Hearts|
|A|Clubs|A|Hearts|
|K|Spades|A|Hearts| -- duplicate
|K|Diamonds|A|Hearts|
|Q|Clubs|A|Hearts|
|J|Spades|A|Hearts|
|A|Hearts|A|Spades| -- duplicate
|A|Clubs|A|Spades|  
|K|Spades|A|Spades|
|K|Diamonds|A|Spades|
|Q|Clubs|A|Spades|
|J|Spades|A|Spades|
|A|Hearts|A|Clubs|
|A|Spades|A|Clubs|
|...|

- But, there are Duplicate Pairs

## Remove Duplicate Pairs

```sql
    SELECT
        pc1.rank as 'rank 1', pc1.suit as 'suit 1',
        pc2.rank as 'rank 2', pc2.suit as 'suit 2'
    FROM playing_cards pc1
    JOIN playing_cards pc2
    ON pc1.id != pc2.id -- this have all the pairs
            AND
        pc1.id < pc2.id -- this eliminate the duplicates
```

|rank 1|suit 1|rank 2|suit 2|
|--|--|--|--|
|A|Spades|A|Hearts|
|A|Clubs|A|Hearts|
|K|Spades|A|Hearts|
|K|Diamonds|A|Hearts|
|Q|Clubs|A|Hearts|
|J|Spades|A|Hearts|
|A|Clubs|A|Spades|  
|K|Spades|A|Spades|
|K|Diamonds|A|Spades|
|Q|Clubs|A|Spades|
|J|Spades|A|Spades|
|A|Spades|A|Clubs|
|...|
