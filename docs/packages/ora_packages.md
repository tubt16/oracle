# Oracle Package

Package là các đối tượng schema NHÓM các object, biến và chương trình con PL/SQL một cách logic

Một Package sẽ có 2 phần bắt buộc

> Package specification (SPEC): Là phần khai báo công khai của một Package, nơi định nghĩa các đối tượng và phương thức mà package cung cấp cho các thành phần bên ngoài. Nó chứa các khai báo của các biến, hằng số, kiểu dữ liệu, các function, procedure ...

> Package body (BODY): Là phần thực hiện các thành phần cụ thể của một Package. Nó chứa các đoạn mã PL/SQL để triển khai các phương thức và xử lý logic của Package được định nghĩa trong Package SPEC. Để tạo được Package BODY trong Oracle, ta cần có một Package SPEC tương ứng đã tạo trước đó

## Table mẫu

Ta sẽ sử dụng table sau cho các ví dụ bên dưới

```sh
SQL> select * from customers;

        ID NAME                        AGE ADDRESS                       SALARY
---------- -------------------- ---------- ------------------------- ----------
         1 Ramesh                       32 Ahmedabad                       2000
         2 Khilan                       25 Delhi                           1500
         3 kaushik                      23 Kota                            2000
         4 Chaitali                     25 Mumbai                          6500
         5 Hardik                       27 Bhopal                          8500
         6 Komal                        22 MP                              4500

6 rows selected.
```

## Cú pháp tạo Package (gồm cả phần SPEC và BODY)

Cú pháp

```sh
-- Tạo Package SPEC
CREATE OR REPLACE PACKAGE my_package AS
   PROCEDURE my_procedure;
   FUNCTION my_function RETURN NUMBER;
   -- Các biến, hằng số và các thành phần khác có thể được định nghĩa ở đây
END my_package;


-- Tạo Package BODY
CREATE OR REPLACE PACKAGE BODY my_package AS
   PROCEDURE my_procedure IS
   BEGIN
      -- Thực thi logic cho thủ tục
   END my_procedure;

   FUNCTION my_function RETURN NUMBER IS
      -- Khai báo biến và logic cho hàm
   BEGIN
      -- Thực thi logic và trả về giá trị
   END my_function;

   -- Các thủ tục, hàm và biến khác có thể được định nghĩa ở đây
END my_package;
```

Trong ví dụ trên, ta đã tạo một package có tên `my_package` với một thủ tục `my_procedure` và một hàm `my_function`. Sau đó chúng ta tạo package body cho package đó để triển khai logic cụ thể cho các thủ tục và hàm. Trong phần package body, chúng ta định nghĩa các thủ tục và hàm bên trong khối BEGIN-END và triển khai logic thực thi cho chúng

## Ví dụ 1 (Tạo Package hoàn chỉnh)

Ví dụ

```sh
-- Tạo Package SPEC
CREATE PACKAGE cust_sal AS 
   PROCEDURE find_sal(c_id customers.id%type); 
END cust_sal; 
/

-- Tạo Package Body
CREATE OR REPLACE PACKAGE BODY cust_sal AS  
   
   PROCEDURE find_sal(c_id customers.id%TYPE) IS 
   c_sal customers.salary%TYPE; 
   BEGIN 
      SELECT salary INTO c_sal 
      FROM customers 
      WHERE id = c_id; 
      dbms_output.put_line('Salary: '|| c_sal); 
   END find_sal; 
END cust_sal; 
/
```

Package này có tên là `cust_sal`, trong package này định nghĩa một `PROCEDURE` có tên là `find_sal` với một tham số đầu vào `c_id` có cùng kiểu dữ liệu với cột `id` trong bảng `customers`. Ký tự `/` cuối cùng được sử dụng để kết thúc câu lệnh và tạo Packages

Câu lệnh `CREATE PACKAGE BODY` được sử dụng để tạo Package Body. Trong phần BODY của Package này, procedure `find_sal` nhận một tham số `c_id` có kiểu dữ liệu `customers.id%TYPE` (Nghĩa là `c_id` sẽ có cùng kiểu dữ liệu với cột `id` trong bảng `customers`) 

Bên trong Procedure, một biến cục bộ `c_sal` được định nghĩa với kiểu dữ liệu `customers.salary%TYPE` (Tức cùng kiểu dữ liệu với cột `salary` trong bảng `customers`)

Tiếp đến câu lệnh SELECT được sử dụng để lấy giá trị `salary` từ bảng `Customers` dựa trên `c_id` được cung cấp. Giá trị `salary` lấy được được hiển thị bằng cách sử dụng hàm `dbms_output.put_line` 

**Kiểm tra**

Sau khi tạo xong Package sẽ có phần `SPEC` và `BODY`, ta kiểm tra lại xem đã tạo thành công hay chưa

![](/images/spec.png)

![](/images/body.png)

## Gọi Package

Các phần tử trong Package (biến, procedure, function) được gọi bằng cú pháp sau:

```sh
package_name.element_name;
```

Ví dụ:

```sh
DECLARE 
   code customers.id%type := &c_id; 
BEGIN 
   cust_sal.find_sal(code); 
END; 
/
```

Đoạn mã trên sẽ gọi đến Procedure `find_sal` trong Package `cust_sal`. 

Output

![](/images/package.png)

![](/images/package1.png)

Khi ta gán biến `c_id = 5` như trong hình, kết quả đã trả về `salary = 8500` tương ứng với `id = 5` trong bảng `Customers`  

# Ví dụ 2

Chương trình sau đây sẽ viết một Package hoàn chỉnh hơn. Chúng ta sẽ sử dụng Table `CUSTOMERS` được lưu trữ trong cơ sở dữ liệu với các bản ghi sau

```sh
SQL> select * from customers;

        ID NAME                        AGE ADDRESS                       SALARY
---------- -------------------- ---------- ------------------------- ----------
         1 Ramesh                       32 Ahmedabad                       3000
         2 Khilan                       25 Delhi                           3000
         3 kaushik                      23 Kota                            3000
         4 Chaitali                     25 Mumbai                          7500
         5 Hardik                       27 Bhopal                          9500
         6 Komal                        22 MP                              5500

6 rows selected.
```

**Tạo Package SPEC**

```sh
CREATE OR REPLACE PACKAGE c_package AS 
   -- Adds a customer 
   PROCEDURE addCustomer(c_id   customers.id%type, 
   c_name customers.Name%type, 
   c_age  customers.age%type, 
   c_addr customers.address%type,  
   c_sal  customers.salary%type); 
   
   -- Removes a customer 
   PROCEDURE delCustomer(c_id  customers.id%TYPE); 
   --Lists all customers 
   PROCEDURE listCustomer; 
  
END c_package; 
/
```

Giải thích:

- Đoạn mã trên sẽ tạo mới Package có tên là `c_package` hoặc sẽ thay thế nếu Package này đã tồn tại, trong Package này định nghĩa 3 thủ tục (Procedure) là `addCustomer`, `delCustomer` và `listCustomer`

- Thủ tục `addCustomer` gồm 5 tham số đầu vào `c_id`, `c_name`, `c_age`, `c_addr` và `c_sal` có cùng kiểu dữ tương ứng với cột `id`, `name`, `age`, `address` và `salary` trong bảng `Customers`

- Thủ tục `delCustomer` có 1 tham số đầu vào `c_id`

- Thủ tục `listCustomer` không có tham số nào


**Tạo Package BODY**

```sh
CREATE OR REPLACE PACKAGE BODY c_package AS 

   -- addCustomer Procedure
   PROCEDURE addCustomer(c_id  customers.id%type, 
      c_name customers.Name%type, 
      c_age  customers.age%type, 
      c_addr  customers.address%type,  
      c_sal   customers.salary%type) 
   IS 
   BEGIN 
      INSERT INTO customers (id,name,age,address,salary) 
         VALUES(c_id, c_name, c_age, c_addr, c_sal); 
   END addCustomer; 
   
   -- delCustomer Procedure
   PROCEDURE delCustomer(c_id   customers.id%type) IS 
   BEGIN 
      DELETE FROM customers 
      WHERE id = c_id; 
   END delCustomer;  
   
   -- listCustomer Procedure
   PROCEDURE listCustomer IS 
   CURSOR c_customers is 
      SELECT  name FROM customers; 
   TYPE c_list is TABLE OF customers.Name%type; 
   name_list c_list := c_list(); 
   counter integer :=0; 
   BEGIN 
      FOR n IN c_customers LOOP 
      counter := counter +1; 
      name_list.extend; 
      name_list(counter) := n.name; 
      dbms_output.put_line('Customer(' ||counter|| ')'||name_list(counter)); 
      END LOOP; 
   END listCustomer;
   
END c_package; 
/
```

Giải thích:

- Đoạn mã trên tạo Package Body `c_package` chứa 3 thủ tục `addCustomer`, `delCustomer` và `listCustomer`

- Thủ tục `addCustomer` được sử dụng để thêm thông tin khách hàng mới vào bảng `customers`. Nó nhận các tham số `c_id`, `c_name`, `c_age`, `c_addr` và `c_sal` tương ứng với cột `id`, `name`, `age`, `address` và `salary` trong bảng `customers`. Thủ tục này thực hiện chèn một hàng mới vào bảng `customers` với các giá trị cho trước

- Thủ tục `delCustomer` được sử dụng để xóa khách hàng từ bảng `customers`, nó nhận một tham số `c_id` tương ứng với cột `id` trong bảng `customers`. Thủ tục này thực hiện việc xóa hàng từ bảng `customers` dựa trên giá trị `c_id` được cung cấp

- Thủ tục `listCustomer` được sử dụng để liệt kê tên các khách hàng trong bảng `Customers`. Nó sử dụng một con trỏ `c_customers` để truy xuất tất cả các tên khách hàng từ bảng `customers` và lưu trữ chúng trong một mảng `name_list` kiểu `c_list`. Sau đó, nó sử dụng vòng lặp FOR để duyệt qua mảng `name_list` và hiển thị tên khách hàng bằng cách sử dụng hàm `dbms_output.put_line`. Dưới đây là phân tích của procedure `listCustomer`

1. Khai báo con trỏ `c_customers` để truy xuất cột `name` từ bảng `customers`

2. Định nghĩa một kiểu tập hợp `c_list` là một Table có kiểu `customers.Name%type`

3. Khai báo biến `name.list` có kiểu `c_list` và khởi tạo nó là một tập hợp RỖNG

4. Khai báo biến đếm `counter` và đặt giá trị ban đầu cho biến bằng 0

5. Bắt đầu thủ tục

6. Sử dụng vòng lặp FOR để lặp qua từng hàng được trả về bởi con trỏ `c_customers`

7. Tăng biến đếm `counter` thêm 1 sau mỗi lần lặp

8. Mở rộng kích thước của tập hợp `name_list` để chứa tên khách hàng mới

9. Gán tên khách hàng từ lần lặp hiện tại vào chỉ số tương ứng trong tập hợp `name_list`

10. Sử dụng `dbms.output.put_line` để hiển thị tên khách hàng cùng với giá trị đếm `counter` tương ứng

11. Kết thúc vòng lặp

12. Kết thúc thủ tục

Sau khi tạo Package sẽ có phần `SPEC` và `BODY`, ta kiểm tra xem đã tạo thành công hay chưa

![](/images/package2.png)

![](/images/package3.png)

**Chạy Package**

Thực hiện chạy Package `c_package` như sau:

```sh
DECLARE 
   code customers.id%type:= 8; 
BEGIN 
   c_package.addcustomer(7, 'Rajnish', 25, 'Chennai', 3500); 
   c_package.addcustomer(8, 'Subham', 32, 'Delhi', 7500); 
   c_package.listcustomer; 
   c_package.delcustomer(code); 
   c_package.listcustomer; 
END; 
/
```

Dưới đây là Output khi chạy Package

```sh
Customer(1)Ramesh
Customer(2)Khilan
Customer(3)kaushik
Customer(4)Chaitali
Customer(5)Hardik
Customer(6)Komal
Customer(7)Rajnish
Customer(8)Subham
Customer(1)Ramesh
Customer(2)Khilan
Customer(3)kaushik
Customer(4)Chaitali
Customer(5)Hardik
Customer(6)Komal
Customer(7)Rajnish
```

![](/images/package4.png)

- Đầu tiên ta thực hiện khai báo biến `code` có kiểu dữ liệu `customers.id%type` và gán cho nó giá trị bằng 8

- Sau đó gọi thủ tục `addcustomer` của package `c_package` để thêm khách hàng với các thông tin như sau (ID: 7, Name: 'Rajnish', Age: 25, Address: 'Chennai', Salary: 3500) và (ID: 8, Name: 'Subham', Age: 32, Address: 'Delhi', Salary: 7500)

- Sau khi thêm 2 hàng vào bảng `customers`, thực hiện gọi thu tục `listcustomer` để hiển thị danh sách khách hàng

- Gọi thủ tục `delcustomer` của gói `c_package` để xóa khách hàng có id = "code" (trong trường hợp này là 8).

- Gọi thủ tục `listcustomer` của gói `c_package` để hiển thị danh sách khách hàng sau khi xóa.