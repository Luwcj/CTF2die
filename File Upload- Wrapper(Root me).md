# [Rootme] Local File Inclusion – Wrappers

Giao diện web cho phép ta tải file ảnh lên, thử tải 1 bức ảnh bất kì lên web và nhấn vào xem thì không có gì và bài này cũng không cho ta xem source ngay từ đầu.
Nên tập trung lại vào cái LFI tại vì nó có page=view?id=xxx, chèn mấy cái kiểu bình thường của FLI thì phát hiện hai cái filter. một là không cho sử dụng php hay là không cho sử dụng “../” . 

Các bạn có thể tham khảo một số cách khai thác ở đây:


 https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion

 https://www.cdxy.me/?p=752


 Cách giải quyết bài này sẽ dùng PHP FLI wrapper với “zip”

```php
 <pre>
   <?php 
   show_source('index.php');
   ?>
</pre>
```

Đầu tiên tạo 1 file php với nội dung như trên rồi tiến hành zip file lại, sau đó đổi tên file zip thành file .jpg rồi tải file lên.(lưu ý các bạn nên để tên file ngắn nhất, mình để tên file là a.php)

Khi up thành công bạn để ý trong source nó sẽ có đường dẫn tới file:

![](https://cdn.discordapp.com/attachments/1124588087931043891/1131146241108095107/image.png)

Dùng payload: ?page=zip://tmp/upload/cfWmnXjX2.jpg%23a

![](https://cdn.discordapp.com/attachments/1124588087931043891/1131147858825986129/image.png)

Vậy là đã thành công trong việc tải lên mã độc nhưng flag không nằm trong source, vậy là cần phải tìm flag đang nằm ở đâu. Hàm system(), shell_exec() bị chặn. Vậy thì còn 1 cách là dùng hàm scandir() (https://vietjack.com/php/ham_scandir_trong_php.jsp)

```php
<pre>
   <?php
    $dir='./';
    $file1 = scandir($dir);
    print_r($file1);
   ?>
</pre>
```
Tạo file a.php với nội dung như trên rồi nén thành file zip và đổi tên thành a.jpg sau đó tải lên và truy cập vào file:

![](https://cdn.discordapp.com/attachments/1124588087931043891/1131150686718021682/image.png)

Flag nằm trong file flag-mipkBswUppqwXlq9ZydO.php, ta làm tương tự như cách ta xem source của file index.php là sẽ có flag.

Mình tham khảo wu của  2 nguồn này:
- https://nhienit.wordpress.com/2020/05/02/rootme-local-file-inclusion-wrappers/

- https://thanhlocpanda.wordpress.com/2018/05/16/write-up-root-me_-local-file-inclusion-wrapper-php_loose-comparison/