# ADsP :: 과목4. 데이터 분석

- [ADsP :: 과목4. 데이터 분석](#adsp--%ea%b3%bc%eb%aa%a94-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%b6%84%ec%84%9d)
- [1. R 기초와 데이터 마트](#1-r-%ea%b8%b0%ec%b4%88%ec%99%80-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ed%8a%b8)
  - [1.1 R 기초](#11-r-%ea%b8%b0%ec%b4%88)
    - [R의 역사](#r%ec%9d%98-%ec%97%ad%ec%82%ac)
    - [R의 특징](#r%ec%9d%98-%ed%8a%b9%ec%a7%95)
    - [R 개발환경](#r-%ea%b0%9c%eb%b0%9c%ed%99%98%ea%b2%bd)
    - [R 패키지 이용](#r-%ed%8c%a8%ed%82%a4%ec%a7%80-%ec%9d%b4%ec%9a%a9)
    - [R 도움말 이용](#r-%eb%8f%84%ec%9b%80%eb%a7%90-%ec%9d%b4%ec%9a%a9)
    - [R의 데이터 구조](#r%ec%9d%98-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ea%b5%ac%ec%a1%b0)
    - [R 외부 데이터 불러오기](#r-%ec%99%b8%eb%b6%80-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%b6%88%eb%9f%ac%ec%98%a4%ea%b8%b0)
    - [R 기초함수](#r-%ea%b8%b0%ec%b4%88%ed%95%a8%ec%88%98)
    - [사용자 정의 함수](#%ec%82%ac%ec%9a%a9%ec%9e%90-%ec%a0%95%ec%9d%98-%ed%95%a8%ec%88%98)
    - [R 그래픽 기능](#r-%ea%b7%b8%eb%9e%98%ed%94%bd-%ea%b8%b0%eb%8a%a5)
  - [1.2 데이터 마트](#12-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ed%8a%b8)
    - [reshape](#reshape)
    - [sqldf](#sqldf)
    - [plyr](#plyr)
    - [데이터 테이블](#%eb%8d%b0%ec%9d%b4%ed%84%b0-%ed%85%8c%ec%9d%b4%eb%b8%94)
  - [1.3 결측값 & 이상값 처리](#13-%ea%b2%b0%ec%b8%a1%ea%b0%92--%ec%9d%b4%ec%83%81%ea%b0%92-%ec%b2%98%eb%a6%ac)
    - [데이터 탐색](#%eb%8d%b0%ec%9d%b4%ed%84%b0-%ed%83%90%ec%83%89)
    - [결측값 처리](#%ea%b2%b0%ec%b8%a1%ea%b0%92-%ec%b2%98%eb%a6%ac)
    - [이상값 검색](#%ec%9d%b4%ec%83%81%ea%b0%92-%ea%b2%80%ec%83%89)
- [2. 통계 분석](#2-%ed%86%b5%ea%b3%84-%eb%b6%84%ec%84%9d)
  - [2.1 통계학 개론](#21-%ed%86%b5%ea%b3%84%ed%95%99-%ea%b0%9c%eb%a1%a0)
    - [통계학](#%ed%86%b5%ea%b3%84%ed%95%99)
    - [모집단](#%eb%aa%a8%ec%a7%91%eb%8b%a8)
    - [표본 (Sample)](#%ed%91%9c%eb%b3%b8-sample)
    - [표본추출 방법](#%ed%91%9c%eb%b3%b8%ec%b6%94%ec%b6%9c-%eb%b0%a9%eb%b2%95)
    - [실험](#%ec%8b%a4%ed%97%98)
    - [자료의 종류](#%ec%9e%90%eb%a3%8c%ec%9d%98-%ec%a2%85%eb%a5%98)
    - [측정](#%ec%b8%a1%ec%a0%95)
  - [2.2 기초 통계 분석](#22-%ea%b8%b0%ec%b4%88-%ed%86%b5%ea%b3%84-%eb%b6%84%ec%84%9d)
  - [2.3 다변량 분석](#23-%eb%8b%a4%eb%b3%80%eb%9f%89-%eb%b6%84%ec%84%9d)
  - [2.4 시계열 예측](#24-%ec%8b%9c%ea%b3%84%ec%97%b4-%ec%98%88%ec%b8%a1)
- [3. 정형 데이터 마이닝](#3-%ec%a0%95%ed%98%95-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ec%9d%b4%eb%8b%9d)
  - [3.1 데이터 마이닝 개요](#31-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ec%9d%b4%eb%8b%9d-%ea%b0%9c%ec%9a%94)
    - [데이터 마이닝 대표 6기능](#%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ec%9d%b4%eb%8b%9d-%eb%8c%80%ed%91%9c-6%ea%b8%b0%eb%8a%a5)
  - [3.2 분류 분석](#32-%eb%b6%84%eb%a5%98-%eb%b6%84%ec%84%9d)
    - [로지스틱 회귀 모형](#%eb%a1%9c%ec%a7%80%ec%8a%a4%ed%8b%b1-%ed%9a%8c%ea%b7%80-%eb%aa%a8%ed%98%95)
    - [신경망 모형](#%ec%8b%a0%ea%b2%bd%eb%a7%9d-%eb%aa%a8%ed%98%95)
    - [의사결정나무 모형](#%ec%9d%98%ec%82%ac%ea%b2%b0%ec%a0%95%eb%82%98%eb%ac%b4-%eb%aa%a8%ed%98%95)
    - [앙상블 모형](#%ec%95%99%ec%83%81%eb%b8%94-%eb%aa%a8%ed%98%95)
    - [분류분석 모형 평가](#%eb%b6%84%eb%a5%98%eb%b6%84%ec%84%9d-%eb%aa%a8%ed%98%95-%ed%8f%89%ea%b0%80)
  - [3.3 군집 분석](#33-%ea%b5%b0%ec%a7%91-%eb%b6%84%ec%84%9d)
    - [계층적 군집](#%ea%b3%84%ec%b8%b5%ec%a0%81-%ea%b5%b0%ec%a7%91)
    - [k-평균 군집](#k-%ed%8f%89%ea%b7%a0-%ea%b5%b0%ec%a7%91)
    - [혼합분포 군집](#%ed%98%bc%ed%95%a9%eb%b6%84%ed%8f%ac-%ea%b5%b0%ec%a7%91)
    - [자기조직화 지도 SOM(Self-Organizing Map)](#%ec%9e%90%ea%b8%b0%ec%a1%b0%ec%a7%81%ed%99%94-%ec%a7%80%eb%8f%84-somself-organizing-map)
  - [3.4 연관 분석 (=장바구니 분석)](#34-%ec%97%b0%ea%b4%80-%eb%b6%84%ec%84%9d-%ec%9e%a5%eb%b0%94%ea%b5%ac%eb%8b%88-%eb%b6%84%ec%84%9d)
    - [연관규칙](#%ec%97%b0%ea%b4%80%ea%b7%9c%ec%b9%99)

# 1. R 기초와 데이터 마트
* 분석 환경 이해
* R과 R 스튜디오 설치방법 숙지
* R 기본 사용법 숙지
* 데이터 마트 정의 이해
* reshape 패키지 이용한 데이터마트 생성법 이해
* plyr 패키지 이해
* 결측값 처리방법 이해
* 이상값 처리방법 이해

## 1.1 R 기초
: 오랜 역사의 오픈소스 통계분석 도구. 강력한 시각화와 다양한 패키지로 인기 증가 

|R|SAS/SPSS|
|---|---|
| 오픈소스 (무료)| 유료 & 고가|
| 작은 용량| 대용량 |
| 다양한 모듈/패키지 지원| 별도 구매|
| 최근 기술/알고리즘 반영 빠름| 느림/다소 느림|
| 공개 논문/자료 많음| 유료 도서 위주|
| 활발한 공개 커뮤니티| NA (답변X)|

### R의 역사
+ 1976년 벨 연구소에서 통게 프로그래밍 언어 S 자체 개발
+ 1988년 S의 문법 구조/언어에 많은 변화
+ 1933년 뉴질랜드 어크랜드 대학에서 R 개발 (Ross Inaka, Robert Gentleman)
+ 1955년 GPL(GNU Public General License)로 무료 공개

### R의 특징
+ 그래픽 처리  
    - 그래픽 옵션 충실 (쉽고 섬세한 작업 가능)
    - 그래픽 처리 빠름
+ 데이터처리 & 계산 능력  
    - 벡터/행렬 등 다양한 데이터 구조 지원
    - 간단한 개별 데이터 접근 절차 (큰 데이터 핸들링 용이)
+ 패키지  
    - 특정 분석기법에 필요한 함수들
    - 쉽게 추가 가능 (높은 확장성)
    - 최신 이론/기법 공유 용이

### R 개발환경
+ R 코어 https://www.r-project.org/ 

    |화면|기능|
    |---|---|
    |메뉴바|기본 기능|
    |아이콘바|자주 사용하는 기능|
    |R콘솔|명령어 입력(한 줄), 결과 출력|
    |스크립트창|여러 줄 코드 작성 & 실행 (한 줄씩)|

+ R 스튜디오 https://www.rstudio.com/
    - R 사용하는 무료 통합개발환경 (IDE: Integrated Dev Env)
    - 메모리에 저장된 변수 정보 바로 접근 가능 (조회/수정 가능)
    - 스크립트 관리/문서화 용이

        |화면|기능|
        |---|---|
        |스크립트창|여러 줄 코드 작성 & 실행 (한 줄씩)|
        |R콘솔|명령어 입력(한 줄), 결과 출력|
        |환경/히스토리|실행 중인 프로젝트 변수/데이터셋 등, 명령어 실행이력|
        |기타|파일, 플롯, 패키지, 도움말 등|

### R 패키지 이용
+ R 코어 패키지 설치  
     : [패키지툴] 메뉴 -[패키지(를)설치하기]-[CRAN mirror]설정 - [Packages]선택
+ R 스튜디오 패키지 설치  
    - 탭 메뉴 설치  
     : [Packages] 탭 - [Install] - 패키지명 입력 후 [Install]
    - 명령어 설치  
        ```
        install.packages("_패키지명_")
        libarary(_패키지명_)  
        ```
### R 도움말 이용
: 도움말 명령어로 거의 모든 함수에 대한 상세한 설명(인자/옵션/예제 등) 확인 가능

|명령어|예시|
|---|---|
|?|?par|
|help|help( par )|

### R의 데이터 구조
|데이터구조|구성|원소 타입|함수|
|---|---|---|---|
|벡터| 하나 이상의 스칼라값| 동일 타입|c|
|행렬| 행/열로 구성된 2차원 데이터구조|동일 타입|matrix, rbind, cbind|
|데이터프레임|행/열로 구성된 2차원 데이터구조|여러 타입<br>(열은 동일타입으로 구성)|data.frame|

+ 벡터  

    ```
    vector1 <- c(1, 3, 5)
    vector2 <- c("사과", "바나나")
    vector3 <- c(vector1, vector2)

    # vector3 출력결과 
    #
    # [1] "1" "3" "5" "사과" "바나나" 
    # => 문자형 벡터와 합쳐질 경우 문자형으로 타입변경됨
    ```
+ 행렬  

    ```
    matrix1 <- matrix(1:6, nrow=2, byrow=T)
    matrix2 <- matrix(c(1,2,3,4,5,6), ncol=3, byrow=T)
    ```

    > nrow: 행 개수  
    > ncol: 열 개수  
    > byrow: 값이 행부터 채워지도록 하는 옵션  

    ```
    # matrix1 & matrix2 출력결과 (동일)
    #
    #      [,1] [,2] [,3]
    # [1,]   1    2    3   
    # [2,]   4    5    6
    ```

    ```
    row1 <- c(7, 8, 9)
    rbind(matrix1, row1)

    # rbind 출력결과
    #      [,1] [,2] [,3]
    # [1,]   1    2    3   
    # [2,]   4    5    6
    # [3,]   7    8    9
    ```

    ```
    col1 <- c(30, 60)
    cbind(matrix1, col1)

    # cbind 출력결과
    #      [,1] [,2] [,3] [,4]
    # [1,]   1    2    3   30
    # [2,]   4    5    6   60
    ```

+ 데이터프레임

    ```
    x1 <- c(100, 80, 60, 40, 30)
    x2 <- c("A", "B", "C", "A", "B")
    dataframe1 <- data.frame(x1, x2)
    ```


### R 외부 데이터 불러오기
|파일종류|함수|데이터타입|
|---|---|---|
|CSV| read.table<br>read.csv (구분자 설정 생략가능)| 데이터프레임|
|TXT| read.table (구분자/형식 정확해야)| 데이터프레임|
|엑셀(XLS/XSLX)| read.csv (CSV로 변환후) <br><br> RODBC::odbcConnectExcel<br>RODBC::sqlFetch| 데이터프레임|  

+ CSV

    ```
    read.table("C:\\data\\test.csv", header = T, sep = ",")
    read.csv("C:\\data\\test.csv", header = T)
    ```
    > sep: 구분자 설정 옵션   

+ TXT

    ```
    read.table("C:\\\data\\\test.txt") 
    ```
+ Excel

    ```
    library(RODBC)
    conn1 <- odbcConnectExcel("C:\\\data\\\확장자_없는_파일명")
    data1 <- sqlFetch(conn1, "워크시트명")
    close(conn1)
    ```

### R 기초함수
+ 수열생성

    |함수|내용|형식|
    |---|---|---|
    | rep | 반복 |rep(vector, times= , each= )|
    | seq | 순열 |seq(from= , to= , by= ) <br> seq(from=, to=, length= )|

    - req (Replicate) 함수

        ```
        > rep(1, times = 3)
        [1] 1 1 1 

        > rep(1, each = 3)
        [1] 1 1 1 

        > rep(1:2, times = 2)
        [1] 1 2 1 2 

        > rep(c("a", "b"), each = 2)
        [1] "a" "a" "b" "b"  

        > rep(1:2, times=2, each = 2) # each가 times보다 우선순위 높음
        [1] 1 1 2 2 1 1 2 2
        ```

    - seq (Sequence) 함수

        ```
        > seq (1, 3)
        > 1:3
        [1] 1 2 3

       
        > seq (1, 8, by=2)
        > seq (1, 7, by=2)  # 지정한 to값 지나지 않도록 증감
        [1] 1 3 5 7

        seq (1, -4, by = -2)
        [1]  1 -1 -3
        
        > seq (1, 9, length=5) # length로 개수 지정 시 자동증감
        [1] 1 3 5 7 9 

        > seq(1, 9 , length = 4) 
        [1] 1.000000 3.666667 6.333333 9.000000
        ```

+ 수치계산

    |연산자|내용|
    |---|---|
    | + | 더하기 |
    | - | 빼기  |
    | * | 곱하기 |
    | / | 나누기 |
    
+ 행렬계산

    |함수/연산자|내용|형식|
    |---|---|---|
    |t | 전치행렬 |t(A)|
    |%*% |행렬곱 | A %*% B|
    |solve|역행렬 |solve(A)|

    - t (Transpose) 함수

        ```
        > a = c(1, 2, 3)     # 1*3 열벡터 
        [1] 1 2 3

        > acol = t(a)         # 3*1 행벡터 
            [,1] [,2] [,3]
        [1,]  1    2   3
        ```

    - 행렬곱

        ```
        > a = c(1, 2, 3) 
        > A = a %*% t(a)
        > A
            [,1] [,2] [,3]
        [1,]    1    2    3
        [2,]    2    4    6
        [3,]    3    6    9

        > A * 10
            [,1] [,2] [,3]
        [1,]   10   20   30
        [2,]   20   40   60
        [3,]   30   60   90
        ```

    - solve (Inverse Matrix) 함수

        ```
        > mx = matrix(c(1,4,3,9), nrow=2)
        > mx 
            [,1] [,2]
        [1,]    1    3
        [2,]    4    9

        > solve(mx)
                [,1]       [,2]
        [1,] -3.000000  1.0000000
        [2,]  1.333333 -0.3333333
        ```

        ![역행렬](https://t1.daumcdn.net/cfile/tistory/247FAE36565B182818)
        https://rfriend.tistory.com/142


+ 통계연산

    |함수|내용|
    |---|---|
    | mean |평균 | 
    | var |분산 | 
    | sd |표준편차| 
    | sum |합 | 
    | median |중앙값 | 
    | log |자연로그 | 
    | cov |공분산 | 
    | cor |상관계수 | 
    | summary |요약(사분위수/최대/최소/중앙/평균) | 

+ 데이터 핸들링

    ```
    mat3 <- matrix(1:12, nrow=3, ncol=4, byrow=T)
    rownames(mat3) <- c("row1", "row2", "row3")
    colnames(mat3) <-c ("col1", "col1", "col1", "col1")

    mat3[2, 3]
    mat3[, 3]     # all, col3
    mat3[,-2]     # except a col
    mat3["row1", ]  # row by name
    mat3[, 2:3]     # range
    mat3[c(1,2), ]  # rows by index
    ```

+ 반복/조건문
    - for  
    : i 증가 & 괄호 안의 조건 하에서   => 중괄호 구문 반복실행 

        ```
        > sumx = 0
        > for (i in 1:10){
            sumx = sumx + i    # 1, 3, 6, ...
        }    
        > sumx 
        [1] 55
        ```

    - while  
    : 괄호 안의 조건 하에서   => 중괄호 구문 반복실행

        ```
        > x = 1
        > while(x < 3){
            x = x + 1
            print(x)
        }
        [1] 2
        [1] 3
        ```

    - if~ else  
    : 조건문 참/거짓 여부에 따라 구문 실행 

        ```
        > x = 3
        > A = c(1:10)
        
        > if( x %in% A){
            print("TRUE")    # x가 A에 포함되는 경우
        } else{ 
            print("FALSE")   # x가 A에 포함 안 되는 경우
        }
        [1] "TRUE"
        ```

+ 문자열

    |함수|내용|형식|
    |---|---|---|
    |paste|입력값 연결| paste(..., sep = )| 
    |substr|문자열에서 일부 추출| substr(word_vector, start, end)|
    |sub|치환(첫번째)|sub(pattern, replacement, word)|
    |gsub|치환(전부)|gsub(pattern, replacement, word)|

    - paste
        ```
        > paste("It is", 10 , "years old") # 문자열 타입으로 자동변환
        [1] "It is 10 years old"
    
        > paste("It is", 10 , "years old", sep="_") # 기본 구분자는 공백
        [1] "It is_10_years old"
    
        > numbers = 1:3
        > words = c("a", "b")

        > paste(numbers, words)
        [1] "1 a" "2 b" "3 a"
        
        > paste(numbers, words, sep=" to ")
        [1] "1 to a" "2 to b" "3 to a"
        ```

    - substr
        ```
        > substr("BigDataAnalysis", 1, 4)
        [1] "BigD"
	
        > country = c("Korea", "England", "Russia")
        > substr(country, 1, 3)
        [1] "Kor" "Eng" "Rus"
        ```

+ 데이터구조 변환

    |함수|내용|비고|
    |---|---|---|
    |as.data.frame(x)| 데이터 프레임으로 변환|
    |as.list(x)| 리스트로 변환| 벡터/행렬 모두 포함 가능 타입
    |as.matrix(x)| 행렬로 변환| 데이터프레임 변환 시 값들 문자형 데이터로 강제변환|
    |as.vector(x)| 벡터로 변환|
    |as.factor(x)| 팩터로 변환| 범주화 (Levels)
    |as.integer(x)| 정수형 벡터로 변환|소수점 자동 제거|
    |as.numeric(x)| 수치형 벡터로 변환| 변환 불가 값은 NA 출력|
    |as.logical(x)| 논리값 벡터로 변환 | 0 => FALSE <br> 0 아닌 값 => TRUE|

    ```
    > as.factor(c("A", "B", "AB", "O"))
    [1] A  B  AB O 
    Levels: A AB B O

    > as.numeric("foo")
    [1] NA
    > as.numeric(FALSE)
    [1] 0
    > as.numeric(TRUE)
    [1] 1

    ```

+ 날짜 변환

    |함수|내용|비고|
    |---|---|---|
    |Sys.Date()| 현재 날짜|
    |as.Date()| 날짜객체로 변환| format 옵션 <br> (기본: "%Y-%m-%d")|
    |format()| 데이터 포맷 변경| format 옵션문자|
    |as.character()| 문자열로 변환|

    ```
    > as.Date("2019-08-20")
    > as.character(Sys.Date())  # 출력결과 (동일)
    > format(Sys.Date())        # 출력결과 (동일)

    [1] "2019-08-20"    
    ```

    - format 함수 옵션

        |옵션문자|의미|	
	    |---|---|
        |%a|요일|
        |%b|월|
        |%Y|연도(4자리)|
        |%y|연도(2자리)|
        |%m|월(2자리)|
        |%d|일(2자리)|	

        ```
        > as.Date("08/20", format="%m/%d")
        [1] 08/20

        > format(Sys.Date(), '%a')
        [1] "Tue"

        > format(Sys.Date(), '%b')
        [1] "Aug"

        > format(Sys.Date(), '%m')
        [1] "08"
        ```

### 사용자 정의 함수
: function  명령어 이용해 직접 함수 정의 => 복잡한 처리 반복 수행 용이

```
> myfunction = function(a, b){
    x = 1
    for(i in a:b){
        x = x * i
    }
    return (x)   # 생략하거나 list로 결과 리턴 가능
}

> myfunction(2,5)
[1] 120
```

### R 그래픽 기능
+ [산점도 그래프](https://warwick.ac.uk/fac/sci/moac/people/students/peter_cock/r/iris_plots/)  
: x, y 값을 한눈에 살펴볼 수 있게 평면/공간에 점 찍어 표현

    ```
    plot(iris$Petal.Length, iris$Petal.Width, main="Edgar Anderson's Iris Data")
    ```

    ** main: 그래프 제목 설정    

    ![](https://warwick.ac.uk/fac/sci/moac/people/students/peter_cock/r/iris_plots/iris_scatter.png?maxWidth=431&maxHeight=451)

+ [산점도 행렬](https://warwick.ac.uk/fac/sci/moac/people/students/peter_cock/r/iris_plots/#plot)   
: 여러 변수에 대해 각각의 산점도 볼 수 있도록 확장된 산점도 행렬

    ```
    pairs(iris[1:4], main="Edgar Anderson's Iris Data",
        pch=21, bg = c("red", "green3", "blue")[unclass(iris$Species)])
    ```

    ** pch: 그래프 점 모양 설정  
    ** bg: 색 지정

    ![](https://warwick.ac.uk/fac/sci/moac/people/students/peter_cock/r/iris_plots/iris_pairs.png?maxWidth=431&maxHeight=451)

+ [히스토그램](https://www.datamentor.io/r-programming/histogram/)  
: 보통 세로축에 도수 표기, 자료분포 파악 쉬움 => 탐색적 자료분석에 이용

    ```
    hist(airquality$Temp)
    hist(airquality$Temp, prob=T)
    ```

    ** prob=T: 상대도수 이용 설정  
    ** col: 막대 색 지정  
    ** xlab: x축 라벨  
    ** ylab: y축 라벨   
    ** xlim: x축 범위  
    ** ylim: y축 범위  
    
    ![](https://cdn.datamentor.io/wp-content/uploads/2017/11/r-histogram.png)

+ [상자그림](https://www.datamentor.io/r-programming/box-plot/)  
: 자료분포 파악 쉬움 => 탐색적 자료분석에 이용

    ```
    boxplot(airquality$Ozone)
    ```

    ** horizontal: 가로 설정  
    ** border: 경계선 색 지정  
    ** notch: 중앙값 휘어짐 설정  

    ![](https://cdn.datamentor.io/wp-content/uploads/2017/11/r-boxplot-1.png)

## 1.2 데이터 마트
: 데이터 웨어하우스의 전체 데이터 중 **일부**를 **특정 사용자**의 요구/관심에 따라 제공

### reshape
- 데이터 탐색 용이하게 하는 데이터 재정렬 기법  
  * 데이터 구조 column-wise 하게 전환
  * 밀집화와 다르게 데이터가 가진 정보 유지
- melt 단계  
  : id에 있는 변수 기준으로 나머지 변수 표준 데이터화  => 변수명:variable, 값:value
  * melt(data, id=, na.rm =)
  * 옵션  
   : na.rm(결측치 제거)
- cast 단계  
  : 엑셀 피벗팅처럼 자료 변환  
  * cast( data, y ~ x_dimension ~ x_measure) 
  * 옵션  
    : mean, range, margin, subset

```
> install.packages("reshape") # 패키지 설치
> library(reshape) # 패키지 로드

> head(airquality) # 앞단 데이터 확인
> names(airquality) = tolower(names(airquality)) # 소문자 헤더

# melt 단계
# month, day 기준으로 표준화 
> apm = melt(airquality, id=c("month", "day"), na.rm=TRUE)

# 변수별로 분리된 결과 출력
> apm
    month day  variable  value
1       5   1     ozone   41.0
2       5   2     ozone   36.0
3       5   3     ozone   12.0
...
119     5   3   solar.r  149.0
120     5   4   solar.r  313.0
...
567     9  29      temp   76.0
568     9  30      temp   68.0

# cast 단계
# 분리된 날짜별 결과 표시
> a <- cast(apm, day ~ month ~ variable)
> a

# 월별 각 변수 평균값 산출
> m <- cast(apm, month ~ variable, mean)
> m

# 월별 최대/최소값 산출 (_X1:최소 , _X2:최대)
> r <- cast(apm, month ~ variable, range)
> r

# 행/열 소계 산출 옵션
> grandm <- cast(apm, month ~ variable, mean, margins=c("grand_row", "grand_col"))
> grandm

# 특정변수에 대한 서브셋 산출
> sub <- cast(apm, day ~ month, mean, subset=variable=="ozone")
> sub
```

** 밀집화(Aggregation):  
데이터 축소/재정렬해 사용편의성 증대   ex) 엑셀의 피벗 테이블  

** 사용자 정의 함수로 가공된 변수 정의/산출 가능

### sqldf
- 표준 SQL 문장 사용 가능 
- 싱글쿼트 이용해 문자값 입력 가능
- Mac 버전에서는 에러 발생할 수도

```
> install.packages("sqldf")
> library(sqldf)
> data(iris)

# Species가 se로 시작하는 데이터 10행 
> sqldf("select * from iris where Species like 'se%' limit 10")

# Species가 se로 시작하는 데이터 개수 
> sqldf("select count(*) from iris where Species like 'se%'")
> count(*)
1   50
```

** * : 모든 항목 
** _ : 1문자 와일드카드  
** % : 여러문자 와일드카드  
** limit: 결과 행수 제한

### plyr
- 데이터 분리/처리/결합 패키지
- apply 함수 기반 
- 데이터/출력변수를 동시에 배열로 치환해 처리
- 입력/출력 데이터 형태에 따라 접두사 결정

    |입력데이터|출력데이터|함수|
    |---|---|---|
    |데이터프레임|데이터프레임|ddply|
    |데이터프레임|리스트|  dlply|
    |데이터프레임|배열|  daply|
    |리스트  |데이터프레임|  ldply|
    |리스트  |리스트|  llply|
    |리스트  |배열|  laply|
    |배열   |데이터프레임|  adply|
    |배열   |리스트|  alply|
    |배열   |배열|  aaply|

```
> library(plyr)
> set.seed(1)
> d = data.frame(year= rep(2012:2014, each = 6), count = round(runif(9,0,20)))
> print(d)

> ddply(d, "year", function(x){
    mean.count = mean(x$count)
    sd.count = sd(x$count)
    cv = sd.count/mean.count
    data.frame(cv.count = cv)
})
   year  cv.count
1 2012  0.5985621
2 2013  0.4382254
3 2014  0.3978489

# 계산 후 새로 생긴 변수만 출력 
> ddply(d, "year", summarise, mean.count = mean(count))
    year  mean.count
1   2012  10.50000
2   2013  11.33333
3   2014  14.16667

# 계산에 사용된 변수도 출력
> ddply(d, "year", transform, total.count = sum(count))
    year  count  total.count
1   2012    5       63
2   2012    7       63
...
17  2014    13      85 
18  2014    13      85
```

** runif(num_of_random_num, min, max)   
** d_ply: 그림그리기(plotting) 명령  
** 여러 개 변수에 따라 데이터 분할 가능

### 데이터 테이블
- 데이터프레임과 유사하나, 검색속도 더 빠름  
  : 하나씩 비교하는 벡터방식 아닌 인덱스 이용한 바이너리 검색
- 빠른 그룹화 / 순서화 / 짧은 문장 지원
- 64Bit 환경에서 RAM 충분할 때 효율적
- DataTable [ i, j, by]
 
```
> install.packages("data.table")
> library(data.table)

> DT = data.table(x=c("b", "a", "a"), v=rnorm(3))
> DT
   x           v
1: b   0.9817528
2: a  -0.3926954
3: a  -1.0396690

> CARS <- data.table(cars)

> tables()
     NAME NROW MB  COLS       KEY
[1,] CARS   50  1  speed,dist
[2,] DT      5  1  x,v


> setkey(DT, x)   # 데이터 테이블 키 설정
> DT              # 지정된 키 순서대로 정렬됨
1: a  -1.0396690
2: a  -0.3926954
3: b   0.9817528

> tables()
     NAME NROW MB  COLS       KEY
[1,] CARS   50  1  speed,dist
[2,] DT      5  1  x,v         x

# 키 설성 전 인덱싱
> DT[2,]
> DT[DT$x == "b", ]

# 키 설정 후 인덱싱
> DT["b",]
> DT["b", mult="first"]
> DT["b", mult="last"]

# 벡터검색
> tt <- system.time(ans1 <- DF[DF$x=="R" & DF$y == "h", ]
> tt
user system elapsed
1.484 0.260  1.804

# 바이너리 검색
> ss <- system.time(ans2<-DT[J("R", "h")])
> ss
user  system elapsed
0.004 0.000   0.004

# 전체합산
>DT[, sum(v)]

# x 기준 합산
>DT[, sum(v), by=x]

# x, y 변수로 요약 & 그룹화
>sss <- System.time(ss<-DT[,sum(v), by="x,y"])
>sss
user  system elapsed
0.187  0.022   0.209
>ss
     x y       V1
1:   A a 7352.143
2:   A b 7349.215
3:   A c 7427.265
...
675: Z y 7404.204
676: Z z 7352.574
```

** rnorm: 정규분포에서 난수 생성

## 1.3 결측값 & 이상값 처리

### 데이터 탐색
: 데이터 분석 전 다각도로 접근 => 데이터 특성 대략적 파악 & 통찰 
+ 범주형 변수
  - 각 범주 빈도수 출력해 데이터 분포 파악 
+ 연속형 변수
  - 기초 통계치 출력 (4분위/최소/최대/중앙/평균)
  - 공분산/상관계수행렬 출력해 변수 간 선형 상관관계 강도 확인

```
data(iris)
head(iris, 10)  # 처음 10줄 (기본6줄)
str(iris)       # 데이터구조
summary(iris)   # 기초통계량

cov(iris[, 1:4]) # 공분산
cor(iris[, 1:4]) # 상관계수
```

### 결측값 처리
- 가능하면 제외하는 것이 좋으나 의미 있는 경우도 있음
- 처리방법에 따라 전체 속도에 많은 영향
- 자동화 할 경우 업무 효율성 매우 향상
- 패키지: Amelia 2, Mice, mistools 등
- NA
- NaN
- 방식: 삭제 / 대표값으로 대체 / imputation

is.na
complete.cases

```
```

### 이상값 검색
원인
- 의도치 않게 잘못 입력한 경우
- 의도치 않게 입력했으나 분석목적에 부합 안 해서 제거 필요한 경우
- 의도지 않은 형상이지만 분석에 포함해야 하는 경우
- 의도된 이상값

식별방법  
- ESD 알고리즘 이용
- MADM 알고리즘 이용
- boxplot 이용
- outliers 패키지

```
```

# 2. 통계 분석
* 모집단 / 표본 / 표본추출 방법
* 척도의 종류
* 확률변수
* 가설검정 과정
* 상관/회귀/주성분 분석, 다차원 척도법
* 시계열 모형

## 2.1 통계학 개론
### 통계학
: 자료로부터 유용한 정보 이끌어내는 학문 = 자료의 수집 / 정리 / 해석

### 모집단
: 유용한 정보의 대상 = 우리가 알고자 하는 전체, 추출단위/원소로 구성

### 표본 (Sample)
: 표본조사의 대상, 모집단의 일부에서 추출한 일부  
=> 모집단 대표하는지 **모집단의 정의, 표본크기/추출방식, 조사기간/방법**  확인 필요

** **총조사 (Census)**: 모집단의 개체 모두를 조사하는 조사 => 비용/시간 소모 큼  
** **표본조사**: 모집단의 일부만 초사해 모집단에 대해 추론하는 조사  
** **모수 (parameter)**: 모집단에 대해 알고자 하는 값  
** **통계량 (static)**: 모수 추론 위해 구하는 표본의 값들  
** **유한/무한 모집단**: 개체수에 따라 나눈 모집단 (무한모집단은 보통 개념적 집단)  


### 표본추출 방법
+ 단순랜덤추출법 (Simple Random Sampling)
	- N개 모집단에 일련번호 부여 
	- n개 번호 임의 선택 (n <= N)  
+ 계통추출법 (Systematic Sampling)
    - N개 모집단에 일련번호 부여 
    - K개씩 구간 분리 
    - 첫 구간 시작점 임의 선택
    - K개씩 띄어 표본추출
+ 집락추출법 (Cluster Sampling)
	- 모집단이 몇 개 집락으로 구성 + 각 집락 원소에 일련번호 부여
	- 일부 집락을 랜덤 선택 
	- 선택된 집락에서 표본 임의 추출
+ 층화추출법 (Stratified Sampling)
	- 각 원소 이질적인 경우
	- 유사한 원소끼리 층(stratum)으로 구분 
	- 각 층에서 표본 랜덤 추출

### 실험
: 특정 목적 하에 대상에게 처리를 가한 후 그 결과를 관측해 자료수집
- 실험집단 + 비교집단 ???

### 자료의 종류
+ 질적자료
+ 양적자료

### 측정
+ 명목척도
+ 순서척도
+ 구간척도
+ 비율척도


## 2.2 기초 통계 분석

## 2.3 다변량 분석

## 2.4 시계열 예측

# 3. 정형 데이터  마이닝

## 3.1 데이터 마이닝 개요
: 대량의 데이터 속에 숨어있는 유용한 정보/패턴 추출

1) 목적 정의
2) 데이터 준비
3) 데이터 가공
4) 데이터 마이닝 기법의 적용  
: 준비한 데이터 + S/W로 목적정보 추출
5) 검증  
: 테스트 마케팅, 과거 데이터 활용

### 데이터 마이닝 대표 6기능
+ 분류 Classification
	- 새로운 현상을 기존 분류/정의에 배정
	- 선분류된 검증집합에 의해 완성
	- 주로 의사결정나무, 메모리기반 추론, 링크 분석 등 이용
+ 추정 Estimation
	- 연속된 변수 값 추정
	- 입력 데이터 사용해 미지의 결과값 추정
	- 주로 신경망 모형 이용
+ 예측 Prediction
	- 미래 양상/값 추정
	- 기존 결과로 검증 불가 (현상 발생해야 확인가능)
	- 장바구니 분석, 메모리기반 추론, 의사결정나무, 신경망 등 이용
+ 연관 분석 Association Analysis
	- 아이템의 연관성 파악
	- 분석결과는 연관 규칙으로 나타남
	- 장바구니 분석 등 이용 
+ 군집 Clustering
	- 이질적인 모집단을 동질성 있는 그룹으로 세분화
	- 선분류 기준 없음 (분류와의 차이)
	- 주로 데이터마이닝/모델링 준비단계로 사용
+ 기술 Description
	- 데이터가 가지고 있는 의미 기술
	- 데이터가 암시하는 바를 설명 & 설명에 대한 답 제시

## 3.2 분류 분석
: 타겟값 기준으로 입력 데이터 학습 => 새로운 데이터의 그룹 분류
  

### 로지스틱 회귀 모형
- 반응변수가 범주형

### 신경망 모형

### 의사결정나무 모형

### 앙상블 모형

데이터 조절 방법
- 배깅
- 부스팅
- 랜덤 포레스트

### 분류분석 모형 평가
+ 기준
	- 일반화 가능성
	- 효율성
	- 예측/분류 정확성
+ 훈련/검증용 자료 추출
	- 홀드아웃 방법 Holdout method
	- 교차검증 Cross-Validation
	- 붓스트랩  Bootstrap
+ 오분류표
+ ROC 그래프
+ 이익도표 & 향상도 곡선

## 3.3 군집 분석
: 각 개체의 여러 변수 값 관측  
=>  n개의 개체를 유사한 성격의 몇 개 군집으로 집단화  
=> 군집들 특성 파악해 군집 간 관계 분석 (다변량 분석)

### 계층적 군집
+ 군집간 거리 측정법
	- 최단연결법
	- 최장연결법
	- 중심연결법
	- 평균연결법
	- 와드연결법
+ 군집간 거리 정의
	- 유클리드 거리
	- 맨하튼/시가 거리
	- 민코우스키 거리
	- 표준화 거리
	- 마할라노비스 거리

### k-평균 군집

### 혼합분포 군집

### 자기조직화 지도 SOM(Self-Organizing Map)

## 3.4 연관 분석 (=장바구니 분석)
: 유용한 규칙/피턴 등 연관규칙 발굴해 품목간의 관계/연관성 탐색  

### 연관규칙
