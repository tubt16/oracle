# Các lệnh truy vấn trong Oracle

# I. Lệnh SELECT trong Oracle

SELECT là câu lệnh cơ bản nhất và được sử dụng rất nhiều trong Oracle, được dùng để lấy dữ liệu từ một bảng hoặc nhiều bảng

### Cú pháp SELECT trong Oracle

Ta có thể sử dụng SELECT để lấy dữ liệu từ table và view, cú pháp như sau:

```sh
SELECT expressions  
FROM tables  
WHERE conditions;
```

Trong đó: 

- `expression`: Là danh sách các column mà bạn muốn lấy, được cách nhau bởi dấu phẩy, nếu muốn lấy tất cả các field thì sử dụng dấu `*`

- `table`: Tên danh sách các bảng mà lệnh SELECT sẽ truy vấn đến và lấy dữ liệu

- `conditions`: Điều kiện để lọc dữ liệu

Ví dụ:

Lấy tất cả dữ liệu từ bảng `cus`

```sh
SELECT * FROM customers;
```

![](/images/select.png)

### Thêm điều kiện lọc dữ liệu với mệnh đề WHERE

Lệnh WHERE dùng để lọc kết quả trả về. Ví dụ muốn lấy thông tin khách hàng có tên là `Tubt5` thì sẽ viết như sau

```sh
SELECT * FROM customers
WHERE name = 'Tubt5';
```

![](/images/select1.png)

Ngoài so sánh bằng `=` thì ta có thể sử dụng các dấu: `>`, `<`, `>=` `<=` ...

![](/images/select2.png)

### Giới hạn kết quả với ROWNUM

Trong Oracle, lệnh select sẽ lấy tất cả record, nhưng đôi khi ta chỉ muốn lấy một số lượng dòng xác định thì ta sử dụng lệnh `ROWNUM`

Cú pháp:

```sh
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;
```

Ví dụ:

```sh
SELECT * FROM customers
WHERE ROWNUM <=2;
```

![](/images/select3.png)

# II. Comparision Operators trong Oracle

### Comparistion Operators là gì?

Trong Oracle Comparision Operators là các toán tử so sánh, nó dùng để so sánh mối liên quan giữa hai vế dữ liệu đó là vế trái và vế phải. Kết quả sẽ trả về TRUE hoặc FALSE tùy vào toán tử

Dưới đây là danh sách các toán tử:

|Comparison Operator|Description|
|---|---|
|=|So sánh bằng|
|<>|So sánh không bằng|
|!=|So sánh không bằng|
|>|	So sánh lớn hơn|
|>=|So sánh lớn hơn hoặc bằng|
|<|So sánh nhỏ hơn|
|<=|So sánh nhỏ hơn hoặc bằng|
|IN ()|Kiểm tra có nằm trong danh sách|
|NOT|Kiểm tra phủ định|
|BETWEEN|Kiểm tra trong khoảng|
|IS NULL|Kiểm tra là giá trị NULL|
|IS NOT NULL|Kiểm tra không phải là giá trị NULL|
|LIKE|So sánh gần giống, sử dụng % and _|
|REGEXP_LIKE|So sánh sử dụng Regular Expression|
|EXISTS|Kiểm tra sub query có trả về dữ liệu không, tối thiểu là 1 row|

### Một vài ví dụ sử dụng Comparistion Operators

Lấy thông tin khách hàng có tên là "Anderson"

```sh
SELECT * FROM customers
WHERE last_name = 'Anderson';
```

Lấy thông tin khách hàng ngoại trừ khách hàng có tên là "Anderson"

```sh
SELECT * FROM customers
WHERE last_name <> 'Anderson';
```

hoặc

```sh
SELECT * FROM customers
WHERE last_name != 'Anderson';
```

Lấy danh sách nhà cung cấp có ID lớn hơn 1000

```sh
SELECT * FROM suppliers
WHERE supplier_id > 1000;
```

Lấy danh sách khách hàng có ID lớn hơn hoặc bằng 1000

```sh
SELECT * FROM suppliers
WHERE supplier_id >= 1000;
```

Lấy danh sách nhà cung cấp có ID là 10, 20 hoặc 30

```sh
SELECT * FROM suppliers
WHERE supplier_id IN (10, 20, 30);
```

Lấy danh sách nhà cung cấp có ID trong khoảng 100 đến 2000

```sh
SELECT * FROM suppliers
WHERE supplier_id BETWEEN 100 AND 200
```

# III. Lệnh Case trong Oracle

CASE sẽ đi với THEN để rẽ nhánh chương trình, cú pháp như sau:

```sh
CASE [ expression ]
 
   WHEN condition_1 THEN result_1
   WHEN condition_2 THEN result_2
   ...
   WHEN condition_n THEN result_n
 
   ELSE result
 
END
```

Các tham số của nó được giải thích như sau:

- `expression`: Là giá trị dùng để so sánh với các condition (1...n) ở phía dưới

- `condition_1`, `condition_2`,... `condition_n` là các giá trị dùng để so sánh với `expression`, nếu cặp nào bằng nhau thì giá trị đằng sau WHEN sẽ được lấy

- `result_1`, `result_2`,... `result_n` là các giá trị sẽ được lấy nếu condition ở đằng trước nó bằng với `expression` 

Nếu các cặp `expression` và `condition` không có cặp nào bằng nhau thì giá trị tại ELSE sẽ được lấy

Ví dụ: Trong bảng sinh viên trường GENDER sẽ lưu trữ thông tin giới tính của sinh viên, nếu gender = 1 thì là Nam, gender = 2 thì là Nữ. Hãy viết câu truy vấn trả về Nam hoặc Nữ thay vì 1 hoặc 2

```sh
SELECT ID, NAME
CASE GENDER
	WHEN 1 THEN 'NAM'
	WHEN 2 THEN 'NU'
	ELSE 'UNKNOW'
END
FROM STUDENTS;
```

Câu lệnh này sẽ trả về UNKNOW nếu gender khác 1 và 2

Select toàn bộ bảng

![](/images/beforeCase.png)

Select với CASE

![](/images/afterCase.png)

### Cách sử dụng khác của CASE

Đôi khi không cần phải truyền vào expression mà sẽ so sánh trực tiếp lại tại condition

```sh
SELECT
CASE
  WHEN a < b THEN 'hello'
  WHEN d < e THEN 'goodbye'
END
FROM suppliers;
```

Cách viết này sẽ trực quan hơn, ta sẽ có thể sử dụng phép so sánh ở condition

### Sử dụng toán tử trong WHEN 

Để sử dụng toán tử trong WHEN, rất đơn giản ra chỉ cần sử dụng các toán tử AND, OR... Ví dụ dưới đấy thể hiện cho cách làm này

```sh
SELECT supplier_id,
CASE
  WHEN supplier_name = 'IBM' and supplier_type = 'Hardware' THEN 'North office'
  WHEN supplier_name = 'IBM' and supplier_type = 'Software' THEN 'South office'
END
FROM suppliers;
```

# IV. Lệnh INSERT trong Oracle

Lệnh INSERT dùng để thêm dữ liệu vào bảng, lệnh này được sử dụng nhiều nhất khi thao tác với cơ sở dữ liệu Oracle

Chúng ta có 2 cách thêm recored là thêm một record bằng lệnh INSERT và nhiều record bằng lệnh INSERT ALL

### Lệnh INSERT trong Oracle

Để thêm mới một record vào bảng ta sử dụng lệnh `INSERT` với cú pháp như sau:

```sh
INSERT INTO table (column1, column2, ... column_n )  
VALUES (expression_1, expression_2, ... expression_n );
```

Trong đó mỗi column sẽ tương ứng với giá trị mà bạn truyền vào ở VALUES, tức thứ tự của nó sẽ tương ứng với nhau

```sh
INSERT INTO (supplier_id, supplier_name)
VALUES (50, 'tubt');
```

### INSERT kết hợp với lệnh SELECT

Giả sử ta muốn thêm dữ liệu vào bảng dữ liệu có sẵn ở một bảng nào đó trong database thì hãy kết hợp với lệnh SELECT với cú pháp như sau:

Cú pháp:

```sh
INSERT INTO table (column_1, column_2,... column_n)
SELECT expression_1, expression_2,... expression_n FROM source_table
WHERE conditions;
```

Trong đó giá trị của các column chính là giá trị của lệnh select trả về

Ví dụ:

```sh
INSERT INTO suppliers (supplier_id, supplier_name)  
SELECT age, address FROM customers  
WHERE age > 20;
```

Câu lệnh trên sẽ lấy dữ liệu của 2 cột `age` và `address` của bảng `customers` chèn vào 2 cột `supplier_id` và `supplier_name` của bảng `suppliers`

Ta có 2 bảng sau:

Bảng `suppliers` trước khi INSERT

![](/images/beforeInsert.png)

Và Bảng `customers`

![](/images/beforeInsert1.png)

Ta sẽ thực thi câu lệnh trên và xem kết quả của bảng `suppliers`

![](/images/afterInsert.png)

### INSERT nhiều dòng cùng một lúc

Nếu muốn thêm nhiều dòng cùng một lúc vào bảng thì ta cần sử dụng lệnh `INSERT ALL`

Cú pháp:

```sh
INSERT ALL 
  INTO table_name (column1, column2, column_n) VALUES (expr1, expr2, expr_n)  
  INTO table_name(column1, column2, column_n) VALUES (expr1, expr2, expr_n)  
  INTO table_name (column1, column2, column_n) VALUES (expr1, expr2, expr_n)  
SELECT * FROM table_name;
```

Lưu ý: Trong các câu INSERT phía trong, ta có thể insert cho nhiều bảng

Ví dụ INSERT 1 bảng:

```sh
INSERT ALL 
  INTO suppliers (supplier_id, supplier_name) VALUES (30, 'Google')  
  INTO suppliers (supplier_id, supplier_name) VALUES (31, 'Microsoft')  
  INTO suppliers (supplier_id, supplier_name) VALUES (32, 'Apple')
SELECT * FROM DUAL;
```

![](/images/insert.png)

Ví dụ INSERT nhiều bảng:

```sh
INSERT ALL 
  INTO suppliers (supplier_id, supplier_name) VALUES (40, 'AWS')  
  INTO suppliers (supplier_id, supplier_name) VALUES (41, 'GCP')  
  INTO customers (age, name, address) VALUES (29, 'tubt', 'VN')  
SELECT * FROM dual;
```

![](/images/insert1.png)

![](/images/insert2.png)

# V. Lệnh UPDATE trong Oracle

Trong Oracle lệnh UPDATE dùng để cập nhật dữ liệu trong một bảng theo một điều kiện nào đó. Ví dụ trong bảng sinh viên sẽ có danh sách sinh viên, nếu muốn cập nhật năm sinh của sinh viên, ta cần dựa vào tên của sinh viên và ta có thể sử dụng lệnh UPDATE kết hợp với mệnh đề WHERE để làm được việc này

## Cú pháp lệnh UPDATE trong Oracle

Dưới đây là cú pháp của lệnh UPDATE 

```sh
UPDATE table_name  
SET column1 = expression1,  
    column2 = expression2,  
    ...  
    column_n = expression_n  
WHERE conditions;
```

Trong đó:

- `table_name`: Là tên của bảng cần UPDATE

- `column = value`: Chính là các cặp dữ liệu cần update tương ứng

- `conditions`: Là điều kiện cần UPDATE, chỉ có record nào thỏa mãn điều kiện này thì câu lệnh update mới có tác dụng

Ngoài ra ta cũng có thể sử dụng kết hợp lệnh Select để lấy giá trị gán vào column

Cú pháp như sau:

```sh
UPDATE table1  
SET column1 = (SELECT expression1  
               FROM table2  
               WHERE conditions)  
WHERE conditions;
```

## Ví dụ lệnh UPDATE trong Oracle 

Sau đây là 2 ví dụ của cả hai cách dùng trên

- UPDATE một column

```sh
UPDATE suppliers  
SET supplier_name = 'tubt2' 
WHERE supplier_id = 2;
```

![](/images/update.png)

- Update nhiều column 

```sh
UPDATE suppliers  
SET supplier_address = 'HN',  
    supplier_name = 'tubt1' 
WHERE supplier_id = 1;
```

![](/images/update1.png)

# VI. Lệnh Delete trong Oracle

Lệnh DELETE sẽ xóa dữ liệu theo một điều kiện nhất định

## Cú pháp lệnh DELETE trong Oracle

Cú pháp của lệnh Delete như sau:

```sh
DELETE FROM table_name  
WHERE conditions;
```

Trong đó:

- `table_name`: Là tên bảng muốn xóa dữ liệu

- `conditions`: Là điều kiện để xóa

Lưu ý, nếu k thiết lập conditions thì nó sẽ xóa hết dữ liệu của bảng `table_name`

## Ví dụ lệnh Delete trong Oracle

Ví dụ 1: Xóa khách hàng có tên `tubt1`

```sh
DELETE FROM customers
WHERE name = 'tubt1';
```

Trước khi DELETE

![](/images/beforeDelete.png)

Sau khi DELETE

![](/images/afterDelete.png)

**VII. Lệnh Truncate trong Oracle**

Lệnh Truncate trong Oracle sẽ xóa toàn bộ các records có trong bảng, điều này đồng nghĩa với việc sử dụng lệnh Delete mà không có nhập điều kiện xóa (conditions)

## Truncate trong Oracle

Khác với delete, khi sử dụng lệnh truncate thì ta không thể rollback, nghĩa là đã xóa là ra đi không có đường trở lại

Lệnh truncate không ảnh hưởng tới các thiết lập khác của bảng như khóa chính, trigger

Cú pháp như sau:

```sh
TRUNCATE TABLE table_name
```

Trong đó `table_name` là tên bảng muốn xóa

Ví dụ: Xóa tất cả dữ liệu của bảng `customers`

```sh
TRUNCATE TABLE customers;
```

Sau khi chạy lệnh này dữ liệu của Table sẽ bị mất sạch, không còn một record nào cả

Trước khi Truncate table

![](/images/beforeTruncate.png)

Sau khi Truncate

![](/images/afterTruncate.png)

# VIII. Subquery trong Oracle

Subquery hay còn gọi là truy vấn con, nghĩa là ta sẽ tạo ra những truy vấn nhỏ để đưa nó vào truy vấn cha

## Subquery trong Oracle

Như chúng ta biết lệnh SELECT sẽ trả về một bảng dữ liệu mới, bảng này sẽ không lưu vào hệ thống mà là một bảng tạm (local temporary) và sẽ tự giải phóng khi câu lệnh kết thúc. Như vậy ta hoàn toàn có thể thực hiện một truy vấn trên bảng đó, và ta chỉ thực hiện được truy vấn tìm kiếm trên đó thôi

Ví dụ: Tìm nhân viên có mức lương cao nhất

```sh
SELECT * FROM employees
WHERE salary = 
(
	SELECT max(salary)
	FROM employees
);
```

Ở lệnh truy vấn con bên trong sẽ trả về row có mức lưowng cao nhất, sau đó sẽ so sánh ở truy vấn cha và đưa ra kết quả

## Một vài ví dụ

- Sử dụng ở FROM

```sh
SELECT count(*) 
FROM
(
    SELECT *
    FROM employees
    WHERE last_name LIKE 'A%'
);
```

Câu truy vấn trên sẽ thực hiện đếm số lượng bản ghi từ một truy vấn con, trong đó lệnh truy vấn con sẽ lấy tất cả các dòng từ bảng `employees` mà có `last_name` bắt đầu bằng chữ `A`

- Sử dụng ở WHERE

```sh
SELECT * FROM employees e1 
WHERE e1.salary = (
    SELECT max(salary) 
    FROM employees e2 
    WHERE e1.department_id = e2.department_id
);
```

Câu truy vấn trên sẽ thực hiện so sánh mức lương của mỗi nhân viên trong bảng `e1.salary` (với e1 là bí danh của bảng `employees`) với mức lương cao nhất `max(salary)` trong cùng phòng ban (`e1.department_id` = `e2.department_id`). Câu truy vấn `SELECT max(salary) FROM employees e2` được sử dụng để lấy mức lương cao nhất trong từng phòng ban. Kết quả của câu truy vấn trên sẽ trả về tất cả các dòng từ bảng `employees` mà mức lương của nhân viên đó bằng với mức lương cao nhất trong cùng phòng ban

- Sử dụng ở SELECT

```sh
SELECT (
    SELECT avg(salary) 
    FROM employees) AS avg_sal, 
    salary 
FROM employees;
```

Câu truy vấn trên sẽ trả về kết quả gồm 2 cột.

Cột đầu tiên được đặt tên là `avg_sal` chứa giá trị trung bình của cột `salary` trong bảng `employees`

Cột thứ hai được đặt tên là `salary` và chứa giá trị của cột `salary` cho mỗi nhân viên trong bảng `employees`