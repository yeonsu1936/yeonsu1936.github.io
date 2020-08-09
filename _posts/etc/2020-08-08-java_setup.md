---
layout: post
title: 자바(JAVA) 다운로드 및 설치하기, 환경변수 설정하기, jdk 13 설치
category: etc
tags: [java]
comments: true
---
# Java SDK 설치하기(Java SE Development Kit 13)
## JDK 다운로드
-  구글에 `Java Development Kit download`라고 입력하면 오라클 홈페이지에서 SDK 다운로드 페이지(<https://www.oracle.com/java/technologies/javase-jdk13-downloads.html>)로 이동할 수 있다.


- 가장 최신 버전인 13버전을 다운받는다.  
  운영체제가 윈도우이므로 `jdk-13.0.2_windows-x64_bin.exe`를 클릭한다.
![java_01](https://user-images.githubusercontent.com/41509536/89724759-48112000-da42-11ea-9f36-1ea8c2a50c2d.JPG)

- check box를 클릭하고 다운을 진행한다.
![java_02](https://user-images.githubusercontent.com/41509536/89724760-48a9b680-da42-11ea-8b85-4db349754814.JPG)

## JDK 설치하기
- 다운받은 파일을 실행한다.
![java_03](https://user-images.githubusercontent.com/41509536/89724761-49424d00-da42-11ea-9e39-088137fb5181.JPG)

- next를 누른다.
![java_04](https://user-images.githubusercontent.com/41509536/89724762-49dae380-da42-11ea-8fc0-a66bafc75d24.JPG)

- 설치하려는 폴더를 지정한다.  
  컴퓨터 여유공간이 없어서 `D:\Program Files\Java\jdk-13.0.2`에 설치를 진행한다.  
  설치 폴더는 나중에 환경변수를 설정할 때 쓰이므로 기억해야 한다.
![java_05](https://user-images.githubusercontent.com/41509536/89724763-49dae380-da42-11ea-8b37-43172c923e68.JPG)

- 설치가 완료되면, 자바를 어디서든 실행할 수 있도록 환경 변수를 설정해야한다.
![java_06](https://user-images.githubusercontent.com/41509536/89724764-4a737a00-da42-11ea-840e-d338b3178339.JPG)
![java_07](https://user-images.githubusercontent.com/41509536/89724765-4a737a00-da42-11ea-8479-c9d67d82f319.JPG)

## 환경변수 설정하기
- 시작이나 검색에서 **시스템 환경 변수 편집** 을 입력 후 클릭한다.
![java_08](https://user-images.githubusercontent.com/41509536/89724766-4b0c1080-da42-11ea-86cf-f3185f18c39c.JPG)

- **시스템 환경 변수 편집 > 고급 > 환경 변수** 를 클릭한다.
![java_09](https://user-images.githubusercontent.com/41509536/89724767-4b0c1080-da42-11ea-97c9-9e362dab95da.JPG)

- **새로 만들기** 를 클릭해서 자바 경로를 잡아줄 시스템 변수를 생성한다.
![java_10](https://user-images.githubusercontent.com/41509536/89724768-4ba4a700-da42-11ea-963b-81d00c347670.JPG)

- 변수 이름은 `Java-HOME`, 변수 값은 `D:\Program Files\Java\jdk-13.0.2 (설치 경로)`을 입력한다.
![java_11](https://user-images.githubusercontent.com/41509536/89724769-4c3d3d80-da42-11ea-9595-ba281d48b1eb.JPG)

- 다음으로 `JAVA_HOME` 변수를 **Path** 에 추가한다. **Path 시스템 변수** 를 클릭한 뒤 **편집** 을 클릭한다.
![java_12](https://user-images.githubusercontent.com/41509536/89724770-4c3d3d80-da42-11ea-9406-a0b23d1688d9.JPG)

- **새로 만들기** 클릭 후 `%JAVA_HOME%\bin\`을 추가한다.
![java_13](https://user-images.githubusercontent.com/41509536/89724771-4cd5d400-da42-11ea-9432-0b06184d2d2b.JPG)

## CMD 콘솔 창에서 JAVA 확인하기
- 환경 변수 설정이 잘 되었는지 확인하기 위해 CMD를 켠다.
![java_14](https://user-images.githubusercontent.com/41509536/89724772-4cd5d400-da42-11ea-95fb-faf3f1233f21.JPG)

- `java -version`과 `javac -version`을 입력해 버전이 맞게 뜨는지 확인한다.  
  ```
  java -version
  java version "13.0.2" 2020-01-14
  Java (TM) SE Runtime Environment (build 13.0.2+8) ~~

  javac -version
  javac 13.0.2
  ```
  ![java_15](https://user-images.githubusercontent.com/41509536/89724773-4d6e6a80-da42-11ea-979e-79e1344da06b.JPG)
