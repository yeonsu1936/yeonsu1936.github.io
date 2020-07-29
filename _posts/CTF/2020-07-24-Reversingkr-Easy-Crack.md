---
layout: post
title: Reversing.kr - Easy Crack
category: CTF 문제풀이
tags: [CTF, 리버싱, reversing.kr, Easy Crack]
comments: true
---
### 문제:Easy Crack, Point:100
<http://reversing.kr>의 첫 번째 문제인 Easy Crack을 풀어보도록 하자.  
문제를 클릭하면 Easy_Crack.exe 파일이 다운받아진다.

## 문제 확인
- 해당 파일을 실행시키면 사용자가 입력할 수 있는 창이 뜬다.
- 임의의 값을 넣으면 Incorrect Password라는 창이 뜨면서 프로그램이 종료된다.
- 출제자가 정해놓은 패스워드를 리버싱으로 알아내고 그것을 입력하면 문제가 풀릴 것이다.
![Easy Crack01](https://user-images.githubusercontent.com/41509536/88357045-d92a9a80-cda4-11ea-9b97-5f1e41c03995.jpg)

## Exeinfo PE
- 리버싱을 하려면 가장 먼저 패킹 여부와 어떤 언어로 프로그램이 제작되었는지 확인이 필요한다.
- Exeinfo PE 툴을 사용하여 이를 확인해본다.
- 이 프로그램은 C++로 제작되었으며 패킹이 되어있지 않음을 알 수 있다.
![Easy Crack02](https://user-images.githubusercontent.com/41509536/88357120-2444ad80-cda5-11ea-878c-4e49e9d3d506.jpg)

## 문자열 확인
- 마우스를 우클릭 하여 [Search for] > [All referenced text strings]를 눌러 문자열을 확인한다.
- 여기서 중요한 문자열인 Congratulation을 더블클릭하여 해당 코드로 이동한다.
![Easy Crack03](https://user-images.githubusercontent.com/41509536/88357155-40484f00-cda5-11ea-9a35-6647b25e2a5b.png){: width="476" height="556"){: .center}
![Easy Crack04](https://user-images.githubusercontent.com/41509536/88357171-548c4c00-cda5-11ea-82f2-4208b5ef2eda.jpg)

## 동적 분석
- 00401135에 Break Point(단축키 F2)를 걸어 Incorrect Password로 점프하지 못하게 한다.
- F9를 눌러 실행을 하면 메세지 박스가 뜨고, 일단 입력 값에 아무 값이나 넣어본다.
![Easy Crack05](https://user-images.githubusercontent.com/41509536/88357174-56560f80-cda5-11ea-829a-ac087572bbde.jpg)

- 004010B0를 보면 ESP+5에 있는 값과 61(Hex 'a')를 비교한다.
- ESP+5는 입력값 중 두번 째 값을 의미한다.
- 따라서 두번 째 값은 'a'이다.
- JNZ(Jump Not Zero)는 0이 아니면 점프하기 때문에 제로 플래그가 1이면 점프하지 않는다.
![Easy Crack06](https://user-images.githubusercontent.com/41509536/88357175-56eea600-cda5-11ea-942b-cd6f35f3d3ca.jpg)

- 아스키로 '5y'인 부분을 스택에 넣고, ESP+A 부분인 세 번째 글자부터 스택에 넣는다.
- 그리고 함수를 호출한다.
- F7로 함수 내부로 들어가면 세 번째, 네 번째 글자와 '5y'를 비교한다.
- 따라서 세 번째, 네 번째 글자는 '5y'이다.
![Easy Crack07](https://user-images.githubusercontent.com/41509536/88357178-56eea600-cda5-11ea-8b50-eb18af9bd97c.jpg)

- 004010D1 부터 다섯 번째 글자부터 끝까지 'R3versing'과 비교를 한다.
- 맨 앞글자를 제외한 나머지 key 값은 'a5yR3versing'이다.
​![Easy Crack08](https://user-images.githubusercontent.com/41509536/88357179-57873c80-cda5-11ea-9de4-bcbc6f75eed3.png)

- 0040110D에서 ESP+4인 첫 번째 글자와 45(Hex 'E')를 비교한다.
- 따라서 첫번 째 값은 'E'이다.
![Easy Crack09](https://user-images.githubusercontent.com/41509536/88357180-57873c80-cda5-11ea-85c0-3d32b437d626.jpg)

- 결과적으로 Key 값은 'Ea5yR3versing'이다.
- exe 파일을 실행한 뒤 키 값을 넣으면 Congratulation!! 문구가 뜬다.
