---
title: HTTP 완벽가이드 - 1. HTTP 기초 및 구조
category: Study
tags:
  - Network
---

# 1부. HTTP 기초 및 구조
- [1부. HTTP 기초 및 구조](#1부-http-기초-및-구조)
  - [1장 HTTP 개관](#1장-http-개관)
    - [TCP 연결](#tcp-연결)
    - [웹의 구성요소](#웹의-구성요소)
  - [2장 URL과 리소스](#2장-url과-리소스)
    - [URL (Uniform Rsc Locator)](#url-uniform-rsc-locator)
    - [단축 URL](#단축-url)
    - [URL 인코딩 규칙](#url-인코딩-규칙)
    - [URI 스킴 목록](#uri-스킴-목록)
    - [URN(Uniform Resource Names)](#urnuniform-resource-names)
  - [3장 HTTP 메시지](#3장-http-메시지)
    - [HTTP 메시지](#http-메시지)
    - [메소드](#메소드)
    - [HTTP 버전번호](#http-버전번호)
    - [상태 코드](#상태-코드)
    - [사유구절 (reason-phrase)](#사유구절-reason-phrase)
    - [헤더](#헤더)

## 1장 HTTP 개관
+ [HTTP (Hypertext Transfer Protocol)](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)
  - 전 세계 웹브라우저, 서버, 웹 어플리케이션의 통신 공용어
  - 신뢰성 있는 데이터전송 프로토콜 (어플리켕션 계층)
  - 전송방식
    * HTTP 요청 (클라이언트 -> 서버)
    * HTTP 응답 (클라이언트 <- 서버)
  - 버전
    https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP

    |버전|상세|
    |---|---|
    |HTTP/0.9 |GET 메서드만 존재 <br> 헤더/버전번호 미지원 => HTML만 전송 가능|
    |HTTP/1.0 |버전번호, 헤더, 메서드 추가 <br> 멀티미디어 처리 가능|
    |HTTP/1.0+ |1990년대 WWW 팽창 <br> keep-alive, 가상호스팅, 프록시 지원|
    |HTTP/1.1 |1997년 초에 공개된 첫 번째 표준 <br> 성능최적화 <- 커낵션 재사용, 청크응답, 파이프라이닝 등|
    |HTTP/2.0 |2010년 전반에 구현된 Google의 SPDY 기반 <br> 2015년에 공식 표준화 <br> 이진 프로토콜, 다중화 프로토콜 (병렬 요청 가능), 오버헤드 제거, 서버푸쉬 => 응답성 증가|

+ 리소스
  - 웹에 콘텐츠를 제공하는 모든 것
  - 종류
    * 정적 파일 ex) txt, jpg, html
    * 동적 콘텐츠 생산 프로그램 ex) stream, search result
  - MIME(Multipurpose Internet Mail Extention) 타입
    * 모든 HTTP객체에 붙는 데이터 포맷 라벨
    * 구성: primary_obj_type/specific_subtype
    * ex) text/html, text/plain, image/jpeg, video/quicktime, application/vnd.ms-powerpoint
  - 통합 자원 식별자 (Uniform Rsc Identifier, URI)  
    * 정보 리소스를 고유 식별 및 위치 지정 
    * 종류
     
      |명칭|구성|특징|
      |---|---|---|
      |[URL (Uniform Rsc Locator)](https://www.ietf.org/rfc/rfc2396.txt)<br>통합 자원 지시자|프로토콜 + 서버주소 + 리소스| 특정 서버 상의 구체적인 리소스 위치 서술|
      |[URN (Uniform Rsc Name)](https://www.ietf.org/rfc/rfc2141.txt)<br>유니폼 리소스 이름| 리소스의 유일한 이름으로 참조 | 리소스 위치 분석 인프라 지원 필요 <br> => 리소스 위치, 접속 프로토콜, 복사 여부 등에 영향X | 

+ 트랜잭션
  - 요청 메시지(명령, URI 등) + 응답 메시지(트랜잭션 결과 등)  
    ![](https://mdn.mozillademos.org/files/13687/HTTP_Request.png)  
    ![](https://mdn.mozillademos.org/files/13691/HTTP_Response.png)
  - 하나의 작업 위해 여러 트랜잭션 발생 가능 
    ex) HTML 뼈대 + 이미지 + 자바 트랜잭션 => 웹 페이지 로드
  - HTTP 요청 메서드
  
    |종류|상세|
    |---|---|
    |GET |클라이언트로 지정 리소스를 전송하라 |
    |PUT |클라이언트가 보낸 데이터를 지정 이름으로 저장하라 |
    |DELETE |지정 리소스를 삭제하라 |
    |POST |클라이언트 데이터를 서버 게이트웨이 어플리케이션으로 전송하라 |
    |HEAD |지정 리소스에 대한 응답 중 HTTP 헤더만 전송하라 |

  - HTTP 응답 상태코드

    |종류|상세|
    |---|---|
    |200 |성공. 문서가 바르게 반환됨 |
    |302 |재전송하라. 다른 위치에서 리소스를 가져가라 |
    |404 |없음. 리소스 검색 실패 |

    * 사유구절은 설명용 => 응답처리에는 상태코드만 사용

+ 메시지
  - 단순한 줄 단위의 문자열
  - 구성
    * 시작줄: 요청-명령, 응답-결과
    * 헤더: 0개 이상 존재, 콜론(:)으로 구분된 필드 쌍, 빈 라인으로 종료
    * 본문: 클라이언트가 요청한 데이터 (문자열, 이진 등 종류 무관)  

    ![](https://mdn.mozillademos.org/files/13827/HTTPMsgStructure2.png)

### TCP 연결
+ TCP (Transmission Control Protocol)
  - 기능
    * 순서에 맞는 전달 => 순서대로 패킷 도착
    * 오류 없는 데이터 전송 => 메시지 유실/손상 발생X
    * 조각나지 않는 데이터 스트림 => 언제/어떤 크기로든 전송 가능
  - 전송계층 
    * 네트워크/HW 특성 감춤 => 범용성
    * 패킷 교환 네트워크 프로토콜의 집합 
    * [OSI 7계층 참고](https://medium.com/harrythegreat/osi%EA%B3%84%EC%B8%B5-tcp-ip-%EB%AA%A8%EB%8D%B8-%EC%89%BD%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-f308b1115359) 
+ IP (Internet Protocol)
  - 네트워크 계층
  - 구성: IP주소 + 포트번호(기본값:80)
  - 변환: DNS(도메인 이름 서비스)를 통해 URL 호스트명을 IP 주소로 변환
  - 통신순서
    * 웹브라우저가 URL에서 호스트명 추출
    * 웹브라우저가 서버 호스트명을 IP로 변환 
    * 웹브라우저가 URL에서 포트번호 추출
    * 웹브라우저-서버의 TCP 연결 성립
    * 웹브라우저가 HTTP 요청 전송
    * 서버가 HTTP 응답 리턴
    * TCP연결 종료 + 웹브라우저가 결과 표시
+ 텔넷
  - 원격 터미널 세션에 주로 사용
  - 컴퓨터 포트로 TCP 커넥션 연결해 서버와 직접 통신
+ netcat
  - 텔넷보다 기능 다양
  - TCP나 UDP 통신 가능
  - 튜토리얼
    * https://www.binarytides.com/netcat-tutorial-for-beginners/
    * https://www.digitalocean.com/community/tutorials/how-to-use-netcat-to-establish-and-test-tcp-and-udp-connections-on-a-vps 

### 웹의 구성요소
+ 프록시
  - 클라이언트-서버 사이의 HTTP 중개자
  - 기능
    * 클라이언트의 모든 요청을 서버에 대신 전달 (보통 요청을 수정)
    * 요청/응답을 필터링 => 보안성 증가
+ 캐시
  - 자주 방문한 페이지 사본을 클라이언트 가까이에 저장하는 HTTP 저장소
  - 기능
    *  효율성 증가
    *  프라이버시 보호
+ 게이트웨이
  - 다른 어플리케이션과 연결된 특별한 웹 서버 
  - 기능
    * 다른 서버 간의 중개자로 동작
    * 클라이언트는 진짜 서버로 인식
    * HTTP 트래픽을 다른 프로토콜로 변환 ex) HTTP <-> FTP
+ 터널
  - HTTP 통신을 단순전달만 하는 특별한 프록시 
  - 기능
    * 두 연결 사이에서 raw 데이터를 열지 않고 그대로 전달
    * 주로 비 HTTP 데이터를 하나 이상의 HTTP 연결을 통해 전송하기 위해 사용 
    * ex) HTTPS 정보를 방화벽 통과시키는 HTTP/SSL 터널
+ 에이전트
  - HTTP 요청 생성하는 웹클라이언트
  - 종류
    * 사용자 에이전트: 웹 브라우저
    * 자동화된 준지능적(semi-intelligent) 에이전트: 스파이더, 웹로봇 등

## 2장 URL과 리소스

### URL (Uniform Rsc Locator)
: 인터넷의 리소스를 가리키는 표준이름 => 위치/접근정보 제시

+ 구성
  - 스킴://서버위치/리소스경로
  - ex)
    
    |프로토콜| URL|
    |---|---|
    |HTTP | http://www.test.kr/board/sub01 |
    |Email | mailto:nobody@bhouse.kr |
    |FTP | ftp://some.ftp.com:2121/user_list.xlsx |
    |RTSP | rtsp://www.kbss.com:554/live/video |

+ 문법
  - <스킴>://<사용자 이름>:<비밀번호>@<호스트>:<포트>/<경로>;<파라미터>?<쿼리>#<프래그먼트>
  - URL 컴포넌트 (9개)
  
    |명칭|상세|기타|
    |---|---|---|
    |스킴 | 서버 접근 위한 프로토콜 | 시작문자:영문자 <br> 구분문자:URL의 첫번째 콜론 <br>대소문자 구별X|
    |사용자명| 리소스 접근 위한 사용자명 | 기본값: anonymous |
    |패스워드 | 사용자의 비밀번호 | 기본값: 브라우저 따라 다름 |
    |호스트 | 서버의 호스트명이나 IP주소| www.test.co.kr <br> 161.28.111.3 |
    |포트 | 호스팅 서버가 열어놓은 포트 번호| 프로토콜 별 기본값 <br> HTTP: 80, HTTPS: 443, FTP: 21|
    |경로 | 슬래시(/)로 구분된 리소스의 서버 상 위치| 경로조각마다 자체 파라미터 사용 가능|
    |파라미터 | 특정 스킴에서 이용되는 입력 파라미터 | 구조:이름-값 쌍 리스트 <br> 구분문자:세미콜론 <br> www.test.co.kr/board;cate=01/list.html;dark=true;sort=date|
    |쿼리 | 스킴에서 어플리케이션(DB, 게이트웨이 등)으로 파라미터 전달 | 구조: URL 끝에 ? 달고 입력 (자유 포맷) <br> www.test.co.kr/search.do?keyword=gain&sort=rate|
    |프래그먼트 | 리소스 조각/일부분 지정 <br> 클라이언트에서만 이용 (서버에 전달 X) | 구조: URL 끝에 # 달고 입력 <br> www.test.co.kr/about.do#contact|

### 단축 URL
+ URL 경로
  - 절대 URL: 리소스 접근에 필요한 컴포넌트 정보 충분
  - 상대 URL 
    * 스킴 & 호스트 생략 
    * 리소스 base 태그에 명시된 기저URL 이용해 절대 URL로 변환
    * 리소스의 기저URL 상속해 절대 URL로 변환
    * 리소스 (http://www.test.co.kr/list.do) + 상대URL (./item1.do)  
      => 절대URL (http://www.test.co.kr/item1.do) 
+ URL 확장 
  - 호스트 명 확장: google -> www.google.com
  - 히스토리 확장: http://www.face -> (방문기록 참조) -> http://www.facebook.com 

### URL 인코딩 규칙
+ URL 문자집합
  - 안전한 전송 + 가독성 보장을 위해 이용 가능 문자 제한
  - 이스케이프 기능: 제한된 문자 전송 후 재인코딩 
+ URL 인코딩
  : 안전하지 않은 문자 => % + ASCII 코드(두 자리 16진수) => 이스케이프 문자

  |문자	| ASCII 코드 | 예시|
  |---|---|---|
  |빈 문자	| 32(0x20) |	http://www.test.co.kr/more%20about.html |
  |%	| 37(0x25)	| http://www.test.co.kr/50%25sale.html |
  |~	| 126(0x7E) |	http://www.test.co.kr/%7EKim |
  |+ | 43(0x2B)| %2B |
  |/ | 47(0x2F)| %2F |
  |= | 61(0x3D)| %3D |

+ 문자 제한
  : URL에서 선점하거나 게이트웨이-프로토콜 혼동 문제 등으로 제한된 문자 존재

  |문자	| 선점/제한|
  |---|---|
  |% | 이스케이프 토큰 |
  |/ | 경로 컴포넌트 구분자 |
  |. | 경로 컴포넌트: 현위치 |
  |.. | 경로 컴포넌트: 부모 위치 |
  |? | 질의 구분자 |
  |# | 프래그먼트 구분자 |
  |; | 파라미터 구분자 |
  |: | 스킴, 사용자명/비밀번호, 호스트/포트 구분자 |
  |$ , + | 선점 |
  |@&= |특정 스킴에서 이용 |
  |{ } | \ ・~ [ ] ' |게이트웨이 등의 전송 에이전트에서 불안전하게 다루므로 제한|
  |< > " | 안전하지 않음. 인코딩 필수|
  | 0x00-0x1F, 0x7F| 인쇄 가능한 US-ASCII 문자 아니므로 제한|
  | > 0x7F| 7비트 US-ASCII 문자 아니므로 제한|

### URI 스킴 목록
+ W3C의 URI 스킴 목록  
  http://www.w3c.org/Addressing/schemes.html
+ IANA의　URI 스킴 목록  
  http://www.iana.org/assignments/uri-schemes

  |스킴|상세|형식|예시|
  |---|---|---|---|
  |http | 하이퍼텍스트 전송 프로토콜 | http://<호스트>:<포트>/<경로>?<질의>#<프래그먼트> | |
  |https | HTTP 커넥션의 양 끝단에 SSL(보안소켓계층) 사용해 암호화| https://<호스트>:<포트>/<경로>?<질의>#<프래그먼트> | |
  |mailto | | mailto:\<RFC-822-addr-spec\> | mailto:whois@goo.gle.co.kr|
  |ftp | 파일전송 프로토콜 <br> 기능:파일 업로드/다운로드/목록조회| ftp://<사용자 이름>:<비밀번호>@<호스트>:<포트>/<경로>;<파라미터>| |
  |rtsp, rtspu | 실시간 스트리밍 프로토콜의 미디어 리소스 식별자 <br> rtspu -> UDP 이용| rtsp://<사용자 이름>:<비밀번호>@<호스트>:<포트>/<경로> <br> rtspu://<사용자 이름>:<비밀번호>@<호스트>:<포트>/<경로>| |
  |file | 호스트 기기에서 바로 접근 가능한 파일표시 <br> 기본 호스트: 로컬 | file://<호스트>/<경로>| |
  |news | 특정 문서/뉴스그룹 접근에 이용 <br> 리소스 위치 정보 미포함 => 뉴스정보 서버 지정 필요| news:\<newsgroup\> <br> news:\<news-article-id\> | news:rec.arts.samtoban|
  |telnet | 대화형 서비스 접근에 이용 | telnet://<사용자 이름>:<비밀번호>@<호스트>:<포트>| telnet://user1:pass1@www.test.com:8080/ |

### URN(Uniform Resource Names)
: 영구적이며 소장위치에 관계없이 정보자원을 식별할 수 있는 식별자

+ 기능적 요건 (RFC1737)
  - 단순한 위치만을 나타내지 않는 범세계적인 이름이어야 한다. 
  - 똑같은 URN이 서로 다른 정 보자원에 할당되지 않도록 고유해야 한다. 
  - URN은 영구적으로 지속되어 평생 하나의 정보자원에 대한 참조로 사용된다. 
  - URN은 네트워크에서 이용가능한 모든 정보자원에 할당되어야 한다. 
  - 스키마는 모든 기존의 네이밍 시스템을 지원해야 한다. 
  - URN의 모든 스키마는 향후 확장성을 가져야 한다. 
  - 조건을 결정하는 것은 이름을 부여하는 권한의 책임으로서 독립적이다. 
  - URN은 변환이 가능해야 한다.
+ 구문
  - urn:<NID>:<NSS>
  - urn:<네임스페이스식별자 > :<네임스페이스상세문자열 > 
+ 문자제한
  - 예약문자 = "%" | "/" | "?" | "#"
  - 기타문자 = "(" | ")" | "+" | "," | "-" | "." | ":" | "=" | "@" | ";" | "$" | "_" | "!" | "*" | "'"
+ 활용사례
  - DOI (디지털 객체 식별자)  
    : 1997년 미국출판협 회(AAP) 주도로 과학·기술·의학분야 전자저널(E-Journal)의 식별과 유통을 위해 고안된 URN 시스템 

    * IDF (국제 DOI재단): DOI 체계를 확립/관장/정책수립, 시스템 서비스 제공자 지정 
    * RA (DOI 등록관리기관): 신청기관에게 DOI 등록 시 필요한 DOI 접두부를 부여 
    * [한국DOI센터](https://www.doi.or.kr/wordpress/about_doi/)
    * [해시넷위키](http://wiki.hash.kr/index.php/URN#cite_note-1)

## 3장 HTTP 메시지

### HTTP 메시지
: HTTP 어플리케이션 간의 주고받은 데이터 블록들
+ 방향
  
  |명칭|상세|
  |---|---|
  |인바운드 | 클라이언트 -> 서버 |
  |아웃바운드| 서버 -> 사용자 에이전트 |
  |업스트림  | 메시지의 발송자 |
  |다운스트림 | 메시지의 수신자 |
  
  > 모든 데이터는 다운스트림 방향으로 흐른다

+ 구조

  |명칭|기능|상세|
  |---|---|---|
  |시작줄 | 어떤 메시지인지 서술| 라인(CRLF)으로 구분된 아스키 문자열 <br> 구분문자:캐리지리턴 + 개행문자 <br> 필드구분: 공백문자|
  |헤더블록 | 속성 서술|형식: 이름 + 콜론 + 공백 + 필드값 + CRLF <br>여러 줄 입력: 문두에 스페이스나 탭 입력 <br>* content-type: 본문 종류 <br> * content-length: 본문크기(Byte) |
  |본문 | 데이터 내포| 예: 텍스트, 이진 데이터, 빈 값 |

+ 문법
  - 요청메시지  
    > <메소드><요청URL><HTTP버전>  
      <헤더>    
      <엔터티 본문>
       
  - 응답메시지 
    > <HTTP버전><상태코드><사유구절>  
      <헤더>    
      <엔터티 본문>
  - HTTP/0.9  
    : 메소드, 요청URL, 엔터티 본문만 존재

### 메소드
: 클라이언트가 요청한 서버의 리소스 처리 동작 

+ 안전한 메소드  
  : 통상 서버에서의 작용이 없는 메소드 ex) GET, HEAD
+ 확장 메소드  
  : 추가적으로 구현한 메소드 (HTTP명세 확장)

  |명칭|상세|
  |---|---|
  |LOCK |리소스 락 => 동시편집 방지 |
  |MKCOL |문서 생성 |
  |COPY |서버 리소스 복사 |
  |MOVE |서버 리소스 이동 |

  * 구현 안 된 경우 501(Not implemented) 코드로 응답

+ 대표적 메소드 

  |명칭|본문 존재|상세|
  |---|---|---|
  |GET |x| 서버에서 리소스 취득| 
  |HEAD |x |서버에서 리소스의 헤더만 취득 <br> => 타입, 존재/변경 여부 확인 가능 | 
  |POST |O |서버가 처리할 입력 데이터 전송 | 
  |PUT |O |서버에 메세지 본문 갱신/생성 + 저장 | 
  |TRACE |x |프록시 등 서버에 도달하는 과정 추적 <br> => 서버가 Loopback 진단 + 요청 메세지 리턴 <br> [** 보안이슈: 쿠키 및 사용자정보 탈취 위험 p.19](https://repository.kisti.re.kr/bitstream/10580/6350/1/2016-056%20웹%20어플리케이션%20취약점%20조치방법.pdf)| 
  |OPTIONS |x |특정 리소스에 대한 서버의 지원 메소드 목록 제공 | 
  |DELETE |x |서버의 리소스 제거 (처리 보장 X)| 

  * 주의: 서버마다 구현 범위 다름


### HTTP 버전번호
: 애플리케이션의 처리능력과 메세지 형식 등 호환성 정보 제공 
+ 형식: HTTP/x.y
+ 주의: x와 y를 분리된 숫자로 취급 (예: 2.22 > 2.3 ; 22 > 3)

### 상태 코드
: 요청 중 발생한 사건을 설명하는 세 자리 숫자

+ 100번대 - 정보

  |코드|사유구절|의미|
  |---|---|---|
  |100 |Continue| 요청 시작 부 수신됐으니 나머지 전송하시오 <br> * 클라이언트가 바로 entitiy 전송하는 경우 응답 생략해야 <br> * 홉 서버가 HTTP/1.1 이전 버전인 경우 프록시는 417(Expectation Failed) 오류 응답해야|
  |101 |Switching Protocols|서버 프로토콜 변경됨 (클라이언트의 upgrade 헤더 나열 중 하나로)|

+ 200번대 - 성공

  |코드|사유구절|의미|포함정보|
  |---|---|---|---|
  |200 |OK| 요청 정상 | 엔티티: 요청된 리소스|
  |201 |Created|리소스 생성 요청 성공 | Location 헤더: 리소스의 구체적 참조 <br> 엔티티: 리소스의 URL|
  |202 |Accepted|요청 수신됨| 엔티티: 요청에 대한 상태 및 추정완료시점|
  |203 |Non-Authoritative Info|엔티티 헤더의 정보가 리소스 사본에서 기원 (리소스 메타정보 검증 실패)|엔티티 헤더|
  |204 |No Content|엔티티 본문 존재X <br> => 이동 없이 문서 갱신||
  |205 |Reset Content|브라우저에게 현재 페이지의 폼 데이터 리셋 요청| |
  |206 |Partial Content|부분/범위 요청 처리 성공|필수:Content-Range, Date헤더, Etag/Content-Location 헤더|

+ 300번대 - 리다이렉션 에러

  |코드|사유구절|의미|
  |---|---|---|
  |300 |Multiple Choices| 여러 리소스 참조하는 URL 요청 시 리소스 목록 리턴 <br> * Location 헤더에 선호 URL 포함 가능|
  |301 |Moved Permanently| 요청한 URL이 이동됨 <br> * Location 헤더에 이동된 현재 URL 포함|
  |302 |Found| 요청한 URL이 임시적으로 이동됨  <br> * Location 헤더에 이동된 URL 포함 <br> [(GET이나 HEAD 요청에 대해서만 응답해야)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302)|
  |303 |See Other| 리소스를 다른 URL에서 가져와야 <br> * Location 헤더에 새 URL 포함|
  |304 |Not Modified|조건부 요청에 대한 응답, 최근에 수정 없음 <br> * 엔티티 본문 없어야|
  |305 |Use Proxy| 프록시로 접근 필요 <br> * Location 헤더에 프록시 위치 포함|
  |306 | - | (현재는 사용 X)|
  |307 |Temporary Redirect| 302와 동일 <br> [(요청 메소드 변경 안 됨 => 더 안전)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/307)|
  |308 |Permanent Redirect | 어플리케이션에 따라 다름 <br> [예) 일반 서버, 구글 드라이브](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/308)|

  * [참고: 302, 307, 308 오류 차이](https://perfectacle.github.io/2017/10/16/http-status-code-307-vs-308/)


+ 400번대 - 클라이언트 에러

  |코드|사유구절|의미|
  |---|---|---|
  |400 |Bad Request| 잘못된 요청 수신|
  |401 |Unauthorized| 인증 필요|
  |402 |Payment Required| [결제 필요 (아직 미도입)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/402)|
  |403 |Forbidden| 요청 거부|
  |404 |Not Found| 존재하지 않는 URL|
  |405 |Method Not Allowed| 지원하지 않는 메소드 (요청에 Allow 헤더 포함된 경우)|
  |406 |Not Acceptrable| 클라이언트가 수신 불가능한 리소스|
  |407 |Proxy Authentification| 프록시 서버의 인증 필요 |
  |408 |Request Timeout| 서버에 설정된 일정 시간 내 요청 완수 실패|
  |409 |Conflict| 리소스 충돌 경고 <br> * 본문에 충돌 설명 포함해야 |
  |410 |Gone| 리소스 제거됨 (원래는 있었지만)|
  |411 |Length Required| 요청에 Content-Length 헤더 필요|
  |412 |Precondition Failed| 조건부 요청 중 일부가 실패|
  |413 |Request Entity Too Large| 요청이 서버의 처리 범위 초과|
  |414 |Request URI Too Long| 요청 URL 길이가 서버의 처리 범위 초과|
  |415 |Unsupported Media Type| 인식/지원 불가능한 엔티티|
  |416 |Requested Range Not Satisfiable| 잘못된 요청 리소스 범위|
  |417 |Expectation Failed| Expect 요청 헤더 내용 지원 불가|

+ 500번대 - 서버 에러

  |코드|사유구절|의미|
  |---|---|---|
  |500 |Internal Server Error|서버에서 처리할 수 없는 에러 발생 |
  |501 |Not Implemented| 지원하지 않는 요청(메소드 등)|
  |502 |Bad Gateway|프록시/게이트웨이가 다음 링크에서 가짜 응답 수신 <br> ex) 부모 게이트웨이 접속 불가한 경우|
  |503 |Service Unavailable|현재는 처리 불가 (나중에는 가능) <br> => 응답에 Retry-After 헤더 포함 가능|
  |504 |Gateway Timeout| 게이트웨이나 프록시에서 타임아웃 발생|
  |505 |HTTP Version Not Supported|지원하지 않는 HTTP 프로토콜 요청|

### 사유구절 (reason-phrase)
: 상태코드의 의미를 '사람이 이해할 수 있게' 설명하는 짧은 문구  
* 규칙 없음  
* 사유구절 달라도 상태코드 같으면 동등한 의미  
   ex) 200 OK = 200 NOT OK

### 헤더
: 클라이언트와 서버가 무엇을 하는지 결정하기 위해 함께 사용  
![](https://mdn.mozillademos.org/files/13821/HTTP_Request_Headers2.png)

+ 일반 헤더  
: 용청/응답 양쪽에 모두 존재 가능

  - 일반 정보 헤더

    |헤더|상세|
    |---|---|
    |Connection | 클라이언트-서버 연결 옵션 설정|
    |Date | 메세지 생성 시각|
    |MIME-Version | 발송자가 사용한 MIME 버전|
    |Trailer chunked transfer | 인코딩된 메세지 끝 부분의 헤더 목록 나열|
    |Transfer-Encoding | 메세지에 적용된 인코딩|
    |Upgrade | 발송자가 업그레이드 원하는 새 버전/프로토콜|
    |Via | 메세지 중개자(프록시, 게이트웨이) 목록|

  - 일반 캐시 헤더  
  : 언제 어떻게 캐시가 되어야 하는지 지시자 제공

    |헤더|상세|
    |---|---|
    |Cache-Control | 메세지 및 캐시 지시자 전달|
    |Pragma | 메세지 및 지시자 전달 <br> (HTTP/1.1에서 Cache-control로 대체)|

+ 요청 헤더  
: 요청에 대한 부가정보(클라이언트의 능력, 선호 등) 제공

  - 요청정보 헤더

    |헤더|상세|
    |---|---|
    |Client-IP | 클라이언트 PC의 IP주소|
    |From | 클라이언트 사용자의 메일주소|
    |Host |요청한 서버의 호스트명과 포트번호 |
    |Referer | 현재의 요청 URL을 포함하고 있는 문서의 URL |
    |UA-Color |클라이언트 기기의 디스플레이 색상 능력 정보|
    |UA-CPU |클라이언트의 CPU 종류 및 제조사|
    |UA-Disp |클라이언트의 디스플레이 능력|
    |UA-OS |클라이언트의 OS명 및 버전|
    |UA-Pixels |클라이언트의 디스플레이 픽셀 정보|
    |User-Agent |요청 보낸 어플리케이션 이름|

  - Accept 관련 헤더

    |헤더|상세|
    |---|---|
    |Accept |클라이언트가 수신 가능한 미디어 종류|
    |Accept-Charset |클라이언트가 수신 가능한 문자집합|
    |Accept-Encoding |클라이언트가 수신 가능한 인코딩|
    |Accept-Language |클라이언트가 수신 가능한 언어|
    |TE |클라이언트가 수신 가능한 확장 전송 코딩(Transfer-Encoding) ??? |

  - 조건부 요청 헤더

    |헤더|상세|
    |---|---|
    |Expect | 요청에 필요한 서버 행동 열거|
    |If-Match | 주어진 엔티티 태그와 일치하는 경우 리소스 취득|
    |If-None-Match |주어진 엔티티 태그와 불일치하는 경우 리소스 취득|
    |If-Modified-Since |주어진 날짜 이후 리소스가 변경된 경우 리소스 취득|
    |If-Unmodified-Since |주어진 날짜 이후 리소스가 그대로인 경우 리소스 취득|
    |If-Range | 문서의 특정 범위에 대해 요청|
    |Range | 리소스의 특정 범위에 대해 요청|

  - 요청 보안 헤더

    |헤더|상세|
    |---|---|
    |Authorization | 클라이언트가 서버에 제공하는 인증 정보|
    |Cookie | 클라이언트가 서버에 토큰 전달 시 이용|
    |Cookie2 | 요청자가 지원는 쿠키 버전 제공|


  - 프록시 요청 헤더

    |헤더|상세|
    |---|---|
    |Max-Forwards|서버 중간에 거칠 수 있는 프록시/게이트의 최대 전달횟수|
    |Proxy-Authorization | 프록시가 서버에 제공하는 인증 정보|
    |Proxy-Connection | 프록시-서버 연걸 시 옵션 설정|

+ 응답 헤더  
: 응답에 대한 부가정보(응답자, 응답자의 능력 등) 제공

  - 응답 정보 헤더

    |헤더|상세|
    |---|---|
    |Age | 응답이 얼마나 오래됐는지|
    |Public | 서버가 특정 리소스에 대해 지원하는 메소드 목록|
    |Retry-After | 리소스가 다시 사용 가능해지는 일시|
    |Server | 서버 어플리케이션의 이름 & 버전|
    |Title | HTML문서의 제목|
    |Warning | 사유구절보다 자세한 경고 메세지|

  - 협상 헤더  
  : 협상 가능한 리소스 정보 전달

    |헤더|상세|
    |---|---|
    |Accept-Ranges | 서버의 수용 범위 형태|
    |Vary | 서버가 확인해야 하는 헤더 목록|

  - 응답 보안 헤더

    |헤더|상세|
    |---|---|
    |Proxy-Authenticate | 프록시에서 클라이언트에 요총한 인증요구 목록|
    |WWW-Authenticate | 서버에서 클라이언트에 요총한 인증요구 목록|
    |Set-Cookie | 서버가 클라이언트 측에 토큰 설정 시 이용|
    |Set-Cookie2 | RFC2965로 정의된 쿠키 ???|

+ Entity 헤더  
: 본문 크기, 콘텐츠/리소스 등 엔티티 관련 광범위한 정보 제공

  - 엔티티 정보 헤더  

    |헤더|상세|
    |---|---|
    |Allow | 엔티티에 적용 간으한 요청 메소드 목록|
    |Location | 엔티티 실제 위치|

  - 콘텐츠 헤더  
  : 콘텐츠의 종류, 크기 등 구체적 정보 제공

    |헤더|상세|
    |---|---|
    |Content-Base | 기저 URL|
    |Content-Encoding |본문의 인코딩|
    |Content-Language | 본문에 가장 적합한 자연어|
    |Content-Length | 본문 길이/크기|
    |Content-Location | 리소스 실제 위치|
    |Content-MD5 | 본문의 MD5 체크썸|
    |Content-Range | 전체 리소스 중 차지 범위|
    |Content-Type | 객체 타입|

  - 엔티티 캐싱 헤더  
  : 언제 어떻게 캐시가 되어야 하는지 지시자 제공

    |헤더|상세|
    |---|---|
    |ETage |엔티티 태그 (리소스의 특정 버전에 대한 식별자) <br> [=> 변하지 않은 경우 캐시 이용, 동시수정 방지](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/ETag)|
    |Expires |유효성 상실 시점|
    |Last-Modified |최종변경일시|

+ 확장 헤더  
: 명세에 정의되지 않은 새로운 헤더 => 의미 몰라도 용인 & 전달해야
