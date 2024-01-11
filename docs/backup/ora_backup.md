# Backup/Restore Oracle Database using Toad

# I. Backup/Restore Table

**Bước 1: Thực hiện export Table**

Tại tab `Database` -> `Export` -> `Export Utility Wizard`

![](/images/export.png)

Tiếp tới chọn `Export tables` -> `Next`

![](/images/export1.png)

**Bước 2: Chọn Table muốn export**

Tại tab `Table to export` chọn schema, sau đó chọn Table mà bạn muốn export trên schema đó (Ở đây mình chọn Table `Orders`) sau đó chọn `Next`

![](/images/export3.png)

Giữ nguyên các giá trị mặc định như trong hình và tiếp tục chọn `Next`

![](/images/export4.png)

Đặt tên cho file backup và chọn nơi lưu trữ cho file backup đó

![](/images/export5.png)

Chọn `Execute now` sau đó chọn `Finish` để hoàn tất quá trình export Table. Tại bước này bạn có thể chọn `Compress export file (.zip)` để nén các file export thành 1 file zip

![](/images/export6.png)

Kiểm tra output khi thực hiện export

![](/images/export9.png)

Kiểm tra file đã được backup thành công chưa (C:\Users\tubt10\AppData\Roaming\Quest Software\Toad for Oracle\15.1\User Files\backup)

![](/images/export7.png)

Như vậy chúng ta đã có file backup của Table `Orders`, ta có thể xóa dữ liệu trong Table này đi và thực hiện import để Restore dữ liệu của Table này

Sử dụng lệnh TRUNCATE để xóa toàn bộ bản ghi có trong Table

```sh
TRUNCATE TABLE orders;
```

![](/images/export8.png)

Như vậy bảng `orders` đã không còn dữ liệu, tiếp đến ta thực hiện import để Restore dữ liệu đã xóa

**Bước 3: Thực hiện import Table**

Tại tab `Database` -> `Import` -> `Import Utility Wizard`

![](/images/import.png)

Tiếp đến chọn `Import tables` -> `Next`

![](/images/import1.png)

Chỉ định Schema source -> Schema destination và chọn method import Table, ở đây bảng order thuộc Schema `TUBT16` và mình muốn import tới Schema `TUBT16` vì thế Schema source và Schema Dest đều có giá trị là `TUBT16`

Method import Table mình chọn là `Manually enter table names` có nghĩa chọn table thủ công để import

![](/images/import2.png)

Tiếp đến ta cần nhập tên của Table cần import và Table mình cần import đến có tên là `orders`

![](/images/import4.png)

Giữ nguyên các giá trị mặc định và chọn `Next`

![](/images/import5.png)

Chọn file để thực hiện import (file này chính là file `orders.DMP` đã export trước đó)

![](/images/import6.png)

Chọn `Import now` sau đó chọn Finish để thực hiện import Table

![](/images/import7.png)

Kiểm tra output sau khi import 

![](/images/import8.png)

Thực hiện truy vấn SELECT đến bảng `orders` để kiểm tra dữ liệu đã được restore thành công hay chưa

```sh
select * from orders;
```

![](/images/import9.png)

Sau khi import dữ liệu của bảng `orders` đã được restore thành công, vậy chúng ta đã import Table thành công

# II. Backup/Restore Schema

Để thực hiện Backup/Restore, ta sẽ cần tạo một schema mới chưa có bất cứ dữ liệu nào và thực hiện import dữ liệu từ schema cũ qua schema mới này. Trong ví dụ này mình sẽ thực hiện backup schema `TUBT16` và sẽ restore sang một schema mới có tên `TEST`

**Bước 1: Thực hiện tạo một schema mới**

```sh
CREATE USER test IDENTIFIED BY tubt160999;

grant dba to test;
```

Sau khi tạo schema mới, ta login để kiểm tra thử

![](/images/schema.png)

Hiện tại schema `TEST` chưa có Table nào được tạo

**Bước 2: Thực hiện export schema `TUBT16`**

Tại tab `Database` -> `Export` -> `Export Utility Wizard`

![](/images/schema1.png)

Tiếp tới chọn `Export users` -> `Next`

![](/images/schema2.png)

**Bước 3: Chọn user/schema cần backup**

Ở đây ta sẽ chọn schema `TUBT16` để export sau đó chọn `Next`

![](/images/schema3.png)

Tiếp đến ta giữ nguyên các giá trị mặc định khi export và chọn `Next`

![](/images/schema4.png)

Đặt tên cho file backup và chọn nơi lưu trữ cho file backup đó

![](/images/schema5.png)

Chọn `Execute now` sau đó chọn `Finish` để hoàn tất quá trình export Table. Tại bước này bạn có thể chọn `Compress export file (.zip)` để nén các file export thành 1 file zip

![](/images/schema6.png)

Kiểm tra output khi thực hiện export

![](/images/schema7.png)

Kiểm tra các file backup trên máy tính sau khi export

![](/images/schema8.png)

Như vậy chúng ta đã có các file backup cần thiết của schema `TUBT16` để thực hiện restore

Tiếp theo chúng ta sẽ import file này đến schema `TEST` để thực hiện khôi phục dữ liệu

**Bước 4: Thực hiện import tới schema `TEST`**

Tại tab `Database` -> `Import` -> `Import Utility Wizard`

![](/images/schema9.png)

Tiếp tới chọn `Import users` -> `Next`

![](/images/schema10.png)

Chọn Schema nguồn và Schema đích, Ở trong ví dụ này Schema nguồn là `TUBT16`, Schema đích cần import tới là `TEST` sau đó nhấn `ADD` -> `Next`

![](/images/schema11.png)

Giữ nguyên các giá trị mặc định khi import user sau đó chọn `Next`

![](/images/schema12.png)

Chọn file để thực hiện import (File này chính là file `scm_tubt16.DMP` đã export ở trên)

![](/images/schema13.png)

Chọn `Import now` sau đó `Finish` để thực hiện import tới Schema `TEST`

![](/images/schema14.png)

**Bước 5: Kiểm tra**

Login Schema `TEST` để kiểm tra dữ liệu đã được restore hay chưa

![](/images/schema15.png)

![](/images/schema16.png)

Như vậy chúng ta đã thực hiện restore schema thành công

# III. Backup/Restore Procedure

**Bước 1: Backup Procedure**

Tại cửa sổ Schema Browser, chọn tab `Procedures` sau đó tìm đến procedure cần backup. Sau khi tìm được procedure cần backup, click chuột phải chọn `Save to file` sau đó chọn nơi lưu trữ file backup trên máy tính cá nhân (Ở đây mình sẽ thực hiện backup procedure `INSERTPEOPLE`)

![](/images/backup_procedure.png)

Lưu ý khi save file, ở phần `encoding` bắt buộc ta phải chọn `UTF-8` để đảm bảo code có tiếng việt có dấu không bị lỗi font khi lưu ra file backup, file backup này dùng để rollback trong trường hợp cần thiết

![](/images/backup_procedure1.png)

Sau khi save file, ta sẽ nhận được 1 file có đuôi `.prc` (Ở trong ví dụ này là `INSERTPEOPLE.prc`), `.prc` là viết tắt của procedure. Chúng ta sẽ sử dụng file này để restore Procedure

**Bước 2: Thực hiện xóa Procedure**

```sh
drop procedure insertpeople;
```

![](/images/backup_procedure2.png)

**Bước 3: Thực hiện Restore Procedure**

Mở file `INSERTPEOPLE.prc` đã backup, copy code và dán vào tab editor sau đó nhấn F5 để tạo lại `Procedure` đã xóa

![](/images/backup_procedure3.png)

**Bước 4: Kiểm tra lại Procedure**

Sau khi Restore ta cần kiểm tra lại xem Procedure đã được restore thành công chưa

![](/images/backup_procedure4.png)

Như vậy chúng ta đã thực hiện backup Procedure thành công

# IV. Backup/Restore Package

**Bước 1: Backup Package**

Tại cửa sổ Schema Browser, chọn tab `Packages` sau đó tìm đến package cần backup. Sau khi tìm được package cần backup, click chuột phải chọn `Save to file` -> `Save` sau đó chọn nơi lưu trữ file backup trên máy tính cá nhân (Ở đây mình sẽ thực hiện backup package `C_PACKAGE`)

Khi lưu file backup, ta có thể chọn `Save SPEC Only` hoặc `Save BODY Only` để thực hiện chỉ lưu phần SPEC hoặc BODY của package

![](/images/backup_package.png)

Lưu ý khi save file, ở phần `encoding` bắt buộc ta phải chọn `UTF-8` để đảm bảo code có tiếng việt có dấu không bị lỗi font khi lưu ra file backup, file backup này dùng để rollback trong trường hợp cần thiết

![](/images/backup_package1.png)

Sau khi save file, ta sẽ nhận được 2 file có đuôi `pks` và `pkb` (Ở trong ví dụ này là `C_PACKAGE.pks` và `C_PACKAGE.pkb`). `.pks` và `.pkb` là viết tắt của package SPEC và package BODY. Chúng ta sẽ sử dụng 2 file này để restore Package SPEC và Package BODY

**Bước 2: Thực hiện xóa Package**

```sh
drop package C_PACKAGE
```

![](/images/backup_package2.png)

**Bước 3: Thực hiện Restore Package**

- Thực hiện Restore phần Package SPEC

Mở file `C_PACKAGE.pks` đã backup, copy code và dán vào tab editor sau đó nhấn F5 để tạo lại Package SPEC đã xóa

![](/images/backup_package3.png)

- Thực hiện Restore phần Package BODY

Mở file `C_PACKAGE.pkb` đã backup, copy code và dán vào tab editor sau đó nhấn F5 để tạo lại Package BODY đã xóa

![](/images/backup_package4.png)

**Bước 4: Kiểm tra lại Package**

Sau khi Restore ta cần kiểm tra lại xem Package đã được restore thành công chưa

![](/images/backup_package5.png)

Như vậy chúng ta đã thực hiện Restore thành công một Package hoàn chỉnh gồm phần SPEC và BODY.