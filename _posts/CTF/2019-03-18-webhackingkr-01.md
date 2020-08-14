---
layout: post
title: webhacking.kr - 1번 문제
category: CTF 문제풀이
tags: [CTF, webhacking, webhacking.kr]
comments: true
---
### webhacking.kr - 1번 문제
<http://webhacking.kr>의 1번 문제를 풀어보자

## 문제 확인
- 상단의 challenges > 1번 문제를 누른다.
![webhackingkr_01](https://user-images.githubusercontent.com/41509536/90215045-76716f80-de35-11ea-9426-bbfda734829b.png)

- 다음과 같은 페이지를 확인할 수 있다.
![webhackingkr_02](https://user-images.githubusercontent.com/41509536/90215046-77a29c80-de35-11ea-9709-3fb96102d981.jpg)

## 문제 풀이
- `오른쪽 마우스 - 페이지 소스 보기`를 클릭한다.
![webhackingkr_03](https://user-images.githubusercontent.com/41509536/90215048-77a29c80-de35-11ea-8105-721a43437acd.jpg)

- 12번째 줄을 보면 `>----index.phps----<`를 클릭(onclick)하면 `index.phps` 페이지로 이동한다는 것을 알 수 있다. `index.phps`를 누른다.
![webhackingkr_04](https://user-images.githubusercontent.com/41509536/90215049-783b3300-de35-11ea-8df8-6b89e890733e.jpg)
```php
if(eregi("[^0-9,.]",$_COOKIE[user_lv])) $_COOKIE[user_lv]=1;
if($_COOKIE[user_lv]>=6) $_COOKIE[user_lv]=1; //COOKIE[user_lv]가 6과 같거나 크면 COOKIE[user_lv]값이 1이 된다.
if($_COOKIE[user_lv]>5) @solve(); //COOKIE[user_lv]가 5보다 크면 해결된다. 즉 5보다 크고 6보다 작아야한다.
```
- 위의 코드를 해석하면 `COOKIE[user_lv]`값이 5보다 크고 6보다 작아야 한다.

- 크롭 웹스토어에서 [EditThisCookie](https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg/related?hl=ko) 도구를 다운받는다.

- 상단의 쿠키 모양을 누르고 값을 5보다 크고 6보다 작은 수를 입력한다.
![webhackingkr_05](https://user-images.githubusercontent.com/41509536/90215051-796c6000-de35-11ea-9a8f-f07b0ac5ad73.jpg)
