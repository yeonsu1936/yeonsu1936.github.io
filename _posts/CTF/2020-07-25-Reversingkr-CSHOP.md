---
layout: post
title: Reversing.kr - CSHOP
category: CTF 문제풀이
tags: [CTF, 리버싱, reversing.kr, CSHOP]
comments: true
---
### 문제:CSHOP, Point:120
<http://reversing.kr>의 **CSHOP** 를 풀어보도록 하자.  
문제를 클릭하면 CSHOP.zip 파일이 다운받아진다.  

## 문제 확인
- zip파일을 풀고 CSHOP.exe 파일을 실행시키면 다음과 같이 회색 화면이 뜬다.
![CSHOP01](https://user-images.githubusercontent.com/41509536/88749300-f89f3a00-d18d-11ea-8f80-02d5ff50042f.jpg)

## Exeinfo PE
- Exeinfo PE 툴을 사용하여 패킹 여부와 프로그램을 제작한 언어를 확인한다.
- c#으로 만들어진 **.net 프로그램** 이다.
![CSHOP02](https://user-images.githubusercontent.com/41509536/88749304-f9d06700-d18d-11ea-9428-3c9e551bba47.jpg)  

- **닷넷(.NET) 프로그램?**
  - 닷넷 프레임워크(.NET Framework)는 마이크로소프트사에서 제공하는 윈도우 프로그램 개발 및 실행환경이다.
  - 네트워크 작업, 인터페이스등의 많은 작업을 캡슐화하여 코딩의 효율성을 증대시켰다.
  - .NET의 특징은 CLS(닷넷 프레임워크의 언어가 반드시 지켜야하는 언어 스펙)을 따르는 언어라면 어떠한 언어라도 닷넷 프레임워크에서 실행가능하며 CLR이라는 가상기계 위에서 작동하기 때문에 플랫폼에 독립적이며 궁극적으로 프로그래머가 코딩(특히 윈도우 프로그램)을 하는데 더 편한 환경을 제공해준다.
  - 출처 : <https://coding-factory.tistory.com/132>

## dotPeek
- dotPeek을 이용해 FrmMain을 살펴보면 **_Click 함수** 에 특정 문자열이 저장되어 있다.
- 문자열을 입력해봤더니 정답은 아니었다.
- 바로 위에 Form1_Load 함수가 있는데 문자열은 비어있었다.
- Form1_Load 함수는 어떤 버튼을 클릭하면 그 이후에 from을 실행하는 기능을 하지 않을까 유추했다.
![CSHOP03](https://user-images.githubusercontent.com/41509536/88749306-fa68fd80-d18d-11ea-9dd0-9429af900b92.jpg)  

- InitializeComponent() 함수를 보면 새로운 Label 객체를 개속 새로 선언한다.
- C#에서 Label은 static 문자열을 화면에 표시하는 역할을 하는 객체이다.
![CSHOP4](https://user-images.githubusercontent.com/41509536/88749307-fb019400-d18d-11ea-9979-a9b40f25cfcf.jpg)  

- Point만 보면 위치가 (43, 123) (90, 123) 등으로 y 좌표는 같고 x 좌표만 다르다.
- 그리고 사이즈가 0인 버튼이 존재한다
- 이 버튼을 누르면 **_Click 이벤트** 가 발생하고, load_form에 있는 문자열(공백)이 특정 문자로 배치되며 각각의 라벨은 각 위치에 로케이션 된다.
![CSHOP05](https://user-images.githubusercontent.com/41509536/88749308-fb019400-d18d-11ea-8f2b-0bd7df1c5a6c.jpg)  

## dnSpy
- .net 디컴파일러인 dnSpy 툴로 버튼의 사이즈를 수정할 것이다.
- button size를 표현한 코드를 우클릭 후 [Edit IL Instructions]를 클릭한다.
![CSHOP06](https://user-images.githubusercontent.com/41509536/88749310-fb9a2a80-d18d-11ea-8230-340424f68b2a.png)  

- size를 100 x 100으로 바꾼 후, 파일을 저장한다.
![CSHOP07](https://user-images.githubusercontent.com/41509536/88751536-c512de80-d192-11ea-9e4e-8d3444814da9.jpg)  

- 버튼을 클릭하면 key값이 보인다.
![CSHOP08](https://user-images.githubusercontent.com/41509536/88751539-c6440b80-d192-11ea-8147-9eb5fbadbdb8.jpg)  
![CSHOP09](https://user-images.githubusercontent.com/41509536/88751542-c6440b80-d192-11ea-8bb7-9cc777e1dcaf.jpg)  

## 문제 후기
- 이 문제는 버튼을 클릭하면 키 값이 보이도록 하는 프로그램이다.
- 이 문제의 원래 의도는 버튼 사이즈를 바꿔 버튼을 클릭해 키 값을 보는 것인데, 문제 의도와는 다르게 엔터나 스페이스바를 눌러도 버튼이 보인다...ㅋㅋㅋ
