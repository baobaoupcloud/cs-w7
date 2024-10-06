---
title : "Tấn công SQL Injection"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---
Một trong những vấn đề thường gặp trong các ứng dụng SQL thực tế là **tấn công SQL Injection**. Đây là một hình thức tấn công trong đó kẻ xấu có thể chèn mã SQL độc hại vào hệ thống thông qua các đầu vào của người dùng.

#### Ví dụ về tấn công SQL Injection

Hãy xem xét một màn hình đăng nhập đơn giản:

![login](https://raw.githubusercontent.com/baobaoupcloud/cs-w7/main/static/images/4.injectionattacks/injectionattacks1.png)

Nếu mã nguồn của hệ thống không có các biện pháp bảo vệ phù hợp, kẻ xấu có thể dễ dàng chèn mã SQL độc hại. Hãy xem xét ví dụ sau:

```python
rows = db.execute("SELECT COUNT(*) FROM users WHERE username = ? AND password = ?", username, password)
```

Ở đây, các dấu chấm hỏi (`?`) đóng vai trò như các **placeholder** giúp đảm bảo rằng mọi dữ liệu người dùng nhập vào sẽ được xác thực trước khi được đưa vào câu truy vấn SQL. Điều này giúp ngăn chặn các đoạn mã độc hại chèn vào hệ thống.

#### Không sử dụng chuỗi định dạng trong truy vấn

Bạn không bao giờ nên sử dụng chuỗi định dạng (formatted strings) trong các câu lệnh truy vấn như ví dụ dưới đây hoặc tin tưởng vào bất kỳ đầu vào nào từ người dùng mà không kiểm tra kỹ.

```python
rows = db.execute(f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'")
```

Cách viết trên dễ dẫn đến nguy cơ tấn công SQL Injection. Thay vào đó, bạn nên sử dụng cú pháp có dấu chấm hỏi (`?`) như trong ví dụ trước.

#### Tính năng bảo vệ của thư viện CS50

Khi bạn sử dụng thư viện CS50, thư viện sẽ tự động xử lý và loại bỏ các ký tự nguy hiểm có khả năng gây ra tấn công SQL Injection. Thư viện này sẽ đảm bảo rằng các đoạn mã do người dùng nhập vào sẽ được xử lý an toàn trước khi truyền vào truy vấn SQL.

Ví dụ:

```python
rows = db.execute("SELECT COUNT(*) FROM users WHERE username = ? AND password = ?", username, password)
```

Đoạn mã trên không chỉ giúp ngăn chặn các cuộc tấn công SQL Injection mà còn đảm bảo rằng dữ liệu đầu vào được xử lý an toàn, tránh rủi ro bảo mật.

Vậy là kết thúc tuần 7 ^^
