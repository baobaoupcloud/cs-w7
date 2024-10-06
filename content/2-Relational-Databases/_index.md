---
title : "Relational Databases"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---
Google, Twitter, and Meta all use relational databases to store their information at scale.
Relational databases store data in rows and columns in structures called tables.
SQL allows for four types of commands:
- Create
- Read
- Update
- Delete


These four operations are affectionately called `CRUD`.
We can create a SQL database at the terminal by typing `sqlite3 favorites.db`. Upon being prompted, we will agree that we want to create `favorites.db` by pressing `y`.

You will notice a different prompt as we are now inside a program called `sqlite3`.

We can put `sqlite3` into csv mode by typing 

```bash
.mode csv
```

Then, we can import our data from our csv file by typing 

```bash
.import favorites.csv favorites
```

It seems that nothing has happened but we successfully imported our data.

To see the structure of the database, we can type

```bash
.schema
```

We can read items from a table using the syntax 

```sql
SELECT columns FROM table
```

For example, we can iterate every row in favorites

```sql
SELECT * FROM favorites;
```

We can get a subset of the data using the command 
```sql
SELECT language FROM favorites;
```

### Common Commands
SQL supports many commands to access data, including:
  - `AVG`
  - `COUNT`
  - `DISTINCT`
  - `LOWER`
  - `MAX`
  - `MIN`
  - `UPPER`


For example, you can count all rows of favorites by typing 

```sql
SELECT COUNT(language) FROM favorites;
```
Further, you can type to get a list of the individual languages within the database, type

```sql
SELECT DISTINCT(language) FROM favorites;
```

You could even get a distinct count of those:

```sql
SELECT COUNT(DISTINCT(language)) FROM favorites;
```

SQL offers additional commands we can utilize in our queries:

  - `WHERE`       -- adding a Boolean expression to filter our data
  - `LIKE`        -- filtering responses more loosely
  - `ORDER BY`    -- ordering responses
  - `LIMIT`       -- limiting the number of responses
  - `GROUP BY`    -- grouping responses together

Notice that we use `--` to write a comment in SQL.
For example, we can execute 
```sql
SELECT COUNT(*) FROM favorites WHERE language = 'C';
```
A count is presented.

Further, we could type 
```sql
SELECT COUNT(*) FROM favorites WHERE language = 'C' AND problem = 'Hello, World;
```
Notice how the `AND` is utilized to narrow our results.

Similarly, we could execute 
```sql
SELECT language, COUNT(*) FROM favorites GROUP BY language;
```
This would offer a temporary table that would show the language and count.

We could improve this by typing
```sql
SELECT language, COUNT(*) FROM favorites GROUP BY language ORDER BY COUNT(*);
```
This will order the resulting table by the count.
We can also `INSERT` into a SQL database utilizing the form 
```sql
INSERT INTO table (column...) VALUES(value, ...);
```

We can execute 
```sql
INSERT INTO favorites (language, problem) VALUES ('SQL', 'Fiftyville');
```

`DELETE` allows you to delete parts of your data. For example, you could:
```sql
DELETE FROM favorites WHERE Timestamp IS NULL;
```
We can also utilize the `UPDATE` command to update your data.

For example, you can execute 
```sql
UPDATE favorites SET language = 'SQL', problem = 'Fiftyville';
``` 
This will result in overwriting all previous statements where C was the favorite programming language.

Notice that these queries have immense power. Accordingly, in the real-world setting, you should consider who has permissions to execute certain commands.

### Example
We can imagine a database that we might want to create to catalog various TV shows. We could create a spreadsheet with columns like `title`, `star`, `star`, and more stars. A problem with this approach is this approach has a lot of wasted space. Some shows may have one star. Others may have dozens.

We could separate our database into multiple sheets. We could have a shows sheet and a people sheet. On the people sheet, each person could have a unique id. On the shows sheet, each show could have a unique id too. On a third sheet called stars we could relate how each show has people for each show by having a show_id and person_id. While this is an improvement, this is not an ideal database.

IMDb offers a database of people, shows, writers, stars, genres, and ratings. Each of these tables is related to one another as follows:

![shows](https://raw.githubusercontent.com/baobaoupcloud/cs-w7/main/static/images/2.relationaldatabase/relationaldatabase1.png)

After downloading [shows.db](https://github.com/cs50/lectures/blob/2022/fall/7/src7/imdb/shows.db), you can execute `sqlite3 shows.db` in your terminal window.

Let’s zero in on the relationship between two tables within the database called shows and ratings. The relationship between these two tables can be illustrated as follows:

![shows](https://raw.githubusercontent.com/baobaoupcloud/cs-w7/main/static/images/2.relationaldatabase/relationaldatabase2.png)

To illustrate the relationship between these tables, we could execute the following command: `SELECT * FROM ratings LIMIT 10;`. Examining the output, we could execute `SELECT * FROM shows LIMIT 10;`.

To understand the database, upon executing `.schema` you will find not only each of the tables but the individual fields inside each of these fields.

As you can see, shows has an id field. The genres table has a `show_id` field which has data that is common between it and the shows table.

Further, `show_id` exists in all of the tables. In the shows table, it is simply called `id`. This common field between all the fields is called a key. **Primary keys** are used to identify a unique record in a table. **Foreign keys** are used to build relationships between tables by pointing to the primary key in another table.

By storing data in a relational database, as above, data can be more efficiently stored.

In sqlite, we have five datatypes, including:
  - `BLOB`       -- binary large objects that are groups of ones and zeros
  - `INTEGER`    -- an integer
  - `NUMERIC`    -- for numbers that are formatted specially like dates
  - `REAL`       -- like a float
  - `TEXT`       -- for strings and the like

 

Additionally, columns can be set to add special constraints: `NOT NULL`, `UNIQUE`


We could execute 
```sql
SELECT * FROM stars LIMIT 10;
```
`show_id` is a foreign key in this final query because `show_id` corresponds to the unique id field in shows. `person_id` corresponds to the unique id field in the people column.

We can further play with this data to understand these relationships. Execute 
```sql
SELECT * FROM ratings;
```
There are a lot of ratings!

We can further limit this data down by executing 
```sql
SELECT show_id FROM ratings WHERE rating >= 6.0 LIMIT 10;
```
From this query, you can see that there are 10 shows presented. However, we don’t know what show each `show_id` represents.

You can discover what shows these are by executing 
```sql
SELECT * FROM shows WHERE id = 626124;
```

We can further our query to be more efficient by executing:

```sql
SELECT title
FROM shows
WHERE id IN (
    SELECT show_id
    FROM ratings
    WHERE rating >= 6.0
    LIMIT 10
)
```
Notice that this query nests together two queries. An inner query is used by an outer query.


### JOINs
We are pulling data from shows and ratings.
How could we combine tables temporarily? Tables could be joined together using the `JOIN` command.
Execute the following command:
```sql
SELECT * FROM shows
  JOIN ratings on shows.id = ratings.show_id
  WHERE rating >= 6.0
  LIMIT 10;
```
Notice this results in a wider table than we have previously seen.
Where the previous queries have illustrated the one-to-one relationship between these keys, let’s examine some one-to-many relationships. Focusing on the genres table, execute the following:
```sql
SELECT * FROM genres
LIMIT 10;
```
Notice how this provides us a sense of the raw data. You might notice that one shows have three values. This is a one-to-many relationship.

We can learn more about the genres table by typing `.schema genres`.
Execute the following command to learn more about the various comedies in the database:
```sql
SELECT title FROM shows
WHERE id IN (
  SELECT show_id FROM genres
  WHERE genre = 'Comedy'
  LIMIT 10
);
```
Notice how this produces a list of comedies, including Catweazle.

To learn more about Catweazle, by joining various tables through a join:
```sql
SELECT * FROM shows
JOIN genres
ON shows.id = genres.show_id
WHERE id = 63881;
```
Notice that this results in a temporary table. It is fine to have duplicate table.

A final relationship is a many-to-many relationship.
We can learn more about the show The Office by executing the following command:
```sql
SELECT person_id FROM stars
WHERE show_id = (
  SELECT id FROM shows
  WHERE title = 'The Office' AND year = 2005
);
```
Notice that this results in a table that includes the person_ids of various stars.
I could learn more about this group of actors by executing the following:
```sql
SELECT name FROM people
WHERE id IN (
    SELECT person_id FROM stars
    WHERE show_id = (
      SELECT id FROM shows
      WHERE title = 'The Office' AND year = 2005
    )
);
```
This results in a top-billed stars.
We can further understand this data by executing:
```sql
SELECT title from shows
WHERE id IN (
  SELECT show_id FROM stars
  WHERE person_id = (
    SELECT id FROM people
    WHERE name = 'Steve Carell'
  )
);
```
This results in a list of titles of shows wherein Steve Carell stared.

The wildcard `%` operator can be used to find all people whose names start with Steve C one could employ the syntax 
```sql
SELECT * FROM people WHERE name LIKE 'Steve C%';
```

### Indexes
While relational databases have the ability to be more fast and more robust than utilizing a CSV file, data can be optimized within a table using indexes.

Indexes can be utilized to speed up our queries.
We can track the speed of our queries by executing `.timer on` in sqlite3.
To understand how indexes can speed up our queries, run the following: 
```sql
SELECT * FROM shows WHERE title = 'The Office';
```
Notice the time that displays after the query executes.
Then, we can create an index with the syntax 
```sql
CREATE INDEX title_index on shows (title);
```
This tells sqlite3 to create an index and perform some special under-the-hood optimization relating to this column title.

This will create a data structure called a B Tree, a data structure that looks similar to a binary tree. However, unlike a binary tree, there can be more than two child notes.

Running the query 
```sql
SELECT * FROM shows WHERE title = 'The Office';
``` 
You will notice that the query runs much more quickly!

Unfortunately, indexing all columns would result in utilizing more storage space. Therefore, there is a tradeoff for enhanced speed.