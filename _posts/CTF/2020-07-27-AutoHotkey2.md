---
layout: post
title: Reversing.kr - AutoHotkey2
category: CTF 문제풀이
tags: [CTF, 리버싱, reversing.kr, AutoHotkey1]
comments: true
---
### 문제 : AutoHotkey2, Point : 130
<http://reversing.kr>의 **AutoHotkey2** 를 풀어보도록 하자.  
문제를 클릭하면 AHK2.zip 파일이 다운받아진다.

## 문제 확인
- ahk2.exe파일을 그냥 실행시키면 "EXE corrupted"라는 메세지 박스가 뜬다.  
![AutoHotkey2_01](https://user-images.githubusercontent.com/41509536/89115418-1be62400-d4c3-11ea-8138-85960c33da93.jpg)
![AutoHotkey2_02](https://user-images.githubusercontent.com/41509536/89115419-1d175100-d4c3-11ea-9077-6b9ea5df1c49.jpg)

## 동적 분석
- AutoHotkey1 문제처럼 OEP로 이동한 후 마우스를 우클릭 하여 [Search for] > [All referenced text strings]를 눌러 문자열을 확인한다.
- "EXE corrupted" 문자열을 확인할 수 있다. 더블클릭하여 "EXE corrupted"로 이동한다.  
![AutoHotkey2_03](https://user-images.githubusercontent.com/41509536/89115420-1dafe780-d4c3-11ea-8537-3071164c06c1.png)

- 프로그램을 실행하면  함수 004508C7 내에서 crc체크에 걸려서 exe corrupted이라는 문자열을 출력하게 된다.
- 따라서 "EXE corrupted" 위에 있는 함수 004508C7에 들어가 내부를 확인해본다.  
![AutoHotkey2_04](https://user-images.githubusercontent.com/41509536/89115422-1dafe780-d4c3-11ea-92ee-c126590dfb40.jpg)  

- 이 부분이 CRC 체크를 진행하는 함수의 반복문이다.
- 해당 코드를 분석해보면, CALL AHK2.00458C2D 함수를 통해 파일의 처음부터 1바이트씩 값을 가져와 이 값을 토대로 CALL AHK2.00450F95 함수에서 CRC 체크섬값을 만든다.
- 이 때 반복문을 탈출하는 값은 00032A1F이다.  
![AutoHotkey2_05](https://user-images.githubusercontent.com/41509536/89115423-1e487e00-d4c3-11ea-996c-3243c164f4ce.png)  

- HxD로 00032A1F를 찾아보려고 했지만 마지막 4바이트 값은 00032A1F가 아니었다.  
![AutoHotkey2_06](https://user-images.githubusercontent.com/41509536/89115424-1ee11480-d4c3-11ea-9c73-080f0e7daf95.jpg)  

- 그래서 다음 로직을 분석해보았다.
- 반복문을 빠져나오면 구해진 체크섬값과 AAAAAAAA를 XOR한 값을 EBP-10의 값과 비교를 한다.
- EBP-10값을 보면 39A464194라는 값이 있는데 ahk2.exe 파일의 마지막 4바이트 값이라는 것을 확인할 수 있다. 그리고 명령에서는 EAX와 522D85D4를 비교한다는 것을 알 수 있다.  
![AutoHotkey2_07](https://user-images.githubusercontent.com/41509536/89115425-1ee11480-d4c3-11ea-95e4-874175e2728a.jpg)

- 따라서 다음과 같이 바꿔준다.  
![AutoHotkey2_08](https://user-images.githubusercontent.com/41509536/89115426-1f79ab00-d4c3-11ea-9eda-2662ad6146c9.png)  

- 이렇게 하면 문제가 풀릴 줄 알았는데 똑같이 "EXE corrupted" 메시지 박스가 뜬다.
- 그래서 Autohotkey 1 문제와 hxd로 비교해봤더니 4바이트가 추가로 더 달랐다.  
  ```
  Autohotkey 1 : 00 28 03 00 F9 34 87 43
  Autohotkey 2 : 90 83 9A DC D4 85 2D 52
  ```
- 따라서 ahk2.exe파일에 00 28 03 00도 체크섬해주면 된다.  
![AutoHotkey2_09](https://user-images.githubusercontent.com/41509536/89115427-1f79ab00-d4c3-11ea-8e98-090cb542538c.jpg)  

- 그런데 또 "EXE corrupted" 메시지 박스가 뜬다.
- ollydbg로 다시 확인해보니 체크썸이 바뀌었다.  
![AutoHotkey2_10](https://user-images.githubusercontent.com/41509536/89115428-20124180-d4c3-11ea-863e-cf5f5987a3a0.jpg)

- 따라서 마지막 4바이트를 다시 33 C6 2B 36으로 바꿔준다.  
![AutoHotkey2_11](https://user-images.githubusercontent.com/41509536/89115429-20aad800-d4c3-11ea-8969-e9916c8b0215.jpg)

- 파일을 저장하고 실행시키면 파일이 제대로 실행된다.
- 따라서 마지막 4바이트를 다시 33 C6 2B 36으로 바꿔준다.    
![AutoHotkey2_12](https://user-images.githubusercontent.com/41509536/89117396-6756fd00-d4d8-11ea-8bf7-d33e86963dc4.png)

- 파일을 저장하고 실행시키면 파일이 제대로 실행된다.  
![AutoHotkey2_13](https://user-images.githubusercontent.com/41509536/89117393-63c37600-d4d8-11ea-8934-1288222b7306.jpg)  

- 플래그가 안나오고 문장이 나온다.
- 구글에 검색하면 단어가 보인다. 이것을 인증하면 문제가 풀린다.
![AutoHotkey2_14](https://user-images.githubusercontent.com/41509536/89117395-66be6680-d4d8-11ea-968a-b49aa484b7c2.jpg)
