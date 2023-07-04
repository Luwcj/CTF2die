#    Local File Inclusion 

##### Statement
Get in the admin section

![](https://cdn.discordapp.com/attachments/1098605833371267172/1125682201879646299/image.png)

Như bạn để ý thì trên URL có 2 tham số để chỉ định đường dẫn ta truy cập (dấu hiệu rất rõ ràng về lỗi RFI/LFI). Dựa vào bên góc phải thấy có 2 kiểu kết nối là guest/admin. Từ đó ta có thể đoán là có 1 thư mục tên là admin. Và cũng ở đề bài là ta phải tìm password trong admin section. Như vậy mục tiêu của ta là phải truy cập được vào file admin kia. Tôi thử lần lượt "../" vào 1 trong 2 tham số cho đến khi xuất hiện file admin. Dưới đây là payload của tôi: http://challenge01.root-me.org/web-serveur/ch16/?files=../admin&f=index.php

![](https://cdn.discordapp.com/attachments/1098605833371267172/1125683571617706014/image.png)

####flag: ' OpbNJ60xYpvAQU8 '
