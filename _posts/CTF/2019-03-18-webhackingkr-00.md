---
layout: post
title: webhacking.kr - 회원가입하기
category: CTF 문제풀이
tags: [CTF, webhacking, webhacking.kr]
comments: true
---
### webhacking.kr - 회원가입하기
<http://webhacking.kr>의 회원가입을 해보자.

## 회원가입하기
- <http://webhacking.kr>에 들어간다.
![webhackingkr_00](https://user-images.githubusercontent.com/41509536/90214198-b97e1380-de32-11ea-807b-08c6b9f9af17.jpg)

- 회원가입을 해야하는데 로그인 버튼만 있고 회원가입 버튼이 없다.  
  `오른쪽 마우스 클릭 - 페이지 소스 보기`를 누른다.
![webhackingkr_01](https://user-images.githubusercontent.com/41509536/90214200-baaf4080-de32-11ea-8635-66bbdb9ed48f.jpg)

- 주석처리된 초록색 부분을 확인할 수 있다.  
  코드를 읽어보면 로그인 버튼이 주석처리 되어있는 것을 알 수 있다.
![webhackingkr_02](https://user-images.githubusercontent.com/41509536/90214203-baaf4080-de32-11ea-900d-94cae8deefbc.png)

- 크롬으로 들어가 도구 `더보기 - 개발자 도구`를 클릭합니다.
![webhackingkr_03](https://user-images.githubusercontent.com/41509536/90214206-bb47d700-de32-11ea-9d43-a056798a232e.jpg)

- 주석 부분의 코드를 찾는다.
![webhackingkr_04](https://user-images.githubusercontent.com/41509536/90214207-bbe06d80-de32-11ea-86ab-e9457b6fe7e6.png)

- 주석처리된 문구에서 `오른쪽 마우스-Edit as HTML`을 클릭한다.
![webhackingkr_05](https://user-images.githubusercontent.com/41509536/90214211-bc790400-de32-11ea-9d26-5cf5c863f723.jpg)

- 주석부분 `<!-- -->`을 삭제하고 관리자 도구를 닫는다.
![webhackingkr_06](https://user-images.githubusercontent.com/41509536/90214213-bd119a80-de32-11ea-8b1b-5e9f1a5daec1.jpg)

- Register 버튼이 생긴다.
![webhackingkr_07](https://user-images.githubusercontent.com/41509536/90214214-bdaa3100-de32-11ea-83dc-3f4c91f8c468.jpg)

- `decode me`에 있는 암호를 복호화해야한다.  
  암호가 ==로 끝나는데, ==는 Base64로 암호화할 때 생긴다.
![webhackingkr_08](https://user-images.githubusercontent.com/41509536/90214216-bdaa3100-de32-11ea-8bc0-67cda29fd86a.png)

- <https://gchq.github.io/CyberChef/>로 들어가 디코딩을 한다.
![webhackingkr_09](https://user-images.githubusercontent.com/41509536/90214220-be42c780-de32-11ea-9524-ccc55c9ad6c6.png)

- output을 해독할 수 있을 때까지 복호화를 진행하면 ip 주소가 나온다.  
  ID, PW, EMAIL,IP값을 입력하고 Submit한다.
![webhackingkr_10](https://user-images.githubusercontent.com/41509536/90214221-bedb5e00-de32-11ea-8f7d-e373aa5a0d91.jpg)
