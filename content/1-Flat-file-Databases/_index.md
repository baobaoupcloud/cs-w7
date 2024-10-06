---
title : "Flat-file Databases"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Data can often be described in patterns of columns and rows.
Spreadsheets like those created in Microsoft Excel and Google Sheets can be outputted to a csv or comma-separated values file.

If you look at a csv file, you’ll notice that the file is flat in that all of our data is stored in a single table represented by a text file. We call this form of data a **flat-file database**.

Python comes with native support for csv files.
First, download [favorites.csv](https://cdn.cs50.net/2023/fall/lectures/7/src7/favorites/favorites.csv) and upload it to your file explorer inside cs50.dev. Second, in your terminal window, type `code favorites.py` and write code as follows:
```python
# Prints all favorites in CSV using csv.reader

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create reader
    reader = csv.reader(file)

    # Skip header row
    next(reader)

    # Iterate over CSV file, printing each favorite
    for row in reader:
        print(row[1])
```
Notice that the csv library is imported. Further, we created a `reader` that will hold the result of `csv.reader(file)`. The `csv.reader` function reads each row from the file, and in our code we store the results in `reader`. `print(row[1])`, therefore, will print the language from the `favorites.csv` file.

You can improve your code as follows:
```python
# Stores favorite in a variable

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create reader
    reader = csv.reader(file)

    # Skip header row
    next(reader)

    # Iterate over CSV file, printing each favorite
    for row in reader:
        favorite = row[1]
        print(favorite)
```
Notice that `favorite` is stored and then printed. Also notice that we use the next function to skip to the next line of our reader.
One of the disadvantages of the above approach is that we are trusting that `row[1]` is always the favorite. However, what would happen if the columns have been moved around?

We can fix this potential issue. Python also allows you to index by the keys of a list. Modify your code as follows:
```python
# Prints all favorites in CSV using csv.DictReader

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create DictReader
    reader = csv.DictReader(file)

    # Iterate over CSV file, printing each favorite
    for row in reader:
        favorite = row["language"]
        print(favorite)
```
Notice that this example directly utilizes the `language` key in the `print` statement.
This could be further simplified to:
```python
# Prints all favorites in CSV using csv.DictReader

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create DictReader
    reader = csv.DictReader(file)

    # Iterate over CSV file, printing each favorite
    for row in reader:
        print(row["language"])
```
To count the number of favorite languages expressed in the csv file, we can do the following:
```python
# Counts favorites using variables

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create DictReader
    reader = csv.DictReader(file)

    # Counts
    scratch, c, python = 0, 0, 0

    # Iterate over CSV file, counting favorites
    for row in reader:
        favorite = row["language"]
        if favorite == "Scratch":
            scratch += 1
        elif favorite == "C":
            c += 1
        elif favorite == "Python":
            python += 1

# Print counts
print(f"Scratch: {scratch}")
print(f"C: {c}")
print(f"Python: {python}")
```
Notice that each language is counted using if statements. Further notice the double equal `==` signs in those if statements.
Python allows us to use a dictionary to count the `counts` of each language. Consider the following improvement upon our code:
```python
# Counts favorites using dictionary

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create DictReader
    reader = csv.DictReader(file)

    # Counts
    counts = {}

    # Iterate over CSV file, counting favorites
    for row in reader:
        favorite = row["language"]
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1

# Print counts
for favorite in counts:
    print(f"{favorite}: {counts[favorite]}")
```
Notice that the value in counts with the key `favorite` is incremented when it exists already. If it does not exist, we define `counts[favorite]` and set it to `1`. Further, the formatted string has been improved to present the `counts[favorite]`.

Python also allows sorting counts. Improve your code as follows:
```python
# Sorts favorites by key

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create DictReader
    reader = csv.DictReader(file)

    # Counts
    counts = {}

    # Iterate over CSV file, counting favorites
    for row in reader:
        favorite = row["language"]
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1

# Print counts
for favorite in sorted(counts):
    print(f"{favorite}: {counts[favorite]}")
```
If you look at the parameters for the sorted function in the Python documentation, you will find it has many built-in parameters. You can leverage some of these built-in parameters as follows:
```python
# Sorts favorites by value using .get

import csv

# Open CSV file
with open("favorites.csv", "r") as file:

    # Create DictReader
    reader = csv.DictReader(file)

    # Counts
    counts = {}

    # Iterate over CSV file, counting favorites
    for row in reader:
        favorite = row["language"]
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1

# Print counts
for favorite in sorted(counts, key=counts.get, reverse=True):
    print(f"{favorite}: {counts[favorite]}")
```
Notice the arguments passed to `sorted`. The key argument allows you to tell Python the method you wish to use to sort items. In this case `counts.get` is used to sort by the values. `reverse=True` tells sorted to sort from largest to smallest.

Python has numerous libraries that we can utilize in our code. One of these libraries is collections, from which we can import `Counter`. `Counter` will allow you to access the counts of each language without the headaches of all the `if` statements seen in our previous code. You can implement as follows:
```python
# Sorts favorites by value using .get

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
        favorite = row["language"]
        counts[favorite] += 1

# Print counts
for favorite, count in counts.most_common():
    print(f"{favorite}: {count}")
```
Notice how `counts = Counter()` enables the use of this imported `Counter` class from `collections`.
