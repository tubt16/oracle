# Lệnh xóa bảng trong Oracle - Drop Table

## Drop Table trong Oracle

Cú pháp:

```sh
DROP [schema_name].TABLE table_name  
[ CASCADE CONSTRAINTS ]  
[ PURGE ];
```

Trong đó:

- `schema_name`: Tên của schema chứa bảng

- `table_name`: Tên của bảng mà bạn muốn xóa ra khỏi hệ quản trị CSDL

- `CASCADE CONTRAINTS`: Nếu bạn thêm từ khóa này nó sẽ xóa bỏ toàn bộ tham chiếu khóa ngoại (Foreign Key) đến bảng này

- `PURGE`: Nếu bạn thêm từ khóa này thì các bảng và đối tượng liên quan sẽ bị đưa vào thùng rác và không thể khôi phục được

Ví dụ:

```sh
DROP TABLE customers;
```

Lệnh trên sẽ xóa bảng `customers`

```sh
DROP TABLE customers PURGE;
```

Sau khi thêm từ khóa `PURGE`, bảng `customers` sẽ bị đưa thẳng vào thùng rác và không thể khôi phục được