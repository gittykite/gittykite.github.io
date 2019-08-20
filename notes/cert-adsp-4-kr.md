# ADsP :: 과목4. 데이터 분석

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
- 밀집화와 다르게 데이터가 가진 정보 유지
- melt 단계
- cast 단계

```
```

** 밀집화(Aggregation): 데이터 축소/재정렬해 사용편의성 증대   
ex) 엑셀의 피벗 테이블 

### sqldf
- 표준 SQL 문장 사용 가능 
- 싱글쿼트 이용해 특수문자 입력 가능
- Mac 버전에서는 에러 발생할 수도

```
sqldf("select * from iris where Species like 'se%' limit 10")
```

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
```

### 데이터 테이블
- 데이터프레임과 유사
- 빠른 그룹화 / 순서화 / 짧은 문장 지원
- 64Bit 환경에서 RAM 충분할 때 효율적

```
```

## 1.3 결측값 & 이상값 처리

### 데이터 탐색
: 데이터 분석 전 다각도로 접근 => 대략의 데이터 특성 파악 & 통찰 
+ 범주형 변수
  - 각 범주 빈도수 출력해 데이터 분포 파악 
+ 연속형 변수
  - 기초 통계치 출력
  - 공분산/상관계수 행렬 출력해 변수 간 선형 상관관계 강도 확인

```
```

```
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
