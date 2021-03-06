---
layout: post
title: 정보보안기사 - Part 02. 암호학 (section 05. 해시함수와 응용)
category: 정보보안기사
tags: [정보보안기사, 암호학, 해시함수]
comments: true
---
> 정보보안기사 알기사 1200제를 풀고 암기가 필요한 부분을 정리해 놓은 포스팅입니다.
개인 공부 후 자료를 남기기 위한 목적이므로 내용상에 오류가 있을 수 있습니다.

# Part 02. 암호학 (section 05. 해시함수와 응용)
## 해시함수가 가져야 할 기본 성질

| 성질 | 설명 |
| :------------: | :----------- |
| 프리이미지 저항성<br>(역상 저항성,<br>일방향성)| 주어진 임의의 출력값 $y$에 대해, $y=h(x)$를 만족하는 입력값 $x$를 찾는 것이 계산적으로 불가능 |
| 제2프리이미지 저항성<br>(두 번째 역상 저항성,<br>약한 충돌 내성) | 주어진 입력값 $x$에 대해 $h(x)=h(x^\prime)$, $x\ne x^\prime$을 만족하는 다른 입력값 $x^\prime$를 찾는 것이 계산적으로 불가능|
| 충돌 저항성<br>(강한 충돌 내성)| $h(x)=h(x^\prime)$를 만족하는 임의의 두 입력값 $x$, $x^\prime$을 찾는 것이 계산적으로 불가능 |

## 랜덤 오라클 모델(Random Oracle Model)
해시 함수에 대한 이상적인 수학적 모델이 갖는 특징
1. 임의의 길이를 갖는 메시지에 오라클은 0과 1로 이루어진 난수 스트링인 고정된 길이의 `메시지 다이제스트`를 생성해서 제공한다.
2. 이미 다이제스트가 존재하는 메시지가 주어지면 오라클은 `저장되어 있던 다이제스트`를 제공한다.
3. 새로운 메시지에 대한 다이제스트는 다른 모든 다이제스트와는 `독립적`으로 선택될 필요가 있다. 이것은 오라클이 다이제스트를 만들기 위해 공식이나 알고리즘을 사용해서는 **안된다**는 것을 의미한다.

해시함수에서 **랜덤 오라클 모델**은 `프리이미지 저항성`, `제2프리이미지 저항성`, `출동 저항성` 3가지 기준을 충족해야 한다.

## SHA 해시 알고리즘 비교

| 구분 | SHA-1 | SHA-224 | SHA-256 | SHA-384 | SHA-512 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| MD 길이 | 160 | 224 | 256 | 384 | 512 |
| 최대 메시지 길이 | $2^{64}-1$ | $2^{64}-1$ | $2^{64}-1$ | $2^{128}-1$ | $2^{128}-1$ |
| 블록 길이 | 512 | 512 | 512 | 1024 | 1024 |
| 위드 길이 | 32 | 32 | 32 | 64 | 64 |
| 단계 수 | 80 | 64 | 64 | 80 | 80 |

##  주요 해시 알고리즘 비교

| 항목 | MD5 | SHA-1 | RIPEMD-160 |
| :---: | :---: | :---: | :---: |
| 다이제스트 길이 | 128비트 | 160비트 | 160비트 |
| 처리 단위 | 512비트 | 512비트 | 512비트 |
| 단계수  | 64(16번의 4라운드) | 80(20번의 4라운드) | 160(16번의 5병행 라운드) |
| 최대 메시지 크기 | 무한($\infty$) | $2^{64}-1$ 비트 | $2^{64}-1$ 비트 |
| 앤디언 | Little-endian | Big-endian | Little-endian |

## n-비트 해시코드에 대한 공격 난이도

| 구분 | 공격 난이도 |
| :---: | :---: |
| 프리이미지 저항성 | $2^n$ |
| 2차 프리이미지 저항성 | $2^n$ |
| 충돌 저항성 | $2^{n/2}$ |

## 일회용 패스워드(Lamport)
- 유효기간 없이 매 세션마다 서로 상이한 패스워드를 사용하기에 특별 세션의 개인 식별 과정에서 해당 패스워드가 노출되어도 다음 세션에 사용될 패스워드를 예측할 수 없음
-  `해시함수의 일방향성`이 이용됨

## 해시 함수 공격 방법
1. `일치블록 연쇄공격`
	1. 새로운 메시지 $M^\prime$를 사전에 다양하게 만들어 놓았다가 공격하고자 하는 메시지 $M$의 해시함수값 $h(M)$과 같은 해시함수 값을 갖는 것을 찾아 사용하는 공격
	2. 웹에서 MD5를 크랙하거나 프로그램을 이용하는 많은 툴이 `일치블록 연쇄공격`기법을 사용
2. `중간자 연쇄공격`
	전체 해시값이 아니라 해시 중간의 결과에 대한 `충돌쌍`을 찾아 **특정 포인트**를 공격 대상으로 함
3. `고정점 연쇄공격`
	1. 암축함수에서 고정점이란 $f(H_{i-1}, x_i) = H_{i-1}$를 만족하는 쌍 $(H_{i-1}, x_i)$을 말함
	2.  메시지 블록과 연쇄변수 쌍을 얻게 되면 연쇄변수가 발생하는 특정한 점에서 임의의 수의 동등한 블록들 $x_i$를 메시지 중간에 삽입해도 전체 해시값이 변하지 않음
4. `차분 연쇄공격`
	1. `다중 라운드 블록암호의 공격` : 다중 라운드 블록암호를 사용하는 해시 함수에서, 입력값과 그에 대응하는 출력값의 차이의 통계적 특성을 조사하는 기법을 사용하는 공격
	2. `해시함수의 공격` : 압축함수의 입출력 차이를 조사하여, 0의 충돌쌍을 주로 찾아내는 방법을 사용하는 공격

## MAC으로 해결할 수 없는 문제
`제3자에 대한 증명`, `부인 방지`

## MAC에서 재전송 공격을 막을 수 있는 방법
- `재전송 공격` : 프로토콜상에서 유효 메시지를 골라 복사한 후 나중에 재전송함으로써 정당한 사용자로 가장하는 공격
- 방어 방법 : `순서번호`, `타임스탬프`, `비표(nonce)`, `시도응답 방법`

## HMAC
HMAC은 text 데이터에 대하여 다음과 같은 연산식을 통해 구한다.
```
H(K XOR opad, h(K XOR ipad, text))
단, ipad는 input pad, opad는 output  pad라는 상수
```
## 해시함수와 MAC 비교

| 기능 | 단계 | 보안 서비스 |
| :---: | :--- | :---: |
| Hash | 1. 발신자는 메시지가 해시 함수를 거쳐 MD 값을 생성<br>2. 발신자는 메시지와 MD 값을 수신자에게 보냄<br>3. 수신자는 메시지만 동일한 해시 알고리즘을 거치게 하여 독립적인 MD 값을 생성<br>4. 수신자는 두 MD값을 비교, 두 값이 동일하면 메시지는 수정되지 않은 것 | 무결성 |
| HMACK | 1. 발신자는 메시지와 비밀키를 연접시켜 해시 함수를 거져 MAC을 생성<br>2. 발신자는 MAC값을 메시지에 첨부하여 수신자에게 전송<br>3. 수신자는 메시지만 취해 자신의 대칭키와 연접시키고, 해시함수를 이용해 독립적인 MAC 값을 생성<br>4. 수신자는 두 MAC값을 비교, 두 값이 동일하면 수신자는 메시지가 수정되지 않았다는 것과 어떤 시스템으로 부터 왔는지를 알게됨 | 무결성, 인증 |
| CBC-MAC | 1. 발신자는 메시지를 CBC 모드에서 대칭 블록 알고리즘으로 암호화<br>2. 마지막 블록이 MAC으로 사용됨<br>3. 평문 메시지와 MAC이 수신자에게 보내짐<br>4. 수신자는 메시지를 암호화하고, 새로운 MAC 값을 생성한다. 두 값을 비교하여 같으면 수신자는 메시지가 수정되지 않았다는 것과 어떤 시스템으로 부터 왔는지를 알게됨 | 무결성, 인증|
| CMAC | CMAC은 CBC-MAC과 동일한 방법으로 동작<br>차이점 : CMAC은 더 복잡한 논리 함수와 수학 함수에 기반함| 무결성, 인증 |

## MAC과 전자서명
- MAC은 무결성, 인증 서비스를 제공
- 전자서명은 무결성, 인증, 부인방지 서비스를 제공
