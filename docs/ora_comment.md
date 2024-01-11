# Comment trong Oracle 

Bất kỳ ngôn ngữ lập trình nào cũng cho phép lập trình viên có thể viết comment, nó giúp lập trình viên ghi lại những lưu ý để sau này dễ chỉnh sửa, hoặc giúp người khác hiểu được ý nghĩa của đoạn code đó

Comment là những đoạn nhận xét mà trình biên dịch sẽ bỏ qua , nó sẽ tự hiểu đoạn code đó là comment và không cần phải biên dịch cũng không báo lỗi

### Comment Single Line

Có hai cách, thứ nhất là sử dụng dấu `--` và thư hai là sử dụng `/* */`

```sh
-- comment goes here
```

```sh
/* comment goes here */
```

Ví dụ 1:

```sh
SELECT suppliers.supplier_id
/* Author: freetuts.net */
FROM suppliers;
```

Ví dụ 2:

```sh
SELECT  /* Author: freetuts.net */  suppliers.supplier_id
FROM suppliers;
```

Ví dụ 3:

```sh
SELECT suppliers.supplier_id  -- Author: freetuts.net
FROM suppliers;
```

### Comment Multiple lines

Ta sử dụng `/* */` để viết comment đa dòng

Ví dụ 1:

```sh
SELECT suppliers.supplier_id
/*
 * Author: freetuts.net
 * Purpose: To show a comment that spans multiple lines in your SQL statement.
 */
FROM suppliers;
```

Ví dụ 2:

```sh
SELECT suppliers.supplier_id /* Author: freetuts.net
Purpose: To show a comment that spans multiple lines in your SQL statement. */
FROM suppliers;
```

Trên là hai cách comment ta hay sử dụng khi viết truy vấn CSDL với Oracle, với chức năng này sẽ giúp ta giải thích những đoạn code phức tạp, giúp chương trình trở nên dễ hiểu và dễ bảo trì hơn