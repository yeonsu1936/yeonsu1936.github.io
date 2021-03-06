---
layout: post
title: Reversing.kr - Position
category: CTF 문제풀이
tags: [CTF, 리버싱, reversing.kr, Position]
comments: true
---
### 문제 : Position, Point : 160
<http://reversing.kr>의 **Position** 를 풀어보도록 하자.   
문제를 클릭하면 Position.zip 파일이 다운받아진다.

## 문제 확인
- Position.exe 파일을 열면 다음과 같은 프로그램이 실행된다.
![Position01](https://user-images.githubusercontent.com/41509536/89156141-2842ad80-d5a5-11ea-86bf-e385d86598b7.png)  

- ReadMe.txt 파일을 보면 시리얼 번호가 76876-77776일때 name을 찾고  password를 찾는 문제임을 알 수 있다. 비밀번호는 _ _ _ p 이다.
![Position02](https://user-images.githubusercontent.com/41509536/89156148-2973da80-d5a5-11ea-872c-f2d4011dd157.png)  

## 동적 분석
- 문자열을 검색해 Correct!로 이동한다.
![Position03](https://user-images.githubusercontent.com/41509536/89156151-2a0c7100-d5a5-11ea-92b1-8766782ccdae.png)  

- Position.00011740 함수를 호출하고 결과에 따라 Correct와 Wrong으로 분기되는 것을 볼 수 있다. 리턴값이 1이어야 Correct가 된다.
![Position04](https://user-images.githubusercontent.com/41509536/89156152-2aa50780-d5a5-11ea-9359-a7ff249b9520.png)  

- Position.00011740 함수 내부로 들어가 살펴보자.  
Position.00011740 함수에서는 Name을 입력받은 후 조건을 검사한다.
  1. [ECX-C] 값과 4를 비교한 후 같으면 Position.000117D9로 점프한다.
![Position05](https://user-images.githubusercontent.com/41509536/89156153-2b3d9e00-d5a5-11ea-8a1c-e72c3f248f83.png)  

  2. 점프하면 첫번째 루프에서 name에 입력한 4글자가 알파벳 소문자인지 확인한다.
![Position06](https://user-images.githubusercontent.com/41509536/89156157-2c6ecb00-d5a5-11ea-8a47-08980a633655.png)  

  3. 두번째 루프에서는 name의 각각의 글자 중에 서로 같은 글자가 있는지 확인한다.
![Position07](https://user-images.githubusercontent.com/41509536/89156161-2d076180-d5a5-11ea-8d8c-a940f69cc66d.png)  

  4. 세번째 루프에서는 Serial의 글자수가 총 11글자이고, Serial의 5번째 문자가 ' - ' 인지 확인한다.
![Position08](https://user-images.githubusercontent.com/41509536/89156165-2d9ff800-d5a5-11ea-92c4-854f4957284a.png)  

- 정리하면 다음의 3가지 조건을 검사한다.
  1. Name의 길이가 4인가?
  2. Name이 a~z까지의 소문자로 구성되어 있는가?
  3. Name의 구성 문자중 중복이 있는가?  

-  그 이후로는 name을 이용해 serial을 만드는 루틴이 나온다. ~~(ida pro 유료버전이 없기 때문에 다른 사람 문제에서 가져왔다...)~~  
  아래 사진 출처 :  <https://gyeongje.tistory.com/249>
![Position09](https://user-images.githubusercontent.com/41509536/89156168-2e388e80-d5a5-11ea-801a-465404be4360.png)  

- GetAt(char* a1, int a2) 함수는 문자열 a1의 a2번째 인덱스의 값을 반환한다.  
  if문으로 10번을 검증하는 루틴이 있다.  
  시리얼 값 중에서 ' - '를 제외하면 10자리이기 때문에 이부분에서 password를 이용해 Serial을 만든다는 것을 알 수 있다.  

- password값 1,2번째 값을 통해 serial 값 ' - ' 앞부분 5자리를 만들고, password값 4,5 번째 값을 통해 serial 값 ' - ' 뒷부분 5자리를 만든다.  

- GetAt(&v61, 0)에서 v61은 password이므로 한자리씩 가져와서 shift연산, and 연산을 통해 입력한 시리얼 값과 비교한다.
- if(v13==v12)에서 v12는 입력한 시리얼값의 첫번째 인덱스, v13은 password를 연산해 나온 시리얼 값의 첫번째 인덱스이다.  

- 이런 식으로 10자리가 모두 맞아야 1을 리턴해 Corret!를 볼 수 있다.  

- Name값은 4자리이기 때문에 Brute Force를 통해 확인하였다.
![Position10](https://user-images.githubusercontent.com/41509536/89156170-2ed12500-d5a5-11ea-97c1-56b80304be5c.png)  

- Name 값은 3개가 나오는데 1개만 답으로 인정된다.
![Position11](https://user-images.githubusercontent.com/41509536/89156171-2f69bb80-d5a5-11ea-948b-6dd0651bb152.png)
