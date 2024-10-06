---
title : "Cơ sở dữ liệu dạng Flat-File"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Dữ liệu thường được mô tả bằng các mẫu cột và hàng. Các bảng tính như Microsoft Excel hoặc Google Sheets có thể được xuất thành tập tin `.csv` (Comma-Separated Values - Giá trị được phân tách bằng dấu phẩy). Khi bạn mở một tập tin `.csv`, bạn sẽ thấy rằng dữ liệu được lưu trữ trong một bảng duy nhất đại diện bằng một tập tin văn bản phẳng, do đó loại dữ liệu này được gọi là **cơ sở dữ liệu dạng tập tin phẳng - Flat-File**.

Python hỗ trợ xử lý các tập tin `.csv` một cách tự nhiên. Để thực hành, trước tiên, hãy tải tập tin [favorites.csv](https://cdn.cs50.net/2023/fall/lectures/7/src7/favorites/favorites.csv) và tải nó lên môi trường làm việc của bạn trong cs50.dev. Sau đó, mở cửa sổ dòng lệnh, nhập `code favorites.py` và viết đoạn mã sau:

```python
# In ra tất cả các mục yêu thích trong tập tin CSV sử dụng csv.reader

import csv

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    # Tạo đối tượng reader
    reader = csv.reader(file)

    # Bỏ qua dòng tiêu đề
    next(reader)

    # Duyệt qua từng dòng trong tập tin CSV và in ra mục yêu thích
    for row in reader:
        print(row[1])
```

Chú ý rằng thư viện `csv` đã được nhập vào. Chúng ta đã tạo một đối tượng `reader` để lưu kết quả của `csv.reader(file)`. Hàm `csv.reader` đọc từng dòng trong tập tin và lưu kết quả vào `reader`. `print(row[1])` sẽ in ra ngôn ngữ yêu thích từ tập tin `favorites.csv`.

Bạn có thể cải thiện đoạn mã như sau:

```python
# Lưu mục yêu thích vào một biến

import csv

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    # Tạo đối tượng reader
    reader = csv.reader(file)

    # Bỏ qua dòng tiêu đề
    next(reader)

    # Duyệt qua từng dòng trong tập tin CSV và in ra mục yêu thích
    for row in reader:
        favorite = row[1]
        print(favorite)
```

Ở đây, giá trị mục yêu thích được lưu vào biến `favorite` trước khi in ra. Cũng lưu ý rằng chúng ta đã sử dụng hàm `next` để bỏ qua dòng tiêu đề.

### Khắc phục lỗi chỉ mục
Một nhược điểm của phương pháp trên là chúng ta giả định rằng `row[1]` luôn là mục yêu thích. Tuy nhiên, nếu các cột bị di chuyển thì sẽ xảy ra lỗi. Để khắc phục, bạn có thể sử dụng `csv.DictReader`, cho phép truy cập giá trị bằng tên cột:

```python
# In tất cả mục yêu thích bằng cách sử dụng csv.DictReader

import csv

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    # Tạo đối tượng DictReader
    reader = csv.DictReader(file)

    # Duyệt qua từng dòng trong tập tin CSV và in ra mục yêu thích
    for row in reader:
        favorite = row["language"]
        print(favorite)
```

Bạn cũng có thể viết gọn lại đoạn mã trên:

```python
# In tất cả mục yêu thích bằng cách sử dụng csv.DictReader

import csv

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    # Tạo đối tượng DictReader
    reader = csv.DictReader(file)

    # Duyệt qua từng dòng trong tập tin CSV và in ra mục yêu thích
    for row in reader:
        print(row["language"])
```

### Đếm số lượng 
Để đếm số lần xuất hiện của từng ngôn ngữ yêu thích trong tập tin `.csv`, bạn có thể làm như sau:

```python
# Đếm các ngôn ngữ yêu thích sử dụng các biến

import csv

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    # Tạo đối tượng DictReader
    reader = csv.DictReader(file)

    # Biến đếm
    scratch, c, python = 0, 0, 0

    # Duyệt qua từng dòng trong tập tin CSV và đếm mục yêu thích
    for row in reader:
        favorite = row["language"]
        if favorite == "Scratch":
            scratch += 1
        elif favorite == "C":
            c += 1
        elif favorite == "Python":
            python += 1

# In kết quả đếm
print(f"Scratch: {scratch}")
print(f"C: {c}")
print(f"Python: {python}")
```

Cách này sử dụng các biến để đếm từng ngôn ngữ yêu thích. Bạn cũng có thể cải thiện mã nguồn bằng cách sử dụng từ điển để lưu trữ số lần xuất hiện:

```python
# Đếm các ngôn ngữ yêu thích bằng từ điển

import csv

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    # Tạo đối tượng DictReader
    reader = csv.DictReader(file)

    # Tạo biến đếm bằng từ điển
    counts = {}

    # Duyệt qua từng dòng trong tập tin CSV và đếm mục yêu thích
    for row in reader:
        favorite = row["language"]
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1

# In kết quả đếm
for favorite in counts:
    print(f"{favorite}: {counts[favorite]}")
```

### Sắp xếp theo giá trị
Python cho phép sắp xếp các giá trị trong từ điển bằng cách sử dụng hàm `sorted`:

```python
# Sắp xếp mục yêu thích theo giá trị

import csv

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    # Tạo đối tượng DictReader
    reader = csv.DictReader(file)

    # Biến đếm
    counts = {}

    # Duyệt qua từng dòng trong tập tin CSV và đếm mục yêu thích
    for row in reader:
        favorite = row["language"]
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1

# In kết quả sau khi sắp xếp
for favorite in sorted(counts, key=counts.get, reverse=True):
    print(f"{favorite}: {counts[favorite]}")
```

Trong đoạn mã trên, `key=counts.get` giúp sắp xếp theo giá trị, và `reverse=True` sắp xếp từ cao đến thấp.

Python có nhiều thư viện hỗ trợ xử lý dữ liệu như `collections`. Bạn có thể sử dụng `Counter` từ thư viện này để đơn giản hóa việc đếm như sau:

```python
# Sử dụng Counter để đếm các mục yêu thích

import csv
from collections import Counter

# Mở tập tin CSV
with open("favorites.csv", "r") as file:
    reader = csv.DictReader(file)

    # Đếm bằng Counter
    counts = Counter(row["language"] for row in reader)

# In kết quả
for favorite, count in counts.most_common():
    print(f"{favorite}: {count}")
```




