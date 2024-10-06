---
title : "SQL với Python"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---
Để hỗ trợ trong quá trình làm việc với SQL trong khóa học này, thư viện CS50 có thể được sử dụng như sau trong mã của bạn:
```python
from cs50 import SQL
```
Tương tự như các lần sử dụng thư viện CS50 trước đây, thư viện này sẽ hỗ trợ các bước phức tạp trong việc sử dụng SQL trong mã Python của bạn.

Bạn có thể đọc thêm về chức năng SQL của thư viện CS50 trong [tài liệu](https://cs50.readthedocs.io/libraries/cs50/python/#cs50.SQL).

Nhớ lại trong ví dụ trước, tập tin `favorites.py` được viết lại như sau:
```python
# Đếm số lượng cụ thể

import csv
from collections import Counter

# Mở tệp CSV
with open("favorites.csv", "r") as file:

    # Tạo DictReader
    reader = csv.DictReader(file)

    # Đếm
    counts = Counter()

    # Duyệt qua tệp CSV và đếm các mục yêu thích
    for row in reader:
        favorite = row["problem"]
        counts[favorite] += 1

# In số lượng
favorite = input("Mục yêu thích: ")
print(f"{favorite}: {counts[favorite]}")

```

Hãy sửa đổi mã của bạn như sau:
```python
# Tìm kiếm mức độ phổ biến của một vấn đề trong cơ sở dữ liệu

import csv
from cs50 import SQL

# Mở cơ sở dữ liệu
db = SQL("sqlite:///favorites.db")

# Yêu cầu người dùng nhập mục yêu thích
favorite = input("Mục yêu thích: ")

# Tìm kiếm tiêu đề
rows = db.execute("SELECT COUNT(*) AS n FROM favorites WHERE problem LIKE ?", favorite)

# Lấy hàng đầu tiên (và duy nhất)
row = rows[0]

# In mức độ phổ biến
print(row["n"])
```
Lưu ý rằng `db = SQL("sqlite:///favorites.db")` cung cấp cho Python vị trí của tệp cơ sở dữ liệu. Sau đó, dòng bắt đầu bằng `rows` thực hiện các lệnh SQL bằng cách sử dụng `db.execute`. Thực tế, lệnh này truyền cú pháp trong dấu ngoặc kép cho hàm `db.execute`. Chúng ta có thể thực hiện bất kỳ lệnh SQL nào bằng cú pháp này.

Hơn nữa, lưu ý rằng `rows` được trả về dưới dạng một danh sách các từ điển. Trong trường hợp này, chỉ có một kết quả, một hàng, được trả về cho danh sách `rows` dưới dạng một từ điển.

### Tình trạng Race (Race Conditions)
Việc sử dụng SQL đôi khi có thể dẫn đến một số vấn đề.  
Bạn có thể tưởng tượng một trường hợp mà nhiều người dùng có thể truy cập cùng một cơ sở dữ liệu và thực hiện các lệnh cùng một lúc.  
Điều này có thể dẫn đến những lỗi mà mã bị gián đoạn bởi hành động của người khác, và có thể dẫn đến mất dữ liệu.  
Các tính năng tích hợp sẵn của SQL như `BEGIN TRANSACTION`, `COMMIT`, và `ROLLBACK` giúp tránh một số vấn đề tình trạng race này.
