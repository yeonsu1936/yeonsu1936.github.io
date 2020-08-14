---
layout: post
title: webhacking.kr - 6번 문제
category: CTF 문제풀이
tags: [CTF, webhacking, webhacking.kr]
comments: true
---
### webhacking.kr - 6번 문제
## Write Up
- <http://webhacking.kr>의 6번 문제를 풀어보자.
![webhackingkr_01](https://user-images.githubusercontent.com/41509536/90215870-ff89a600-de37-11ea-9249-9b37cf456fc6.jpg)

- index.phps를 클릭한다.
```php
<?php
if(!$_COOKIE[user]) // user라는 cookie값이 존재하지 않는다면 if문 수행
{
    $val_id="guest";  // $val_id 변수에 "guest" 저장
    $val_pw="123qwe";  // $val_pw 변수에 "123qwe" 저장

    for($i=0;$i<20;$i++) // for문으로 20번 반복
    {
        $val_id=base64_encode($val_id); // $val_id에 저장되어 있는 "guest"라는 문자열을 20번 base64 인코딩
        $val_pw=base64_encode($val_pw); // $val_pw에 저장되어 있는 "123qwe"라는 문자열을 20번 base64 인코딩

    }

    $val_id=str_replace("1","!",$val_id); // str_replace로 인해 "1" 이라는 값이 $val_id에 저장되어있으면 "!"로 치환해서 $val_id에 저장한다.
    $val_id=str_replace("2","@",$val_id); // 이하 동일
    $val_id=str_replace("3","$",$val_id);
    $val_id=str_replace("4","^",$val_id);
    $val_id=str_replace("5","&",$val_id);
    $val_id=str_replace("6","*",$val_id);
    $val_id=str_replace("7","(",$val_id);
    $val_id=str_replace("8",")",$val_id);

    $val_pw=str_replace("1","!",$val_pw); // str_replace로 인해 "1" 이라는 값이 $val_pw에 저장되어있으면 "!"로 치환해서 $val_pw에 저장한다.
    $val_pw=str_replace("2","@",$val_pw); // 이하 동일
    $val_pw=str_replace("3","$",$val_pw);
    $val_pw=str_replace("4","^",$val_pw);
    $val_pw=str_replace("5","&",$val_pw);
    $val_pw=str_replace("6","*",$val_pw);
    $val_pw=str_replace("7","(",$val_pw);
    $val_pw=str_replace("8",")",$val_pw);

    Setcookie("user",$val_id); // base64로 20번 인코딩되고 str_replace로 치환된 후의 $val_id 변수에 저장된 값을 user 쿠키에 저장
    Setcookie("password",$val_pw); // base64로 20번 인코딩되고 str_replace로 치환된 후의 $val_pw 변수에 저장된 값을 user 쿠키에 저장

    echo("<meta http-equiv=refresh content=0>");
}
?>

<html>
<head>
<title>Challenge 6</title>
<style type="text/css">
body { background:black; color:white; font-size:10pt; }
</style>
</head>
<body>

<?

$decode_id=$_COOKIE[user]; // $decode_id 변수에 user 쿠키에 저장된 값을 저장
$decode_pw=$_COOKIE[password]; //  $decode_pw 변수에 password 쿠키에 저장된 값을 저장

$decode_id=str_replace("!","1",$decode_id); // $decode_id 변수에 저장된 값 str_replace 치환
$decode_id=str_replace("@","2",$decode_id);
$decode_id=str_replace("$","3",$decode_id);
$decode_id=str_replace("^","4",$decode_id);
$decode_id=str_replace("&","5",$decode_id);
$decode_id=str_replace("*","6",$decode_id);
$decode_id=str_replace("(","7",$decode_id);
$decode_id=str_replace(")","8",$decode_id);

$decode_pw=str_replace("!","1",$decode_pw); // $decode_pw 변수에 저장된 값 str_replace 치환
$decode_pw=str_replace("@","2",$decode_pw);
$decode_pw=str_replace("$","3",$decode_pw);
$decode_pw=str_replace("^","4",$decode_pw);
$decode_pw=str_replace("&","5",$decode_pw);
$decode_pw=str_replace("*","6",$decode_pw);
$decode_pw=str_replace("(","7",$decode_pw);
$decode_pw=str_replace(")","8",$decode_pw);


for($i=0;$i<20;$i++) // for문으로 20번 반복
{
    $decode_id=base64_decode($decode_id); // $decode_id의 값을 반복적으로 base64 decode하여 $decode_id변수에 저장
    $decode_pw=base64_decode($decode_pw); // $decode_pw의 값을 반복적으로 base64 decode하여 $decode_pw변수에 저장
}

echo("<font style=background:silver;color:black>&nbsp;&nbsp;HINT : base64&nbsp;&nbsp;</font><hr><a href=index.phps style=color:yellow;>index.phps</a><br><br>");
echo("ID : $decode_id<br>PW : $decode_pw<hr>"); // $decode_id와 $decode_pw 값 출력(메인화면에 guest와 123qwe가 출력되는 부분)

if($decode_id=="admin" && $decode_pw=="admin")  // $decode_id에 저장된 값과 $decode_pw에 저장된 값이 admin이면 문제가 해결
{
    @solve(6,100);
}


?>

</body>
</html>
```
- admin을 20번 인코딩하고 str_replace 치환한 값을 user쿠기와 password쿠키에 넣어 변조시키면 decode 코드로 인해 디코드되어서 문제가 해결될 것이다.
![webhackingkr_02](https://user-images.githubusercontent.com/41509536/90215874-00bad300-de38-11ea-9036-9c76b63a6605.png)

- 인코딩 한 후 password와 user에 인코딩한 값을 넣고 새로고침한다.
![webhackingkr_03](https://user-images.githubusercontent.com/41509536/90215877-031d2d00-de38-11ea-8a12-9f6d7c815fa3.png)
