---
title: 파이썬 클린 코드 - 1. 좋은 코드와 객체지향
category: Python
tags:
  - Architecture
---

|항목|상세|
|---|---|
|제목 | [파이썬 클린 코드](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=179504817)|
|저자 | 마리아노 아나야|
|Title | [Clean Code in Python: Refactor your legacy code base](https://www.amazon.com/Clean-Code-Python-Refactor-legacy/dp/1788835832)|
|Author | Mariano Anaya|
|Published | 2018 (2019)|

|챕터|내용|
|---|---|
|1. 소개, 코드 포매팅과 도구|주요 개발환경 설정 도구, 정적분석/문서화/타입검사 도구 소개|
|2. 파이썬스러운(pythonic) 코드|파이썬 기능 및 파이썬다운 코드의 근본 개념 소개|
|3. 좋은 코드의 일반적인 특징|유지보수 쉬운 코드를 위한 일반원칙 및 개념 검토|
|4. SOLID 원칙|객체지향 디자인 원칙을 파이썬에 적용|
|5. 데코레이터 사용한 코드 개선|데코레이터와 코드재사용/책임분리/기능세분화|
|6. 디스크립터로 더 멋진 객체 만들기|디스크립터를 이용한 코드 가독성 및 재사용성 강화|
|7. 제너레이터 사용|제너레이터, 이터레이터, 코루틴(coroutine), 비동기 프로그래밍|
|8. 단위 테스트 & 리팩토링|단위 테스트의 중요성 & unittest, pytest 소개|
|9. 일반적인 디자인 패턴|유지보수 위한 파이썬 디자인 패턴|
|10. 클린 아키텍처|내용 전반 정리|

# 0. 서문 
+ 예제 관련 (p.4-5)
  - 모든 주제는 예제를 통해 설명 (파이썬 3.7 기준, 플랫폼 비종속적)
  - 예제는 대부분 표준 라이브러리 & 순수 파이썬으로 작성
  - 예제 소스
    *  https://github.com/PacktPublishing/Clean-Code-in-Python 
+ 대상 독자
  - 객체지향 원리에 어느 정도 익숙하고 코드를 작성해본 경험이 있는 독자를 가정
  - 파이썬의 관점에서만 보면 이 책은 모든 수준의 개발자에게 적합함
  - 각 장의 내용은 점점 복잡해지는 구조이므로 차례로 학습하기 좋음
  - 이 책은 실용서이며 학술서적 아님
    * 사용된 정의/비고/권장사항은 주의깊게 살펴야
    * 권장사항은 절대적 법칙 아니므로 실용적 측면에서 바라봐야

# 1. 소개, 코드 포매팅과 도구

## 1.1 클린 코드란
+ 의미 (p.18-19)
  - 유일하고 엄격한 정의는 없으며 공식적으로 측정할 방법도 없음
    * 체커(checker)로 문법을 체크하거나, 린터(linter)로 취약점을 찾거나, 정적 분석기로 분석할 수는 있음 
  - 결국 기계나 스크립트가 아닌 전문가(=다른 엔지니어)가 판단하는 것 
    * 코드 가독성 및 유지관리 가능 여부로 결정됨
  - 주의
      > 우리는 코드를 작성하는 것보다 읽는데 훨씬 많은 시간을 소비하므로   
      > 기존 코드 수정하거나 기능 확장 시 해당 코드의 환경을 먼저 읽어야.   
      > 파이썬이라는 언어 자체는 의사소통을 하기 위한 도구일 뿐.  
+ 중요성 (p.19-20)
  - 장점
    * 유지보수성 향상
    * 기술부채 감소  
     : 나쁜 결정이나 적당한 타협의 결과로 발생한 소프트웨어적 결함의 감소 
    * 애자일 개발을 통한 효과적인 프로젝트 진행  
    : 민첩한 개발 + 지속적 배포  
      => 잘 관리된 도로를 달리는 것처럼 **꾸준하고 예측가능한 속도로 진척 가능**
  - 기술부채  
    * 코드는 지금 바꾸는 것보다 미래에 바꾸는 것이 어려움  
      => 부채가 이자를 유발하듯 **코드수정이 점점 더 어려워짐**
    * 기술부채에 대한 비용 지불   
      : 코드 수정하고 리팩토링하기 위해 멈추는 것
    * 프로젝트에 장기적/근본적 문제를 잠재시킴   
      : 언젠가 프로젝트의 **돌발변수를 유발**하게 되어있음

+ 코드 포매팅 (p.20-21)
  - 코딩 표준, 포매팅, 린팅 도구 검사, 코드 레이아웃 설정 등을 넘어서야  
    =>  **품질 좋은 S/W 개발 + 견고하고 유지보수 쉬운 시스템 구축 + 기술부채 회피**
  - `PEP-8` 100% 준수해도 여전히 클린코드 아닐 수 있음
    * [Pythonic은 무엇인가? (PEP 8 정리)](https://codechacha.com/ko/pythonic-and-pep8/)
    * [PEP-8(Python Enhancement Proposal)](https://www.python.org/dev/peps/pep-0008/)
    * [How to Write Beautiful Python Code With PEP 8](https://realpython.com/python-pep8/)

## 1.2 프로젝트 코딩 스타일 가이드
  - 코드가 일관되게 구조화 => 가독성 높아지고 이해 용이
  - 모든 팀원이 표준화된 구조 사용 => 신속한 패턴파악 및 오류감지 가능 
    * ex) Perception in Chess (1973)  
      : 논리적 순서(일관성 & 패턴) 여부에 따른 암기력 차이

### PEP-8의 특징
+ 검색 효율성(Grepability)
  - 표준 준수 시 코드에서 토큰을 `grep`(텍스트 검색 명령어) 가능
  - ex) 키워드 인자(띄어쓰기 X) vs 변수 할당(띄어쓰기 O) 검색

    |명령어 예시|결과 예시|
    |---|---|
    |파라미터 검색<br>`$ grep -nr "location=" .`|./core.py:13: location=current_location,|
    |변수 할당 검색<br>`$ grep -nr "location =" .`|./core.py:10: current_location = get_location()|

+ 일관성
  - 코드 레이아웃, 문서화, 작명규칙 등이 일관됨
  - => **신규 입사자나 경험 적은 개발자도 빠르게 적응 가능**
+ 코드 품질
  - 버그 및 실수 쉽게 파악 가능
  - 코드 품질 도구 이용 시 잠재적 버그 파악 가능 

## 1.3 Docstring과 어노테이션
+ p.23
  - 코드를 문서화하는 것은 주석을 추가하는 것과 다름
  - 주석보다는 문서화를 통해 데이터 타입 설명하고 예제 제공해야
    * 주석 업데이트 누락 시 소스와 괴리 발생
    * 다만, 외부 라이브러리에 오류 있는 경우에는 추가해야
  - 파이썬은 타입이 동적으로 결정되므로 변수/객체 정보 제공해야 이해 쉬워짐
  - 어노테이션 이용 시 `Mypy`와 같은 도구로 타입 힌트 자동 검증 가능 

### Docstring
+ 정의 (p.23-24)
  - `docstring`은 소스 코드에 포함된 문서(documentation)
  - 기본적으로 리터럴 문자열이며 로직의 일부분(함수의 입출력 등)을 문서화하며 설명함
  - 단위 테스트 시에도 활용 가능
+ 출력 (p.25-26)

    |접근방법|상세|
    |---|---|
    |`??` 명령어 실행| `$ dict.update??` |
    |`__doc__` 속성 이용| `$ dict.update.__doc__`|

  - Sphinx(스핑크스) 활용
    * 프로젝트 문서화 위한 기본 골격 자동 생성 가능
    * sphinx.ext.autodoc: 코드에서 `docstring` 가져와 문서화 페이지 자동생성
+ 주의 (p.26)
  - 문서와 프로젝트가 하나가 되도록 도구 오픈해야
    * 모든 팀원의 참여 중요
  - 오픈소스 프로젝트의 경우 
    * 브랜치나 버전별 문서 자동 생성되도록 설정 가능
  - 상세하게 기입해야 효과적
  - but)
    * 코드 변경 시 위키/매뉴얼/README/docstring 등 관련 내용 모두 업데이트 해야
    * 코드가 커지는 이슈 발생
+ 참고
  - [파이썬 (doc) 스타일 가이드에 대한 정리](https://medium.com/@kkweon/%ED%8C%8C%EC%9D%B4%EC%8D%AC-doc-%EC%8A%A4%ED%83%80%EC%9D%BC-%EA%B0%80%EC%9D%B4%EB%93%9C%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A0%95%EB%A6%AC-b6d27cd0a27c)

### 어노테이션
+ 정의
  - 함수 인자로 어떤 값이 와야 하는지 힌트를 주는 것
  - 변수의 예상 타입 및 메타데이터 지정 가능
+ 적용
  - `callable`
    * 콜백이나 유효성 검사 함수로 이용 가능
  - `__annotations__` 속성
    * 어노테이션의 이름과 값을 매핑한 사전 타입의 값
    * 문서생성, 유효성 검증, 타입체크에 활용 가능
  - ex)
    ```python
    # 3.6 버전부터 변수에 직접 설정 가능
    class Point:
        lat: float
        long: float
    ```

    ```python
    class Point:
        def __init(self, lat, long):
            self.lat = lat
            self.long = long

    # 함수에 설정
    def locate(latitude: float, longitude: float)  -> Point:       
    ```
  - cmd)  
      ```m
      $ locate.__annotations__   
      > {'latitude': float, 'longitude': float, 'return': __main__.Point} 
      ```
+ PEP-484
  - 타입 힌팅의 기본원리를 정의
    > "파이썬은 여전히 동적인 타입의 언어로 남을 것. 타입 힌트를 필수로 하자거나 심지어 관습으로 하자는 것은 전혀 아님"
  - 참고
    * https://www.python.org/dev/peps/pep-0484/

### 어노테이션 관련 업데이트
+ Final annotation
  - Java의 `final` 키워드가 데코레이터와 타입힌팅으로 들어옴(타입검사기에서만 반영)
  - ex)
    ```python
    from typing import Final

    name1: Final[str] = 'kim'
    name2: Final = 'lee'
    ```
  - 참고
    * [Python 3.8 업데이트 요약](https://xo.dev/whats-new-in-python-3-8/)
+ typing 모듈의 클래스 빌트인
  - `List`, `Dict` 등 표준 라이브러리 클래스를 바로 타입힌트 가능
  - 타입힌트가 연산속도에 도움이 되지는 않음
  - before)
    ```python
    from typing import Index, Dict

    def show_all(vals: Index[str]):
        for item in vals:
            print(item)
    ```

  - after)
    ```python
    def show_all(vals: list[str]):
        for item in vals:        
            print(item)
    ```
  - 참고
    * [PYTHON 3.9에 등장한 상큼한 8가지 FEATURES](https://tech.madup.com/Python3.9/) 

## 1.4 기본 품질 향상
+ 방법   
  - 개발자의 노력
    * 코드리뷰에 시간 투자
    * 훌륭한 코드가 무엇인지 이해
    * **코드의 가독성** 및 **이해 용이성**에 대해 고민
  - 동료의 코드 리뷰 시 질문
    * 이 코드를 **동료 개발자**가 쉽게 이해하고 따라갈 수 있을까?
    * **업무 도메인**에 대해서 말하고 있는가?
    * 팀에 **새로 합류하는 사람**도 쉽게 이해하고 효과적으로 작업할 수 있을까?
  - 실제 사용패턴 및 코드의 의미/가치 이해에 주력
    * 코드 포매팅, 일관된 레이아웃, 적절한 들여쓰기 검사로는 불충분
    * 높은 수준의 엔지니어에게는 당연한 사항임
  - 모든 검사는 자동화
    * 지속적 통합 빌드(continuous integration build)의 일부로 포함시킬 것
    * 검사 실패 시 빌드도 실패해야 코드 구조의 연속성 확보 가능
    * 팀에서 참고할 객관적 지표로 활용 가능
+ 도구
  - Mypy
    * 프로젝트의 모든 파일 분석해 타입 불일치를 검사
    * 잘못 탐지하는 경우 ignore 주석 추가해 무시 가능
    * ex)
        ```
        type_to_ignore = "sth" # type: ignore
        ```  
  - Pylint
    * `PEP-8` 준수 여부 검사하는 도구
    * `.pylintrc` 파일로 규칙 활정화/비활성화 등 설정 변경 가능
  - Black
    * 자동 코드 포매팅 도구
    * 참고 
      * [Black으로 파이썬 코드 스타일 통일하기](https://www.daleseo.com/python-black/)
