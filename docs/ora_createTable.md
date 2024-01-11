# Lệnh tạo bảng trong Oracle - Create Table

Trong Oracle, để tạo một bảng mới ta có thể sử dụng lệnh `CREATE TABLE` và phải chạy nó trong một database cụ thể 

Chúng ta có hai thành phần quan trọng khi tạo Table đó là **khóa ngoại**, **khóa chính**, **giá trị mặc định**, **tăng tự động**...

# Lệnh Create Table

Sau đây là cú pháp lệnh tạo bảng:

```sh
CREATE TABLE table_name  
(   
  column1 datatype [ NULL | NOT NULL ],  
  column2 datatype [ NULL | NOT NULL ],  
  ...  
  column_n datatype [ NULL | NOT NULL ]  
);
```

Trong đó:

- `table_name`: Là tên của table, nó phải là duy nhất trong một database, nghĩa là không được trùng tên table

- `column 1`, `column 2` ... `colume n`: Là các fied của table, mỗi field sẽ có các thông tin bổ sung khác như kiểu dữ liệu, giá trị dữ liệu, giá trị mặc định ... và NULL hoặc NOT NULL

Sau đây là ví dụ, tạo bảng `customers`

```sh
CREATE TABLE customers  
( customer_id number(10) NOT NULL,  
  customer_name varchar2(50) NOT NULL,  
  city varchar2(50)  
);
```

![](/images/createTable.png)

Trong bảng này chứa 3 cột sau:

- `customer_id`: Mã số của khách hàng, đây sẽ là khóa chính, kiểu dữ liệu của nó là number có chiều dài tối đa 10 ký tự, và không được NULL, nghĩa là phải nhập dữ liệu lúc thêm

- `customer_name`: Tên của khách hàng, kiểu dữ liệu của nó là `varchar2` (chuỗi) có chiều dài tối đa là 50 ký tự, có thẻ NULL

- `city`: Tên thành phố mà nhân viên đang ở, đây là kiểu `varchar2` (chuỗi) có chiều dài tối đa là 50 ký tự, không có thẻ NULL (tức là có thể bỏ trống)

![](/images/createTable1.png)

# Thêm khóa chính vào Table

Nếu bạn muốn bổ sung khóa chính ngay trong lúc tạo bảng thì hãy sử dụng lệnh CONSTRAINT 

Như ở ví dụ này, tạo khóa chính như sau:

```sh
CREATE TABLE customers  
( customer_id number(10) NOT NULL,  
  customer_name varchar2(50) NOT NULL,  
  city varchar2(50),  
  CONSTRAINT customers_pk PRIMARY KEY (customer_id)  
);
```

Thực hiện insert thêm dòng sau vào Table

```sh
INSERT INTO customers (customer_id, customer_name, city)
VALUES (1, 'tubt', 'Ha Noi');
```

![](/images/primaryKey.png)

Tiếp tục insert thêm 1 dòng nữa vào Table, và giữ nguyên giá trị của cột `customer_id`

```sh
INSERT INTO customers (customer_id, customer_name, city)
VALUES (1, 'tubui', 'Dong Anh');
```

![](/images/primaryKey1.png)

Sau khi insert dòng trên chúng ta đã nhận được thông báo lỗi `ORA-00001: unique constraint (SYS.CUSTOMERS_PK) violated` do chúng ta đã cố gắng thêm một record vào một bảng có unique constraint (ràng buộc) và giá trị của cột duy nhất này đã tồn tại trong bảng (giá trị cột `customer_id` bị lặp lại).

Đó là công dụng của việc sử dụng Primary key

Khóa chính là những field có dữ liệu không được trùng, nó được dùng để phân biệt các record trong bảng (các dòng). Ví dụ như khi bạn đi học sẽ có mã sinh viên, mỗi sinh viên sẽ có một mã duy nhất. Hoặc chứng minh nhân dân hay thẻ căn cước của bạn cũng là duy nhất

# Lệnh Create Table AS

Ngoài cách tạo bảng trên thì bạn còn một cách khác đó là tạo bảng từ một lệnh `SELECT` khác, nó sẽ thực hiện copy tất cả columns và dữ liệu từ lệnh `SELECT` vào bảng mới

Cú pháp như sau:

```sh
CREATE TABLE new_table  
AS (SELECT * FROM old_table);
```

Ta có một bảng `customers` như sau

![](/images/createTableAS.png)

**Ví dụ: Hãy tạo một bảng mới newcustomers từ bảng customers**

```sh
CREATE TABLE newcustomers  
AS (SELECT * FROM customers  WHERE customer_id < 3);
```

![](/images/createTableAS1.png)

Lệnh trên đã sao chép dữ liệu từ bảng `customers` sang bảng `newcustomers` và sử dụng mệnh đề `WHERE` để lấy các record có `customer_id < 3`

Lệnh này sẽ chậm hơn vì phải mất thời gian thực hiện lệnh select rồi mới tạo bảng 

**Ví dụ: Hãy tạo bảng `newcustomers` từ bảng `customer` và chỉ lấy 2 columns `custumor_id`, `customer_name`**

```sh
CREATE TABLE newcustomers1
AS (SELECT customer_id, customer_name
    FROM customers
    WHERE customer_id < 3);
```

![](/images/createTableAS2.png)

Tương tự lệnh trên nhưng lần này chỉ sao chép cột `customer_id` và `customer_name`

Ngoài ra bạn cũng có thể sử dụng lệnh JOIN trong lệnh select để ghép nhiều bảng với nhau, điều này sẽ giúp bạn tạo bảng từ nhiều cột từ nhiều table khác nhau