---
title : "SQL in Python"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---
To assist in working with SQL in this course, the CS50 Library can be utilized as follows in your code:
```python
from cs50 import SQL
```
Similar to previous uses of the CS50 Library, this library will assist with the complicated steps of utilizing SQL within your Python code.

You can read more about the CS50 Library’s SQL functionality in the [documentation](https://cs50.readthedocs.io/libraries/cs50/python/#cs50.SQL).


Recall where we last left off in favorites.py. Your code should appear as follows:
```python
# Gets a specific count

import csv

from collections import Counter

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create DictReader
    reader = csv.DictReader(file)

    # Counts
    counts = Counter()

    # Iterate over CSV file, counting favorites
    for row in reader:
        favorite = row["problem"]
        counts[favorite] += 1

# Print count
favorite = input("Favorite: ")
print(f"{favorite}: {counts[favorite]}")
```

Modify your code as follows:
```python
# Searches database popularity of a problem

import csv

from cs50 import SQL

# Open database
db = SQL("sqlite:///favorites.db")

# Prompt user for favorite
favorite = input("Favorite: ")

# Search for title
rows = db.execute("SELECT COUNT(*) AS n FROM favorites WHERE problem LIKE ?", favorite)

# Get first (and only) row
row = rows[0]

# Print popularity
print(row["n"])
```
Notice that `db = SQL("sqlite:///favorites.db")` provide Python the location of the database file. Then, the line that begins with rows executes SQL commands utilizing `db.execute`. Indeed, this command passes the syntax within the quotation marks to the db.execute function. We can issue any SQL command using this syntax. 

Further, notice that rows is returned as a list of dictionaries. In this case, there is only one result, one row, returned to the rows list as a dictionary.

### Race Conditions
Utilization of SQL can sometimes result in some problems.
You can imagine a case where multiple users could be accessing the same database and executing commands at the same time.
This could result in glitches where code is interrupted by other people’s actions. This could result in a loss of data.
Built-in SQL features such as `BEGIN TRANSACTION`, `COMMIT`, and `ROLLBACK` help avoid some of these race condition problems.