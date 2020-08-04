---
layout: post
title: Reversing.kr - Replace
category: CTF 문제풀이
tags: [CTF, 리버싱, reversing.kr, Replace]
comments: true
---
### 문제 : Replace, Point : 150
<http://reversing.kr>의 **Replace** 를 풀어보도록 하자.  
문제를 클릭하면 Replace.zip 파일이 다운받아진다.

## 문제 확인
- zip파일을 풀고 Replace.exe 파일을 실행하면 다음과 같은 내용을 확인할 수 있다.  
  숫자를 입력하고 Check 버튼을 누르면 에러가 발생한다.
![Replace01](https://user-images.githubusercontent.com/41509536/89118758-4942ca00-d4e3-11ea-8341-e24cd5ec2904.jpg)

## Exeinfo PE
- 먼저 Exeinfo PE 툴로 Replace.exe파일을 확인한다.  
  패킹이 되어있지 않으니 바로 Ollydbg로 분석을 시작한다.
![Replace02](https://user-images.githubusercontent.com/41509536/89118759-49db6080-d4e3-11ea-81d4-fd84e07a3f30.jpg)

## 동적 분석
- Ollydbg로 파일을 실행시켜 숫자를 입력하고 check를 클릭하면 0040466F에서 EAX에 0x90(NOP)을 저장하는데 이 부분에서 문제가 발생한다.
![Replace03](https://user-images.githubusercontent.com/41509536/89118761-4a73f700-d4e3-11ea-8e4b-d5cf715aa481.jpg)

- 그 이유는 숫자를 입력하고 Check를 클릭했을 때, 0040466F에서 접근할 수 없는 메모리(60160A9D)에 0x90(NOP)이라는 데이터를 넣었기 때문이다.  
  60XXXXXXXX으로 시작하는 주소는 당연히 접근할 수 없고, 접근할 수 없는 영역에 값을 덮어 쓰려고 하니 에러가 발생하게 된다.
![Replace04](https://user-images.githubusercontent.com/41509536/89118762-4b0c8d80-d4e3-11ea-80cf-e294d3d3e885.jpg)

- 마우스를 우클릭 하여 [Search for] > [All intermodular calls]를 눌러 API를 확인한다.  
  숫자를 입력받는 함수인 GetDlgItemInt에 BP를 걸고 실행하면 0040105A에서 멈춘다.
![Replace05](https://user-images.githubusercontent.com/41509536/89118763-4ba52400-d4e3-11ea-9c37-c8ed47cca928.jpg)

- GetDlgItemInt의 로직은 다음과 같다.
  - 1. 0040105A : USER32.dll 함수에 속한 GetDlgItemInt 함수를 호출한다.
  (숫자를 입력받고 16진수로 변환한 후 EAX에 저장)
  - 2. 00401060 : **0x4084D0** 에 EAX값 저장(입력한 값)한다.
  - 3. 00401065 : **004046FF 함수** 를 호출한다.
  - +) 0040105A : DS(데이터 영역) 0040509C 주소에 접근하여 4 바이트 만큼의 데이터를 입력받는다.

- 따라서 **004046FF 함수** 내부로 들어와 분석해보자.
![Replace06](https://user-images.githubusercontent.com/41509536/89118764-4ba52400-d4e3-11ea-8db3-bbc48ff20790.jpg)
- **0040106A** 에서는 다음과 같는 과정이 발생한다.
  - 0040466F : 0040467A 함수를 호출한다.
  - 0040467A : 0x406016 주소에 **0x61906EB** 값을 넣는다.
  - 0040684 : 0040489 함수를 호출한다.
  - 0040489 : **0x4084D0** 주소의 값을 **1 증가** 시킨다.
  - 004048F : 0040489로 돌아가 **한번 더 0x4084D0** 주소의 값을 **1 증가** 시키고 **00404674** 주소로 RETN한다.

- **0040106A** 에서는 EAX를 초기화하고 00404690 주소로 점프한다.  
![Replace07](https://user-images.githubusercontent.com/41509536/89149254-f62a4f00-d596-11ea-840a-81fcfb9af446.jpg)

- 정리하면,  
![Replace08](https://user-images.githubusercontent.com/41509536/89149255-f6c2e580-d596-11ea-8f0b-a1f49077142b.png)
  - 00404690 : **0x4084D0**  주소의 값을 EAX에 넣는다.
  - 004069A : **0x4084D0**  주소의 값에 2를 더해주는 함수인 **00404689**를 호출한다.
  - 0040469F : 0x390000C6 값을 **0040466F** 주소에 넣는다.
  - 00404A9 : **0040466F** 주소를 호출한다.  
  여기서 **0040466F** 주소는 접근할 수 없는 주소에 0x90을 저장하려고 해서 에러가 발생한 부분이다.
![Replace09](https://user-images.githubusercontent.com/41509536/89149256-f75b7c00-d596-11ea-9dcc-a5e1feefc4ff.jpg)

- 따라서 EAX에 접근 가능한 정상적인 주소가 들어있다면 그 주소에 NOP을 넣고, 문제없이 004046C4 주소에서 00401071로 점프할 것이다.
![Replace10](https://user-images.githubusercontent.com/41509536/89149257-f7f41280-d596-11ea-9115-55e4ebb9b48a.png)

- 00401071에서 00401084 주소로 점프시킨다. 00401073~0040107E 부분에서 성공 문자열을 출력시키는 것을 볼 수 있다.
  하지만 위에 있는 00401071 주소의 코드 때문에 성공 문자열을 출력시켜주는 곳을 지나가게 된다.

- 아까 0040466F 함수를 두 번 호출할 때 EAX의 연속적인 2바이트를 0x90(NOP)으로 바꿀 수 있었다.
  따라서 00401071~00401072 값이 NOP으로 바뀌려면 0x4084D 주소의 값을 EAX에 넣을 때 0x4084D 주소의 값이 00401071이 되어야 한다.

- 즉, `사용자가 입력한 숫자 + 4 + **0x60165C7** = 401071`
  `FFFF FFFF - **0x60165C7** - 4 + 00401071`을 하면 원하는 값이 나온다.

- 10진수로 바꾸고 프로그램에 입력한 후 Check를 누르면 문제가 풀린다.
![Replace11](https://user-images.githubusercontent.com/41509536/89149258-f9253f80-d596-11ea-8b2f-d16e2ae4c0c5.jpg)
