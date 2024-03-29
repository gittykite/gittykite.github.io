---
title: "[ADsP] 과목4. 데이터 분석"
category: certificate
tags:
  - ADsP
---

## Index

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
    - [표본추출 방법](#%ed%91%9c%eb%b3%b8%ec%b6%94%ec%b6%9c-%eb%b0%a9%eb%b2%95)
    - [실험](#%ec%8b%a4%ed%97%98)
    - [측정 (Measurement)](#%ec%b8%a1%ec%a0%95-measurement)
  - [2.1.2 통계 분석 (Statistical Analysis)](#212-%ed%86%b5%ea%b3%84-%eb%b6%84%ec%84%9d-statistical-analysis)
    - [통계적 추론 (Statistical Inference)](#%ed%86%b5%ea%b3%84%ec%a0%81-%ec%b6%94%eb%a1%a0-statistical-inference)
    - [확률](#%ed%99%95%eb%a5%a0)
    - [조건부 확률 & 독립 사건](#%ec%a1%b0%ea%b1%b4%eb%b6%80-%ed%99%95%eb%a5%a0--%eb%8f%85%eb%a6%bd-%ec%82%ac%ea%b1%b4)
    - [확률변수(Random Variable)](#%ed%99%95%eb%a5%a0%eb%b3%80%ec%88%98random-variable)
    - [확률변수의 통계치](#%ed%99%95%eb%a5%a0%eb%b3%80%ec%88%98%ec%9d%98-%ed%86%b5%ea%b3%84%ec%b9%98)
    - [확률분포](#%ed%99%95%eb%a5%a0%eb%b6%84%ed%8f%ac)
    - [추정](#%ec%b6%94%ec%a0%95)
    - [가설검정](#%ea%b0%80%ec%84%a4%ea%b2%80%ec%a0%95)
    - [비모수 검정 (Nonparametric Method)](#%eb%b9%84%eb%aa%a8%ec%88%98-%ea%b2%80%ec%a0%95-nonparametric-method)
  - [2.2 기초 통계 분석](#22-%ea%b8%b0%ec%b4%88-%ed%86%b5%ea%b3%84-%eb%b6%84%ec%84%9d)
    - [기술통계 Descriptive Statistic](#%ea%b8%b0%ec%88%a0%ed%86%b5%ea%b3%84-descriptive-statistic)
    - [회귀분석 (Regression Analysis)](#%ed%9a%8c%ea%b7%80%eb%b6%84%ec%84%9d-regression-analysis)
    - [모형 적절성 확인](#%eb%aa%a8%ed%98%95-%ec%a0%81%ec%a0%88%ec%84%b1-%ed%99%95%ec%9d%b8)
    - [최적회귀방정식 선택](#%ec%b5%9c%ec%a0%81%ed%9a%8c%ea%b7%80%eb%b0%a9%ec%a0%95%ec%8b%9d-%ec%84%a0%ed%83%9d)
  - [2.3 다변량 분석](#23-%eb%8b%a4%eb%b3%80%eb%9f%89-%eb%b6%84%ec%84%9d)
    - [상관분석 (Correlation Analysis)](#%ec%83%81%ea%b4%80%eb%b6%84%ec%84%9d-correlation-analysis)
    - [다차원 척도법 (MIDS, Multidimensional Scaling)](#%eb%8b%a4%ec%b0%a8%ec%9b%90-%ec%b2%99%eb%8f%84%eb%b2%95-mids-multidimensional-scaling)
    - [주성분 분석 (PCA, Principal Component Analysis)](#%ec%a3%bc%ec%84%b1%eb%b6%84-%eb%b6%84%ec%84%9d-pca-principal-component-analysis)
  - [2.4 시계열 예측](#24-%ec%8b%9c%ea%b3%84%ec%97%b4-%ec%98%88%ec%b8%a1)
    - [시계열 모형 종류](#%ec%8b%9c%ea%b3%84%ec%97%b4-%eb%aa%a8%ed%98%95-%ec%a2%85%eb%a5%98)
- [3. 정형 데이터 마이닝](#3-%ec%a0%95%ed%98%95-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ec%9d%b4%eb%8b%9d)
  - [3.1 데이터 마이닝 개요](#31-%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ec%9d%b4%eb%8b%9d-%ea%b0%9c%ec%9a%94)
    - [데이터 마이닝 대표 6기능](#%eb%8d%b0%ec%9d%b4%ed%84%b0-%eb%a7%88%ec%9d%b4%eb%8b%9d-%eb%8c%80%ed%91%9c-6%ea%b8%b0%eb%8a%a5)
  - [3.2 분류 분석](#32-%eb%b6%84%eb%a5%98-%eb%b6%84%ec%84%9d)
    - [로지스틱 회귀 모형 (Logistic Regression Analysis)](#%eb%a1%9c%ec%a7%80%ec%8a%a4%ed%8b%b1-%ed%9a%8c%ea%b7%80-%eb%aa%a8%ed%98%95-logistic-regression-analysis)
    - [신경망 모형](#%ec%8b%a0%ea%b2%bd%eb%a7%9d-%eb%aa%a8%ed%98%95)
    - [의사결정나무 모형](#%ec%9d%98%ec%82%ac%ea%b2%b0%ec%a0%95%eb%82%98%eb%ac%b4-%eb%aa%a8%ed%98%95)
    - [알고리즘과 분류 기준변수 선택법](#%ec%95%8c%ea%b3%a0%eb%a6%ac%ec%a6%98%ea%b3%bc-%eb%b6%84%eb%a5%98-%ea%b8%b0%ec%a4%80%eb%b3%80%ec%88%98-%ec%84%a0%ed%83%9d%eb%b2%95)
    - [앙상블 모형](#%ec%95%99%ec%83%81%eb%b8%94-%eb%aa%a8%ed%98%95)
    - [분류 분석 모형 평가](#%eb%b6%84%eb%a5%98-%eb%b6%84%ec%84%9d-%eb%aa%a8%ed%98%95-%ed%8f%89%ea%b0%80)
  - [3.3 군집 분석 (Cluster Analysis)](#33-%ea%b5%b0%ec%a7%91-%eb%b6%84%ec%84%9d-cluster-analysis)
    - [계층적 군집 (Hierarchical Clustering)](#%ea%b3%84%ec%b8%b5%ec%a0%81-%ea%b5%b0%ec%a7%91-hierarchical-clustering)
    - [k-평균 군집](#k-%ed%8f%89%ea%b7%a0-%ea%b5%b0%ec%a7%91)
    - [혼합분포 군집(Mixture Distribution Clustering)](#%ed%98%bc%ed%95%a9%eb%b6%84%ed%8f%ac-%ea%b5%b0%ec%a7%91mixture-distribution-clustering)
    - [자기조직화 지도 SOM(Self-Organizing Map)](#%ec%9e%90%ea%b8%b0%ec%a1%b0%ec%a7%81%ed%99%94-%ec%a7%80%eb%8f%84-somself-organizing-map)
  - [3.4 연관 분석 (=장바구니 분석)](#34-%ec%97%b0%ea%b4%80-%eb%b6%84%ec%84%9d-%ec%9e%a5%eb%b0%94%ea%b5%ac%eb%8b%88-%eb%b6%84%ec%84%9d)
    - [연관규칙(Association Rule)](#%ec%97%b0%ea%b4%80%ea%b7%9c%ec%b9%99association-rule)
    - [연관규칙의 측정지표](#%ec%97%b0%ea%b4%80%ea%b7%9c%ec%b9%99%ec%9d%98-%ec%b8%a1%ec%a0%95%ec%a7%80%ed%91%9c)
    - [연관분석 절차 (Apriori 알고리즘)](#%ec%97%b0%ea%b4%80%eb%b6%84%ec%84%9d-%ec%a0%88%ec%b0%a8-apriori-%ec%95%8c%ea%b3%a0%eb%a6%ac%ec%a6%98)
    - [연관분석 장점](#%ec%97%b0%ea%b4%80%eb%b6%84%ec%84%9d-%ec%9e%a5%ec%a0%90)
    - [연관분석 단점](#%ec%97%b0%ea%b4%80%eb%b6%84%ec%84%9d-%eb%8b%a8%ec%a0%90)
    - [순차패턴](#%ec%88%9c%ec%b0%a8%ed%8c%a8%ed%84%b4)
    - [연관규칙 도출 실습](#%ec%97%b0%ea%b4%80%ea%b7%9c%ec%b9%99-%eb%8f%84%ec%b6%9c-%ec%8b%a4%ec%8a%b5)

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
    | median |중앙값 | 
    | var |분산 | 
    | sd |표준편차| 
    | sum |합 | 
    | log |자연로그 | 
    | cov |공분산 | 
    | cor |상관계수 | 
    |max|최대|
    |min|최소|
    |quantile( x) |사분위수|
    |quantile( x, 1/4) |1사분위수|
    |quantile( x, 3/4) |3사분위수|
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
  : 벡터검색(하나씩 비교) 아닌 바이너리 검색(인덱스 이용)
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

** rnorm(): 정규분포에서 난수 생성  
** System.time(): 실행시간 측정   

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

  - ** 공분산(covariance)  
  : 2개의 확률변수의 상관정도를 나타내는 값  
    - 양/음의 상관관계 존재
    - 분산과 별개의 값
    - 변수 측정단위에 따라 크기 다름 => 상관관계 정도와 무관

  - ** 상관계수(Correlation coefficient)  
  : 두 변수간의 관계의 강도
    - 0 < ρ ≤ +1 : 양의 상관관계
    - ρ  =  0 : 선형 상관관계 아님 (상관관계 존재 가능)
    - -1 ≤ ρ < 0 : 음의 상관관계  
    -  +/-0.7 ~ +/-1:  강한 상관관계 

### 결측값 처리
: 보통 제외하고 처리하는 것이 좋으나 의미 있는 경우도 있음

+ 처리방법에 따라 전체 속도에 많은 영향
    => 자동화 할 경우 업무 효율성 매우 향상
+ R의 결측값
  - NA (Not Available)
  - NaN (Not a number): 무한대 / 0
+ 패키지: Amelia 2, Mice, mistools 등
+ 처리방식
  -  삭제 / 대표값 대체 : na, complete.cases 함수 이용

        ```
        > x <- c(1, 2, NA)
        > is.na(x)            # 결축치 여부 확인
        [1] FALSE FALSE TRUE

        > mean(x)             
        [1] NA
        > mean(x, na.rm=T)    # 결축치 제거 옵션
        [1] 2

        > complete.cases(airquality)   # 결측치 없으면 TRUE 반환

        # 결측치 제거된 데이터셋
        > airquality_ok <- airquality[complete.cases(airquality), ] 

        # 특정 컬럼 결측치 제거된 데이터셋
        > ozone_ok <- airquality$Ozone[complete.cases(airquality$Ozone), ] 
        ```

  -  전가 (imputation) : [Amelia](http://r-bong.blogspot.com/p/amelia-ii-part01-1-introduction.html) 이용

        ```
        > install.packages("Amelia")
        > library(Amelia)

        # 다중전가 생성
        > a.out <- amelia(freetrade, m=3, ts="year", cs="country")

        # 전가 데이터셋 저장 (아멜리아 출력목록)
        > save(a.out, file = "inputations.RData")

        # 전가 데이터셋 각각 저장 (csv 파일)
        > write.amelia(obj=a.out, file.stem = "outdata")
        outdata1.csv
        outdata2.csv
        outdata3.csv

        # 결측지도 시각화
        > missmap(a.out)

        # 전가 처리 후 결측지도 시각화
        > freetrade$tariff <- a.out$imputation[[5]]$tariff
        > missmap(freetrade)
        ```

        ** m: 전가데이터셋 개수  
        ** ts: Time Series 시계열 정보   
        ** cs: Cross-sectional 분석정보   
        
        ** 결측지도(Missingness Map)  
        : 결측 상태의 데이터셋 그리드와 컬러 그리드를 시각화 (열:변수, 행, 관측)

        ![](https://4.bp.blogspot.com/-oJc3Xd-mbsw/WDQkK-XkPtI/AAAAAAAABkk/XydKVTfVkacaU2iYFjbPli0P_acNdnz-wCLcB/s1600/%25EA%25B7%25B8%25EB%25A6%25BC%2B12.png)

### 이상값 검색
: FDS(부정사용방지 시스템) 프로젝트 아닌 이상 많은 시간 투입 불필요  
=> summary 사분위수(Q1, Q3) 및 변수별 플롯 확인해 특성 파악

+ 원인
  - a1: 의도치 않게 잘못 입력한 경우
  - a2: 의도치 않게 입력했으나 분석목적에 부합 안 해서 제거 필요한 경우
  - a3: 의도지 않은 형상이지만 분석에 포함해야 하는 경우
  - b1: 의도된 이상값  
    => 분석목적/영향 고려해 기준 수립하고, 기준에 따라 이상치 무시하고 진행해야

+ 식별방법  
  - ESD 알고리즘 이용  
  : 평균에서 k* 표준편차만큼 떨어져 있는 값 탐색 (보통 k=3)  
  => 널리 알려진 방법이지만 이상값에 매우 민감 
  - MADM 알고리즘 이용
  - boxplot 이용

    ```
    > x = rnorm(100)
    > boxplot(x)

    > x = c(x, 19, 28, 30)  # 이상값 추가
    > outwith = boxplot(x)

    > outwith$out           # 이상값 출력
    [1] 19 28 30
    ```

    ![](https://learningstatisticswithr.com/book/img/graphics2/boxplot1_annotated.png)

  - outliers 패키지 이용

    ```
    > install.packages("outliers")
    > library(outliers)

    > set.seed(1234)
    > y = rnorm(100)

    > outlier(y)                 # 평균과 가장 먼 값 출력
    [1] 2.548991

    > outlier(y, opposite=TRUE)  # 평균과 가장 먼 값 출력 (반대방향)
    [1] -2.345698

    > dim(y) <-c(20, 5)          # 행렬화 (20 * 5)
    > outlier(y)                 # 열별로 열 평균과 가장 먼 값 출력
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

+ 모집단  
  - 유용한 정보의 대상 = 우리가 알고자 하는 전체
  - 추출단위/원소로 구성

+  표본 (Sample)  
   - 표본조사의 대상
   - 모집단의 일부에서 추출한 일부 => 대표성 확인/명시 필요 

+ 모집단 대표성 확인 항목
  - 모집단의 정의
  - 표본크기/추출방식
  - 조사기간/방법

+ 용어
  + 총조사 Census  
  : 모집단 개체 전부 조사 => 큰 비용/시간 소모
  + 표본조사  
  : 모집단 일부 조사 => 모집단에 대해 추론  
  + 모수 Parameter  
  : 알고자 하는 모집단의 값  
  + 통계량 Static   
  : 모수 추론 위한 표본의 값  
  + 유한/무한 모집단 
    - 개체수에 따른 모집단 
    - 무한 모집단은 보통 개념적 집단

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
: 특정 목적으로 대상에게 처리 가함 => 결과를 관측해 자료수집
+ 실험집단  
: 처리를 가하는 집단  
+ 통제집단  
: 비교 위해 처리를 가하지 않는 집단   
ex) 치료제 대신 위약 투여

### 측정 (Measurement)
: 표본/실험에서 추출된 원소/실험단위 관측해 자료 얻는 것

+ 질적자료 (qualitative data)

    |측정방법|상세|예시|
    |---|---|---|
    |명목척도 <br> Nominal scale |대상의 소속집단 분류하는 척도 |ex) 성별, 거주지|
    |순서척도 <br> Ordinal scale |특성에 대한 서열관계 관측 척도 |ex) 선호도|

+ 양적자료 (quantitative data)

    |측정방법|상세|예시|
    |---|---|---|
    |구간척도 <br> Interval Scale | 절대적 원점 없는 속성의 수량 척도 |ex) 온도, 지수|
    |비율척도 <br> Ratio Scale | 절대적 0값 존재하는 수량 척도 <br>  제일 정보 많이 갖는 척도 |ex) 나이, 연간소득|

## 2.1.2 통계 분석 (Statistical Analysis)
: 특정 집단 & 불확실한 현상에 대한 자료수집 => 정보추출  => 통계분석방법  
=> 의사결정 하는 과정  

### 통계적 추론 (Statistical Inference)  
: 수집된 자료로 대상집단(모집단)에 대해 의사결정 하는 것
 
|추론종류|상세|
|---|---|
|추정 <br> Estimation | 모집단의 특성값(모수) 추측 |
|가설검정 <br> Hypothesis Test | 모집단에 가설 설정 => 가설채택 여부 결정 |
|예측 <br> Forecasting | 미래의 불확실성 해결 => 효율적 의사결정|

### 확률
: 특정 사건이 일어날 가능성의 척도
- 모든 확률값은 0과 1사이에 있다 
- 전제 집합의 확률은 1이다
- 서로 배반인 사건들의 합집합의 확률은 각 사건들의 확률의 합니다

  + 표본공간 (Sample Space)  
   : 통계적 실험 시 발생가능한 모든 결과 집합  
  + 사건 (Event)  
   : 표본공간의 부분집합  
  + 근원사건  
   : 사건 중 원소가 오직 1개인 사건  
  + 배반사건  
   : 교집합이 공집합인 사건들

### 조건부 확률 & 독립 사건
+ 조건부 확률 (Connditional Probability)  
: 사건 A가 일어났다는 가정하의 사건 B의 확률
  - P(B|A) = P(A∩B) / P(A)  
  - (P(A) > 0 일 때만 정의 가능)

+ 독립사건  
: 사건A의 발생여부와 사건B의 확률이 상관없는 경우
  - P(A∩B) = P(A)P(B)
  - P(B|A) = P(A∩B) / P(A) = P(B)  
  => A 발생해도 B 발생확률 그대로   
  => B의 조건부확률이 원래 발생확률과 동일

### 확률변수(Random Variable)  
: 특정값이 나타날 가능성이 확률적으로 주어지는 변수 
  - 이산형 확률변수
    * 값: 유한하거나 셀 수 있음
    * 사건의 확률 = 사건에 속한 점들의 확률의 합
  - 연속형 확률변수
    * 값: 연속적인 구간 내에 존재
    * 사건의 확률 = 확률밀도함수의 면적  

### 확률변수의 통계치
+ 이산형 확률변수의 기대값      
   ![equation](https://latex.codecogs.com/gif.latex?%7B%5Cdisplaystyle%20%5Coperatorname%20%7BE%7D%20%5BX%5D%3D%5Csum%20_%7Bi%7Dp_%7Bi%7Dx_%7Bi%7D%7D)
   $${\displaystyle \operatorname {E} [X]=\sum _{i}p_{i}x_{i}}$$  

+ 연속형 확률변수의 기대값   
    ![equation](https://latex.codecogs.com/gif.latex?%7B%5Cdisplaystyle%20%5Coperatorname%20%7BE%7D%20%5BX%5D%3D%5Cint%20_%7B-%5Cinfty%20%7D%5E%7B%5Cinfty%20%7Dxf%28x%29%5C%20%5Coperatorname%20%7Bd%7D%20x%7D)   
    $${\displaystyle \operatorname {E} [X]=\int _{-\infty }^{\infty }xf(x)\ \operatorname {d} x}$$

+ 분산  
  : 평균과의 차이값을 제곱해 평균낸 값  
   ![equation](https://latex.codecogs.com/gif.latex?%7B%5Cbegin%7Baligned%7D%5Coperatorname%20%7BVar%7D%28X%29%26%3D%5Coperatorname%20%7BE%7D%5Cleft%5B%28X-%5Coperatorname%20%7BE%7D%5BX%5D%29%5E%7B2%7D%5Cright%5D%5Cend%7Baligned%7D%7D)   
   $${\begin{aligned}\operatorname {Var}(X)&=\operatorname {E}\left[(X-\operatorname {E}[X])^{2}\right]\end{aligned}}$$

+ 표준편차  
  : 분산을 제곱근 => 제곱된 차이값 보정 => 확률변수의 흩어진 정도  
  ![equation](https://latex.codecogs.com/gif.latex?%7B%5Cdisplaystyle%20%5Csigma%20_%7BX%7D%3D%7B%5Csqrt%20%7B%5Coperatorname%20%7BVar%7D%28X%29%7D%7D%7D)   
  $${\displaystyle \sigma _{X}={\sqrt {\operatorname {Var}(X)}}}$$

+ 연속형 확률변수의 백분위수  
  : 크기 있는 자료 순서대로 나열했을 때의 백분율 위치 값  
  ![equation](https://latex.codecogs.com/gif.latex?%24%24%7BP%28X%20%5Cleqq%20x_%7Bq%7D%29%20%3D%20q%20/100%7D%24%24)   
  $${P(X \leqq x_{q}) = q /100}$$

### 확률분포
: 확률변수의 값과 확률이 퍼져있는 분포   
= 확률변수가 특정 값을 가질 확률을 나타내는 함수

+ 이산형 확률분포  (Discrete Probability distribution)

    |명칭|상세|
    |---|---|
    | 베르누이 확률분포  | 결과가 2개만 나오는 경우|
    | 이항분포 | 베르누이 시행 n번 반복 시 k번 성공할 확률|
    | 기하분포 | 성공확률 p인 베르누이 시행에서 <br> 첫번째 성공이 있기까지 x번 실패할 확률|
    | 다항분포 | 이항분포의 확장 <br> 3가지 이상 결과를 가지는 반복 시행의 확률 분포|
    | 포아송분포 | 시간 + 공간 속 사건 발생횟수|

+ 연속형 확률분포 (Continuous Probability distribution)

    |명칭|상세|
    |---|---|
    |균일분포 |  모든 확률변수 X의 확률이 균일한 분포|
    |지수분포 |  특정 사건발생 시점까지 경과시간에 대한 연속확률분포|
    |정규분포 |  평균이 M, 표준편차가 sigma인 x의 확률밀도함수|
    |t-분포   | 평균이 0 을 중심으로 좌우가 동일한 분포 <br> => 두 집단 평균 동일성 검증에 활용|
    |x^2-분포 | 모분산 가설검정에 사용 (모평균/모분산 모르는 모집단) <br> 두 집단 간 동질성 검정에 활용|
    |F-분포   | 두 집단간 분산의 동일성 검정|

+ 결합확률분포 (Joint Probability distribution)  
: 다수의 확률변수 함께 고려하는 확률분포

  - 이산형 -> 결합확률질량함수  
   ![equation](https://latex.codecogs.com/gif.latex?%24%24%7B%5Cdisplaystyle%20p_%7BX%2CY%7D%28x%2Cy%29%3D%5Cmathrm%20%7BP%7D%20%28X%3Dx%5C%20%5Cmathrm%20%7Band%7D%20%5C%20Y%3Dy%29%7D%24%24)   
   ![equation](https://latex.codecogs.com/gif.latex?%24%24%7B%5Cdisplaystyle%20%5Csum%20_%7Bx%7D%5Csum%20_%7By%7DP%28X%3Dx%5C%20%5Cmathrm%20%7Band%7D%20%5C%20Y%3Dy%29%3D1%5C%3B%7D%24%24)   

   $${\displaystyle p_{X,Y}(x,y)=\mathrm {P} (X=x\ \mathrm {and} \ Y=y)}$$  
   $${\displaystyle \sum _{x}\sum _{y}P(X=x\ \mathrm {and} \ Y=y)=1\;}$$

  - 연속형 -> 결합확률밀도함수  
   ![equation](https://latex.codecogs.com/gif.latex?%24%24%7B%5Cdisplaystyle%20f_%7BX%2CY%7D%28x%2Cy%29%3Df_%7BY%5Cmid%20X%7D%28y%5Cmid%20x%29f_%7BX%7D%28x%29%3Df_%7BX%5Cmid%20Y%7D%28x%5Cmid%20y%29f_%7BY%7D%28y%29%7D%24%24)   
   ![equation](https://latex.codecogs.com/gif.latex?%24%24%7B%5Cdisplaystyle%20%5Cint%20_%7Bx%7D%5Cint%20_%7By%7Df_%7BX%2CY%7D%28x%2Cy%29%5C%3Bdy%5C%3Bdx%3D1%7D%24%24)   
   $${\displaystyle f_{X,Y}(x,y)=f_{Y\mid X}(y\mid x)f_{X}(x)=f_{X\mid Y}(x\mid y)f_{Y}(y)}$$  
   $${\displaystyle \int _{x}\int _{y}f_{X,Y}(x,y)\;dy\;dx=1}$$

+ 정의역 Domain  
  : y = f(x) 에서 x의 집합
+ 치역 Range  
  : y = f(x)에서 f(x)의 집합 (함수의 출력값 집합)

### 추정 
: 표본으로부터 미지의 모수 추측하는 것

  + 점추정 
    - 표본 기반으로 모수의 값 추정  
      ex) 표본평균 -> 모평균 추정, 표본분산 -> 모분산 추정
  + 구간추정 
    - 점추정의 정확성 부족 보완
    - 신뢰구간(confidence interval) 선언  
    => 모수가 일정 신뢰수준으로 존재하는 특정 구간  
    - 신뢰수준 (confidence level)  
    : 동일 방법/자료수의 확률표본 무한히 추출해 구한 신뢰구간이 미지의 모수 포함할 확률 (주로 90%, 95%, 99% 이용)

### 가설검정 
: 모집단에 대한 가설 설정 후 표본관찰 통해 가설 채택여부 결정

  + 귀무가설(null hypothesis, H0)  
  : 대립가설과 반대의 증거 찾기 위해 정한 가설
  + 대립가설(Alternative hypothesis, H1)  
     - 확실하게 증명하고 싶은 가설
     - 뚜렷한 증거가 있어야 채택 가능한 가설
     - 결과가 값비싼 가설
  + 검정통계량(test static, T(X))  
  : 관찰된 표본으로부터 구하는 통계량, 검정 시 가설의 진위를 판단하는 기준
  + P-값  
  : 귀무가설이 사실일 때, 관측된 검정통계량 값보다 대립가설을 더 지지하는 검정통계량 나올 확률
  + 유의수준(significance level, a)  
  : 귀무가설 기각 기준 = 귀무가설이 옳은데도 기각할 확률
    - p-값이 더 작을 경우 기각
    - 보통: 0.01, 0.05, 0.1 중 하나
  + 기각역(critical region, C)  
  : 귀무가설 기각하는 통계량의 영역 
    - 검정통계량 분포에서 유의수준이 alpha인 부분
  + 가설검정 오류 종류

    |사실 |   가설검정결과   |  오류|
    |---|---|---|
    |사실임    |사실 판정      | 옳은 결정 |
    |사실임    |사실 아님 판정  | 제1종 오류 |
    |사실 아님 | 사실 판정     |제2종 오류 |
    |사실 아님 | 사실 아님 판정 |옳은 결정 |

### 비모수 검정 (Nonparametric Method)
: 관측 자료가 특정분포 따른다고 가정 할 수 없는 경우, 모집단 분포에 아무 제약 없이 검정 실시
+ 모집단 분포 가정 안 함  
 => 분포의 형태가 동일한지 여부만 설정
+ 관측값의 절대적 크기(표본평균/분산)에 의존 안 함   
 => 관측값 순위/차이의 부호 등 이용해 검정
+ 대표적 방법
  - 관측 표본 부호 검정(sign test)
  - 윌콕슨의 순위합검정(rank sum test)
  - 윌콕슨의 부호순위합검정(signed rank test)
  - 만-위트니의 U검정
  - 런검정(run test)
  - 스피어만의 순위상관계수 

## 2.2 기초 통계 분석
### 기술통계 Descriptive Statistic  
: 자료 정리/요약 위해 사용되는 기초통계 => 분석 전 이해/통찰 얻는 데 유리  
  - 숫자 : 평균, 최빈값, % 등
  - 그림 : 그래프, 차트 등
  - 공분산/상관분석 => 데이터의 인과관계

### 회귀분석 (Regression Analysis)
: 여러 변수의 선형관계 모형화

 + 단순회귀
   - 설명변수: 1개   
   - 반응변수와 관계: 직선
   - Y = b0 + b1X + c

        ```
        set.seed(2)
        x = runif(10, 0, 11)
        y = 2 + 3*x + rnorm(10, 0, 0.2)

        # 쌍으로 묶인 x,y 데이터 생성
        xyframe = data.frame(x, y) 

        # 단순회귀분석
        lm(y~x, data=xyframe)

        # Call:
        # lm(formula = y ~ x, data = xyframe)
        #
        # Coefficients:
        # (Intercept)          x  
        #     2.213        2.979     

        # 회귀방정식: y = 2.213x + 2.979    
        ```

 + 다중회귀
   - 설명변수: k개   
   - 반응변수와 관계: 선형 (1차 함수)
   - Y = b0 + b1X1 + b2X2 + ... + c
     * 다중선형회귀 
 
        ```
        set.seed(2)
        u = runif(10, 0, 11)
        v = runif(10, 11, 20) 
        w = runif(10, 1, 30)

        # 반응변수 y와 3개의 독립변수 데이터 생성 
        y = 3 + 0.1*u + 2*v - 3*w + rnorm(10, 0, 0.1) 
        dtrame = data.frame(y, u, v, w)
        
        # 다중선형회귀
        m <- lm(y ~ u+v+w)
        m
        
        Call:
        lm(formula = y ~ u + v + w)

        Coefficients:
        (Intercept)           u            v            w  
            3.0417       0.1232       1.9890      -2.9978 

        # 회귀식
        # y = 3.0417 + 0.1232u + 1.989v -2.9978w
        ```

     * 통계량과 유의수준 확인
        ```
        summary(m)

        # Residuals:
        #     Min        1Q    Median        3Q       Max 
        # -0.188562 -0.058632 -0.002013  0.080024  0.143757 
        #
        # Coefficients:
        #             Estimate Std. Error  t value Pr(>|t|)    
        # (Intercept)  3.041653   0.264808   11.486 2.62e-05 ***      
        # u            0.123173   0.012841    9.592 7.34e-05 ***    
        # v            1.989017   0.016586  119.923 2.27e-11 ***    
        # w           -2.997816   0.005421 -552.981 2.36e-15 ***    
        # ---
        # Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
        #
        # Residual standard error: 0.1303 on 6 degrees of freedom
        # Multiple R-squared:      1,	Adjusted R-squared:      1     
        # F-statistic: 1.038e+05 on 3 and 6 DF,  p-value: 1.564e-14     

        # 회귀계수 모두 0.001 미만: 회귀계수 추정치가 통계적으로 유의미
        # 결정계수=1, 수정결정계수=1 :데이터 설명력 높음
        # F통계량과 p값 낮음: 회귀모형이 통계적으로 매우 유의미
        ```
 + 다항회귀
   - 설명변수: k개   
   - 반응변수와 관계: 1차 함수 이상  (k=1 -> 2차 함수 이상)
   - Y = b0 + b1X1 + b2X2 + b11X1^2 + b22X2^2 + ... + c

        ```
        data(cars)
        
        # 제곱속도 열 추가
        speed2 <- cars$speed^2
        cars <- cbind(speed2, cars)

        # 다중회귀
        lm(dist~speed+speed2, data = cars)

        # Call:
        # lm(formula = dist ~ speed + speed2, data = cars)
        # 
        # Coefficients:
        # (Intercept)        speed       speed2  
        #     2.47014      0.91329      0.09996  

        # 회귀식
        # y = 2.47014 + 0.91329speed + 0.09996speed^2
        ```

        ** 산점도가 n차 방정식 (곡선패턴)인 경우 항 추가 => 모형 비교

 + 비선형회귀
   - 미지의 모수들이 선형관계 아닌 모형   
   - Y = g(b0 + b1X + ... + bkXk) + c
  
### 모형 적절성 확인
+ 모형의 통계적 유의미함  
=> 유의수준 5% 하에서 F통계량 p값이 0.05 미만인 경우 
+ 회귀계수의 통계적 유의미함  
=> 계수의 t통계량 / p값 / 신뢰구간 확인
+ 모형의 설명력   
=> 결정계수 확인 (범위:0~1, 클 수록 설명력 큼)
+ 모형 데이터 적합성  
=> 잔차 그래프 & 회귀진단 이용 
+ 데이터의 모형 가정 만족 여부  
  
    |가정|내용|
    |---|---|
    |선형성| 독립변수 변하면 종속변수도 일정크기로 변화|
    |독립성|잔차와 독립변수 값이 무관|
    |등분산성| 독립변수의 모든 값에 대해 오차들의 분산이 일정|
    |비상관성| 관측치들의 잔차들끼리 무관|
    |정상성| 잔차항이 정규분포|

### 최적회귀방정식 선택
1) 가능한 모든 조합 회귀분석   
: 모든 독립변수 조합의 회귀모형 => AIC/BIC 기준으로 선택
   - AIC (Akaike info criterion)  
      * AIC = -2 log L(0) + 2k     
      * L(0):  가능도함수  
      * k : 모형의 모수 개수  
      * 값 작을수록 적합
   - BIC (Bayesian info criterion)  
     * BIC = -2 Log L(0) + K log(n)
     * n: 자료의 개수 
     * 값 작을수록 적합

1) 단계적 변수선택 (Stepwise Variable Selection)  
: 직접 변수 추가/제거하거나 [step 함수](https://www.rdocumentation.org/packages/stats/versions/3.6.1/topics/step)  이용  

   - 전진선택법  (forward Selection)  
     1) 절편만 있는 상수모형으로 시작   
     2) 중요한 설명변수부터 모형에 추가   
     3) 가장 제곱합 기준으로 가장 설명 잘하는 변수 고려해 유의하면 추가

        ```
        head(state.x77)
        states <- as.data.frame(state.x77[,c("Murder","Population","Illiteracy","Income")])
        
        # 회귀분석 (종속변수:Murder, 설명변수:나머지)
        # fit <- lm(Murder~Population+Illiteracy+Income,data = states) 
        fit <- lm(Murder~.,data = states) 

        # 상수항만 포함된 회귀모형
        fit.con <- lm(Murder~1, states)

        # 전진선택
        fit.forward <- step(fit.con, scope=list(lower=~1, upper=~Population+Illiteracy+Income), direction="forward")

        # 출력되는 선택 과정
        #              Df Sum of Sq    RSS     AIC
        # + Illiteracy  1    329.98 337.76  99.516
        # + Population  1     78.85 588.89 127.311
        # + Income      1     35.35 632.40 130.875
        # <none>                    667.75 131.594
        #
        # Step:  AIC=99.52
        # Murder ~ Illiteracy
        #
        #              Df Sum of Sq    RSS     AIC
        # + Population  1    48.517 289.25  93.763
        # <none>                    337.76  99.516
        # + Income      1     4.916 332.85 100.783
        #
        # Step:  AIC=93.76
        # Murder ~ Illiteracy + Population
        #
        #          Df Sum of Sq    RSS    AIC
        # <none>                289.25 93.763
        # + Income  1  0.057022 289.19 95.753

        fit.forward

        # 선택결과 출력 (AIC 작은 값 선택)
        # Call:
        # lm(formula = Murder ~ Illiteracy + Population, data = # states)
        #
        # Coefficients:
        # (Intercept)   Illiteracy   Population  
        # 1.6515497    4.0807366    0.0002242 

        # 선택된 회귀모형
        # Murder = 1.6515497 + 4.0807366Illiteracy + 0.0002242Population
        ```

        ** step(lm(종속변수~설명변수, data),...) : 선형회귀 정의  
        ** scope: 분석 시 고려할 변수 범위 (1->상수항, 최대->모든 설명변수)  
        ** direction: 변수선택방법 (forward, backward, both)  

   - 후진제거법 (backward elimination)  
      1) 제곱합 기준 가장 적은 영향 변수부터 하나씩 제거   
      2) 유의하지 않은 변수 없을 때까지 설명변수 제거

        ```
        # 후진제거법
        fit.backward <- step(fit, scope=list(lower=~1, upper=~Population+Illiteracy+Income), direction="backward")
        fit.backward
        ```

   - 단계적방법 (stepwise method)   
     1) 전진선택법으로 변수 추가   
     2) 새로운 변수 추가로 중요도 약해진 변수는 제거  
     3) 추가/제거 변수 없을 때까지 반복

        ```
        # 단계적방법
        fit.both <- step(fit.con, scope=list(lower=~1, upper=~Population+Illiteracy+Income), direction="both")
        fit.both
        ```

## 2.3 다변량 분석
### 상관분석 (Correlation Analysis)
: 데이터 내의 두 변수 관계 분석

+ 피어슨 상관계수(Pearson Correlation)  
  - 등간척도로 측정되는 두 변수 간 상관관계 측정
  -  선형관계의 크기만 측정 가능 (한 변수를 단조증가 함수로 변환)
  - 공분산이 언제나 -1 ~ 1 사이  
   ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/93185aed3047ef42fa0f1b6e389a4e89a5654afa)

    ```
    install.packages("Hmisc")
    library(Hmisc)

    data(mtcars)
    drat <- mtcars$drat
    disp <- mtcars$disp

    # drat과 disp의 산점도 & 상관계수 출력
    plot(drat, disp)
    cor(drat, disp)

    # 강한 음의 상관관계 (-1이나 1에 가까움)
    # [1] -0.7102139    

    # 피어슨 상관계수 & 상관계수 p값 출력
    rcorr(as.matrix(mtcars), type="pearson")
    cov(mtcars)
    ```

+ 스피어만 상관계수(Spearman Correlation)  
  - 서열척도의 두 변수 상관관계 측정  
  - 비선형관계도 표현 가능     
  : 두 변수를 순위로 변환 => 순위 사이의 피어슨 상관계수로 정의
  - 자료에 이상점 있거나 표본크기 작을 때 유용

    ```
    rcorr(as.matrix(mtcars), type-"spearman")
    ```

** rcorr 함수  
- 모든 변수들 사이의 상관계수
- 가설 H0:p = 0 에 대한 p값 출력


### 다차원 척도법 (MIDS, Multidimensional Scaling)
: 여러 대상 간 거리가 주어질 때, 동일한 상대적 거리 가진 실수공간의 점들로 배치하는 방법  
=> 특정 변수의 관측치 없어도 개체간 유사성 이용 가능  
=> 관측치들 간 상대적 관계 이해하는 시각화에 주로 이용  

```
data(eurodist)
data

loc <- cmdscale(eurodist) # 각 도시 좌표 계산
loc

x <- loc[,1]
y <- loc[,2]
plot(x, y, type="n", asp=1, main="Metric MDS") # 산점도
text(x, y, rownames(loc), cex=0.7)
abline(v=0, h=0, lty=2, lwd=0.5)       # 그래프에 수직선 추가
```

![](https://t1.daumcdn.net/cfile/tistory/99F4AB505BF557732D)

### 주성분 분석 (PCA, Principal Component Analysis)
: 상관관계의 변수끼리 결합해 분산 극대화하는 변수 생성  
=> 선형결합으로 변수 축약해 희생되는 정보 최소화
=> 차원 줄여 예측모델 만들 때 이용 가능

+ 주성분분석  

    ```
    library(datasets)
    data(USArrests)
    summary(USArrests)

    # 주성분분석
    fit <- princomp(USArrests, cor=TRUE) 

    summary(fit)

    # 4개 주성분의 표준편차, 분산의 비율, 분산의 누적비율 출력
    # Importance of components:
    #                           Comp.1    Comp.2    Comp.3     Comp.4
    # Standard deviation     1.5748783 0.9948694 0.5971291 0.41644938
    # Proportion of Variance 0.6200604 0.2474413 0.0891408 0.04335752
    # Cumulative Proportion  0.6200604 0.8675017 0.9566425 1.00000000

    # 주성분1이 62%의 분산을 설명
    # 주성분1,2가 86%의 분산을 설명
    ```

    ** cor옵션: 공분산행렬 아닌 상관계수 행렬로 주성분분석

+ 주성분 로딩벡터 출력

    ```
    # 주성분들의 로딩 벡터 출력
    loadings(fit)

    # Loadings:
    #          Comp.1 Comp.2 Comp.3 Comp.4
    # Murder   -0.536  0.418 -0.341  0.649
    # Assault  -0.583  0.188 -0.268 -0.743
    # UrbanPop -0.278 -0.873 -0.378  0.134
    # Rape     -0.543 -0.167  0.818        
    # 
    #                Comp.1 Comp.2 Comp.3 Comp.4
    # SS loadings      1.00   1.00   1.00   1.00
    # Proportion Var   0.25   0.25   0.25   0.25
    # Cumulative Var   0.25   0.50   0.75   1.00

    # 주성분 결과
    # Y1 = - 0.536Murder - 0.583Assault - 0.278UrbanPop - 0.543Rape
    # Y2 = 0.418Murder + 0.188Assault -0.873UrbanPop -0.167Rape
    # ...
    ```

+ 스크리 그림 출력  
: 각 주성분 분산크기 도식화 그래프

    ```
    # 스크리 그림(Scree plot)
    plot(fit, type="lines") 
    ```

    ![](https://bookdown.org/egarpor/SSS2-UC3M/SSS2-UC3M_files/figure-html/unnamed-chunk-217-1.png)

+ 가중치 바이플롯 출력  
  : 방향 비슷하고 수직에 가까울 수록 큰 가중치 적용된 것

    ```
    # 각 관측치 주성분들로 표현
    fit$scores

    #                     Comp.1      Comp.2      Comp.3       Comp.4
    # Alabama        -0.98556588  1.13339238 -0.44426879  0.156267145
    # Alaska         -1.95013775  1.07321326  2.04000333 -0.438583440
    # Arizona        -1.76316354 -0.74595678  0.05478082 -0.834652924
    # ...

    # 관측치를 주성분1,2 좌표에 그림
    biplot(fit) 
    ```

    ![](https://coolstatsblog.files.wordpress.com/2017/01/biplot.png)

## 2.4 시계열 예측
: 시계열 자료 분석해 미래 수치 예측  

+ 정상성 조건  
: 시점에 상관없이 시계열 특성이 일정 (반드시 만족해야 분석 가능)
  - 평균이 일정
  - 분산이 시점에 의존 안 함
  - 공분산이 시차에만 의존 (시점에는 의존 안 함)

+ 정상 시계열화
  - 이상점 -> 제거
  - 개입 (Intervention) -> 회귀분석 수행
    * 개입 (Intervention): 
  - 분산 불일정 -> 변환 
    * 변환 (Transformation): 
  - 추세 (Trend) -> 차분
    * 차분(Difference): 현 시점 자료값에서 전 시점 자료값 빼기

### 시계열 모형 종류
+ 자기회귀모형 (AR: Autoregressive)  
: 현 시점 자료가 p시점 전의 유한개의 과거자료로 설명 가능
  - 식별방법
    * 자기상관함수(ACF, Auto-Correlation Function)  
    : 시차가 증가함에 따라 점차 감소
    * 부분자기상관함수(PACF, Partial Auto-Correlation Function)     
    : p +1  시차 이후 급격히 감소, 절단된 형태 
  - 백색잡음과정(White noise process)  
    * 대표적 정상 시계열
    * 시계열 분석에서 오차항 의미  

+ 이동평균모형(MA: Moving Average)  
: 현시점 자료를 유한개의 백색잡음의 선형 결합으로 표현
  - 항상 정상성 만족 (정상성 가정 불필요) 
  - 식별방법
    * 자기상관함수(ACF, Auto-Correlation Function)  
    : p +1  시차 이후 급격히 감소, 절단된 형태 
    * 부분자기상관함수(PACF, Partial Auto-Correlation Function)     
    : 시차가 증가함에 따라 점차 감소

+ 자동회귀누적이동평균모형(ARIMA: AR Integrated MA)  
: 기본적으로 비정상 시계열 모형  
 => 차분/변환 통해 AR/MA/ARMA 모형으로 정상화 가능
  - ARIMA(p, d, q)
    * p: AR 모형과 관련
    * d: MA 모형과 관련
    * q: ARMA로 정상화 시 몇 번 차분했는지
  - d=0 => ARMA(p,q) 모형 => 정상성 만족
  - p=0 => IMA(d, q) 모형 => d번 차분 => MA(q)
  - q=0 => ARI(p, d) 모형 => d번 차분 => AR(p)
  - 차분 

    ```
    plot(Nile)    # 비정상 시계열 (계절성 없지만 평균이 변화하는 추세)

    # 1번 차분
    Nile.diff1 <- diff(Nile, differences=1)
    plot(Nile.diff1)

    # 2번 차분
    Nile.diff1 <- diff(Nile, differences=1)
    plot(Nile.diff1)
    
    # => 차분할 수록 점차 정상성 만족
    ```

  - 모델 적합 및 결정

    ```
    # 자기상관함수 확인
    acf(Nile.diff2, lag.max=20) # lag 수 너무 많으면 그래프로 모형식별 판단 어려움
    acf(Nile.diff2, lag.max=20, plot=FALSE)

    # 결과: lag=1, 8 제외하고 모두 신뢰구간 안에 존재    

    # 부분자기상관함수 확인
    pacf(Nile.diff2, lag.max=20) # lag 수 너무 많으면 그래프로 모형식별 판단 어려움
    pacf(Nile.diff2, lag.max=20, plot=FALSE)

    # 결과: lag=1~8에서 신뢰구간 넘어 음의 값 가지고, lag=9 에서 절단됨

    # 종합결과
    # ARMA(8, 0): 부분자기상관함수 그래프에서 lag=9 에서 절단
    # ARMA(0, 1): 자기상관함수 그래프에서 lag=2 에서 절단
    # ARMA(p, q): AR모형과 MA모형 혼합해 모형 식별하고 결정해야   

    # ARIMA 모형 자동 결정 (forcast 패키지)
    auto.arima(Nile)
    ```

  - ARIMA 모형 이용한 예측

    ```
    Nile.arima <- arima(Nile, order = c(1, 1, 1))
    Nile.arima

    Nile.forecasts <- forecast(Nile.arima, h=10) # h: 10개 연도만 예측
    Nile.forecasts
    plot(Nile.forecasts)
    ```

+ 분해 시계열  
 : 일반적인 시계열 영향요인 분리해 분석  
 => 정확하게 분리하기 어렵고 이론적 약점 있으나 경제분석/예측에 널리 사용됨
 
  - 시계열 구성요소(분해식: Zt = f(Tt, St, Ct, It)  )  
    - 추세요인 (Tt, Trend factor)  
    : 자료가 특정 형태인 경우 ex) 선형, 이차식, 지수 등 
    - 계절요인 (St, Seasonal factor)  
    : 고정된 주기에 따라 자료 변하는 경우 ex) 요일, 월, 분기 등
    - 순환요인 (Ct, Cyclical factor)  
    : 경제/자연적 이유 아닌 알려지지 않은 주기로 자료 변하는 경우 
    - 불규칙요인 (It, Irregular factor)  
    : 위 세 가지 요인으로 설명불가능한 회귀분석에서 오차에 해당하는 요인  

    ```
    plot(ldeaths) # 연도별로 계절성 있는 사망자 수 데이터

    #  시계열 자료 4가지 요인으로 분해
    ldeaths.decompose <- decompose(ldeaths)
    ldeaths.decompose$seasonal
    plot(ldeaths.decompose)

    # 계절요인 제거
    ldeaths.decompose.adj <- ldeaths - ldeaths.decompose$seasonal
    plot(ldeaths.decompose.adj)
    ```   

# 3. 정형 데이터  마이닝

## 3.1 데이터 마이닝 개요
: 대량의 데이터 속에 숨어있는 유용한 정보/패턴 추출

1) 목적 정의
2) 데이터 준비
3) 데이터 가공  
   - 모델개발 단계에서 부하 커짐  
   => 데이터 가공 및 개발환경 구축 선행 필요   
   - ex) 분류모델링 시 CRM 마트 형식으로 데이터 가공
4) 데이터 마이닝 기법의 적용  
   - 데이터 + S/W로 목적정보 추출  
   - 목적과 데이터에 맞는 S/W 기법 이용해야
5) 검증  
   - 추출된 정보 검증
   - 테스트 마케팅, 과거 데이터 활용
6) 종료
   - IT부서와 자동화 방안 협의 
   - 보고서 작성  
      => 추가수익 ROI 등 기대효과 경영진에 전달

### 데이터 마이닝 대표 6기능
+ 분류 Classification
	- 새로운 현상을 기존 분류/정의에 배정
	- 선분류된 검증집합에 의해 완성
	- ＝＞ 의사결정나무, 메모리기반 추론, 링크 분석 등 이용
+ 추정 Estimation
	- 연속된 변수 값 추정
	- 입력 데이터 사용해 미지의 결과값 추정
	- ＝＞ 신경망 모형 이용
+ 예측 Prediction
	- 미래 양상/값 추정
	- 기존 결과로 검증 불가 (현상 발생해야 확인가능)
	- ＝＞장바구니 분석, 메모리기반 추론, 의사결정나무, 신경망 등 이용
+ 연관 분석 Association Analysis
	- 아이템의 연관성 파악
	- 분석결과는 연관 규칙으로 나타남
	- ＝＞장바구니 분석 등 이용 
+ 군집 Clustering
	- 이질적인 모집단을 동질성 있는 그룹으로 세분화
	- 선분류 기준 없음 (분류와의 차이)
	- ＝＞ 데이터마이닝/모델링 준비단계로 사용
+ 기술 Description
	- 데이터가 가지고 있는 의미 기술
	- 데이터가 암시하는 바를 설명 & 설명에 대한 답 제시

## 3.2 분류 분석
: 타겟값 기준으로 입력 데이터 학습 => 새로운 데이터의 그룹 분류
  

### 로지스틱 회귀 모형 (Logistic Regression Analysis)
: 반응변수가 범주형인 경우 이용되는 회귀 분석  
=> 관심범주에 속할 확률 선형화

+ 특성
  - 새로운 설명/예측변수 값이 각 범주에 속할 확률 추정 (예측모형)
  - 추정된 확률을 기준치에 따라 분류 (분류모형)  
    * π(X) > 기준값: Y=1 집단
    * π(X) < 기준값: Y=0 집단  
    * 사후확률: 모형 적합으로 추정된 확률
  - 기준값 결정
    * 사전정보 이용
    * 손실함수 이용
    * 지표(정분류율, 민감도, 특이도 등) 이용
  - 오즈 관점 해석 가능  
    * $exp(b1)$ = x1에 대한 성공 오즈의 단위당 증가율  
    (나머지 변수 (x1...xk) 주어질 때) 
  - 프로빗 모형과 유사  
    : 표준정규분포의 누적분포함수로 성공 확률 나타낸 모형  
    * $$ 𝛷(π(X))^-1 = β0 + β1X1 + ... + βkXk$$

+ 수식
  - 로짓함수  
     : 비선형적인 관계를 선형식으로 일반화
     $$log(π(X)) = log(π(X)) / (1 − π(X)) = β0+β1X1 + ... + βkXk$$  

  - 다중로지스틱 함수 
     $$π(X) = exp(β0 + β1X1 + ... + βkXk) / (1 + exp(β0 + β1X1 + ... + βkXk))$$   
     $$ = F(β0 + β1X1 + ... + βkXk)$$ 
    * b1 > 0: S자 형태 (설명변수 1개일 때)
    * b1 < 0: 역S자 형태 (설명변수 1개일 때)
    * 표준로지스틱 분포의 누적분포함수로 성공의 확률 설명/추정 

+ glm 함수

    ```   
    data(iris)
    a <- subset(iris, Species=="setosa" | Species=="versicolor")

    # Species 변수 범주화
    a$Species <- factor(a$Species)
    str(a)

    # 로지스틱 회귀 수행
    b <- glm(Species~Sepal.Length, data=a, family=binomial) # binominal -> 이항분포
    summary(b)

    # 결과
    # Call:
    # glm(formula = Species ~ Sepal.Length, family = binomial, data = a)

    # Deviance Residuals: 
    #     Min        1Q    Median        3Q       Max  
    # -2.05501  -0.47395  -0.02829   0.39788   2.32915  

    # Coefficients:
    #             Estimate Std. Error z value Pr(>|z|)    
    # (Intercept)   -27.831      5.434  -5.122 3.02e-07 ***       
    # Sepal.Length    5.140      1.007   5.107 3.28e-07 ***         # ***: Sepal.Length가 유의미
    # ---                                                           # 회귀계수: 5.140
    # Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

    # (Dispersion parameter for binomial family taken to be 1)

    #     Null deviance: 138.629  on 99  degrees of freedom         # 완전모형으로부터의 이탈도
    # Residual deviance:  64.211  on 98  degrees of freedom         # Sepal.Length가 추가된 적합모형(귀무가설) 이탈도
    # AIC: 68.211

    # Number of Fisher Scoring iterations: 6

    
    coef(b)
    # (Intercept) Sepal.Length 
    #  -27.831451     5.140336 

    exp(coef(b))["Sepal.Length"]
    # Sepal.Length 
    #     170.7732 

    # 오즈 증가량 계산
    # : Sepal.Length 1단위 증가 시 Versicolor(Y=2)일 오즈 exp(5.140) ≈ 170배 증가

    # 결과 반환값 확인
    ls(b)            # 반환항목 리스트
    b$coef
    b$fitted
    b$resid
    b$effects
    ```   

+ confint 함수

    ```   
    # 회귀계수 신뢰구간
    confint(b, parm = "Sepal.Length")
    #     2.5 %   97.5 % 
    # 3.421613 7.415508 
    
    # 오즈 증가량 신뢰구간
    exp(confint(b, parm = "Sepal.Length"))
    #     2.5 %     97.5 % 
    # 30.61878 1661.55385 
    ```   

+ fitted 함수

    ```  
    # 적합결과 확인
    fitted(b)[c(1:5, 96:100)]
    
    #         1          2          3          4          5         96         97 
    # 0.16579367 0.06637193 0.02479825 0.01498061 0.10623680 0.81282396 0.81282396 
    #         98         99        100 
    # 0.98268360 0.16579367 0.81282396 

    # 예측 (편의로 초기 데이터 a 이용)
    predict(b, newdata=a[c(1, 50, 51, 100), ], type="response")
    
    #         1        50        51       100 
    # 0.1657937 0.1062368 0.9997116 0.8128240 
    ```   

+ cdplot 함수

    ```  
    cdplot(Species~Sepal.Length, data=a)
    plot(a$Sepal.Length, a$Species, xlab="Sepal.Length")
    x = seq(min(a$Sepal.Length), max(a$Sepal.Length), 0.1)
    lines(x, 1 + (1/(1+(1/exp(-27.831+5.140*x)))), type="1", col="red")
    ```   

### 신경망 모형
: 입력된 개별신호 강도에 따라 가중 -> 활성함수 -> 출력 계산  
=> 많은 데이터 학습/훈련 거쳐 원하는 결과 나오도록 가중치 조정 됨
+ 단층신경망 (=퍼셉트론)  
  : 은닉층 없는 단층신경망 네트워크 
  - $${\displaystyle z = w'x+ w_{0} = w_{0} + \sum _{i=1}^{m}w_{i}x_{i}}$$
    * w=(w1,...wd)': 가중치 (의사결정경계 방향표시 모수)
    * W0: 절편 (bias, 의사결정 경계위치 결정 모수)
    * 가중치와 절편의 갱신: 오차제곱합 최소화 되도록 갱신
+ 활성함수  
: z에 적용되어 최종목표값 계산하는 함수

  |함수명|결과|결과범위|
  |---|---|---|
  |부호/threshold 함수 |이진형| y = -1, (z <0) <br> y = 1, (z >= 0)|
  |계단 함수|이진형|y = 0, (z <0) <br> y = 1, (z >= 0)
  |시그모이드 함수|연속형| 0<= y <= 1|
  |Softmax 함수 | 출력값 z 여러 개(L개)|목표치 다범주인 경우 <br>범주 속할 사후확률 제공|
  |tanh 함수| 연속형|-1 <= y <= 1|
  |가우스 함수|연속형|0<= y <=1|

  - Sigmoid 함수
    $${\displaystyle S(x)={\frac {1}{1+e^{-x}}}}$$ 
  - Softmax 함수      
    $$y_{i}={\frac {e^{x_{i}}}{\sum _{j=1}^{c}e^{x_{j}}}}$$
  - tanh 함수   
    $${\displaystyle f(x)=\tanh x={\frac {e^{x}-e^{-x}}{e^{x}+e^{-x}}}}$$    

+ 은닉층 결정
  -  
+ 노드 수 결정

    ```
    library(nnet)
    nn.iris <- nnet(Species~., data=iris, size=2, range=0.1, decay=52-4, maxit=200)
    summary(nn.iris)
    plot.nnet(nn.iris)
    ```
    ** size:   
    ** range:   
    ** decay:   
    ** maxit:    

    ```
    library(clusterGeneration)
    library(scales)
    library(reshape)
    plot(nn.iris )
    ```

### 의사결정나무 모형
1) 목표변수와 관계된 설명변수 선택  
2) 나무 생성  
   - 분리기준/정지규칙 설정 (분석목적/자료구조에 맞게)
3) 가지치기
   - 부적절한 가지 제거  
4) 모형평가
   - 이익 / 위험 / 비용 고려  
5) 분류 & 예측  

### 알고리즘과 분류 기준변수 선택법

|목표변수| 분류기준 |알고리즘| 상세|
|---|---|---|---|
|이산형| 다지분할 |카이제곱 통계량|
|이산형| 이진분할 |지니지수|
|이산형| C4.5 |엔트로피 지수|
|연속형| 다지분할 |ANOVA F-통계량|
|연속형| 이진분할 |분산감소량|

rpart()
predict()
prune()
rpart.control()

비용-복잡도 모수
교차타당성오차

### 앙상블 모형
: 여러 개 분류모형 결과 종합해 분류 정확도 높이는 방법  
- 훈련용 데이터 집합 다수 만들어 각 데이터 집합 마다 분류기 생성 => 결과 앙상블

+ 데이터 조절 방법
  - 배깅  
    : 크기 같은 표본 복원추출 => 분류기 생성 => 결과 앙상블(집계)  
    * 복원추출  
    : 같은 데이터 여러번 추출되거나 아예 추출 안 되는 경우 발생
  - 부스팅  
    : 분류 잘못된 데이터에 더 큰 가중 주고 표본추출 
    * 붓스트랩 표본 추출 => 분류기 생성 => 각 개체 추출확률 조정 (반복)  
    * 붓스트랩(Bootstrap)  
    : 중복 허용해 m개 추출 후 추출값의 통계값 산출
    * ex) AdaBossting (adaptive boosting) 알고리즘
  - 랜덤 포레스트  
    * 배깅에 랜덤 과정 추가한 방법  
    * 붓스트랩 표본 추출  
    * 노드별 예측변수 최적분할 안 함  
      : 예측변수 임의 추출 후 추출값 내에서 최적분할  
    * 새로운 자료 예측 
      * 분류 -> 다수결
      * 회귀 -> 평균

### 분류 분석 모형 평가
+ 평가기준
	- 일반화 가능성  
        : 같은 모집단의 타 데이터에 확장적용 시 결과 안정적인지
	- 효율성  
        : 분석모형의 효율성 (필요 입력변수 적을수록 효율적) 
	- 예측/분류 정확성  
        : 분석모형의 정확성 
+ 훈련/검증용 자료 추출방식
  - 홀드아웃 방법 Holdout method  
    * 원천 데이터를 훈련/검증용으로 랜덤하게 분류
    * 보통 70%를 훈련용으로 이용

    ```
    data(iris)
    nrow(iris)
    # [1] 150

    set.seed(1234)

    # 홀드아웃 수행
    idx <- sample(2, nrow(iris), replace=TRUE, prob=c(0.7, 0.3))
    trainData <- iris[idx==1,]
    testData <- iris[idx==2,]

    nrow(trainData)
    # [1] 112
    nrow(testData)
    # [1] 38
    ```    

  - 교차검증 Cross-Validation
    * 주어진 데이터로 성과 반복 측정해 평균낸 결과로 평가
    * k-fold 교차검증  
    : k개 하부집합으로 분할 -> k번째 하부집합으로 검증 -> k번 반복측정해 평균으로 최종평가

    ```
    data(iris)
    set.seed(1234)
    k=10

    # 데이터 무작위 섞음
    iris <- iris[sample(nrow(iris)),]

    # k 묶음 생성
    folds <- cut(seq(1, nrow(iris)), breaks=k, labels=FALSE)
    
    # 교차검증 수행
    trainData = list(0)
    testData = list(0)
    for(i in 1:k){
        testIdx <- which(folds==i, arr.ind=TRUE)
        testData[[i]] <- iris[testIdx, ]
        trainData[[i]] <- iris[-testIdx,]
    }
    ```

  - 붓스트랩 Bootstrap  
    * 복원추출법 기반: 관측치를 한번 이상 훈련용 자료로 사용
    * 보통 0.632 붓스트랩을 이용  
      : 관측치의 63.2%를 훈련용 자료로 사용
    * 데이터량 크지 않은 경우 모형평가에 가장 적합

+ 오분류표 (confusion matrix)

    |실제값|예측치|오분류값|예측|
    |---|---|---|---|
    |True|True|TP (True Positive)| 성공|
    |False|False|TN(True Negative)| 성공|
    |False|True|FP (False Positive)| 실패|
    |True|False|FN (False Negative)| 실패|

+ 오분류표 활용 지표
  - 정분류율
    * accuracy = (TP+TN) / (P+N)
    * 전체 관측치 중 예측 적중한 정도
    * 범주 분포 균형 이룰 때 효과적
  - 오분류율 
    * erroar rate = (FP+FN) / (P+N)
    * 전체 관측치 중 예측 실패한 정도
  - 민감도 
    * sensitivity = TP / P
    * 실제값 TURE인 관축치 중 예측 적중한 정도
    * 범주불균형 문제 대응
  - 특이도 
    * spcificity = TN / N
    * 실제값 False인 관축치 중 예측 적중한 정도
    * 범주불균형 문제 대응
  - 정확도 
    * Percision = TP / (TP+FP)
    * 예측값 TRUE인 관축치 중 예측 적중한 정도
  - 재현율 
    * Recall = TP / (TP+FN) = TP / P
    * 실제값 TRUE인 관측치 중 에측 적중한 정도
    * 민감도와 동일
    * 모형의 완전성 평가
  - F1 지표 
    * 정확도와 재현율에 같은 가중치 부여해 평균 = 조화평균
    * F1 = (2 * Percision * Recall) / (Percision + Recall)
  - Fb 지표 
    * 양수 b만큼 재현율에 가중치 부여해 평균
    * Fb = ((1 + b) ^2 * Percision * Recall) / (b^2 * Percision + Recall)  

+ confusionMatrix 함수

    ```
    ...
    library(nnet)
    library(rpart)

    # 신경망 학습    
    nn.iris <- nnet(Species~., data=trainData, size=2, range=0.1,   decay=52-4, maxit=200)
    # 의사결정나무 학습
    dt.iris <- rpart(Species~., data=trainData)

    # 모형별 예측치 도출
    m_pred <- predict(nn.iris, testData, type="class")
    dt_pred <- predict(nn.iris, testData, type="class")

    # 모형별 오분류표 도출
    install.packages("e1071")
    library(caret)
    nn_con = confusionMatrix(nn_pred, testData$Species)
    dt_con = confusionMatrix(nn_pred, testData$Species)
    nn_con$table

    # 정분류율
    accuracy <- c(nn_con$overall['Accuracy'], dt_con$overall  ['Accuracy'])
    # 정확도 　
    percision <- c(nn_con$byClass['Pos Pred Value'], dt_con$byClass  ['Pos Pred Value'])
    # 재현율　
    recall <- c(nn_con$byClass['Sensitivity'], dt_con$byClass  ['Sensitivity'])
    # F1지표 　
    f1 <- 2 * (percision * recall) / (percision + recall)

    # 결과출력: 신경망이 의사결정나무 모형보다 적중률 높음
    result <- data.frame(rbind(accuracy, precision, recall, f1))
    names(result) <- c("Nueral Network", "Decision Tree")
    result
    ```

+ ROC(receiver Operating Characteristic) 그래프
  - x축에 FP지수(1-특이도), y축에 민감도 나타내 모형평가
  - AUC 면적 넓을수록 좋은 모형  
    * AUC (Area Under the ROC Curve) : ROC 그래프의 밑부분
  - 분류 완벽한 경우  
    * x=0, y=1 -> AUC = 1     

    ```
    library(Epi)
    nerual_ROC <- ROC(form=case~net_pred, data=testData, plot="ROC")
    dtree_ROC <- ROC(form=case~dt_pred, data=testData, plot="ROC")
    ```

    ![]()

+ 이익도표(Gain Chart)   
: 목표범주에 속하는 개체들이 각 등급에 얼마나 분포되어 있는지 누적값 연결한 도표  
  - 분류분석모형으로 분류된 관측치가 각 등급별로 얼마나 포함되는지 표시   

    ```
    library(ROCR)

    # 모형별 예측
    n_r <- prediction(testData$net_pred, testData$case)
    d_r <- prediction(testData$dt_pred, testData$case)
    
    # 모형별 이익 도표
    n_p <- performance(n_r, "tpr", "fpr")
    d_p <- performance(d_r, "tpr", "fpr")
    
    # 모형별 도표 한꺼번에 시각화
    # 신경망 (색:검정)
    plot(n_p, col="red")
    # 의사결정나무 (색:검정)
    par(new=TRUE)
    plot(d_p, col="blue")
    # random 모델 그래프 (색:검정)
    abline(a=0, b=1)
    ```

    ![]()

+ 향상도 곡선  
: 랜덤모델과 비교해 성과가 얼마나 향상되었는지 등급별로 파악하는 그래프
  - 상위등급으로 갈수록 크고 하위등급으로 갈수록 작은 것이 바람직
  - 등급과 무관하게 값 차이 없는 경우 예측력 낮은 것

    ```
    # 향상도 곡선 (신경망 모형)
    n_lift <- performance(n_r, "lift", "rpp")
    plot(n_lift, col="red")
    abline(v=0.2)
    ```

    ![]()

## 3.3 군집 분석 (Cluster Analysis)
+ 반응(종속)변수 불필요 <- 개체간 유사성만으로 군집화
+ 과정 
  * 각 개체의 여러 변수값 관측   
  * 유사한 성격의 군집들로 집단화  
  * 군집들 특성 파악  
  * 군집 간 관계 분석  
+ 이상값 탐지, 사회/심리/경영 등 여러 분야에 이용

### 계층적 군집 (Hierarchical Clustering)
: 가장 유사한 개체들 묶는 과정 반복해 특정 개수 군집 형성  
=> 지역적 최적화이므로 전역적(global) 최적해 보장 불가

+ 군집 형성방법
  - 병합적 방법 (Agglomerative)  
  : 작은 군집 -> 큰 군집으로 병합 (군집 형성 후 개체 이동 불가)
  - 분할적 방법 (Divisive)  
  : 큰 군집 -> 작은 군집으로 분리

+ 군집간 거리 측정법  
: 그룹/항목간 상대적 거리 가까울수록 유사성 높음 

  |거리측정법|상세|
  |---|---|---|
  |최단연결법<br>= 단일연결법|각 군집 관측값 사이 최소거리 측정
  |최장연결법<br>= 완전연결법|각 군집 관측값 사이 최대거리 측정
  |중심연결법|- 두 군집의 중심간 거리 측정 <br>- 새 군집의 평균은 가중평균으로 도출|
  |평균연결법|모든 항목과 평균거리 도출하며 군집화<br>=> 불필요한 계산 증가 가능<br> => 크기 큰 군집과 병합 유도 가능|  
  |와드연결법| 군집내 오차제곱합 작아지도록 군집화<br>=> 크기 비슷한 군집과 병합하는 경향 <br> => 크기 큰 군집과 병합 유도 가능|

  ![](https://media.springernature.com/original/springer-static/image/chp%3A10.1007%2F978-3-030-17767-6_12/MediaObjects/473416_1_En_12_Fig8_HTML.png) 

+ 군집간 거리 정의
  - 연속형 자료

    |거리 정의|종류|상세|
    |---|---|---|
    |유클리드 거리| 수학적| n차원 공간의 두 점 사이 최단 거리 <br> => 대각선
    |맨하튼/시가 거리| 수학적| 격자형 공간 통과하는 최단거리 <br> => 너비 + 높이 
    |민코우스키 거리| 수학적| 맨하탄 거리 & 유클리디안 거리 한번에 표현 <br> => 설정값 따라 계산치 변화
    |표준화 거리| 통계적<br>표본분산 대각행렬| 변수의 측정단위 표준화
    |마할라노비스 거리| 통계적<br>표본공분산 행렬| 변수 표준화 + 변수 분포 고려

  - 명목형 자료  
    * 거리  
    : d(i,j) = (i와 j에서 다른 값 가지는 변수의 수)/(총 변수의 수)  
    * 단순일치계수(SMC, Simple Matching Coefficient)  
    : 일치하는 변수의 수 / 총 변수의 수 
    * 자카드 계수(Jaccard coefficient)  
    : A ∩ B / A ∪ B 
  - 순서형 자료
    * 순위상관계수(rank correlation coefficient)   
      ex) 스피어만 상관계수  

+ 계층적 군집 함수
  - hclust 함수 (병합적 방법)

    ```
    data(USArrests)
    str(USArrests)

    # 거리/비유사성 행렬 도출
    d <- dist(USArrests, method="maximum")
    d <- dist(USArrests, method="manhattan")
    d <- dist(USArrests, method="binary")
    d <- dist(USArrests, method="minkowski")
    d <- dist(USArrests, method="euclidean")

    # 계층적 군집분석
    fit <- hclust(d, method="ward.D") # ward 옵션명 갱신됨
    fit <- hclust(d, method="single")
    fit <- hclust(d, method="complete")
    fit <- hclust(d, method="centroid")
    fit <- hclust(d, method="average")
    fit <- hclust(d, method="ave")

    # 그래프 표시 프레임 설정
    par(mfrow=c(1,2))    # make frame -> 1 row, 2 col

    # 댄드로그램 시각화
    plot(fit)
    plot(fit, hang = -1)

    # 계층적 군집 결과로 그룹화
    groups <- cutree(fit, k=6)   # h=tree 높이, k=그룹 수  
    groups

    # 댄드로그램 시각화 (그룹별 사각형 표시)
    par(mfrow=c(1,1))
    plot(fit)
    rect.hclust(fit, k=6, border="red")

    # 그룹 일부만 표시
    rect.hclust(fit, h=50, which=c(2,7), border=3:4) 
    ```
    ![](https://z-images.s3.amazonaws.com/thumb/d/dc/Rplot06.png/600px-Rplot06.png)

  - agnes 함수 (병합적 방법)

    ```
    library(cluster)

    # 계층적 군집 수행 (거리:맨해튼 거리)
    agn1 <- agnes(USArrests, metric="manhattan", stand=TRUE)
    par(mfrow=c(1,2))
    plot(agn1)

    # 계층적 군집 수행 (거리: daisy 함수로 계산)
    agn2 <- agnes(daisy(USArrests), diss=TRUE, method="complete")
    plot(agn2) 

    # 계층적 군집 수행
    agn3 <- agnes(USArrests, method ="flexible", par.meth=0.6)
    plot(agn3)
    par(mfrow=c(1,1))
    ```

    ** daisy함수  
    : 수치형 자료 아니어도 적용 가능한 거리측정 함수    
    (metric="euclidean", "manhattan", "gower"(범주형 자료용)

### k-평균 군집
: 초기값 k개(목표 군집수) 지정해 초기군집 형성 => 군집평균 계산 반복 => 최종군집 형성 

+ 군집과정

  1) k개 개체를 군집중심으로 임의 선택
  2) 각 자료를 가장 가까운 군집중심에 할당  
    (할당방식: 군집중심점(평균)과 오차제곱합 최소화) 
  3) 군집내 자료 평균 계산 -> 군집중심 갱신
  4) 군집중심 변화 거의 없거나 최대 반복수까지 갱신 반복

+ 특징
  - 초기값 서로 멀수록 바람직
  - 초기값에 따라 결과 크게 변화  
  => set.seed()로 초기값 고정 가능  
  - 부분최적화 수행  
    * 탐욕적 알고리즘으로 간주 
    * 안정된 군집 보장
    * 전역적 최적해 보장 불가
  - 장점
    * 알고리즘 단순 
    * 빠른 처리속도 
    * 계층군집보다 대량자료 소화 
  - 단점
    * 연속 변수만 이용가능
    * 잡음/이상값에 민감  
    => 탐색적 자료분석으로 제거 후 군집  
    => k-중앙값 군집으로 보완가능  
    * [볼록하지 않은(non-convex)](http://sanghyukchun.github.io/63/)  군집 존재 시 성능저하   
    ex) U자형 군집  

+ 집단내 제곱합 그래프

    ```
    # 잔차제곱합 그래프 정의
    wssplot <- function(data, nc=15, seed=1234){
        wss <- (nrow(data) - 1) * sum(apply(data, 2, var))
        for(i in 2:nc){
            set.seed(seed)
            wss[i] <- sum(kmeans(data, centers=i)$withinss)
            }
        plot(1:nc, wss, type="b", xlab="Number of Clusters", ylab="Within groups sum of squares")    
    }

    # 잔차제곱합 그래프 출력
    wssplot(USArrests)
    ```
    ![](https://i1.wp.com/www.dodomira.com/wp-content/uploads/2016/02/some-of-square.png?resize=660%2C489)

+ 군집수 결정

    ```
    library(NbClust)
    set.seed(1234)

    # 지수별 최적 군집수 투표결과 출력
    nc <- NbClust(USArrests, min.nc=2, max.nc=15, method="kmeans")
    talbe(nc$Best.n[1,])

    # 0 1 2 3 10 12 14 15   # 군집수
    # 2 1 4 15 1  1  1  1   # 해당 군집수 추천한 지수 개수
    ```

+ k-군집 수행

  ```
  set.seed(1234)

  # k-군집 생성(군집수: 3개)
  fit.km <- kmeans(USArrests, 3, nstart=25)

  fit.km$size
  fit.km$centers

  # 군집별 크기
  # [1] 14 16 20

  # 군집 & 변수별 중심(평균)값
  #      Murder  Assault UrbanPop     Rape
  # 1  8.214286 173.2857 70.64286 22.84286
  # 2 11.812500 272.5625 68.31250 28.37500
  # 3  4.270000  87.5500 59.75000 14.39000

  # 시각화
  plot(USArrests, col=fit.km$cluster)
  points(fit.km$center, col=1:3, pch=8, cex=1.5)

  # 군집별 변수 요약값 출력
  aggregate(USArrests[-1], by=list(fit.km$cluster), mean)

  # 군집결과 정오분류표 출력
  names(USArrests)[1] <- "State"
  ct.km <- table(USArrests$State, fit.km$cluster)
  ct.km

  # 수정된 순위지수 (군집간 일치도) 출력
  library(flexclust)
  randIndex(ct.km)      

  # 순위지수 범위
  # -1: no agreement
  #  1: perfect agreement
  ```

+ k-평균군집 수행 (kcca 함수)

    ```
    library(flexclust)
    data("Nclus")

    # k-평균군집 생성
    cl <- kcca(Nclus, k=4, family=("kmeans"))

    # 시각화
    image(cl)
    points(Nclus)

    barplot(cl)     # 군집별 중심과 전체군집 중심 비교  
    stripes(cl)     # 군집내 자료와 군집평균 거리 줄무늬로 표시
    ```

    ** familiy 옵션: kmeans, kmedians, angle, jaccard, ejaccard

+ k-평균군집 수행 (cclust 함수)

    ```
    # k-평균군집 생성
    library(cclust)
    cl.1 <- cclust(Nclus, 4, 20, method="kmeans")
    
    # 2차원 군집그래프 
    library(cluster)
    clusplot(Nclus, cl.1$cluster)
    ```

### 혼합분포 군집(Mixture Distribution Clustering)
: 모형(model)기반 군집 <- k개의 모수적 모형의 가중합으로 표현되는 모집단 모형을 가정
+ 군집방식   
  : k개 모형 중 어디서 유래됐는지의 확률에 따라 군집화 
+ 복잡한 모집단(다봉형/원형 등) 분포 근사 가능 
  - 충분히 많은 정규분포 고려 필요
  - 모형이 꼭 정규분포일 필요는 없음 
+ 모수와 가중치 추정  
  : [EM (기대값 최대화) 알고리즘](https://zetawiki.com/wiki/%EA%B8%B0%EB%8C%93%EA%B0%92_%EC%B5%9C%EB%8C%80%ED%99%94_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98) 사용 
  - 진행과정
    * E단계: 잠재변수 Z 기대치 계산
    * M단계: 잠재변수 Z 기대치로 모수 추정
    * 도출: EM 반복해 수렴값 도출  => 최대가능도 추정치
  - 잠재변수 Z  
   : 각 자료가 속하는 집단(클래스)의 정보
  - 각 집단은 정규분포 따른다고 가정
+ 특징 
  - k-평균군집과 유사한 절차에 확률분포 도입한 방식
  - 데이터 클 경우 수렴에 시간소모
  - 데이터 작을 경우 추정도 하락
  - 이상값 자료에 민감 => 사전에 제거 필요
+ normalmixEM 함수

    ```
    library(mixtools)
    
    # 미국 올드스페이스풀 간헐천 분출 간의 시간 자료
    data(faithful)
    attach(faithful)

    # 정규혼합분포 추정 (EM알고리즘 이용)
    wait1 <- normalmixEM(waiting, lambda=.5, mu = c(55, 80), sigma=5)
    # number of iterations = 9

    # 혼합분포 결과 요약
    summary(wait1)
    
    # summary of normalmixEM object:
    #          comp 1   comp 2
    # lambda  0.36085  0.63915
    # mu     54.61364 80.09031
    # sigma   5.86909  5.86909
    # loglik at estimate:  -1034.002 

    # 혼합분포 시각화
    plot(wait1, density=TRUE, cex.axis=1.4, cex.lab=1.4, cex.main=1.8, main2="Time between Faithful eroptions", xlab2="Minutes")
    # 모수 추정 과정에서 반복 2회만에 로그-가능도함수가 최대가 됨
    ```

    ![](https://i.stack.imgur.com/t1V54.jpg)

+ Mclust 함수

    ```
    library(mclust)

    # 군집분석
    mc <- Mclust(iris[,1:4], G=3)

    # 혼합분포 모수추정치 & 각 군집별 해당 자료 요약결과 출력
    summary(mc, parameters=TRUE)

    # model-based clustering plots
    plot.Mclust(mc)

    # 각 개체가 분류된 그룹 확인
    str(mc)
    mc$classification

    # 새로운 자료 분류
    predict(mc, data=)
    ```

### 자기조직화 지도 SOM(Self-Organizing Map)
: 고차원 데이터 -> 저차원 뉴런으로 정렬 -> 지도 형태로 형상화한 비지도 신경망   

+ 특성
  - 입력변수 정보와 관계가 지도 상에 그대로 나타남   
    * 이해 용이  
    * 변수 위치관계 보존
  - 전방패스(Feed Forward Flow) 방식 
    * 신경망 역전파 방식보다 속도 빠름  
    * 실시간 학습처리 가능 (잠재적)
  - 패턴발견/이미지 분석 등에 뛰어남 

+ 구성  
: 2개의 인공신경망 층으로  구성
  - 입력층
    * 입력벡터 받는 층
    * 뉴런 수: 입력변수의 개수 
  - 경쟁층
    * 입력벡터 군집되는 층
    * 2차원 격차로 구성
    * 뉴런 수: 목표 군집 개수
+ 학습 알고리즘

  1) SOM 노드에 대한 연결강도 초기화
  2) 입력벡터 제시 
  3) 입력벡터 & 프로토타입 벡터 유사도 계산 => BMU 탐색  
     * 프로토타입 벡터  
        : 경쟁층의 각 뉴런  
     * BMU  
       : 가장 가까운 프로토타입 벡터 (유클리드 거리 이용)
  4) BMU와 위상학적 이웃의 연결강도 재조정
     * 조정시 승자독점 학습규칙 이용
     * mi(t+1) = mi(t) + a(t)hei(t)[x-mi(t)]  
       - t:시간
       - a(t): 학습률
       - hei(t): BMU 중심에 있는 이웃커널  
  5) 2단계부터 반복 수행
     * 경쟁층에는 승자 뉴런만 나타남
     * 승자와 유사한 연결강도 갖는입력패턴이 동일한 경쟁 뉴런으로 배열

+ som 함수  
: som(data, grid=somgrid(), rlen=100, alpha=c(0.05, 0.01), init, toroidal=FALSE, keep.data=TRUE)

    |인수|상세|비고|
    |---|---|---|
    | grid | 결과 표시 그리드
    | rlen | 학습횟수 |기본값: 100
    | alpha | 학습률 |기본값: 0.05, 0.01
    | radius | 이웃의 초기반경
    | init | SOM 학습으로 얻은 <br> 코드북 벡터의 옵션 행렬 ??? | 기본: 랜덤한 객체
    | toroidal | 맵의 엣지 조인 여부
    | keep.data | 반환데이터 저장 

    |반환인자|상세|
    |---|---|
    | grid| somegrid 객체 ???
    | changes| 코드벡터로부터 평균편차의 벡터 ???
    | classif| 승자 유닛 ???
    | toroidal| toroidal 맵 사용여부 ???
    | data| 데이터 행렬

    ```
    library(kohonen)
    data("wines")
    str(wines)

    wines.sc <- scale(wines)
    set.seed(7)

    # SOM 군집
    wine.som <- som(data = wines.sc, grid=somgrid(5, 4, "hexagonal"), rlen=100, alpha=(0.05, 0.01), toroidal=FALSE, keep.data=TRUE)

    summary(wine.som)
    plot(wine.som, main="Wine data")
    ```

+ plot.kohonen 함수  
: plot.kohonen(x, type=c("changes"...), classif, labels=NULL, pchs=NULL, main=NULL, palette.name=heat.colors, nclors, zlim=NULL, property, heatkey=TRUE, contin, ...) 

    |인수|상세|비고|
    |---|---|---|
    |x| 코호넨 객체
    |type| 플롯 타입| changes, codes, counts, mapping, prediction, property    
    |classif| predict.kohonen에서 반환된 분류 객체
    |labels| 플롯 라벨 |타입이 분류인 경우
    |pchs| 플롯 심볼 |타입이 분류인 경우
    |palette.name| type 위한 유닛 배경색
    |ncolors| 색상 수 |기본값: 20
    |zlim| 유닛 배경색 범위
    |property| type의 속성값
    |heatkey| 히트키 생성 여부 ??? | 타입이 count/property 인 경우
    |contin| 데이터 연속형/범주형 여부

    ```
    par(mfrow=c(1,3))
    plot(wine.som, type="counts", main="wine data: counts")
    plot(wine.som, type="quality", main="wine data: mapping Quality")
    plot(wine.som, type="mapping", main="mapping plot", labels=wine.classes, col=wine.classes)

    # 색상 지정
    color1 <- tricolor(wine.som$grid)
    color2 <- tricolor(wine.som$grid, phi=c(pi/6, 0, -pi/6))
    color3 <- tricolor(wine.som$grid, phi=c(pi/6, 0, -pi/6), offset=.5)

    plot(wine.som, type="mapping", bg=rgb(color1))
    plot(wine.som, type="mapping", bg=rgb(color2))
    plot(wine.som, type="mapping", bg=rgb(color3))

    # 거리계산
    par(mfrow=c(1,2))
    dists <- unit.distances(wine.som$grid, toroidal=FALSE)
    
    # 6색 표시
    plot(wine.som, type="property", property=dists[1,], main="Distances to unit1", zlim=c(0,6), palette=rainbow, ncolors=7, contin=TRUE)
    # 2색 표시
    plot(wine.som, type="property", property=dists[1,], main="Distances to unit1", zlim=c(0,2), palette=rainbow, ncolors=2, contin=TRUE)
    ```

+ 유사도 변화

    ```
    ...
    # SOM 군집(학습:100회,500회)
    wine.som <- som(data = wines.sc, grid=somgrid(5, 4, "hexagonal"), rlen=100, alpha=(0.05, 0.01), toroidal=FALSE, keep.data=TRUE)
    wine.som_1 <- som(data=wines.sc, grid = somgrid(5, 4, "hexagonal"), rlen=500, alpha=c(0.05, 0.01), toroidal=FALSE, keep.data=TRUE)

    # 유사도 변화 그래프 비교
    par(mfrow=c(1,2))
    plot(wine.som, type="changes", main="Wine Data: SOM(Learning no=100)")
    plot(wine.som_1, type="changes", main="Wine Data: SOM(Learning no=500)")
    ```

+ ggplot

    ```
    library(ggplot2)

    wines.sc = as.data.frame(wines)
    wines.sc$clusterX <- wine.som$grid$pts[wine.som$unit.Classif, "x"]
    wines.sc$clusterX <- wine.som$grid$pts[wine.som$unit.Classif, "y"]

    # SPSS Modeler와 유사하게 도식화
    p <- ggplot(wines.sc, aes(clusterX, clusterY))
    p + geom_jitter(position=position_jitter(width=0.4, height=0.3))
    ```

## 3.4 연관 분석 (=장바구니 분석)
: 유용한 규칙/피턴 등 연관규칙 발굴해 품목간의 관계/연관성 탐색  
=> 마케팅 분야에 많이 사용  
ex) 장바구니 데이터 분석 등

** 장바구니 (market basket) 데이터  
** 거래 (transaction): 장바구니 하나에 해당하는 정보

### 연관규칙(Association Rule)  
: 항목들간의 조건-결과 식으로 표현되는 유용한 패턴
-  유용성  
   : 사소한/일반적 사실 아닌 분명하고 유용해야  
    ex) [컴퓨터 구매 -> 마우스/키보드 구매]는 당연하므로 유용한 규칙 아님  
-  보통 조건과 반응 형태  
   : if- A then B

### 연관규칙의 측정지표
: 도출된 연관규칙의 유의미함 평가지표

+ 지지도(support)  
: 전체 거래 중에서 두 품목이 동시에 포함되는 거래 비율
  - 지지도 = 동시에 포함된 거래수 / 전체거래수
  - 높을수록 적용성 높음
  - A -> B: A 샀던 고객이 B 산 비율  

+ 신뢰도(confidence)  
: A가 포함된 거래 중 A, B 동시에 포함하는 거래일 확률
  - 신뢰도 = A & B 동시에 포함된 거래수 / A 포함하는 거래수
  - 연관성의 정도 파악 가능 

+ 향상도(lift)  
: B 구매한 고객 대비 A 구매후 B 구매하는 고개에 대한 확률
  - 향상도 = A & B 포함하는 거래수 / (A 포함 거래수 * B 포함 거래수)   
    * 향상도 = 1 : 서로 독립
    * 향상도 > 1 : 양의 상관관계 (연관성 높음)
    * 향상도 < 1 : 음의 상관관계 (연관성 없음)

|지표|수식|의미|
|---|---|---|
|지지도|A & B 동시포함 거래수 / 전체거래수| 적용성|
|신뢰도|A & B 동시포함 거래수 / A 포함 거래수| 연관성정도|
|향상도|A & B 동시포함 거래수 / (A 거래수 * B 거래수) | 상관관계|

=> 최소 지지도 정해서 낮은 값으로 낮춰가며 규칙 도츨 시 효율적

### 연관분석 절차 (Apriori 알고리즘)
1) 최소 지지도 설정
2) 최소 지지도 넘는 모든 품목 탐색
3) 탐색된 품목 중 최소 지지도 넘는 2가지 품목 집함 탐색
4) 탐색된 품목 집합 결합해 최소 지지도 넘는 3가지 품목 집합 탐색
5) 반복 수행해 최소지지도 넘는 빈발품목 집합 탐색

### 연관분석 장점
+ 탐색적 기법 => 이해 쉬움
+ 비목적성 기법 => 분석방향/목적 없는 경우 유용
+ 사용 편리한 데이터 형태 => 거래 데이터 변환없이 분석 가능
+ 계산 용이성 => 분석 계산식이 간단한 편   

### 연관분석 단점
+ 계산과정 증가 => 품목수에 따라 기하급수적 증가
+ 세분화 심한 경우 => 무의미한 결과 나올수도 
+ 품목의 비율차이 => 거래량 적은 품목 무시됨

### 순차패턴
: 구매시점 정보 존재 => 구매순서 고려해 연관성 측정 및 연관규칙 탐색  
ex) 컴퓨터 신규구매 고객 중 25%가 다음달에 레이저 프린터 구입

### 연관규칙 도출 실습

```
library(arules)
data(Adult)

# 연관규칙 발굴
# : 지지도/신뢰도 설정 없어 무의미한 rhs(right hand side) 결과 발생
rules <- apriori(Adult)

# 발굴된 규칙 출력 
inspect(head(rules))

# 연관규칙 발굴
adult.rules <- apriori(Adult, parameter=list(support=0.1, confidence=0.6), appearance = list(rhs=c("income=small", "income=large"), default="lhs"), control=list(verbose=F))

adult.rules.sorted <- sort(adult.rules, by="lift")
inspect(head(adult.rules.sorted))

# 연관규칙 시각화
library(arulesViz)

# 연관규칙의 지지도/신뢰도/향상도 산점도
plot(adult.rules.sorted, method="scatterplot")

# 연관규칙 관계도
plot(adult.rules.sorted, method="graph", control=list(type="items", alpha=0.5))
```
