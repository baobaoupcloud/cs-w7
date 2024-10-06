---
title : "Cơ sở dữ liệu quan hệ"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---
### Cơ sở dữ liệu quan hệ (Relational Databases)
Các công ty lớn như Google, Twitter, và Meta đều sử dụng **cơ sở dữ liệu quan hệ** để lưu trữ thông tin ở quy mô lớn. Cơ sở dữ liệu quan hệ lưu trữ dữ liệu dưới dạng các bảng (tables), mỗi bảng chứa các dòng và cột. Bạn có thể thao tác với dữ liệu này bằng ngôn ngữ truy vấn có cấu trúc SQL với bốn lệnh cơ bản: `Create`, `Read`, `Update`, và `Delete` - được gọi tắt là `CRUD`.

### Các hoạt động chính trong cơ sở dữ liệu quan hệ: `CRUD`

Các thao tác chính trong cơ sở dữ liệu quan hệ được gọi là **CRUD**, bao gồm:

- **Create**: Tạo mới dữ liệu
- **Read**: Đọc dữ liệu
- **Update**: Cập nhật dữ liệu
- **Delete**: Xóa dữ liệu

Chúng ta có thể tạo một cơ sở dữ liệu SQL bằng cách nhập lệnh `sqlite3 favorites.db` trên dòng lệnh. Khi được hỏi, bạn hãy nhấn `y` để xác nhận việc tạo cơ sở dữ liệu `favorites.db`. Sau khi xác nhận, bạn sẽ thấy giao diện của chương trình `sqlite3`. Để chuyển `sqlite3` sang chế độ CSV, gõ lệnh:

```bash
.mode csv
```

Sau đó, bạn có thể nhập dữ liệu từ tập tin CSV bằng lệnh:

```bash
.import favorites.csv favorites
```

Tuy bạn không thấy gì thay đổi, nhưng dữ liệu đã được thêm vào cơ sở dữ liệu. Để xem cấu trúc của cơ sở dữ liệu, bạn có thể dùng lệnh:

```bash
.schema
```

### Các lệnh cơ bản để truy vấn dữ liệu
Bạn có thể đọc dữ liệu từ bảng bằng cú pháp:

```sql
SELECT column FROM table;
```

Ví dụ: lệnh sau sẽ duyệt qua từng dòng trong bảng `favorites`:

```sql
SELECT * FROM favorites;
```

Hoặc, để lấy một phần dữ liệu cụ thể hơn:

```sql
SELECT language FROM favorites;
```

SQL hỗ trợ nhiều lệnh để truy xuất dữ liệu, bao gồm:

- **`AVG`**: Tính giá trị trung bình
- **`COUNT`**: Đếm số lượng
- **`DISTINCT`**: Lấy ra các giá trị duy nhất
- **`LOWER`**: Chuyển đổi chuỗi thành chữ thường
- **`MAX`**: Lấy giá trị lớn nhất
- **`MIN`**: Lấy giá trị nhỏ nhất
- **`UPPER`**: Chuyển đổi chuỗi thành chữ in hoa

Ví dụ:

```sql
SELECT COUNT(language) FROM favorites;
```

Lệnh này sẽ đếm số lần xuất hiện của mỗi ngôn ngữ trong bảng `favorites`. Bạn có thể sử dụng lệnh sau để đếm các ngôn ngữ riêng biệt:

```sql
SELECT DISTINCT(language) FROM favorites;
```

Để lấy tổng số lượng các ngôn ngữ riêng biệt, dùng lệnh:

```sql
SELECT COUNT(DISTINCT(language)) FROM favorites;
```

### Các lệnh SQL nâng cao
SQL cung cấp nhiều lệnh khác để tùy chỉnh truy vấn dữ liệu:

- **`WHERE`**: Thêm điều kiện để lọc dữ liệu
- **`LIKE`**: Tìm kiếm các kết quả gần đúng
- **`ORDER BY`**: Sắp xếp kết quả
- **`LIMIT`**: Giới hạn số lượng kết quả
- **`GROUP BY`**: Nhóm các kết quả lại với nhau

Để viết chú thích trong SQL, chúng ta dùng `--`.
Ngoài ra có các câu lệnh sau, để đếm số dòng có ngôn ngữ là `C`:

```sql
SELECT COUNT(*) FROM favorites WHERE language = 'C';
```

Bạn cũng có thể kết hợp nhiều điều kiện bằng lệnh `AND`:

```sql
SELECT COUNT(*) FROM favorites WHERE language = 'C' AND problem = 'Hello, World';
```

Lệnh này sẽ đếm số lần xuất hiện của ngôn ngữ `C` với bài tập `Hello, World`.

### Thêm và xóa dữ liệu trong cơ sở dữ liệu

Để thêm dữ liệu vào cơ sở dữ liệu:

```sql
INSERT INTO favorites (language, problem) VALUES ('SQL', 'Fiftyville');
```

Để xóa các dòng dữ liệu thỏa điều kiện:

```sql
DELETE FROM favorites WHERE Timestamp IS NULL;
```

Bạn cũng có thể sử dụng lệnh `UPDATE` để cập nhật dữ liệu:

```sql
UPDATE favorites SET language = 'SQL', problem = 'Fiftyville';
```

Lệnh này sẽ thay thế toàn bộ giá trị hiện tại bằng giá trị mới trong các dòng thỏa mãn điều kiện.


### Ví dụ: 
Xây dựng cơ sở dữ liệu cho các chương trình TV.
Hãy tưởng tượng rằng bạn muốn tạo một cơ sở dữ liệu để lưu trữ thông tin về các chương trình truyền hình. Bạn có thể tạo một bảng tính với các cột như `title` (tên chương trình), `star` (ngôi sao), và nhiều ngôi sao khác. Tuy nhiên, cách tiếp cận này có thể lãng phí không gian lưu trữ vì một số chương trình có rất ít diễn viên, trong khi số khác có rất nhiều.

Một cách tiếp cận tốt hơn là phân tách dữ liệu thành nhiều bảng. Ví dụ:

- Bảng `shows`: Chứa thông tin về các chương trình (với ID duy nhất cho mỗi chương trình).
- Bảng `people`: Chứa thông tin về diễn viên (với ID duy nhất cho mỗi diễn viên).
- Bảng `stars`: Liên kết các chương trình và diễn viên bằng cách sử dụng `show_id` và `person_id`.

IMDb là một ví dụ tiêu biểu cho cách lưu trữ thông tin của các chương trình truyền hình và phim ảnh. Họ có một cơ sở dữ liệu bao gồm các bảng liên kết như: diễn viên, chương trình, biên kịch, thể loại, và xếp hạng. Mỗi bảng có mối liên hệ chặt chẽ với nhau.

![shows](https://raw.githubusercontent.com/baobaoupcloud/cs-w7/main/static/images/2.relationaldatabase/relationaldatabase1.png)

Sau khi tải về tập tin [shows.db](https://github.com/cs50/lectures/blob/2022/fall/7/src7/imdb/shows.db), bạn có thể mở nó bằng lệnh:

```bash
sqlite3 shows.db
```

Quan hệ giữa bảng `shows` và `ratings` có mối quan hệ như sau:

![shows](https://raw.githubusercontent.com/baobaoupcloud/cs-w7/main/static/images/2.relationaldatabase/relationaldatabase2.png)

Bạn có thể kiểm tra mối quan hệ giữa 2 bảng trên bằng cách nhập các lệnh như:

```sql
SELECT * FROM ratings LIMIT 10;
```

Hoặc:

```sql
SELECT * FROM shows LIMIT 10;
```

Lệnh `.schema` sẽ hiển thị cấu trúc của cơ sở dữ liệu, bao gồm các bảng và các trường trong từng bảng. 

Mỗi bảng có một trường `id` duy nhất, gọi là **primary key** (khóa chính). Trường `show_id` trong bảng `genres` là **foreign key** (khóa ngoại) liên kết với trường `id` trong bảng `shows`. Khóa chính được sử dụng để định danh duy nhất một bản ghi trong bảng, trong khi khóa ngoại được sử dụng để liên kết dữ liệu giữa các bảng khác nhau.

### Các kiểu dữ liệu trong SQLite
SQLite hỗ trợ năm loại dữ liệu cơ bản:

1. **`BLOB`**: Lưu trữ dữ liệu nhị phân (hình ảnh, tệp tin).
2. **`INTEGER`**: Số nguyên.
3. **`NUMERIC`**: Số liệu với định dạng đặc biệt (ví dụ như ngày tháng).
4. **`REAL`**: Số thực (float).
5. **`TEXT`**: Chuỗi ký tự.

Bạn có thể thêm các ràng buộc đặc biệt cho từng cột như:

- **`NOT NULL`**: Cột không được phép để trống.
- **`UNIQUE`**: Giá trị của cột phải là duy nhất.

Ví dụ:

```sql
SELECT * FROM stars LIMIT 10;
```

---

### Kết hợp bảng với `JOIN`
Bạn có thể kết hợp hai hoặc nhiều bảng bằng cách sử dụng `JOIN`. Ví dụ:

```sql
SELECT * FROM shows
JOIN ratings ON shows.id = ratings.show_id
WHERE rating >= 6.0
LIMIT 10;
```

Lệnh trên kết hợp hai bảng `shows` và `ratings` để lấy các chương trình có xếp hạng từ 6.0 trở lên.

### Chỉ mục
Chỉ mục (Index) giúp tăng tốc độ truy vấn bằng cách tạo ra các cấu trúc dữ liệu đặc biệt. Tạo chỉ mục cho cột `title` bằng lệnh:

```sql
CREATE INDEX title_index ON shows (title);
```
Câu lệnh này yêu cầu sqlite3 tạo một chỉ mục và thực hiện một số tối ưu hóa đặc biệt liên quan đến cột title. Điều này tạo ra một cấu trúc dữ liệu gọi là B Tree, một cấu trúc dữ liệu có hình dạng tương tự như cây nhị phân. Tuy nhiên, khác với cây nhị phân, có thể có nhiều hơn hai nút con.

Sau khi tạo chỉ mục, truy vấn sẽ chạy nhanh hơn đáng kể:

```sql
SELECT * FROM shows WHERE title = 'The Office';
```

Tuy nhiên, việc tạo chỉ mục cho tất cả các cột có thể tốn nhiều không gian lưu trữ, vì vậy bạn cần cân nhắc khi sử dụng.

