---
title: HTTP 완벽가이드 - 3. 확장 및 보안
category: Study
tags:
  - Network
---

## 8장 통합점: 게이트웨이, 터널, 릴레이

### 8.1 게이트웨이
: 서로 다른 프로토콜/어플레이션 간의 HTTP 인터페이스

+ 기능
  - 어플리케이션이 요청한 리소스 경로 안내
  - 동적 콘텐츠 생성 및 DB에 쿼리 전송 가능
  - HTTP 프로토콜을 다른 프로토콜로 자동 변환 가능
+ 동작 예시
  - 클라이언트가 서버에 질의 요청
  - 서버가 게이트웨이에 질의 전달
  - 게이트웨이가 DB에 질의 전송해 결과 서버로 전달
  - 서버가 클라이언트에게 결과 응답 
+ 표시
  - 웹 게이트웨이는 클라이언트와 서버 측 프로토콜을 빗금으로 구분해 기술
  - ex) 
    * HTTP/FTP 서버 측 FTP 게이트웨이
    * HTTP/CGI 서버 측 어플리케이션 게이트웨이
    * HTTP/NNTP 서버 측 게이트웨이 
    * HTTPS/HTTP 클라이언트 측 보안 게이트웨이

### 8.2 프로토콜 게이트웨이
+ 설정
  - 특정 프로토콜에 대해 설정 시 자동변환 처리 가능
  - HTTP 프로토콜도 게이트웨이 거치도록 설정 가능
    * 브라우저에서 명시적으로 게이트웨이 거치도록 설정 가능
    * 게이트웨이를 대리서버(리버스 프록시)로 설정 가능
+ **HTTP/\*** 서버 측 웹 게이트웨이
  - 클라이언트에서 HTTP 요청이 원 서버로 들어오는 시점에 해당 요청을 다른 프로토콜로 변환
  - ex) `HTTP/FTP` 서버 측 예시
    * `USER` + `PASS` 명령 전송해 서버에 로그인
    * `CWD` 명령으로 적절한 서버 내 디렉터리로 변경 
    * 다운로드 형식을 `ASCII`로 설정
    * `MDTM` 명령으로 문서의 최근 수정시간 취득
    * `PASV` 명령으로 서버에 수동형 데이터 검색 예정 알림
    * `RETR` 명령으로 객체 검색 및 파일전송 요청
    * 제어 채널에서 반환된 포트로 FTP 데이터 커넥션 맺고 
    * 데이터 채널이 열리면 객체가 게이트웨이로 전송됨
+ **HTTP/HTTPS** 서버 측 보안 게이트웨이
  - 사용자의 모든 세션을 암호화 하여 개인정보 보호 및 보안 제공
  - ex) 클라이언트 --HTTP--> 보안게이트웨이 --HTTPS--> 서버
+ **HTTPS/HTTP** 클라이언트 측 가속 게이트웨이
  - 웹 서버의 앞단에서 보이지 않는 인터셉트 게이트웨이나 리버스 프록시 역할 맡음
  - 보안 HTTPS 트래픽 수신 및 복호화해 서버로 HTTP 요청 전송
  - 효율적인 복호화 S/W 내장해 서버 부하 절감 가능 
    * 최근에는 인프라 대부분 SSL 암/복호화 모듈 내장
    * 로드밸런스가 SSL 모듈 내장해 `HTTPS/HTTP` 게이트웨이 역할 하기도
  - but) 게이트웨이-서버 간 네트워크가 안전해야

### 8.3 리소스 게이트웨이
+ 애플리케이션 서버
  - 가장 일반적인 게이트웨이 형태 
  - 목적지 서버와 게이트웨이를 하나의 서버로 결합
  - 클라이언트의 HTTP 요청을 API를 통해 서버 상의 여러 애플리케이션으로 전달
+ CGI 
  - Common Gateway Interface
  - 최초의 서버 확장이자 가장 널리 쓰인 서버 확장 API
  - HTTP 요청에 따라 프로그램 실행, 출력수집, 응답 회신하기 위한 표준 인터페이스 집합
  - 서버와 분리되며 사용자가 내부 처리 알 수 없음 
    * 다양한 언어로 구현 가능
    * 문제가 많은 확장으로부터 서버를 보호
    * 모든 CGI 요청마다 새로운 프로세스를 만들어 부하 발생   
      => `Fast CGI`는 데몬으로 동작해 프로세스 생성/제거로 인한 부하 없음
+ 서버 확장 API
  - Application Programming Interface
  - 웹 개발자가 자신의 모듈/코드를 HTTP와 직접 연결하게 하는 인터페이스
  - ex) MS의 FPSE(FrontPage Server Extension)  
       : 클라이언트에서 전송되는 RPC(원격 프로시져 호출) 명령 인식해 웹 출판 서비스 지원  

### 8.4 애플리케이션 인터페이스
+ 특성
  - 서로 다른 형식의 웹 어플리케이션 통신에 이용
  - HTTP 헤더로 표현 어려운 정보 교환 가능 
+ 웹서비스
  - 각 웹 애플리케이션이 서로 통신하는데 사용할 표준/프로토콜 집합
  - 애플리케이션이 정보 공유하는데 사용하는 새로운 메커니즘 의미
  - HTTP 등 표준 웹 기술 위에서 개발 
  - 과거엔 SOAP 통해 XML로 정보를 교환
  - **현재는 REST 방식 및 JSON 데이터를 주로 사용**
+ [SOAP](https://www.w3.org/TR/soap12/)
  - Simple Object Access Protocol
  - HTTP 메세지에 XML 데이터 담는 방식에 관한 표준
  - W3C에서 유지/관리하는 공식 프로토콜 
+ REST
  - Representational State Transfer
  - Roy Fielding이 제안한 유연하고 가벼운 API 구조

    |조건|상세|
    |---|---|
    |Client-Server| 클라이언트, 서버, 리소스로 구성된 아키텍처 구조|
    |Stateless| 서버에 클라이언트 콘텐츠/상태 저장 안 함|
    |Cache| HTTP의 캐싱 기능 사용 가능해야|
    |Uniform Interface| 애플리케이션 종류와 무관하게 통합적 이용 가능한 표준화된 인터페이스 (핵심사항)|
    |Layered System| 클라이언트-서버 간 상호작용 계층적 조작 가능한 시스템|
    |Code on Demand | 실행 가능한 코드 전송해 클라이언트 기능 확장 가능 (선택사항)|

+ JSON vs XML

  |명칭|공통점|차이점|
  |---|---|---|
  |JSON <br>(JavaScript Object Notation)|- 자기묘사(self describing)적 <br> - 인간/기계 모두 읽을 수 있음| - 엔드태그 불필요 => 더 짧고 처리 빠름 <br> - Array 배열 사용 가능 <br> - 표준 Javascript 함수로 파싱 가능 <br> => 바로 객체화 됨|
  |XML <br> (eXtensible Markup Langugage)|- 계층적 구조 <br> - 여러 프로그래밍 언어 및 <br> XMLHttpRequest 에서 파싱 가능| - 파싱에 XML 파서 필요|

+ 참조
  - [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)
  - [REST와 SOAP 비교](https://www.redhat.com/ko/topics/integration/whats-the-difference-between-soap-rest)
  - [REST by Roy Fielding](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
  - [MDN: JSON으로 작업하기](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)
  - [JSON vs XML](https://www.w3schools.com/js/js_json_xml.asp)

### 8.5 터널
+ 특성 
  - 다른 프로토콜을 HTTP 위에 올릴 수 있음  
    * => 웹 트래픽만 허용하는 방화벽에 다른 트래픽 접근 가능 
  - HTTP가 아닌 트래픽을 HTTP 커넥션으로 전송 시 사용   
    * => HTTP 프로토콜 미지원 애플리케이션에 HTTP 애플리케이션이 접근 가능
  - HTTP의 `CONNECT` 메스드를 사용해 커넥션을 맺음
+ HTTP CONNECT 메소드  
  - 특성
    * 터널 게이트웨이가 목적 서버와 포트에 TCP 커넥션 맺고 
    * 클라이언트-서버 간 데이터를 무조건 전달하도록 요청
    * 모든 서버/프로토콜에 TCP 커넥션 맺기 가능
  - 동작 예시
    1) 클라이언트가 게이트웨이에 `CONNECT` 요청 전송
    2) 게이트웨이는 서버와 TCP 커넥션 생성
    3) 게이트웨이가 HTTP 커넥션 준비 메세지 반환
    4) 클라이언트-게이트웨이 사이에 터널 생성돼 데이터 양방향 전달 (커넥션 종료 시까지)  
  - CONNECT 요청
    * 요청 URI는 호스트 명으로 대체하고 콜론(:)에 이어 포트를 기술
    * 시작줄 다음에는 타 HTTP와 동일하게 요청 헤더 필드 존재 가능
    * 보통 각 행은 CRLF로 종료
    * 헤더 목록 끝은 빈 줄의 CRLF로 종료 
    * ex)
      ```
      CONNECT mysite.co.kr:443 HTTP/1.0
      User-agent: Mozilla/5.0
      ``` 
  - CONNECT 응답
    * `200 응답코드`는 성공을 의미
    * 편의상 응답 사유구절은 `Connection Established`로 기술
    * 일반 HTTP 응답과 달리 `Content-Type` 헤더 불필요 (Byte 그대로 전달)
    * ex) 
      ```
      HTTP/1.0 200 Connection Established
      Proxy-agent: Netscape-Proxy/1.1
      ``` 
+ 데이터 터널링
  - 터널링된 데이터는 게이트웨이에게 비가시적   
    * 패킷 순서/흐름 가정 불가  
    * 게이트웨이는 커넥션 맺는 대로 헤더 포함 모든 데이터를 서버로 전송해야
  - 터널 양 끝단은 언제든 패킷 수신하고 데이터 즉시 전달해야
    * 터널링된 프로토콜이 데이터 의존성 포함하는 경우 문제 발생
    * 터널 한쪽 끝단이 입력 데이터 무시할 수 있음 
    * 터널 한쪽 끝단이 데이터 미소비 시 다른 쪽에서 행/교착상태 발생
  - 클라이언트가 `CONNECT` 요청 후 응답 받기 전에 터널데이터 전송 가능
    * but) 게이트웨이의 처리능력 확인 필요
  - 클라이언트는 인증요구나 200 외의 응답 수신 시 요청 재전송할 수 있어야    
    * TCP 요청의 패킷 외 나머지 영역보다 큰 데이터를 파이프라인으로 전달 금지
    * 파이프라인으로 패킷 받다가 커넥션 끊기는 경우 TCP 리셋 발생 가능
    * TCP 리셋은 게이트웨이로부터 받은 응답 유실 초래 => 통신실패의 원인 인식 불가
  - 터널의 끝단 중 한쪽 커넥션이 끊기면   
    * => 끊기지 않은 쪽에서 전송된 데이터 버려짐
+ SSL 터널링
  - SSL 등 암호화된 프로토콜은 구식 필터링 라우터/프록시에서 처리 불가  
    * 터널 이용 시 SSL 트래픽을 HTTP 메세지에 담겨 방화벽 통과 가능
    * but) 악의적인 트래픽이 유입되는 경로가 될 수도 있음
  - 비교

    |HTTP/HTTPS 게이트웨이| SSL터널링|
    |---|---|
    |클라이언트-게이트웨이 사이에 일반 HTTP 사용 <br> => 낮은 보안|클라이언트-서버 사이에 SSL 세션이 적용됨|
    |프록시가 인증 담당하므로 서버에 SSL 클라이언트 인증 불가|프록시에 SSL 구현 불필요|
    |게이트웨이의 완전한 SSL 지원 필요||

+ 터널 인증
  - 프록시 인증 기능 이용해 클라이언트가 터널 사용할 권한 있는지 검증 가능
  - 동작
    * 클라이언트가 `CONNECT` 요청 전송 시 인증요구를 반환 
    * 클라이언트가 인증정보와 `CONNECT` 요청 재전송
    * 커넥션 및 터널 생성
+ 보안 이슈
  - 통신하고 있는 프로토콜이 올바른 용도로 사용하고 있는지 검증 불가  
  - 예) 게임, 악의적인 텔넷 세션 생성, 사내 이메일 차단 우회  
  => 특정 포트만 터널링 허용해야

### 릴레이
: HTTP 미준수하는 단순한 HTTP 프록시 => 한 번에 1개 홉에 데이터 전달
+ 단순 필터링/진단/콘텐츠 변환 등에 사용
+ 단순 맹목적 릴레이는 Connection헤더 처리 불가  
  => keep-alive 커넥션 이용 시 행(hang) 문제 발생 가능

## 9장 웹 로봇
+ 특성
  - 인간과 상호작용 없이 웹 트랜잭션을 자동/연속적으로 수행하는 S/W 프로그램
  - 자동으로 웹 사이트 탐색, 콘텐츠 수집, 데이터 처리 등을 수행
  - 종류에 따라 크롤러, 스파이더, 웜, 봇 등으로 불림
+ 예시

  |명칭|역할|
  |---|---|
  |주식 그래프 로봇| 매 분 주식시장 서버로부터 데이터 수집해 추이 그래프를 생성|
  |웹 통계조사 로봇| 웹을 탐색하며 페이지 수/크기/언어/미디어타입 등 수집 <br> => 웹의 규모와 진화를 기록|
  |검색엔진 로봇| 발견한 모든 문서 수집해 검색 DB 구축|
  |가격비교 로봇| 온라인 쇼핑몰 페이지 수집해 상품에 대한 가격 DB 구축|

### 9.1 크롤러와 크롤링
+ 크롤러
  - 웹 링크를 재귀적으로 따라가는 로봇
  - 동작: 문서 검색 -> 문서 내의 URL 링크들 파싱 -> 파싱된 링크를 크롤링 목록에 추가 -> 다음 문서 검색
+ 스파이더
  - 검색엔진이 검색 DB를 구축하기 위해 문서 수집 시 사용하는 크롤러
  - 수집된 문서는 후처리를 통해 검색 가능한 DB화
+ 루트집합
  - == 크롤링 위한 시드 목록
  - 크롤러가 방문을 시작하는 URL의 초기 집합
  - 모든 링크를 크롤링하면 관심있는 페이지의 대부분이 수집되도록 설정 필요
  - 예시
    * A, G, S 선택
    * ![루트집합 예시 그림](http://tlog.tammolo.com/static/141830e92a932461f96af6f300bed3a8/eac10/Untitled-06436312-7456-4303-8d7f-9bb0a186518e.png)
+ 순환
  - 문제점
    * 루프 (loops): 크롤러를 루프에 빠뜨려 동일 페이지 수집 및 시간/대역폭 낭비 초래 가능 
    * 서비스 방해: 페이지 반복요청 시 서버 부담 증가 및 사용자 접근 저해 가능 => 법적문제 발생 소지
    * 중복 (dups): 중복 페이지 수집 시 애플리케이션에 쓸모없는 콘텐츠 쌓이게 됨
  - => 크롤러가 방문한 URL 기억해 순환 방지 필요
+ 방문기록 관리
  - 수억 개의 URL은 많은 공간을 차지 (예: 40Byte * 1억 = 약 4GB)
  - URL 관리방법

    |명칭|상세|
    |---|---|
    |트리 & 해시 테이블| 검색트리나 해시테이블 자료구조로 URL을 저장|
    |느슨한 존재 비트맵|해시함수로 변환된 존재 비트 배열 (presence bit array)로 관리 <br> => 저장공간 최소화 (but 느슨한 배열로 충돌가능성 존재)|
    |체크포인트|갑작스런 중단 대비해 방문 목록 저장되었는지 확인|
    |파티셔닝|- 대규모 웹로봇에게 URL의 특정 부분 할당해 처리 <br> - 로봇들은 서로 돕거나 소통하며 활동을 조정|

    > 농장(farm): 분리된 컴퓨터로 존재하는 각 로봇이 동시에 일하고 있는 곳

#### 9.1.6 별칭(alias) & 로봇순환
+ URL이 별칭 가질 수 있으므로 서로 다른 URL이 동일 리소스에 할당될 수 있음
+ 예시
  - 기본 포트: `www.test.kr:80`
  - URL인코딩: `www.test.kr/~kim` = `www.test.kr/%7kim`
  - 태그 링크: `www.test.kr/home.html#title`
  - 대소문자 비구별: `www.test.kr/home.html` = `www.test.kr/HOME.html`
  - 기본 index: `www.test.kr` = `www.test.kr/index.html`
  - IP주소: `www.test.kr` = `217.233.88.99`

#### 9.1.7 URL 정규화
+ 많은 로봇이 URL 정규화를 통해 중복 가능성 제거 시도 
+ 방식
  - 포트번호 명시되지 않은 경우 호스트명에 `:80` 추가
  - 모든 ％ 이스케이핑 문자를 대응되는 문자로 변환
  - `#` 태그 제거 
+ but) 대소문자, index, IP주소 문제는 서버 설정 알아야 대응 가능

#### 9.1.8 파일 시스템 링크 순환
+ 파일 시스템의 심벌릭 링크로 인해 순환 발생 가능
+ 대부분 실수로 생성되나 악의적으로 생성되기도
+ 예시
  - 부모 가리키는 심볼릭 링크를 실제 디렉토리로 착각해 루프 발생
  - `www.test.kr/list.html` -> `www.test.kr/updir/list.html` -> `www.test.kr/updir/updir/list.html`

#### 9.1.9 동적 가상 웹 공간
+ 요청 시마다 새로운 가상 URL과 콘텐츠 만드는 동적 공간 존재 시 루프 발생
+ 예시: 다음달 계속 표시하는 달력 콘텐츠

#### 9.1.10 루프와 중복 피하기
+ 모든 순환 피하는 방법 없으므로 순환 회피 위한 휴리스틱 집합 필요
+ 웹 로봇의 동작기법

  |명칭|상세|
  |---|---|
  |URL 정규화|URL을 표준형태로 변환해 중복 리소스로 연결되는 URL 수집을 일부 회피|
  |너비 우선 크롤링|횡적 검색 우선해 순환에 의한 부작용 최소화|
  |스로틀링|웹 사이트에서 시간당 수집하는 페이지 수를 제한 => 접근/중복 횟수 제한|
  |URL 크기 제한|일정 길이(예:1KB) 넘는 URL 제외 <br> => 긴 URL에 의한 서버실패/순환 방지 <br> but) 수집 못하는 페이지 발생 가능|
  |URL/사이트 블랙리스트|순환 등의 문제 발생하는 사이트를 블랙리스트에 추가해 회피|
  |패턴 발견|URL의 반복요소 검사해 심볼릭 링크 등에 의한 순환을 회피|
  |콘텐츠 지문|페이지의 체크썸을 계산해 중복 콘텐츠 회피 <br> but) 동적 콘텐츠/동작 존재 시 감지 어려움|
  |사람의 모니터링|로봇은 결국 어떤 기법으로도 해결 안 되는 문제 봉착할 수 있음 <br> => 상용 수준의 로봇은 사람이 진단/로깅 등으로 모니터링해야|

### 9.2 로봇의 HTTP
+ 요청 헤더
  - 로봇도 클라이언트처럼 HTTP 명세 지켜야
  - 많은 로봇이 요구사항 적은 HTTP/1.0 요청을 보냄
+ 신원식별 헤더
  - 로봇의 능력/신원/출신 알리는 기본적 헤더를 전송해야
    * 잘못된 크롤러 소유자 탐색 시 유용
    * 서버가 로봇의 콘텐츠 처리 능력 확인 가능 
  - 기본적 신원식별 헤더

    |명칭|상세|
    |---|---|
    |User-Agent| 로봇의 이름 제공|
    |From| 로봇의 사용자/관리자 이메일 주소 제공|
    |Accept| 수신 가능한 미디어 타입|
    |Referer| 요청 URL 포함하고 있던 문서의 URL 제공|

+ 가상 호스팅
  - 가상 호스팅이 널리 퍼져있으므로 요청에 Host 헤더 포함해야 정확한 콘텐츠 탐색 가능
  - 예시: 여러 사이트 운영되는 서버에 index 요청하는 경우
+ 조건부 요청
  - 시간/엔터티 태그 비교해 마지막 수집 시점 이후의 문서만 수집 가능
  - => 수집 콘텐츠량 현저히 감소 가능
+ 응답 다루기
  - 상태코드
    * 일반적인 상태코드와 그들의 분류 다룰 수 있어야 (예: 200, 404)
    * 에러 메세지를 상태코드 200으로 처리하는 경우에 유의
  - 엔터티
    * 로봇은 헤더에 임베딩된 정보로 엔터티 자체의 정보 취득 가능
    * 예: http-equib meta 태그 파싱해 헤더에 덮어쓸 정보를 취득
      ```
      <!-- Refresh 헤더: 1초 후 home.html로 리다이렉트하라-->
      <meta http-equiv="Refresh" content="1; URL=home.html">
      ```
+ User-Agent 타겟팅
  - 웹 관리자들은 로봇의 방문과 요청 예상해 대응 전략 세워야
  - 로봇 접근 시 frame 미지원 오류 등 발생하지 않도록 대응 필요  
    => 저기능 브라우저 및 로봇 지원하는 콘텐츠 개발해야

### 9.3 부적절한 로봇 동작

|명칭|상세|
|---|---|
|폭주|사람보다 빠른 요청 속도로 인해 서버에 과부하 유발 가능|
|오래된 URL|없어진 URL 요청 시 에러로그 및 부하 증가|
|길고 무의미한 URL|순환/오류 등으로 긴 URL 요청 시 서버 처리능력/로그에 부정적 영향|
|민감 데이터 수집|비밀번호/카드정보 등 민감 데이터 수집 시 무시/제거해야|
|동적 게이트웨이 접근|게이트웨이 애플리케이션 콘텐츠 요청 시 처리비용 크게 증가 가능|

### 9.4 로봇 차단하기
+ robots.txt
  - 1994년 제안된 로봇의 동작/접근 제어하는 단순하고 자발적인 기법
  - 정식명칭은 Robots Exclusion Standard, 그냥 `robots.txt`로도 불린다
  - 로봇에게 접근 권한 안내하는 문서로 주로 서버의 문서 루트에 생성함
  - 파일 시스템이 아닌 게이트웨이나 애플리케이션에서 동적으로 생성하기도
  - 표준 따르는 로봇은 크롤링 전에 `robots.txt`를 먼저 확인함 

#### 9.4.1 로봇 차단 표준
+ 특성
  - 임시방편으로 마련된 표준이며 로봇의 접근제어 능력이 불완전하나
  - 주류 업체 대부분과 검색엔진 크롤러가 표준을 지원
+ 버전
  - 대부분의 로봇이 v0.0이나 v1.0을 지원
  - v2.0은 복잡해 채택률 낮음 
   
  |버전|날짜|상세|
  |---|---|---|
  |0.0|1994.06|- Martjin Koster의 로봇 배제 표준 <br> - Disallow 지시자 지원하는 오리지널 robots.tx 메커니즘|
  |1.0|1996.11|- Martjin Koster의 웹 로봇 제어방법 <br> - Allow 지시자 지원 추가된 IETF 초안|
  |2.0|1996.11|- Sean Conner의 로봇차단을 위한 확장 표준 <br> - 정규식/타이밍 정보가 포함되나 널리 지원되지는 않음 <br> - [An Extended Standard for Robot Exclusion](http://www.conman.org/people/spc/robots2.html)|

+ 참조
  - [로봇 배제 표준](https://ko.wikipedia.org/wiki/%EB%A1%9C%EB%B4%87_%EB%B0%B0%EC%A0%9C_%ED%91%9C%EC%A4%80) 
  - [Robots Exclusion Protocol](http://www.robotstxt.org/robotstxt.html) 
  - [robots.txt Maker/Generator](https://w3seo.info/robots-txt) 
  - [로봇 배제 프로토콜 (REP) 표준 공식화 (2019)](https://webmaster-ko.googleblog.com/2020/04/rep.html)/[Eng](https://developers.google.com/search/blog/2019/07/rep-id)
  - [Robots Exclusion Protocol (2020)](https://tools.ietf.org/html/draft-rep-wg-topic-00) 

#### 9.4.2 robots.txt 파일
+ 규칙
  - 한 사이트에 `robots.txt` 존재 시 로봇은 특정 URL 방문 전 반드시 해당 파일 확인해야
  - 호스트명 + 포트 조합에 대한 `robots.txt` 파일은 하나만 존재
  - 가상 호스팅 이용 중이라면 각 가상 docroot 마다 `robots.txt` 파일 존재 가능
  - 웹 마스터는 사이트의 모든 콘텐츠에 대한 차단 규칙이 종합적으로 기술된 `robots.txt` 파일 생성할 책임 가짐
+ robots.txt 요청
  - HTTP GET 메소드로 요청
    * `robots.txt` 파일 존재 시 text/plain 본문으로 반환됨
    * `404(Not Found)` 응답 시 로봇 접근 제한하지 않는 것으로 인식
  - 식별헤더를 통해 신원정보 및 연락처 제공해야
  - ex)
    ```
    GET /robots.txt HTTP/1.0
    Host: www.test.kr
    User-Agent: Slurp/2.0
    Date: Wed Dec 17 18:22:35 EST 1969
    ```

+ 응답코드에 따른 동작

  |응답코드|동작|
  |---|---|
  |2XX (OK)|응답 콘텐츠 파싱해 차단규칙 취득하고 따름|
  |404 (Not Found)|차단규칙 없는 것으로 판단|
  |401 (Unauthorized) <br> 403 (Forbidden))|사이트 접근 완전히 제한된다고 판단해야|
  |503 (Service Unvailable)|리소스 검색을 뒤로 미뤄야|
  |3XX (Redirect)|리소스 발견 시까지 리다이렉트 따라가야|

#### 9.4.3 robots.txt 파일 형식
+ 구성
  - 빈 줄, 주석 줄, 규칙 줄로 구성
  - 규칙 줄
    * HTTP 헤더처럼 기술 (필드:값)
    * 패턴 매칭에 사용 (대소문자 구별X)
    * 레코드로 구분되며 순서대로 적용
  - 레코드
    * 특정 로봇들 집합에 대한 차단 규칙의 집합을 기술 => 로봇별로 규칙 적용 가능
    * User-Agent: 해당되는 로봇 기술
    * Allow/Disallow: 해당되는 URL 기술
  - 주석 줄
    * 파일 내 어디에나 기입 가능
    * `#`로 시작해 `줄바꿈` 문자로 종료 
+ ex) 
  ```
  # slurp과 webcrawler에게 secure 이외 부분 크롤링 허용
  # 나머지 로봇들은 사이트의 어디에도 접근 불가
  
  User-Agent: slurp
  User-Agent: webcrawler
  Disallow: /secure
  
  User-Agent: *
  Disallow:
  ```

+ 접두매칭(prefix matching)
  - Allow/Disasllow 규칙은 규칙 경로의 길이만큼 일치해야 하며 대소문자도 동일해야 
  - % 이스케이핑된 문자는 원래 값으로 복원후 비교되며 대소문자 구별 X
  - 빈 문자열은 모든 값에 매칭
  - `v1.0`은 부모 디렉토리와 무관하게 특정 디렉토리명에 대한 규칙 지정 불가

#### 9.4.4 robots.txt 파싱규칙
+ `robots.txt` 파일 명세 발전해 추가 필드 포함될 수 있음
+ 로봇은 이해 못하는 필드 존재 시 무시해야
+ `v0.0`은 Allow 줄 미지원 => 일부 로봇은 Allow줄 무시할 것
+ 하위 호환성 때문에 한 줄을 여러 줄로 나누기 허용 안 됨

#### 9.4.5 robots.txt 캐싱 & 만료
+ 로봇은 서버 부하 줄이기 위해 주기적으로 `robots.txt` 파일 가져와 캐시해야
+ 캐시된 `robots.txt` 파일은 만료시까지 사용되며 HTTP 캐시 메커니즘 따라 관리됨
+ 많은 크롤러가 HTTP/1.1 클라이언트 아님에 주의 필요
  * 캐시 지시자 이해 못 할 수도
  * 로봇 명세 초안은 Cache-Control 지시자 존재 시 7일간 캐싱 권고하나 실무적으로 너무 긴 편
  * 일부 대규모 웹 크롤러는 활발하게 활동하는 동안에는 `robots.txt` 매일 새로 가져옴

#### 9.4.6 로봇 차단 라이브러리
+ 공개된 펄(Perl) 라이브러리가 몇 가지 존재
+ WWW:RobustRules
  - CPAN 공개 펄 아카이브의 모듈
  - `WWW:RobustRules` 객체는 URL에 접근 금지 확인할 수 있는 메소드 제공
  - `WWW:RobustRules` 객체로 여러 `robots.txt` 파일 파싱 가능

#### 9.4.7 HTML 로봇제어 메타태그
+ 특성
  - 메타 테그는 서버 관리자가 아닌 페이지 저자도 직접 작성 가능
  - 로봇들은 문서 취득할 수 있으나 로봇 차단 태그 존재 시 해당 문서 무시할 것
  - `robots.txt` 표준과 동일하게 준수가 권장되나 강제는 아님
  - 지시어 중복/충돌하면 안 됨
  - name, content 값은 대소문자 구별 X
  - ex) `<meta name="robots" content=dir-list>`
+ 로봇 meta 태그 지시어

  |명칭|상세|
  |---|---|
  |INDEX| 콘텐츠 인덱싱 가능하다고 로봇에게 안내|
  |NOINDEX| 콘텐츠 인덱싱 불가하다고 로봇에게 안내|
  |FOLLOW| 이 페이지가 링크한 페이지 크롤링 가능하다고 로봇에게 안내|
  |NOFOLLOW|이 페이지가 링크한 페이지 크롤링 불가하다고 로봇에게 안내|
  |ALL| INDEX + FOLLOW|
  |NONE| NOINDEX + NOFOLLOW|
  |NOARCHIVE|이 페이지 캐시 사본 만들지 말라고 로봇에게 안내|

+ 검색엔진 meta 태그

  |name|content|상세|
  |---|---|---|
  |DESCRIPTION|<텍스트>|페이지의 짧은 요약 정의 <br> `<meta name="description" content="뫄뫄의 기술블로그에 오신 것을 환영합니다">`|
  |KEYWORDS|<쉼표 목록>|- 페이지 기술하는 키워드 목록 <br> - 구분문자: 쉼표(,) <br> `<meta name="keywords" content="python,statics,analysis">`|
  |REVISIT-AFTER|<숫자 days>|- 재방문 권장 시점 (페이지 변경 예측되므로)<br> - 널리 지원되지는 않음 <br> `<meta name="revisit-after" content="7 days">`|

### 9.5 로봇 에티켓
+ 특성
  - 1993년 마틴 코스터(Martijn Koster)가 웹 로봇 제작자를 위한 가이드라인 목록 작성
  - 참고: https://www.robotstxt.org/guidelines.html
+ 가이드라인

  |항목|세부항목|상세|
  |---|---|---|
  |1) 신원식별|로봇의 신원|HTTP `User-Agent`에 로봇의 명칭/목적/정책URL 등 기술|
  ||기계의 신원|IP주소 안내해 역방향 DNS로 로봇의 소속 식별 가능하게 해야|
  ||연락처|연락 가능한 이메일 주소 제공해야|
  |2) 동작|긴장|로봇에 문의/항의 들어올 수 있음<br>로봇이 노련해지기 전까지 모니터링 필요|
  ||대비|조직의 대역폭 소비하므로 사용 전에 미리 알려야|
  ||감시와 로그|진행상황/동작상태 등 확인 가능하도록 진단/로깅 기능 갖춰야|
  ||학습과 조정|크롤링 시마다 새롭게 배운 것을 반영해 로봇을 조정/개선해야 <br>(예: 함정 피하기)|
  |3) 자가 제한|URL 필터링|이해할 수 없거나 불필요한 데이터의 URL은 무시해야 <br>(예:zip 파일)|
  ||동적 URL 필터링|로봇은 보통 동적 게이트웨이 콘텐츠 수집할 필요 없음 <br>(예:cgi 포함된 URL)|
  ||Accept 헤더 필터링|`Accept` 헤더 사용해 수신 가능 콘텐츠 서버에 알려야|
  ||robots.txt 준수|방문한 웹 사이트의 `robots.txt` 준수해야|
  ||스스로 억제|시간당 요청 수 및 총 접근횟수 제한필요 <br>=> 사이트의 트래픽 소모 방지|
  |4) 문제들 <br>(루프/중복 등)|응답코드 대응|반드시 모든 상태코드 대응 가능해야 <br> 응답코드에 대해 로깅 및 모니터링 필요|
  ||URL 정규화|URL 정규화하여 중복된 접근 방지 필요|
  ||순환 회피|순환 감지/회피 노력 필요 <br> 운영 과정을 피드백 루프화 하여 크롤러 계속 개선해야|
  ||함정 감시|의도적/악의적 순환은 탐지 어려움 <br> 낯선 URL 가진 사이트에 접근 많은 경우 함정일 가능성 높음|
  ||블랙리스트 관리|이상 사이트는 블랙리스트에 추가해 다시는 방문 말아야 <br> (예: 함정/사이클/깨진사이트/로봇 붙잡아두는 사이트)|
  |5) 확장성|공간/규모|웹의 규모, 로봇의 메모리 소모량 등 미리 계산해야|
  ||대역폭|대역폭 사용량 계산 및 모니터링해 최적화 필요 <br> 나가는 요청이 들어오는 요청보다 훨씬 적을 것|
  ||시간|크롤링 소요시간 추정치와 실제치 비교해 문제 파악 필요|
  ||분할|대규모 크롤링 시 여러 기기 필요 <br> 예: 여러 네트워크 가진 대형 멀티프로세서, 협력하는 작은 컴퓨터들 등|
  |6) 신뢰성|사전 테스트|정식 크롤링 실행 전 작은 규모의 테스트 크롤링 진행해봐야 <br> 성능/메모리 소모량 등 분석해 문제 파악 필요 |
  ||체크포인트|여러 종류 실패 발생할 수 있으므로 체크포인트/재시작 기능 설계해야|
  ||실패에 유연|실패 발생해도 계속 동작하도록 설계해야|
  |7) 소통|준비|로봇에 대한 항의 빠르게 대응 가능하도록 준비해야 <br>정책 안내 페이지  생성하고 `robots.txt` 생성방법 안내해야|
  ||이해|항의자에게 로봇의 중요성보다 로봇 차단규칙을 설명하는 것이 효율적 <br> 항의 지속 시 해당 사이트에서 크롤러 제거하고 블랙리스트에 추가해야|
  ||즉시 대응|항의자에게 즉시/전문적 대응하지 않고 로봇 내버려면 상대가 화낼 것|

### 9.6 검색엔진
+ 특성
  - 웹 로봇을 가장 광범위하게 사용
  - 웹 크롤러들이 검색엔진에 문서를 수집해줘서 문서 내 단어에 대한 색인 생성 가능
  - 수십 억의 웹 페이지에 대해 대규모 크롤러가 병렬로 작업 수행 
+ 검색엔진 아키텍처
  - 전 세계 웹 페이지에 대해 풀 텍스트 색인이라고 하는 로컬 DB 생성
  - 풀 텍스트 색인에 크롤러는 웹 페이지 수집해 추가하고 사용자는 질의를 전송
+ 풀 텍스트 색인
  - full-text indexes
  - 일종의 카탈로그처럼 동작하며 특정 단어 포함된 문서 즉시 알려줄 수 있는 DB
  - 색인 생성 후에는 검색할 필요 없음 (결과 이미 정리된 상태므로)
  - ex) 단어 the -> A문서, B문서 
+ 질의 전송
  - 사용자가 폼 입력 시 HTTP GET이나 POST 요청으로 검색엔진 게이트웨이에 전송
  - 게이트웨이 프로그램은 검색 질의 추출해 풀 텍스트 색인 검색용 표현식으로 변환
+ 검색결과 정렬
  - 게이트웨이 애플리케이션은 사용자 위한 질의 결과 페이지 즉석에서 생성
  - 많은 페이이지가 검색어 내포할 수 있으므로 결과에 순위 매기는 알고리즘 필요
  - 검색어와 관련성 높은 순서대로 문서를 표시하기 위해 결과에 점수 매기고 정렬작업 수행 
    *  = 랭킹(relevancy ranking)
    * 관련 알고리즘/팁/기법은 검색엔진의 기밀
    * 크롤링 시 해당 페이지로 들어오는 링크 수 등 통계 수집해 인기도 측정하기도
+ 스푸핑
  - 속임을 이용한 공격의 총칭
  - 사이트가 검색엔진 상위에 표시되도록 검색 엔진 속이는 기술
  - 예시 
    * 많은 키워드 나열된 가짜 페이지 생성
    * 특정 단어에 대해 가짜 페이지 생성하는 게이트웨이 애플리케이션 사용

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

### HTTP 보안 요건
+ 보안성

  |명칭|상세|
  |---|---|
  |서버/클라이언트 인증 | 서버-클라이언트가 위조되지 않았음을 증명 가능|
  |무결성 | 서버-클라이언트의 데이터 위조 방지|
  |암호화 | 서버-클라이언트 간 통신의 도청 방지|

+ 효율성  
  : 저렴한 기기도 이용 가능하도록 알고리즘 속도 빨라야
+ 이식성/편재성(Ubiquity)  
  : 거의 모든 기기에서 지원 가능해야
+ 관리 확장성  
  : 누구든/어디서든 즉각적인 보안통신 가능해야
+ 적응성  
  : 알려진 최신/최선의 보안방식 지원
+ 사회적 생존성  
  : 사회의 문화/정치적 요구 충족 필요

### HTTPS 개요
+ 개요
  - 넷스케이프에서 개척한 가장 인기있는 HTTP 보안 방식
  - 모든 주류 브라우저/서버가 지원
  - 프로토콜이나 자물쇠 모양 보안아이콘으로 확인 가능
+ 보안계층
  - HTTPS는 모든 요청/응답 데이터를 전송 전 암호화
  - HTTP와 TCP 사이에 보안계층을 제공 => 계층 내에서 인/디코딩 처리
  - 보안계층은 SSL(Secure Sockets Layer)이나 TLS(Transport Layer Security) 로 구현하지만 SSL로 통칭   
    => [현재는 TLS가 SSL을 대체 (출처: MDN)](https://developer.mozilla.org/ko/docs/Glossary/SSL)
+ 참고
  - [알아두면 쓸데없는 신비한 TLS 1.3](https://b.luavis.kr/server/tls-1.3)
  - [중학교 수학으로 이해하는 디피-헬만 키 교환 (Diffie-Hellman Key Exchange)](https://tramamte.github.io/2018/07/20/diffie-hellman/) 
  - [안전한 TLS 인증서 배치를 위해 피해야 할 보편적인 실수 5가지](https://www.itworld.co.kr/tags/8242/TLS/125649 ) 

### 디지털 암호학
+ 암호(cipher)  
  : 메세지를 특정 방법으로 인코딩/디코딩해 열람자 제한하는 것    
  - 평문: 암호화 안 된 원본 텍스트
  - 인코딩: 평문 -> 암호문
  - 디코딩: 암호문 -> 평문  
+ 키  
  : 암호의 인코딩/디코딩 동작 설정하는 매개변수
+ 디지털 암호  
  : 디지털 키 값을 이용하는 암호 <-> 기계식 암호 (i.e. 에니그마 암호기계)

### 대칭키 암호법
: 인코딩/디코딩에 동일키 사용하는 알고리즘
+ 방식
  - 발신/수신자가 동일한 비밀키를 공유 (=공유키)
  - 인코딩/디코딩에 해당 비밀키를 사용
+ 알고리즘
  - DES
  - Triple-DES
  - RC2
  - RC4 
+ 특성
  - 열거공격(Enumeration Attack: 가능한 모든 키값 시도)에 취약
  - 키값이 길수록 더 많은 시도 필요 => 보안성 증가  
  - 발신-수신자 쌍마다 공유키 발급 => 관리부담 증가

### 공개키 암호법
: 공개키(공개된 인코딩키) + 비밀키(비공개된 개인 디코딩키) 사용하는 암호화 방식
+ 알고리즘
  - RSA: MIT에서 발명되어 RSA 데이터 시큐리티에서 상용화 
+ 특성
  - 서버의 공개키만 알면 암호화된 메세지 발송 가능
  - PKI(Public-Key Infra)를 통해 기술 표준화  
  - [ 공개키 + 타겟 암호문 + 임의의 평문 + 임의의 평문 인코딩값 ] 알아도 개인키 계산 어려워야 보안성 보장 가능
  - 알고리즘 계산이 느린 편 => 혼성 암호체계 사용 
    * 공개키 암호 채널에서 대칭키 생성 후 대칭키 암호 사용해 효율성 증대

### 디지털 서명
: 메세지의 위변조 여부 입증하는 암호 체크썸
+ 특성
  - 보통 비대칭 공개키 암호화 방식으로 생성
  - 보통 개인키는 소유자만 인지 => 식별수단으로 사용 가능
  - 소유자만 체크썸 계산 가능 => 체크썸을 서명처럼 사용 가능
+ 과정
  - 메세지 정제(=길이값 고정) -> 요약 생성 -> 서명함수에 요약 + 개인키 적용 -> 체크썸 생성
  - 메세지 발송 시 체크썸 추가해 전송
  - 수신노드가 역함수에 체크썸 + 공개키 적용 -> 위변조 확인용 요약 생성 -> 요약 비교 

### 디지털 인증서
: 신뢰 가능한 기관에 의해 서명 및 검증된 신원확인 정보
+ X.509 v3  
  : 일반적으로 사용되는 디지털 인증서 서식 표준 [(RFC5280)](https://tools.ietf.org/html/rfc5280)  
+ 필드
  - 인증서 정보
    * 인증서 포맷 버전 번호
    * 인증서 일련번호
    * 인증서 서명 알고리즘 정보
    * 유효기간
  - 인증대상 정보
    * 대상의 이름(개인/서버/조직 등)
    * 대상의 공개키
    * 대상의 고유 ID (선택)
  - 인증발급자 정보
    * 인증서 발급자 (CA, Certificate Authorities)
    * 인증서 발급자의 고유 ID (선택)
  - 기타 확장 정보
    * 기본제약: 인증대상과 인증기관과의 관계
    * 인증서 정책: 인증서를 승인한 정책
    * 키 사용: 공개키의 사용방식 제한 
  - 디지털 서명  
    : 나머지 모든 필드에 대한 인증기관의 디지털 서명 (디지털 서명 알고리즘으로 생성) 

### HTTPS 세부사항
+ 스킴
  - 접두사: https://
  - 기본포트: 443
+ 보안전송 셋업
  - 443 포트로 TCP 커넥션 수립
  - SSL 핸드셰이크 (보안 매개변수/교환 키 협상하며 초기화)
  - SSL을 통해 암호화 된 메세지 전송
  - SSL 닫힘 통지
  - TCP 커넥션 종료
+ SSL 핸드셰이크
  - 프로토콜 버전 번호 교환
  - 클라이언트가 암호 후보 전송 및 인증서 요구
  - 서버가 선택한 암호 및 인증서를 전송
  - 클라이언트가 비밀정보 전송 
  - 클라이언트-서버가 키 생성 
  - 클라이언트-서버가 서로에게 암호화 시작 알림 
+ 서버 인증서
  - HTTPS로 접속시 브라우저가 서버로부터 자동으로 취득 및 검증
    * 서버 인증서 없는 경우 HTTPS 연결 실패
    * 발급자가 브라우저에 내장된 기관 아닌 경우 신뢰여부 묻는 대화상자 표시
  - 보안 이메일, 인트라넷에서 접근제어 위해 사용하기도
  - 필드 (X.509 v3에서 파생) 
    * 인증서 일련번호
    * 인증서 유효기간
    * 사이트의 조직 이름 
    * 사이트의 DNS 호스트 명
    * 사이트 공개키
    * 인증서 발급자 이름
    * 인증서 발급자 서명 
+ 사이트 인증서 검사

  |명칭|상세|실패 처리|
  |---|---|---|
  |날짜검사 | 인증서 활성화/만료 여부 검사| 오류 표시|
  |서명자 신뢰도 검사 | 인증기관이 신뢰할 만한 기관 목록에 포함되어 있는지 검사| 경고 표시|
  |서명 검사| 공개키 이용해 서명의 체크섬 검증| |
  |사이트 신원 검사| 인증서의 사이트 정보와 현재 사이트 비교 (사이트명, 호스트명)| 사용자에게 알리거나 커넥션 종료|

+ 가상 호스팅  
: 서버 프로그램이 하나의 인증서만 지원하는 경우 경고 표시될 수도  
=> 인증서에 맞는 도메인으로 리다이렉트 후 보안 통신 진행

### 진짜 HTTPS 클라이언트
: SSL은 복잡한 바이너리 프로토콜이므로 직접 구현하지 말고 라이브러리 이용해야 
+ [OpenSSL](https://www.openssl.org)
+ [LibreSSL](https://www.libressl.org/)

### 프록시 통한 보안 트래픽 터널링
: 프록시는 보안 트래픽의 헤더 인식 못하므로 전달 불가  
  => HTTPS SSL 터널링 프로토콜 사용해 서버-클라이언트 사이 터널 수립 (양방향으로 데이터만 전달)
