# SQL Primary Key

Trong Oracle, Primary Key là một ràng buộc (constraint) được sử dụng để định nghĩa một trường hoặc một tập hợp các trường trong một bảng làm khóa chính. Một Primary Key đảm bảo tính duy nhất và không trùng lặp của các giá trị trong Khóa chính

Khi một Primary Key được định nghĩa cho một Table, Oracle sẽ tự động tạo một chỉ mục (index) để tăng tốc độ truy xuất dữ liệu dựa trên khóa chính (Primary Key). Chỉ mục này giúp Oracle tìm kiếm và xác định dòng dữ liệu một cách nhanh chóng khi sử dụng câu lệnh SELECT hoặc các thao tác tham chiếu đén khóa chính

Để tạo Primary key trong Oracle, ta có thể sử dụng cấu lệnh `CREATE TABLE` để tạo bảng và định nghĩa Primary Key cùng lúc, hoặc sử dụng câu lệnh `ALTER TABLE` để thêm Primary Key vào một bảng đã tồn tại trước đó

```sh
-- Tạo bảng và định nghĩa primary key cùng lúc
CREATE TABLE employees (
    employee_id NUMBER,
    first_name VARCHAR2(50),
    last_name VARCHAR2(50),
    CONSTRAINT pk_employees PRIMARY KEY (employee_id)
);

-- Thêm primary key vào bảng đã tồn tại
ALTER TABLE employees
ADD CONSTRAINT pk_employees PRIMARY KEY (employee_id);
```

Trong ví dụ trên, chúng ta tạo một bảng `employees` với các trường `employee_id`, `first_name` và `last_name`. Bằng cách sử dụng từ khóa `CONSTRAINT`, chúng ta định nghĩa primary key với tên ràng buộc `pk_employees` và áp dụng cho cột `employee_id`. Điều này đảm bảo rằng các giá trị trong trường `employee_id` là duy nhất và không bị trùng lặp

NOTE: Một bảng chỉ có 1 Primary key và Primary Key không cho phép giá trị NULL

**Kiểm tra**

Thực hiện insert vào bảng `employees` trên để kiểm tra

```sh
INSERT INTO employees (employee_id, first_name, last_name)
VALUES (1, 'Tu', 'Bui_1');

INSERT INTO employees (employee_id, first_name, last_name)
VALUES (2, 'Tu', 'Bui_2');
```

Kiểm tra thông tin bảng 

![](/images/key.png)

Thực hiện insert vào bảng `employees` dòng sau

```sh
INSERT INTO employees (employee_id, first_name, last_name)
VALUES (1, 'Tu', 'Bui_3');
```

![](/images/key1.png)

Sau khi Insert dòng trên chúng ta nhận được thông báo lỗi `ORA-00001: unique constraint (TUBT16.PK_EMPLOYEES) violated` do chúng ta đã cố gắng thêm một record vào một bảng có Unique Contraint (ràng buộc) và giá trị của cột duy nhất này đã tồn tại trong bảng (ở đây giá trị của cột `employee_id` đã bị lặp lại)

Đó chính là công dụng của Primary Key, là những field có dữ liệu không được trùng, được sử dụng để phân biệt các record trong bảng. Ví dụ như mã sinh viên, mỗi sinh viên sẽ có một mã duy nhất. Hoặc chứng minh nhân dân hay thẻ căn cước của bạn cũng là duy nhất

# SQL Foreign Key

Trong Oracle, foreign key (khóa ngoại) là một ràng buộc (constraint) được sử dụng để thiết lập quan hệ giữa 2 bảng. Nó đảm bảo tính nhất quán và liên kết giữa dữ liệu trong các cột được định nghĩa là khóa ngoại và khóa chính của bảng tương ứng

Khi một foreign key được định nghĩa cho một cột trong bảng, nó yêu cầu các giá trị trong cột đó phải tồn tại trong cột khóa chính của bảng tham chiếu. Nếu giá trị trong cột không tồn tại trong cột khóa chính, hoặc nếu giá trị trong cột khóa chính được thay đổi hoặc xóa , ràng buộc foreign key sẽ ngăn chặn hoặc thực hiện các hành động tương ứng để đảm bảo tính nhất quán dữ liệu

Để tạo foreign key trong Oracle, bạn có thể sử dụng câu lệnh `CREATE TABLE` để tạo bảng và định nghĩa foreign key cùng lúc hoặc sử dụng câu lệnh `ALTER TABLE` sau khi bảng đã được tạo

```sh
-- Tạo bảng "departments"
CREATE TABLE departments (
    department_id NUMBER PRIMARY KEY,
    department_name VARCHAR2(100)
);

-- Tạo bảng "employees" với foreign key
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50),
    last_name VARCHAR2(50),
    department_id NUMBER,
    CONSTRAINT fk_employees_dept
        FOREIGN KEY (department_id)
        REFERENCES departments (department_id)
);
```

Trong ví dụ trên chúng ta tạo 2 bảng `departments` và `employees`. Trong bảng `employees`, chúng ta có cột `department_id` được định nghĩa là một khóa ngoại bằng cách sử dụng từ khóa `CONSTRAINT` và `FOREIGN KEY`, chúng ta định nghĩa ràng buộc với tên `fk_employees_dept`. Ràng buộc này áp dụng cho cột `department_id` và tham chiếu đến cột `department_id` trong bảng `departments` 

Ràng buộc Foreign Key đảm bảo rằng các giá trị trong cột `department_id` của bảng `employees` phải tồn tại trong cột `department_id` của bảng `departments`. Nếu giá trị không tồn tại hoặc bị thay đổi/xóa trong bảng `departments`, ràng buộc Foreign Key sẽ ngăn chặn hoặc thực hiện các hành động tương ứng để đảm bảo tính nhất quán dữ liệu 

**Kiểm tra**

Thực hiện tạo 2 bảng `departments` và `employees` như trên sau đó INSERT vào 2 bảng để kiểm tra

INSERT vào bảng `departments`

```sh
INSERT INTO departments (department_id, department_name)
VALUES (1, 'tubt1');

INSERT INTO departments (department_id, department_name)
VALUES (2, 'tubt2');
```

Kiểm tra dữ liệu của bảng `departments`

![](/images/key2.png)

Thực hiện INSERT vào bảng `employees` giá trị như sau

```sh
INSERT INTO employees (employee_id, first_name, last_name, department_id)
VALUES (808220, 'Tu', 'Bui', 3);
```

Sau khi INSERT dòng trên chúng ta nhận được thông báo lỗi `ORA-02291: integrity constraint (TUBT16.FK_EMPLOYEES_DEPT) violated - parent key not found`. Lỗi trên xảy ra khi chúng ta cố gắng thêm hoặc cập nhật một dòng dữ liệu trong bảng có ràng buộc khóa ngoại, nhưng giá trị của cột khóa ngoại không được tìm thấy trong cột khóa chính của bảng tham chiếu 

Để khắc phục lỗi này ta cần đảm bảo rằng giá trị trong cột khóa ngoại có tồn tại trong cột khóa chính của bảng tham chiếu trước khi thực hiện thao tác thêm hoặc cập nhật dữ liệu

Thực hiện INSERT lại vào bảng `employees` giá trị sau (Lần này chúng ta sẽ insert giá trị hợp lệ)

```sh
INSERT INTO employees (employee_id, first_name, last_name, department_id)
VALUES (808220, 'Tu', 'Bui', 1);
```

![](/images/key3.png)

Vậy chúng ta đã thêm thành công Foreign Key