# Alter Table

Trong oracle chúng ta có thể sử dụng lệnh `ALTER TABLE` để chỉnh sửa cấu trúc của table nhuw theme column, xóa column, đổi kiểu dữ liệu, đổi tên table...

## Thêm một column vào Table - Oracle

Khi bạn muốn bổ sung một column vào bảng nhưng không biết bắt đầu từ đâu, vậy thì hãy sử dụng lệnh `ALTER TABLE` với cú pháp sau:

```sh
ALTER TABLE table_name ADD column-definition;
```

Trong đó:

- `table_name`: Là tên bảng muốn thay đổi

- `column_name`: Là tên column muốn thêm vào

- `column-definition`: Là các thông số dành cho column đó 

Ví dụ: Trong lúc tạo bảng customers ta đã quên thêm column `customers_age`, nên mình sẽ bổ sung nó bằng câu lệnh SQL theo cú pháp ở trên

Tuy nhiên trước tiên ta sẽ xác định các thông số cho column đó là: Kiểu varchar2 và chiều dài tối đa là 50 ký tự. Và đây là câu lệnh:

```sh
ALTER TABLE customers
ADD customers_age varchar2(50);
```

![](/images/alterTable.png)

![](/images/alterTable1.png)

## Thêm nhiều column vào Table - Oracle

Nếu bạn muốn thêm nhiều column thì chỉ cần sử dụng cặp dấu ngoặc `()` và mỗi column cách nhau bởi dấu phẩy

```sh
ALTER TABLE table_name
ADD (column_1 column-definition_1,
	column_2 column-definition_2,
	...
	column_n column-definition_n);
```

Ví dụ:

```sh
ALTER TABLE customers
ADD (
customer_type varchar2(50),
customer_address varchar2(50)
);
```

![](/images/alterTable2.png)

Chạy 2 câu lệnh này xong là ta đã thêm 2 column `customer_type` và `customer_address` thành công

## Thay đổi cấu trúc của một Column - Oracle

Ta sẽ sử dụng từ khóa `MODIFY` để thay đổi cấu trúc cho column

Cú pháp:

```sh
ALTER TABLE table_name
MODIFY column_name column_type;
```

Ví dụ:

```sh
ALTER TABLE customers
MODIFY customer_name varchar2(100);
```

Chạy lệnh trên xong là ta đã thay đổi thành công cấu trúc của column `customer_name`

Trước khi thay đổi cấu trúc của column `customer_name`

![](/images/modify.png)

Sau khi thay đổi cấu trúc, cột `customer_name` cho phép nhận vào giá trị thay đổi từ 50 -> 100

![](/images/afterModify.png)


## Thay đổi cấu trúc nhiều Column - Oracle

Tương tự, bạn sẽ cần phải sử dụng `()` và dấu phẩy để ngăn giữa các column

Cú pháp:

```sh
ALTER TABLE table_name  
MODIFY (
column_1 column_type,  
column_2 column_type,  
...  
column_n column_type
);
```

Ví dụ:

```sh
ALTER TABLE customers  
MODIFY (
customer_name varchar2(20) not null,  
city varchar2(100)
);
```

Lệnh trên sẽ thay đổi cấu trúc cho cả 2 cột `customer_name` và `city` trong bảng `customers`

Trước khi `MODIFY`

![](/images/before.png)

Sau khi `MODIFY`

![](/images/after.png)

## Xóa một cột ra khỏi Table - Oracle

Để xóa một cột ra khỏi Table thì bạn sẽ sử dụng từ khóa `DROP COLUMN`

Cú pháp

```sh
ALTER TABLE table_name  
DROP COLUMN column_name;
```

Ví dụ:

```sh
ALTER table customers
DROP column customer_address;
```

Lệnh trên sẽ xóa cột `customer_address` trong bảng `customers`

Trước khi `DROP`

![](/images/beforeDrop.png)

Sau khi `DROP`

![](/images/afterDrop.png)

## Thay đổi tên colume của Table - Oracle

Để thay đổi tên của một colume trong Table ta sử dụng từ khóa `RENAME COLUMN`

Cú pháp:

```sh
ALTER TABLE table_name  
RENAME COLUMN old_name to new_name;
```

Ví dụ: 

```sh
ALTER TABLE customers
RENAME COLUMN customer_name to c_name;
```

Lệnh trên sẽ đổi tên cột `customer_name` thành `c_name`

Trước khi `RENAME COLUMN`

![](/images/rename.png)

Sau khi `RENAME COLUMN`

![](/images/afterRename.png)

## Đổi tên của Table - Oracle

Tên của Table rất ít khi thay đổi nhưng ta có thể thay đổi được bằng cách sử dụng từ khóa `RENAME TO`

Cú pháp:

```sh
ALTER TABLE table_name  
RENAME TO new_table_name;
```

Ví dụ:

```sh
ALTER TABLE customers
RENAME TO cus;
```

Lệnh trên sẽ đổi tên bảng `customers` thành `cus`

Trước khi `RENAME`

![](/images/beforeRename.png)

Sau khi `RENAME`

![](/images/afterRename1.png)

Như vậy là ta đã thực hiện đổi tên cho table thành công