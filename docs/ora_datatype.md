# Data type trong Oracle

Để tối ưu trong việc lưu trữ thì Oracle phân chia dữ liệu thành nhiều kiểu khác nhau, mỗi loại sẽ có độ dài nhất định và phù hợp với từng trường hợp. Chúng ta có các nhóm chính như `character`, `numberic`, `date/time` và `LOB`

Chúng ta sẽ nói về 4 kiểu dữ liệu chính sau

1. Character Datatypes

2. Number Datatypes

3. Data/Time Datatypes

4. Large Object (LOB) Datatypes

# Character Datatypes

Character Datatypes hay còn gọi là kiểu dữ liệu kí tự, nó là kiểu chuỗi và mỗi kiểu sẽ có độ dài khác nhau

|Data Type Syntax|Oracle 9i|Oracle 10g|Oracle 11g|
|---|---|---|---|
|char(size)|2000 bytes|2000 bytes|2000 bytes|
|nchar(size)|2000 bytes|2000 bytes|2000 bytes|
|nvarchar2(size)|4000 bytes|4000 bytes|4000 bytes|
|varchar2(size)|4000 bytes|4000 bytes|4000 bytes|
|long|2GB|2GB|2GB|
|raw|2000 bytes|2000 bytes|2000 bytes|
|long raw|2GB|2GB|2GB|

Trong đó:

- Kiểu dữ liệu CHAR chỉ định chuỗi ký tự có độ dài **cố định** trong bộ ký tự cơ sở dữ liệu. Khi bạn tạo một Table có cột CHAR, bạn sẽ cần phải chỉ định đỗ dài của cột theo kích thước tùy ý

- Kiểu dữ liệu NCHAR chỉ định chuỗi ký tự có độ dài **cố định** trong bộ ký tự cở sở dữ liệu theo quốc gia (Ví dụ UTF8 để có thể xuất tiếng việt) 

- Kiểu dữ liệu VARCHAR2 chỉ định chuỗi ký tự có độ dài **thay đổi** trong bộ ký tự cơ sở dữ liệu 

- Kiểu dữ liệu NVARCHAR2 chỉ định chuỗi kỹ tự có độ dài **thay đổi** trong bộ ký tự cơ sở dữ liệu theo quốc gia (Ví dụ UTF8)

# Numberic Datatypes

Kiểu dữ liệu Numberic lưu trữ các số cố định, số thập phân, số âm, dương, số 0, vô cùng, NAN (Not a number) ...

|Data Type Syntax|Oracle 9i|Oracle 10g|Oracle 11g|
|---|---|---|---|
|number(p,s)|range from 1 to 38, Scale can range from -84 to 127|range from 1 to 38, Scale can range from -84 to 127|range from 1 to 38, Scale can range from -84 to 127|
|numeric(p,s)|range from 1 to 38|range from 1 to 38|range from 1 to 38|
|float||||
|dec(p,s)|range from 1 to 38|range from 1 to 38|range from 1 to 38|
|decimal(p,s)|range from 1 to 38|range from 1 to 38|range from 1 to 38|
|integer||||
|int||||
|smallint||||
|real||||
|double precision||||

# Date/Time Datatypes

Các kiểu dữ liệu ngày giờ 

|Data Type Syntax|Oracle 9i|Oracle 10g|Oracle 11g|
|---|---|---|---|
|date|between Jan 1, 4712 BC and Dec 31, 9999 AD|between Jan 1, 4712 BC and Dec 31, 9999 AD|between Jan 1, 4712 BC and Dec 31, 9999 AD|
|timestamp (fractional seconds precision)|number between 0 and 9. (default is 6)|number between 0 and 9. (default is 6)|number between 0 and 9. (default is 6)|
|timestamp (fractional seconds precision) with time zone|number between 0 and 9. (default is 6)|number between 0 and 9. (default is 6)|number between 0 and 9. (default is 6)|
|timestamp (fractional seconds precision) with local time zone|number between 0 and 9. (default is 6)|number between 0 and 9. (default is 6)|a number between 0 and 9. (default is 6)|
|interval year (year precision) to month|number of digits in the year. (default is 2)|number of digits in the year. (default is 2)|number of digits in the year. (default is 2)|
|interval day (day precision) to second (fractional seconds precision)|number between 0 and 9. (default is 2), number between 0 and 9. (default is 6)|number between 0 and 9. (default is 2),number between 0 and 9. (default is 6)|number between 0 and 9. (default is 2),number between 0 and 9. (default is 6)|

# Large Object (LOB) Datatypes

|Data Type Syntax|Oracle 9i|Oracle 10g|Oracle 11g|
|---|---|---|---|
|bfile|Maximum file size of 4GB|Maximum file size of 232-1 bytes|Maximum file size of 264-1 bytes|
|blob|Store up to 4GB of binary data|Store up to (4 gigabytes -1) * (the value of the CHUNK parameter of LOB storage)|Store up to (4 gigabytes -1) * (the value of the CHUNK parameter of LOB storage)|
|clob|Store up to 4GB of character data|Store up to (4 gigabytes -1) * (the value of the CHUNK parameter of LOB storage) of character data|Store up to (4 gigabytes -1) * (the value of the CHUNK parameter of LOB storage) of character data|
|nclob|Store up to 4GB of character text data|Store up to (4 gigabytes -1) * (the value of the CHUNK parameter of LOB storage) of character text data|Store up to (4 gigabytes -1) * (the value of the CHUNK parameter of LOB storage) of character text data|
