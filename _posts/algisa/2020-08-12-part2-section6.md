---
layout: post
title: 정보보안기사 - Part 02. 암호학 (section 06. 전자서명과 PKI)
category: 정보보안기사
tags: [정보보안기사, 암호학, 전자서명, PKI]
comments: true
---
> 정보보안기사 알기사 1200제를 풀고 암기가 필요한 부분을 정리해 놓은 포스팅입니다.
개인 공부 후 자료를 남기기 위한 목적이므로 내용상에 오류가 있을 수 있습니다.

# Part 02. 암호학 (section 06. 전자서명과 PKI)
## 전자서명
- 전자서명은 메시지와 메시지를 생성한 `사람`과의 인증을 의미하며,  전자 데이터로 되어 있는 메시지에 전자적인 `서명`을 하는 것을 의미함
- 전자서명은 `사용자`와 메시지에 대한 인증 기능을 포함

## 전자서명 서비스
`인증`, `무결성`, `부인방지`, `기밀성`

## 전자서명의 주요 기능

| 구분 | 설명 |
| :---: | :--- |
| `위조 불가` | 합법적인 서명자만이 전자서명을 생성 가능 |
| `서명자 인증` | 전자서명의 서명자를 불특정 다수가 검증할 수 있어야 함 |
| `부인 방지` | 서명자는 서명행위 이후에 서명한 사실을 부인할 수 없어야 함 |
| `변경 불가` | 서명한 문서의 내용을 변경할 수 없어야 함 |
| `재사용 불가` | 전자문서의 서명을 다른 전자문서의 서명으로 사용할 수 없어야 함 |

## 공개키 알고리즘과 전자서명
- **공개키 알고리즘**에서는 **암호화**에 사용하는 키는 `수신자의 공개키`이고, **복호화**에 사용하는 키는 `수신자의 개인키`이다.  
- **전자서명**에서 **서명 작성**에 사용하는 키는 `송신자의 개인키`이고(`부인방지`), **검증**에 사용하는 키는 `송신자의 공개키`이다(`서명자 인증 조건` 만족).

## 전자서명의 안전성
- `일반적 위조 불가` : 서명의 위조가 불가능한 문서가 존재해야 함
- `선택적 위조 불가` : 어떤 정해진 문서 이외에 대해서는 서명의 위조가 불가능해야 함
- `존재적 위조 불가` : 어떠한 문서에 대해서도 서명의 위조가 불가능해야 함

## 전자서명의 구조

| 구분 | 설명 | 기반 이론 |
| :---: | :--- | :---: |
| `RSA` | RSA 암호 방식의 안전도와 일치<br>해시 알고리즘의 강도에 따라 위험성이 좌우됨<br>d는 개인키이고 e와 n은 공개 | 소인수분해 |  
| `ElGamal` | ElGamal 암호시스템과 동일한 키를 사용하지만 알고리즘은 전혀 다름<br> 실제로 거의 사용되지 않으며 변형인 DSA를 사용함<br>RSA 서명 길이의 2배, 거듭제곱의 계산량은 거의 4배에 이름<br>랜덤성도 제공해야 하기 때문에 더 느려짐 | 이산대수 |
| `DSA` | 미국의 전자서명 표준<br>ElGamal 전자서명방식과 유사하지만 서명과 검증에 소요되는 계산량을 획기적으로 줄임<br>`Schnorr`는 이산대수를 이용하는 전자서명의 효율성을 높이기 위해 $q(p-1)$인 소수의 $p,q$의 사용을 처음 제안함<br>`Schnorr`는 위수 $p-1$을 갖는 원시원소를 사용하는 대신, $p-1$의 소인수 $q$를 위수로 갖는 생성원을 사용 | 이산대수 |
| `DSS` | Schnorr 구조에서 빌려온 아이디어를 ElGamal에 더하여 수립한 디지털 서명 알고리즘<br>서명과 검증에 소유되는 계산량을 획기적으로 **줄임** | 이산대수 |
| `ECDSA` | 타원곡선상에서 이산대수 문제를 이용하는 방식<br>다른 공개키 방식에 비해 키 길이가 짧지만 높은 안전성을 제공 | 타원곡선상의 이산대수 |

## 특수 전자서명 방식
1. `다중 서명`
n명의 서로 다른 서명자가 같은 메시지에 대해 서명하지만 그 결과로 n개의 서명이 아니라 하나의 서명을 얻게 되는 기법  
따라서 n개의 서명 대신에 하나의 서명을 확인하여 n명이 서명한 사실을 알 수 있음

2. `은닉 서명`
기본적으로 임의의 전자서명을 만들 수 있는 서명자와 서명 받을 메시지를 제공하는 제공자로 구성되어 있는 서명 방식  
제공자의 신원과 (메시지, 서명) 쌍을 연결시킬 수 없는 익명성을 유지할 수 있는 서명

3. `부인방지 서명`
서명자의 도움 없이는 서명검증이 불가능한 서명방식  
서명자는 자신의 서명임을 검증자에게 확인시키며, 부인과정을 통해 불법적인 서명에 대해 자신이 서명하지 않았음을 증명함

4. `의뢰부인방지 서명`
부인방지서명이 서명자의 익명성을 보장하지 못하는 점을 부분적으로 개선한 방식  
임의의 검증자가 부인과정을 수행할 수 없도록 하는 대신에 오직 특정인만이 부인과정을 수행하도록 함으로써 익명성 보장에 대한 취약점을 부분적으로 제거한 방식

5. `수신자지정 서명`
지정된 수신자만이 서명을 확인할 수 있는 방식으로 서명자조차도 서명을 확인할 수 없도록 구성

## 그룹 서명

## 검증자 지정 서명
- 서명자가 검증자를 지정하여 서명을 생성하며, 이렇게 생성된 서명은 지정된 검증자만이 서명자로부터 생성되었는지를 확인할 수 있음
- 즉, 지정된 검증자 이외의 어떤 누구도 이렇게 생성된 서명이 어떤 서명자에 의해 생성된 서명인지 알아낼 수 없음

## 메시지 복원형 전자서명
- 서명자가 자신의 개인키를 이용하여 메시지를 암호화하여 전송하면 검증자가 서명자의 공개키를 이용하여 서명된 암호문을 복호화하여 그 결과가 일정한 규칙을 만족하는 의미 있는 메시지가 되는지 확인함으로써 서명을 검증함
- 장점 : `공개키 암호방식`을 이용하므로 별도의 전자서명 프로토콜이 필요하지 않음
- 단점 : 메시지를 일정한 크기의 블록으로 나누어 그 각각의 블록에 서명을 해야 하므로 많은 시간이 소요되어 실제로는 사용하지 않음

## 메시지 부가형 전자서명
- 임의의 길이로 주어진 메시지를 해시 알고리즘을 이용하여 일정한 길이로 압축하고, 해시한 결과에 `서명자의 개인키(송신자의 개인키)`를 이용하여 전자서명 한 후 메시지에 덧붙여 전송한다.
- 전자서명의 검증은 수신된 메시지를 해시한 결과와 `서명자의 공개키(송신자의 공개키)`를 이용하여 전자서명을 복호화한 값을 비교함으로써 이루어짐
- 메시지 부가형 전자서명은 메시지 이외의 전자서명을 따로 전송해야 하므로 전송량이 약간 늘어나는 반면, 메시지가 아무리 길어도 단 한 번의 서명 생성과정만이 필요하므로 효율적이어서 실제로 많이 사용되는 방법임

## 특수 전자서명

| 구분 | 설명 |
| :---: | :--- |
| `부인방지 전자서명` | 서명을 검증할 때 반드시 서명자의 도움이 있어야 검증이 가능한 전자 서명 방식 |
| `의뢰 부인방지 서명` | 임의의 검증자가 부인 과정을 수행하지 못하고 특정한 자(ex. 재판관)만이 부인 과정을 수행 |
| `수신자 지정 서명` | 서명의 검증 시 특정 검증자만이 서명을 확인할 수 있도록 하되, 만일 그 서명이 문제가 되는 경우라도 검증자의 비밀서명 생성정보를 노출시키지 않고 제3자에게 서명의 출처를 증명함으로써 분쟁 해결 기능을 제공하는 서명 방식 |
| `은닉 서명`<br>`(블라인드 서명)` | 서명자가 서명문 내용을 알지 못하는 상태에서 서명토록 한 방식 |
| `위임 서명` | 위임 서명자(proxy signer)로 하여금 서명자(original signer)를 대신해서 대리로 섬ㅇ할 수 있도록 구성한 서명 방식 |
| `다중 서명` | 동일한 전자문서에 여러 사람이 서명하는 것 |

## 전자투표 방식별 주요 특징

| 구분 | 투표 장치 | 선거 관리 | 기술적 쟁점 | 내용  
| :---: | :---: | :---: | :---: | :--- |
| `PSEV 방식` | 전자투표기 | 상 | 하 | 이미 정해져 있는 기존 투표소에서 전자투표기를 이용, 투표기록장치를 개표소로 옮겨와 집계 |
| `키오스크 방식`<br>(Kiosk) | 전자투표기 | 중 | 중| 비지정 임의 투표소에서 키오스크를 이용, 투표결과는 자동적으로 집계되어 개표소로 전송됨 |
| `REV 방식` | 모바일, 디지털TV, PC | 하 | 상 | 인터넷을 통해 투표 후 중앙관리센터로 보내져 자동적으로 집계 |

## PKI
- **PKI**는 인증서의 발급, 사용 및 취소와 관련된 서비스를 통해 `기밀성`, `무결성`, `접근제어`, `인증`, `부인방지`의 보안서비스를 제공
- `인증기관`, `등록기관`, `디렉터리 서비스`, `사용자` 등의 요소로 구성

## 전자 인증서
- 전자 인증서는 `사용자의 공개키`와 `사용자 ID 정보`를 결합한 후 `인증기관`이 서명한 문서
- 인증기관(CA)은 자신의 `개인키`를 사용하여 전자서명을 생성하여 인증서에 첨부하며, 인증서의 유효성 확인에는 CA의 `공개키`가 사용됨

## 공인인증서 운영을 위한 계층 구조
1. `정책승인기관(PAA)`  
	PKI 전반에 사용되는 정책과 절차를 생성하고 PKI 구축의 루트 CA 역할을 수행
2. `정책인증기관(PCA)`  
	PAA 아래 계층으로 자신의 도메인 내의 사용자와 인증기관(CA)이 따라야 할 정책을 수립하고 인증기관의 공개키를 인증하고 인증서, 인증서 폐지 목록 등을 관리
3. `인증기관(CA)`  
	공개키 인증서를 **발급**하고 필요에 따라 취소함. 공개키를 사용자에게 전달  
	인증서/인증서 취소 목록 등을 보관함
4. `등록기관(RA)`  
	- 사용자와 CA가 서로 원거리에 있는 경우, 사용자와 CA의 중간위치에서 사용자의 인증서 요구를 받고 이를 확인한 후, <U>CA에게 인증서 발급을 요청</U>하고, 발급된 인증서를 사용자에게 **전달**
	- 인증기관을 대신해 사용자의 신분을 확인하고, 발급된 인증서(인증서 효력정지 및 폐지목록) 및 해당 CA 또는 상위기관의 공개키를 사용자에게 전달하는 역할

## 공개키 기반구조(PKI)의 요구사항
1. 비용 및 성능을 고려한 사용자의 편의성 제공
2. 전자상거래에 대한 법적 효력 유지
3. 사용자의 보안 정책 및 관리체계 반영
4. 인증서의 생성, 획득, 취소 및 검증기능
5. PKI 기반의 전자상거래 실체 인증
6. 메시지 무결성 보장 및 변조 검출
7. 메시지 및 전자상거래 관련자의 기밀성 보장
8. 기타의 정보보호 서비스 제공

## PKI 형태별 장단점

| 구분 | 계층적 구조 | 네트워크형 구조 |
| :---: | :--- | :--- |
| 장점 | - 정부와 같은 관료조직에 적합<br>- 인증경로 탐색이 용이함<br>- 모든 사용자가 최상위 CA 공개키를 알고있으므로 인증서 검증 용이 | - 유연하며 실질적인 업무관계에 적합<br>- CA 상호인증이 직접 이루어지므로 인증경로 단순<br>- CA의 비밀키 노출 시 국소적 피해 |
| 단점 | - 최상위 CA에 집중되는 오버헤드 발생<br>- 협동업무 관계에는 부적합<br>-최상위 CA의 비밀키 노출 시 피해 규모 막대함 | - 인증경로 탐색이 복잡함<br>-인증정책 수립 및 적용 어려움 |

## X.509
- S/MIME, IP Security, SSL/TLS, SET에 사용됨
- 공개키 암호 시스템과 디지털 서명에 기반
- 표준문서 RSA를 권장하는 것을 제외하고는 특별한 알고리즘의 사용을 명시하지 않음
- 인증서는 `사용자의 공개키`를 포함하고 신뢰하는 `인증기관의 개인키`로 서명된다.

## X.509 인증서 기본영역
- 버전(Version)
- 일련번호(Serial Number)
- 알고리즘 식별자(Algorithm Identifier)
- 발행자(Issuer)
- 유효 개시시간(Validity From)
- 유효 만기시간(Validity To)
- 주체(Subject)
- 주체 공개키 정보(Subject Public-Key Information)
- 알고리즘(Algorithm)
- 서명(Signature)

## X.509 V2 추가된 영역
- `인증기관 고유 식별자(Issuer Unique ID)`
- `주체 고유 식별자(Subject Unique ID)`

## X.509 V3 추가된 영역 : 확장 영역
- 인증기관 키 식별자(Authority Key ID)
- 주체 키 식별자(Subject Key ID)
- `키 사용 목적(Key Usage)`
- 확장 키 사용(Extended Key Usage)
- CRL 배포지점(Certification Revocation List Distribution Points)
- 주체의 대체 이름(Subject Alternative Names)
- 발행자 대체 이름(Issuer Alternative Names)
- 기본제한(Basic Constraints)
- `소유자 비밀키 유효기간(Private Key Usage Period)` : 전자서명 생성키의 유효기간이 인증서의 유효기간보다 짧은 경우에 전자서명 생성키의 유효기간을 명시
- `발급자 공개키 식별자(Authority Key Identifier)` : 하나의 CA가 여러 개의 키를 보유할 수 있기에 이를 확인하기 위한 기능을 수행
- `인증서 정책(Certificate Policies)` : 인증서를 발급하는 데 적용된 인증기관의 인증서 정책을 나타냄

## 인증서 효력정지 및 폐지 목록
국내 인증서 효력정지 및 폐지 목록 표준은 `X.509 v3`, `RFC 3280`, `전자서명법`을 기반으로 개발됨

## CRL
- 인증기관이 폐기한 인증서 목록
- 폐기된 인증서의 일련번호의 목록에 인증기관이 전자서명을 붙인 것

## CRL 기본 영역
- 버전(Version)
- 서명(Signature)
- 발행자 이름(Issuer name)
- 현재 갱신(Last update)
- 다음 갱신(Next update)
- 폐지된 인증서
- 서명

## CRL의 확장자 영역 : 기본확장자 + 개체확장자
- `기본 확장자`
	- CA 키 고유 번호
	- 발급자 대체 이름
	- CRL 발급 번호
	- 발급 분배점
	- 델타 CRL 지시자
- `개체 확장자`
	- 취소 이유 부호(원인 코드)
	- 명령 부호(정지 지시 코드)
	- 무효화 날짜
	- 인증서 발급자

## 델타 CRL
- CRL의 크기를 줄이는 또 다른 방법을 제공
- 마지막으로 발표한 CRL 이후 새로 취소된 인증서 목록만을 발행

## 간접 CRL
- 해당 인증서 발행 CA가 아닌 다른 개체로부터 CRL이 발행될 수 있게 함
- 여러 인증기관에 의해 발행된 CRL를 하나로 모음

## OCSP(온라인 인증서 상태 프로토콜)
- 인증서 폐기 목록을 주기적으로 갱신해야하는 CRL의 단점을 보완하기 위한 프로토콜
- 사용자가 서버에 접근을 시도하면 인증서 상태 정보를 실시간으로 요청하며, 인증서 상태를 관리하고 있는 서버는 유효성 여부에 관한 응답을 즉시 보냄
- CRL 형식을 따르지 않고 `RFC 2560`을 따름
- OCSP는 특정 CA 기관과 사용계약을 맺어야 하고 사용량에 따라서 추가 비용을 지불해야 함

## OCSP 응답자 서비스를 지원하는 CA를 구성할 때의 작업
1. OCSP 응답 서명 인증서의 인증서 탬플릿 및 발급 속성을 구성한다.
2. 온라인 응답자를 호스팅할 컴퓨터에 대한 등록 권한을 구성한다.
3. Windows Server 2003 기반 CA인 경우 발급된 인증서에서 OCSP 확장을 사용하도록 설정한다.
4. CA의 기관 정보 액세스 확장에 온라인 응답자 또는 OCSP 응답자의 위치를 추가한다.
5. CA에 대한 OCSP 응답 서명 인증서 템플릿을 사용하도록 설정한다.

## OCSP의 서비스
- `ORS` : 온라인 취소 상태 확인 서비스
- `DPD` : 대리 인증 경로 발견 서비스
- `DPV` : 대리 인증 경로 검증 서비스

## 난수의 성질
`재현 불가능성`, `예측 불가능성`, `무작위성`

## 공개키 기반구조(PKI)이 응용분야
`SET`, `S/MIME`, `PGP`

## Kereros
개방된 컴퓨터 네트워크 내에서 서비스 요구를 인증하기 위해 대칭 암호기법에 바탕을 둔 티켓 기반 인증 프로토콜
