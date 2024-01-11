# Các lệnh quản lý View trong Oracle

View là một dạng table đặc biệt, là một table ảo và không hề tồn tại trong danh sách table vật lý vì nó được tạo ra khi câu lệnh được thực hiện

Đặc điểm của View là dữ liệu của nó có thể được lấy từ nhiều bảng khác nhau, bởi vì nó được tạo ra từ câu lệnh select, mà trong lệnh select thì có lệnh JOIN giúp chúng ta lấy được dữ liệu từ nhiều bảng

## Lệnh Create VIEW

Để tạo một view ta sử dụng cú pháp sau:

```sh
CREATE VIEW view_name AS 
SELECT columns  
FROM tables  
WHERE conditions;
```

Giả sử ta có 2 bảng như sau

`Suppliers Table`

```sh
CREATE TABLE SUPPLIERS
(
SUPPLIER_ID number,
SUPPLIER_NAME varchar(50),
SUPPLIER_ADDRESS varchar(50)
);
```

![](/images/view.png)

`Olders table`

```sh
CREATE TABLE ORDERS  
(
ORDER_NO NUMBER,   
QUANTITY NUMBER,   
PRICE NUMBER  
);
```

![](/images/view1.png)

Bây giờ chúng ta sẽ tạo một view với dữ liệu là tất cả đơn hàng của nhà cung cấp có tên là `SAM SUNG`

```sh
CREATE VIEW sup_orders AS 
SELECT suppliers.supplier_id, orders.quantity, orders.price  
FROM suppliers  
INNER JOIN orders  
ON suppliers.supplier_id = supplier_id  
WHERE suppliers.supplier_name = 'SAM SUNG';
```

Sau khi chạy lệnh này ta sẽ có một VIEW gồm 3 column `suppliers_id`, `quantity` và `price` trong đó `supplier_id` được get từ `supplier_name = 'SAM SUNG'`

![](/images/view3.png)

## Xem dữ liệu và xóa VIEW

Vì View cũng là một dạng Table nên ta có thể sử dụng SELECT

```sh
SELECT * FROM sup_orders;
```

Để xóa View thì ta sử dụng lệnh DROP VIEW theo cú pháp như sau:

```sh
DROP VIEW view_name;
```

Ví dụ:

```sh
DROP VIEW sup_orders;
```

## Update View

Để cập nhật View thì ta sẽ dùng lệnh `CREATE OR REPLACE VIEW`, lệnh này sẽ tạo View mới nếu chưa tồn tại hoặc sẽ cập nhật View cũ nếu đã tồn tại

```sh
CREATE OR REPLACE VIEW view_name AS 
SELECT columns  
FROM table 
WHERE conditions;
```

Quay lại ví dụ trên, ta sẽ cập nhật lại View sẽ chứa danh sách order của nhà cung cấp tên là `Apple`

```sh
CREATE OR REPLACE VIEW sup_orders AS 
SELECT suppliers.supplier_id, orders.quantity, orders.price  
FROM suppliers  
INNER JOIN orders  
ON suppliers.supplier_id = supplier_id  
WHERE suppliers.supplier_name = 'Apple';
```

![](/images/afterUpdateView.png)

Sau khi Update giá trị của cột `SUPPLIER_ID` đã đổi thành `3` (Chính là giá trị `SUPPLIER_ID` của `Apple`)

