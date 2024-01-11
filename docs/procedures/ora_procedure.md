# Procedure trong Oracle

- Procedure (thủ tục) là một mã SQL được chuẩn bị sẵn mà bạn có thể lưu lại, do đó mã này có thể được sử dụng lại nhiều lần

- Vì vậy, nếu có ta một truy vấn SQL mà phải viết đi viết lại nhiều lần, hãy lưu nó dưới dạng một Procedure, sau đó chỉ cần gọi nó để thực thi nó 

## Cú pháp Procedure trong Oracle

Cú pháp:

```sh
CREATE [OR REPLACE] PROCEDURE procedure_name 
[(parameter_name [IN | OUT | IN OUT] type [, ...])] 
{IS | AS} 
BEGIN 
  < procedure_body > 
END procedure_name; 
```

Mỗi Procedure bắt đầu bằng một tiêu đề ghi rõ tên của nó và một list tham số tùy chọn (parameter_list)

Mỗi tham số có thể ở chế độ IN, OUT hoặc INOUT.

Trong đó

- `procedure_name`: Là tên của procedure

- `[OR REPLACE]`: Khai báo tham số này thì nó sẽ xóa procedure có tên trùng với của procedure đang chạy

- `(parameter_name) `: Là các tham số truyền vào procedure

Mỗi tham số truyền vào được xác định bởi ba loại như sau:

- IN: Đây là kiểu mặc định, tham số này là chỉ đọc. Ta có thể tham chiếu tham số IN bên trong một Procedure, nhưng ta không thể thay đổi được giá trị của nó

- OUT: Tham số này có thể ghi được giá trị. Thông thường, ta sẽ đặt giá trị trả về cho tham số OUT và trả về nó khi gọi Procedure

- IN OUT: Là tham số đặc biệt, vừa là đầu vào vừa là đầu ra, và thường thì giá trị đầu ra sẽ bị thay đổi 

**Nội dung của một Procedure**

Nội dung của Procedure gồm 3 thành phần. Phần thực thi là bắt buộc,phần khai báo và phần xử lý là tùy chọn. Phần thực thi phải chứa ít nhất một câu lệnh thực thi

- Declarative part (Phần khai báo): Trong phần này, ta có thể khai báo các biến, hằng ...

- Executable part (Phần thực thi): Phần này chứa một hoặc nhiều câu lệnh thực thi

- Exception-handling part (Phần xử lý): Là phần tùy chọn, xử lý các lỗi run-time

## Ví dụ tạo Procedure trong Oracle

Tạo một table `people` gồm ID và NAME như sau:

```sh
create table people
(
id number(10) primary key,
name varchar2(100)
);
```

Bây giờ ta sẽ viết một procedure (thủ tục) có nhiệm vụ là thêm mới một record vào bảng `PEOPLE`

Vì bảng `PEOPLE` có hai column đó là `ID` và `NAME`, vì vậy thủ tục này sẽ có hai tham số 

```sh
CREATE or REPLACE procedure "INSERTPEOPLE"
(
ID IN NUMBER,
NAME IN VARCHAR2
)
IS
BEGIN
	insert into people values (ID, NAME);
END;
```

Khi chạy lệnh này chương trình báo `Procedure created` thì tức là bạn đã tạo thành công

![](/images/procedure.png)

## Gọi chương trình Procedure trong Oracle

Tạo xong Procedure rồi, muốn gọi nó ta có 2 cách sau:

**Gọi Procedure từ PL/SQL block**

```sh
BEGIN
	insertpeople(1, 'TuBT');
END;
```

**Sử dụng từ khóa `EXECUTE`**

```sh
EXECUTE insertpeople(2, 'TuBT2');
```

Chạy 2 lệnh trên là ta đã gọi đến Procedure `INSERTPEOPLE` để thêm mới 2 record vào table `PEOPLE`

![](/images/procedure1.png)

## Xóa Procedure trong Oracle

Nếu muốn xóa một procedure nào đó thì ta có thể sử dụng lệnh `DROP PROCEDURE`, nó sẽ giúp giải phóng bộ nhớ cho database, giúp tiết kiệm tài nguyên

Cú pháp:

```sh
DROP PROCEDURE procedure_name;
```

Ví dụ:

```sh
DROP PROCEDURE INSERTPEOPLE;
```

