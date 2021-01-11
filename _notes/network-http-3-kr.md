---
title: HTTP 완벽가이드 - 3. 확장 및 보안
category: 통신/보안
tags:
  - HTTP
---

## 8장 통합점: 게이트웨이, 터널, 릴레이
## 8.1 게이트웨이
## 8.2 프로토콜 게이트웨이
## 8.3 리소스 게이트웨이
## 8.4 애플리케이션 인터페이스와 웹 서비스
## 8.5 터널
## 8.6 릴레이
## 8.7 추가 정보

## 9장 웹 로봇
## 9.1 크롤러와 크롤링
## 9.2 로봇의 HTTP
## 9.3 부적절하게 동작하는 로봇들
## 9.4 로봇 차단하기
## 9.5 로봇 에티켓
## 9.6 검색엔진
## 9.7 추가 정보

## 10장 HTTP/2.0

### HTTP/2.0 등장 배경
+ HTTP/1.1의 단순성과 접근성 중심 설계로 회전지연(latency)문제 발생
+ 커넥션 하나 당 요청/응답을 하나만 처리 => 병렬/파이프라인 커넥션으로 해결 시도
+ 새로운 프로토콜 모색
  - HTTP 작업그룹의 HTTP-NG 프로젝트 (1997년 7월~)
  - Roy Fielding의 WAKA 프로토콜
  - MS의 S+M(Speed+Mobility) 프로토콜
  - Google의 SPDY 프로토콜 (2009년~)
+ HTTP/2.0 프로토콜 채택
  - 2012년 SPDY를 HTTP/2.0 프로토콜로 채택 
  - 2015년 5월 [공식표준 (RFC 7540)](https://www.rfc-editor.org/rfc/rfc7540.txt) 채택
  - 참조
    * [IETF 산하 HTTPbis 그룹의 HTTP 업데이팅 ](https://http2-explained.haxx.se/ko/part4)
    * [MDN - HTTP의 진화](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP#HTTP2_%E2%80%93_%EB%8D%94_%EB%82%98%EC%9D%80_%EC%84%B1%EB%8A%A5%EC%9D%84_%EC%9C%84%ED%95%9C_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)

### HTTP/2.0 개요
|항목|상세|
|---|---|
|TCP 이용 | - 서버-클라이언트 사이의 TCP 커넥션 위에서 동작 <br> - 클라이언트가 TCP 커넥션을 초기화|
|프레임 이용 | 요청/응답을 여러 바이너리 프레임에 나눠 저장|
|헤더 압축 | HTTP 헤더를 압축해 전송|
| 스트림 전송 | - 각 프레임은 스트림을 통해 전송 <br> - 하나의 스트림이 한 쌍의 요청/응답 처리 <br> - 커넥션 당 여러 스트림 생성 가능하며 혼잡제어/우선순위부여 기능 제공|
| 서버 푸시 | 클라이언트가 요청하지 않은 리소스도 필요 시 능동적으로 제공 가능
| 호환성 | - 요청/응답 메세지 의미는 HTTP/1.1과 동일 <br> - Content-Length: 명칭 `:content-length`로 변경 <br> - 404 Not Found: 상태줄 -> `:status` 헤더에 404 설정|

### HTTP/1.1과 차이점
+ 프레임 (Binary Framing)  
: 모든 메세지는 프레임에 담겨 전송된다
  ![](https://hpbn.co/assets/diagrams/ae09920e853bee0b21be83f8e770ba01.svg)

  - 크기: 헤더(8 Byte) + 페이로드(최대 16383 Byte)
  - 헤더필드
  
    |명칭|길이|상세|
    |---|---|---|
    |R|2 Bit| 의미 미정의 <br> 기본값: 0 <br> 수신자는 값 무시해야|
    |길이|14 Bit| 페이로드의 길이 (프레임 헤더 길이 제외) <br> 타입: unsigned integer|
    |종류|8 Bit| 프레임 종류|
    |플래그|8 Bit| 프레임 종류에 따라 다른 의미|
    |R|1 Bit|의미 미정의 <br> 기본값: 0 <br> 수신자는 값 무시해야|
    |스트림식별자|31 Bit|스트림 식별값 (만들어진 순으로 증가) <br> 0: 커넥션 전체와 연관된 프레임 의미 <br> 홀수: 클라이언트가 초기화 <br> 짝수:서버가 초기화|

  - 종류
    * DATA
    * HEADERS
    * PRIORITY
    * RST_STREAM
    * SETTINGS
    * PUSH_PROMISE
    * PING
    * GOAWAY
    * WINDOW_UPDATE: 스트림이 서로 간섭되지 않도록 [흐름제어](https://developers.google.com/web/fundamentals/performance/http2?hl=ko#%ED%9D%90%EB%A6%84_%EC%A0%9C%EC%96%B4) 시 이용
    * CONTINUATION 
+ 스트림과 멀티플렉싱  
: 스트림은 클라이언트-서버 사이에서 교환되는 프레임들의 독립된 양방향 시퀀스
  ![](https://hpbn.co/assets/diagrams/fa9ac7cba0327c032c5e1b57325496a4.svg)

  - 한쌍의 요청-응답이 전송되고 나면 해당 스트림은 닫힘
  - 한 커넥션에 여러 스트림 생성가능하며 우선순위 가질 수 있음 (우선순위대로의 처리 보장X)
  - 식별값은 나중에 만들어질수록 증가
    * 규칙 위반 시 PROTOCOL_ERROR로 응답해야
    * 식별값 고갈 시 커넥션 다시 맺어야
  - 스트림은 서버/클라이언트가 상대와 협상없이 일방적으로 생성 => 협상 시간 절감
+ 헤더압축  
  - 메세지 헤더를 HPACK 명세대로 압축하고 헤더블록조각들로 쪼개서 전송
  - 수신자는 쪼개진 조각들을 이어 압축을 해제하고 원래 헤더집합으로 복원
  - 압축 콘텍스트는 압축해제 시 변경됨   
    => 수/송신 측이 싱크되려면 항상 압축을 해제하거나 COMPRESSION_ERROR 보내야 
  - [참조: 구글 - HTTP2 헤더압축](https://developers.google.com/web/fundamentals/performance/http2?hl=ko#%ED%97%A4%EB%8D%94_%EC%95%95%EC%B6%95)  
+ ~~서버푸시~~ [(2020.11 크롬에서 제거)](https://www.chromestatus.com/feature/6302414934114304#details)  
  : HTTP/2.0은 하나의 요청에 대한 응답으로 여러 리소스 전송 가능  
  => 링크된 리소스 함께 제공해 트래픽/회전지연 감소 가능 
  - 푸시 전에 클라이언트에게 `PUSH_PROMISE` 프레임 보내서 알려야
  - 클라이언트는 `RST_STREAM` 프레임 보내 거절 가능 
  - 클라이언트는 `SETTINGS` 프레임 보내 서버 푸시 비활성화 가능 (값 = 0)
  - 주의점
    * 프록시가 서버 푸시의 추가 리소스 전달 안 할 수 있음
    * 프록시가 추가 리소스를 마음대로 전달할 수 있음 (서버가 전달하지 않아도)
    * 서버는 1) 안전하고 2) 캐시 가능하고 3) 본문 없는 요청에 대해서만 푸시 가능
    * 푸시 리소스는 클라이언트의 명시적인 요청과 연관돼야
    * `PUSH_PROMISE` 프레임은 원 요청으로 만들어진 스트림을 통해 전송됨
    * 클라이언트는 푸시 리소스가 Same-origin 인지 검사해야

      > [동일출처(same-origin)](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)  
      : URL의 프로토콜, 호스트, 포트가 모두 동일해야 동일출처로 인정 

### 알려진 보안 이슈
+ 중개자 캡슐화 공격 (Intermediary Encapsulation Attcks)  
  : HTTP/2.0은 헤더필드 값을 바이너리로 인코딩해 어떤 문자든 사용 가능  
  => 프록시 HTTP/1.1 메세지로 번역 시 문제 발생 가능
+ 커넥션 유지로 인한 개인정보 누출  
  : HTTP/2.0은 회전지연 줄이기 위해 커넥션을 오래 유지  
  => 이전 브라우저 사용자의 정보 유출 가능

### 성능이슈
+ HTTP/1 보다 적은 수의 TCP 커넥션 생성하므로 패킷손실률이 2% 이상인 환경에서는 성능이 더 낮음
+ [참조: HTTP2의 HOL(Head of Line) 문제](https://http3-explained.haxx.se/ko/why-quic/why-tcphol)

# 3부 식별/보안

## 11장 클라이언트 식별과 쿠키

### 개별 접촉
: HTTP는 익명으로 사용되며 모든 요청이 독립적/일회성인 무상태(stateless) 요청   
=> 개인화(맞춤추천, 세션추적, 사용자정보 저장)에 추가 처리 필요  

### HTTP 헤더
: 사용자 정보 전달하는 대표적 헤더
|명칭|헤더타입|상세|한계|
|---|---|---|---|
|From|요청|사용자 이메일 주소|확실한 식별 어려움|
|User-Agent|요청|사용자 브라우저/OS 정보|확실한 식별 어려움|
|Referer|요청|링크 타기 전 근원 페이지|확실한 식별 어려움|
|Authorization|요청|사용자명+패스워드|보안취약|
|Client-ip|확장(요청)|클라이언트 IP주소<br>(함수로 취득 가능)|확실한 식별 어려움 <br>예) 여러 사용자가 한 기기 사용하는 경우<br> 유동IP, NAT방화벽, 프록시 사용하는 경우|
|X-Forwarded-For|확장(요청)|클라이언트 IP주소|
|Cookie|확장(요청)|서버가 생성한 ID라벨|

### 뚱뚱한 URL (Fat URL)
+ 정의
  - URL의 처음이나 끝에 사용자 식별정보값 추가해 제공하는 방식  
  - ex) www.test.kr/wishlist/ref=asia/01-124035-21098838
+ 문제점
  - URL의 비가독성 증가해 사용자에게 혼란 초래
  - 개인정보 유출 가능
  - URL이 변경되므로 캐시 이용 불가 => 서버 부하 증가
  - 타 사이트 접근하거나 특정 URL 호출 시 이탈 발생
  - 세션 간 정보 지속 불가 (URL 따로 저장하지 않는 한)

### 쿠키
+ 특성
  - 사용자 식별 및 세션 유지에 사용되는 가장 일반적인 방식
  - 모든 브라우저가 지원
  - 캐시와 충돌 가능성이 있어 쿠키의 내용물은 캐싱 안 됨
  - 리스트 (이름=값) 형태로 정보 저장  
  - ex) Cookie: name="Jane Doe"; phone="123-1233"
+ 종류

  |명칭|상세|용도|
  |---|---|---|
  |세션쿠키<br>(session cookie)|한 브라우저 세션 동안 존재<br>파기시점 설정: Discard, Expires, Max-Age 파라미터|사이트 탐색 시의 설정/선호 저장|
  |지속쿠키<br>(persistent cookie)|브라우저 닫거나 기기 재시작 후에도 잔존|자주 방문하는 사이트, 로그인/설정 정보 등 유지|

+ 쿠키 관리 (클라이언트 측 상태, HTTP State Mng Mechanism)
  - 사이트A 방문 시 서버가 사용자에게 식별값을 할당
  - 할당값을 Set-Cookie 등의 응답헤더에 기술해 사용자에게 전달
  - 브라우저가 전달 받은 쿠키값을 쿠키DB에 저장
  - 사이트A 재방문 시 쿠키DB의 쿠키값을 Cookie 헤더에 기술해 서버에 전송
  - 보통 사이트마다 2~3개의 쿠키를 전송 => 성능저하 방지 
+ 세션추적
  - 사이트에서 여러 트랜잭션 만드는 사용자 추적에 사용
  - 과정 예시
    * 브라우저가 사이트의 루트 페이지를 요청
    * 서버가 전자상거래 URL로 리다이렉트
    * 클라이언트가 리다이렉트 URL로 요청
    * 서버가 2개의 세션쿠키 기술한 응답을 전송 + 사용자 전용 URL 생성해 리다이렉트 (뚱뚱한 URL)
    * 클라이언트가 세션쿠키 2개 첨부해 사용자 전용 URL 요청   
    * 서버가 쿠키 2개 더 첨부하고 home.html로 리다이렉트
    * 클라이언트가 쿠키 4개 첨부해 home.html 요청
    * 서버가 콘텐츠 전송
+ 쿠키 캐싱
  - 캐싱 시 이전 사용자 쿠키 할당 및 개인정보 누출 가능
  - 원칙

    |원칙|적용예시|
    |---|---|
    |캐시 불가 문서 표시 | Cache-Control: no-cache="Set-Cookie"|
    |캐시 가능 문서 표시 | Cache-Control: public|
    |Set-Cookie 헤더 제거하거나 재검사 강제  | Cache-Control: must-revalidate, max-age=0|
    |Set-Cookie 헤더 있는 이미지 재검사 강제 | Cache-Control: must-revalidate, max-age=0|

+ 보안
  - 비할성화 가능
  - 개인정보 및 브라우징 습관 추적 가능
  - 1998년 미 재무성 컴퓨터 사고 자문단은 쿠키의 위험성이 과대됐다고 평가

### EU 개인정보보호법(GDPR, General Data Protection Regulation) 관련
: 2018년 EU의 개인정보보호법(GDPR)에서 쿠키를 개인정보로 인정  
  => 수집 시 명확한 목적 제시 및 명시적 동의 필요  
+ 유효한 동의확보 방법 (프랑스 CNIL 가이드라인)
  - 사용자가 자유롭고 구체적이며 충분한 정보에 입각해 명확한 긍정 의사를 명시적으로 표시한 경우만 유효
    * 묵시적 동의 인정 불가 (예: 브라우저의 이용 설정, 사이트 이용 지속)
    * 일반 이용약관에 대한 포괄적 동의 인정 불가
  - 사전에 의사 표기된 동의박스 이용 불가
  - 쿠키수집 비동의 시 접근 막는 쿠키월(cookie walls) 적용 불가
  - 용이하고 사용자 친화적인 동의 철회 솔루션 필요
  - 쿠키 및 추적도구 적용하는 이해관계자의 사용자 동의확보 증명 메커니즘 항시 구현 의무
  - 동의 요청 전 안내 필요 사항
    * 데이터 컨트롤러의 신원
    * 데이터 처리 활동 목적
    * 동의 철회 권리
+ 동의 의무 면제
  - 전송오류/데이터손실 여부 추적에 필수거나 원활한 온라인 서비스 위해 적용된 쿠키
    * 사용자가 해당 쿠키의 적용사실 및 목적을 인지 필요   
      => 개인정보처리방침이나 쿠키정책에 적시 필요
  - 사용자가 명시적으로 요청한 서비스 제공 위해, 사용자의 온라인 활동 침해하지 않는 선에서 적용된 사용자 분석용 온라인 추적도구
    * 어떤 유형의 온라인 추적 도구가 적용되는지 사용자에게 고지 
    * 추적 목적을 익명통계 생성으로 엄격히 제한 
    * 데이터 이용 수명 제한
    * 수집된 개인정보의 제3자 양도 및 다른 처리와 병합 불가
+ 참조  
  - [한국인터넷진흥원 해외 개인정보보호 동향 보고서 2019년 9월 2주](https://www.privacy.go.kr/cmm/fms/FileDown.do?atchFileId=FILE_000000000836920&fileSn=0&nttId=9792&toolVer=&toolCntKey_1=)
  - [구글 EU 사용자 동의 정책 관련 도움말](https://www.google.com/about/company/user-consent-policy-help/)
  - [cookiechoices 동의 관련 안내](http://www.cookiechoices.org/intl/ko/)

### 브라우저별 쿠키 관리
+ 구글 크롬
: Cookies라는 SQLite 파일에 쿠키를 저장  

  |필드명|상세|
  |---|---|
  |creation_utc | 쿠키 생성시점 <br> 형식: GMT 기준 초단위|
  |host_key | 쿠키의 도메인 |
  |name | 쿠키 이름 |
  |value | 쿠키 값 |
  |path | 쿠키 관련 도메인 경로|
  |expire_utc | 쿠키 파기시점 <br> 형식: GMT 기준 초단위|
  |is_secure | SSL 커넥션일 경우에만 전송|

+ IE
  - 쿠키를 캐시 디렉토리에 각각 개별파일로 저장
  - 자체 형식으로 저장
  - 도메인당 크기 제한
    * 쿠키 수: 50 -> 180 (2018년 6월 이후 버전)
    * 쿠키헤더: 4KB 
    * 참조: [MS 공식문서](https://docs.microsoft.com/ko-kr/internet-explorer/kb-support/ie-edge-faqs)
  - 형식: 여러 행으로 기술 
    * 1행: 쿠키명 (Name)
    * 2행: 쿠키값 (Value)
    * 3행: 도메인, 경로 (Domain/path)
    * 나머지행: IE만을 위한 추가 데이터 


### 쿠키 명세
+ Version 0 (Persistent Client State)
  - Netscape에서 정의한 최초의 쿠키 표준
  - 서버 전송 시 해당되는 쿠키 값을 Cookie 헤더에 모두 이어 붙여 기술
  - Set-Cookie 헤더 속성
  
    |명칭|필수|상세|예시|
    |---|---|---|---|
    |name=value|필수|서버 방문 시 전달할 이름=값 조합<br>구분문자:세미콜론(;) <br>제외문자: 세미콜론(;), 쉼표(,), 등호(=), 공백( )|Set-Cokie: country=KR|
    |Expires|선택|쿠키 파기일자 <br>기본값:세션 만료시까지<br>구분자:하이픈(-)|Set-Cookie: name=lee; expires=Wednesday, 17-Dec-69 23:03:00 GMT|
    |Domain|선택| 쿠키를 전달 받는 사이트 제한 <br>비교:후방일치 시 허용<br>기본값:응답 생성 서버의 호스트명|Set-Cookie: order=recent; domain="myspace.io"|
    |Path|선택| 쿠키를 전달 받는 사이트 경로 제한<br>비교:전방일치 시 허용<br>기본값: 목표 URL 경로|Set-Cookie: level=25; path=/info|
    |Secure|선택|HTTP SSL 보안 연결 시에만 전송|Set-Cookie: seq_id=412398750; secure|

+ Version 1 (RFC 2965)
  - 속성 더 많은 Set-Cookie2 (RFC2109) 헤더 이용해 Version 0 확장
  - 2011년 RFC 6265 (HTTP State Mng Mechanism) 으로 대체
  - 현재는 사용 안 함 (HISTORIC 상태)
  - 참고: [HTTP 쿠키와 톰캣 버전별 이슈](https://meetup.toast.com/posts/172)

## 12장 기본 인증

### 기본 인증
: HTTP는 사용자 식별을 위해 자체적인 인증 기능을 제공
+ 과정
  - 요청  
    : 클라이언트가 서버에 요청을 전송 
  - 인증요구  
    : 서버가 WWW-Authenticate 헤더와 상태코드 401(Anauthoized) 전송
  - 인증
    : 클라이언트가 Authorization 헤더에 인증정보(사용자명+비밀번호) 기술해 요청 재전송
  - 성공
    : 인증 성공한 경우 서버가 정상 응답 (Authentication-Info 헤더에 추가 정보 기술하기도)
+ 영역
  : 보통 영역(realm)에 따라 다른 사용자 권한 부여  
   => WWW-Authenticate 헤더의 realm　파라미터에 영역명이나 호스트명 기술
   ```
   HTTP/1.0 401 Unahthorized
   WWW-Authenticate: Basic realm="Company Spiral"    
   ```
+ 인코딩
  : 클라이언트가 Authorization 응답 시 인증정보를(사용자명:비밀번호) Base-64로 인코딩해 전송
+ 프록시
: 기본인증과 헤더명 및 상태코드만 다르며 같은 과정 거침
  
  |웹서버|프록시서버|
  |---|---|
  |WWW-Authenticate / 401| Proxy-Authenticate|
  |Authorization| Proxy-Authorization|
  |Authentication-Info| Proxy-Authentication-Info|

### 기본인증의 보안결함
+ 인증정보 쉽게 디코딩 가능
+ 인증정보 가로채서 서버 접근 가능 (=재전송 공격)
+ 사용범위 제한해도 동일 인증정보 사용하는 타 사이트 해킹 가능성 존재
+ 요청정보 수정되거나 프록시/중개자 개입된 경우 정상동작 보장 불가 
+ 가짜서버/게이트의 위장에 취약
=> SSL 등 암호화된 전송기술 연계하거나 다이제스트 인증 사용해야

## 13장 다이제스트 인증
### 13.1 다이제스트 인증의 개선점
### 13.2 요약 계산
### 13.3 보호 수준(Quality of Protection) 향상
### 13.4 실제 상황에 대한 고려
### 13.5 보안에 대한 고려사항
### 13.6 추가 정보

## 14장 보안 HTTP
### 14.1 HTTP를 안전하게 만들기
### 14.1.1 HTTPS
### 14.2 디지털 암호학
### 14.3 대칭키 암호법
### 14.4 공개키 암호법
### 14.5 디지털 서명
### 14.6 디지털 인증서
### 14.7 HTTPS의 세부사항
### 14.8 진짜 HTTPS 클라이언트
### 14.9 프락시를 통한 보안 트래픽 터널링
### 14.10 추가 정보 388