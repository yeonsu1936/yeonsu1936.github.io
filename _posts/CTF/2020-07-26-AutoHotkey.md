 ---
layout: post
title: Reversing.kr - AutoHotkey1
category: CTF 문제풀이
tags: [CTF, 리버싱, reversing.kr, AutoHotkey1]
comments: true
---
### 문제 : AutoHotkey1, Point : 130
<http://reversing.kr>의 **AutoHotkey1** 를 풀어보도록 하자.  
문제를 클릭하면 AHK.zip 파일이 다운받아진다.

## 문제 확인
- zip파일을 풀고 readme.txt 파일을 열면 다음과 같은 내용을 확인할 수 있다.
![AutoHotkey1_01](https://user-images.githubusercontent.com/41509536/88932271-d35b1a80-d2b8-11ea-9bfd-ee2dc266b25a.png)  

- DecrytKey와 EXE's Key를 각각 md5로 복호화해서 AuthKey를 완성시켜야 한다.
![AutoHotkey1_02](https://user-images.githubusercontent.com/41509536/88932274-d48c4780-d2b8-11ea-9b1b-d1616f2cf02f.jpg)
![AutoHotkey1_03](https://user-images.githubusercontent.com/41509536/88932277-d524de00-d2b8-11ea-836e-08a9bddfa18a.jpg)  
- AuthKey는 "visual studio"이다.
- ahk.exe 파일을 열면 입력을 받는 창이 나오고 프로그램이 종료된다.  
![AutoHotkey1_04](https://user-images.githubusercontent.com/41509536/88932279-d5bd7480-d2b8-11ea-9832-789925ddbf73.png)

## Exeinfo PE
- 먼저 Exeinfo PE 툴로 ahn.exe파일을 확인한다.
- UPX로 패킹된 파일임을 알 수 있다.  
![AutoHotkey1_05](https://user-images.githubusercontent.com/41509536/88932281-d5bd7480-d2b8-11ea-9c3b-271455d697ad.jpg)  

## UPX 언패킹
- UPX 패킹을 풀어준다.  
![AutoHotkey1_06](https://user-images.githubusercontent.com/41509536/88932283-d6560b00-d2b8-11ea-9959-04a799e1aa6d.jpg)  

- UPX 패킹을 풀고 프로그램을 실행하면 "EXE corrupted"라는 메세지 박스가 뜬다.  
![AutoHotkey1_07](https://user-images.githubusercontent.com/41509536/88932286-d6eea180-d2b8-11ea-8fb5-f7083f2acd65.png)  

## 동적 분석
- 언패킹한 파일을 ollydbg로 열고 문자열을 확인해 EXE corrupted 문자열 위에 있는 함수 ahk.004508c7에 들어가 구조를 확인했다.  
![AutoHotkey1_08](https://user-images.githubusercontent.com/41509536/88932288-d6eea180-d2b8-11ea-9edd-b5f0c4eda914.jpg)  

- 해당 메세지가 호출되는 함수 내부를 보면 특정 함수 반환값을 조건으로 "EXE corrupted" 메세지 박스를 띄우고 종료하는 분기와 ">AUTOHOTKEY SCRIPT<" 문자열을 사용하는 함수를 호출하는 분기로 나뉘고 있다.
- sub_004508c7는 CRC 체크를 진행하는 함수임을 알 수 있다.
- CRC 체크를 하는데 언패킹한 파일을 사용하면 에러가 나기 때문에 패킹된 파일을 가지고 분석을 진행한다.  

- UPX로 패킹된 파일의 OEP를 찾아 패킹된 파일의 실제 프로그램 시작 부분으로 이동해야 한다.
  - OEP?
    - OEP : Original Entry Point - 패킹된 파일의 실제 프로그램 시작 부분이다.
    - OEP 이전의 실행 부분은 패킹된 파일이 메모리에 로드되어 압축을 푸는 명령어가 있다.
  - UPX?
    - UPX 패킹의 특징은 PUSHAD ~ POPAD 명령어 내부에서 압축해제 코드가 실행 되고 POPAD 명령어 실행 후 JMP 명령어를 통해 OEP로 이동한다.
    ![AutoHotkey1_09](https://user-images.githubusercontent.com/41509536/88932292-d7873800-d2b8-11ea-94c8-d7d7eeebda67.png)  

- 패킹된 파일을 열면 PUSHAD가 보인다.  
![AutoHotkey1_10](https://user-images.githubusercontent.com/41509536/88932293-d81fce80-d2b8-11ea-9160-87aacb61c19a.jpg)  

- 그리고 밑으로 내리다 보면 POPAD도 나온다.  
![AutoHotkey1_11](https://user-images.githubusercontent.com/41509536/88932294-d8b86500-d2b8-11ea-9265-896bea71e0e9.jpg)  

- POPAD 명령어가 실행되고 JMP 명령어가 뒤따라 오는데 JMP 명령어 뒤에 오는 주소가 바로 OEP이다.
![AutoHotkey1_12](https://user-images.githubusercontent.com/41509536/88932297-d950fb80-d2b8-11ea-8a59-bcefa6d3ce03.jpg)  

- 이동한 후 Ctrl+a를 눌러보면 언패킹한 ahk.exe 파일에서 봤었던 실행코드를 확인 할 수 있다.
  - Ctrl+a : 코드 다시 읽기
  - 메모리에 코드를 풀게 되면 주소창이 회색이 되는데 그때 Ctrl+a 를 하면 재분석해서 보여준다.
![AutoHotkey1_13](https://user-images.githubusercontent.com/41509536/88932300-d9e99200-d2b8-11ea-8c57-063833e9918d.jpg)  
- Ctrl+a 후의 화면
![AutoHotkey1_14](https://user-images.githubusercontent.com/41509536/88932305-db1abf00-d2b8-11ea-9692-3d519e2a2629.jpg)  

- 마우스를 우클릭 하여 [Search for] > [All referenced text strings]를 눌러 문자열을 확인한다.
- "EXE corrupted" 문자열을 확인할 수 있다.
- 더블클릭하여 "EXE corrupted"로 이동한다.  
![AutoHotkey1_15](https://user-images.githubusercontent.com/41509536/88932306-dbb35580-d2b8-11ea-9071-c5445f43df76.jpg)  

- "EXE corrupted" 위에 있는 함수 004508C7에 들어가 내부를 확인해본다.  
![AutoHotkey1_16](https://user-images.githubusercontent.com/41509536/88932308-dbb35580-d2b8-11ea-9def-0066026e3a9d.jpg)   

- 밑으로 내리다 보면 0x00450A7A에서 Registers 창의 DecrytKey를 찾을 수 있다.  
![AutoHotkey1_17](https://user-images.githubusercontent.com/41509536/88932310-dc4bec00-d2b8-11ea-9eac-58afb4a8fad6.jpg)   

- 그리고 0x00448298에서 Registers 창의 EXE's Key를 확인할 수 있다.  
![AutoHotkey1_18](https://user-images.githubusercontent.com/41509536/88932312-dce48280-d2b8-11ea-82e1-eeeda3bb980c.jpg)  

- 0x00448298의 Registers 창은 다음과 같다.
- 그런데 md5 해시 길이보다 pwd가 너무 짧게 나온다.  
![AutoHotkey1_19](https://user-images.githubusercontent.com/41509536/88932313-dd7d1900-d2b8-11ea-9426-585a450fea02.jpg)  

- hex dump로 pwd를 확인한다.  
![AutoHotkey1_20](https://user-images.githubusercontent.com/41509536/88932315-dd7d1900-d2b8-11ea-8f39-79665ddd36e1.jpg)  

- 이렇게 구한 DecrytKey와 EXE's Key를 각각 md5로 복호화해서 AuthKey를 완성시킨다.  
![AutoHotkey1_21](https://user-images.githubusercontent.com/41509536/88932316-de15af80-d2b8-11ea-8ede-e5a5328919d1.jpg)
