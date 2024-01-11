# More example

## Ví dụ 1: Tạo một Procedure đơn giản hiển thị chuỗi `Bui Thanh Tu` lên màn hình khi thực hiện
```sh
CREATE OR REPLACE PROCEDURE info
AS
BEGIN
	dbms_output.put_line('Bui Thanh Tu');
END;
/
```

-> Thực thi Procedure

```sh
EXECUTE info;
```

hoặc

```sh
BEGIN 
   info; 
END; 
/
```

Output

```sh
PL/SQL procedure successfully completed.
Bui Thanh Tu
```

![](/images/procedure2.png)


## Ví dụ 2: IN & OUT (Sử dụng IN, OUT)

Ví dụ này sẽ viết một Procedure tìm giá trị nhỏ nhất trong 2 số cho trước. Procedure này sẽ lấy hai số bằng mode IN và trả về giá trị nhỏ nhất trong 2 số đó bằng tham số OUT

```sh
DECLARE 
   a number; 
   b number; 
   c number;
PROCEDURE findMin(x IN number, y IN number, z OUT number) IS 
BEGIN 
   IF x < y THEN 
      z:= x; 
   ELSE 
      z:= y; 
   END IF; 
END;

BEGIN 
   a:= 11; 
   b:= 123; 
   findMin(a, b, c); 
   dbms_output.put_line(' Minimum of (11, 123) : ' || c); 
END; 
/
```

Output:

```sh
PL/SQL procedure successfully completed
Minimum of (11, 123) : 11
```

![](/images/procedure3.png)

## Ví dụ 3: IN & OUT Part 2 (Sử dụng IN OUT)

Procedure này tính bình phương giá trị của một giá trị được truyền. Ví dụ này sẽ cho chúng ta thấy một tham số có thể nhận giá trị sau đó trả về một kết quả khác

```sh
DECLARE 
   a number; 
PROCEDURE squareNum(x IN OUT number) IS 
BEGIN 
  x := x * x; 
END;  
BEGIN 
   a:= 12; 
   squareNum(a); 
   dbms_output.put_line(' Square of (12): ' || a); 
END; 
/
```

Output

```sh
PL/SQL procedure successfully completed.

Square of (12): 144
```

![](/images/procedure4.png)

