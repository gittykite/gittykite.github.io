---
title: "[Python] 국회의원 발의법률안 API + 크롤링"
categories:
  - code
tags:
  - Python
  - 크롤링
---

## 0. 파이썬 환경구축

작업에 사용된 파이썬 환경은 다음과 같다. 

+ 가상환경: 아나콘다 4.9.2
+ 파이썬: 3.8.6
+ 패키지
  - pandas: 데이터 조작 및 저장
  - bs4 (beautifulsoup): 크롤링 라이브러리
  - lxml: bs4가 의존하는 XML 조작 라이브러리
  - requests: API 요청 처리

사용하는 파이썬/가상 환경에 따라 패키지를 설치한다.

일반 파이썬
```
pip install
```

아나콘다 환경
```
conda install pandas bs4 lxml requests
```


## 1. 열린국회 API 

열린국회에 접속하면 국회에서 제공하는 API 목록을 확인할 수 있으며, API별 메타데이터, 샘플URL 등을 확인할 수 있다. 

열린국회 홈페이지  
> https://open.assembly.go.kr/portal/mainPage.do

인증키 발급을 위해서는 회원가입이 필요하며, 로그인 후 [OPEN API 서비스](https://open.assembly.go.kr/portal/openapi/main.do)에 접속해 [마이페이지](https://open.assembly.go.kr/portal/openapi/openApiActKeyPage.do)에 들어가면 신규발급 또는 기존 인증키 확인이 가능하다.  

### 1.1. 국회의원 발의법률안 API 
열린국회에서 제공하는 API 중 이번에 사용할 API는 "국회의원 발의법률안 API"다. 국회 의안정보시스템에는 예산안 및 결산안, 동의안 등 여러 종류의 의안이 올라오며, 법률안 또한 정부안, 위원회안 등으로 나눠지는데. 이 중에서 국회의원이 발의안 법률안 만을 제공하는 API인 것이다.

해당 API의 [상세 페이지에](https://open.assembly.go.kr/portal/data/service/selectAPIServicePage.do/OK7XM1000938DS17215)서 제공되는 주요 정보를 살펴보면 다음과 같다.

|명칭|URL|
|---|---|
|기본 API URL | https://open.assembly.go.kr/portal/openapi/nzmimeepazxkubdpn|
|샘플 URL|https://open.assembly.go.kr/portal/openapi/nzmimeepazxkubdpn?AGE=21 <br> * 21대 국회의원 발의법률안 요청 (형식:xml, 페이지:1, 페이지당 건수: 5)|

|요청인자|명칭|특성|기타|
|---|---|---|---|
|KEY            |인증키|KEY|공통필수|default: sample key|
|Type           |출력형식|Type|공통필수|xml, json <br> default: xml|
|pIndex         |페이지 위치|pIndex|공통필수|default: 1 <br> sample: 1|
|pSize          |페이지당 건수|pSize|공통필수|default: 100 <br> sample: 5|
|필수            |대수|AGE|필수|ex) 21|
|BILL_ID        |의안ID|BILL_ID|-|ex) PRC_Z2Z1Z0Z3X2L4M0H9A2V6K5R0V7P2H1|
|BILL_NO        |의안번호|BILL_NO|-|ex) 2113663|
|BILL_NAME      |법률안명|BILL_NAME|-|ex) 신용협동조합법 일부개정법률안|
|COMMITTEE_ID   |소관위원회ID|COMMITTEE_ID|-|ex) 9700516|
|COMMITTEE      |소관위원회|COMMITTEE|-|ex) |
|PROC_RESULT    |처리상태|PROC_RESULT|-|ex) 대안반영폐기|
|PROPOSER       |제안자(대표발의)|-|ex) 강경식의원|

|No |출력변수|명칭|
|---|---|---|
|1	|BILL_ID	    |의안ID|
|2	|BILL_NO	    |의안번호|
|3	|BILL_NAME	    |법률안명|
|4	|COMMITTEE	    |소관위원회|
|5	|PROPOSE_DT	    |제안일|
|6	|PROC_RESULT	|처리상태|
|7	|AGE	        |대수|
|8	|DETAIL_LINK	|상세페이지|
|9	|PROPOSER	    |제안자|
|10 |MEMBER_LIST	|제안자목록링크|
|11 |RST_PROPOSER	|대표발의자|
|12 |PUBL_PROPOSER	|공동발의자|
|13 |COMMITTEE_ID	|소관위원회ID|


### 1.2. 모듈로드 및 API 클래스 작성
우선 사용할 모듈들의 import 구문을 아래와 같이 작성한다.

```python
# 모듈 로드
# 기본모듈
import datetime
import time
from enum import Enum, unique

# 라이브러리
import pandas as pd
import requests 
import urllib.request
from urllib.parse import urlparse, parse_qs, quote
from bs4 import BeautifulSoup 
```

상수 변수 적용을 위한 데코레이터도 추가해준다.
```python
# 데코레이터
def constant(func):
    def func_set(self, value):
        raise TypeError

    def func_get(self):
        return func()
    return property(func_get, func_set)
```

재활용 가능성이 높은 유틸리티 함수를 아래와 같이 작성하였다. 타임스탬프는 파일 저장 시 파일명을 설정할 때, 데이터프레임 변환은 다른 API를 추가할 때 활용할 수 있을 것이다.
```python
# 유틸리티 함수
# 타임스탬프(초단위)
def now_timestamp():
    return datetime.datetime.now().strftime("%y%m%d%H%M%S")

# XML -> 데이터프레임 변환
def xml_to_df(xml, row_tag="row", headers_dic={}):
    buf = []
    cols = headers_dic.values()

    # 헤더 설정
    headers = headers_dic.keys()

    # XML에서 데이터 추출 (헤더 기준)
    rows = xml.find_all(row_tag)
    for item in rows:
        item_data = []
        for header in headers:
            value = item.find(header).text
            item_data.append(value)
        buf.append(item_data)    

    # 데이터프레임 객체 생성
    df = pd.DataFrame(buf, columns=cols)
    return df
```

OPEN API 이용 시 주로 활용되는 변수들을 묶어 작성한 아래 클래스를 추가한다.
```python
# OpenAPI 공통 클래스
class OpenAPI:
    # 초기화 함수
    def __init__(self, api_name, api_key, search_url, url_delimeter):
        self._api_name = api_name
        self._api_key = api_key
        self._search_url = search_url
        self._url_delimeter = url_delimeter

    # 문자열 출력 함수
    def __str__(self):
        return f'str : {self.api_name} - {self.search_url}'

    # 클래스 변수
    # API 명칭
    @property
    def api_name(self):
        return self._api_name

    # API 인증키
    @property
    def api_key(self):
        return self._api_key

    # 조회 URL
    @property
    def search_url(self):
        return self._search_url

    # URL 파라미터 구분자
    @property
    def url_delimeter(self):
        return self._url_delimeter

    # URL 생성 함수
    def params_url(self, base_url, delimeter="",  *args):
        url = base_url    
        for param in args[0]:
            url += delimeter + str(param)
        
        print("URL:", url)
        return url    

    # CSV 파일 요청 함수 
    def get_csv(self, path, url):
        urllib.request.urlretrieve(url, path)
        return path        

    # JSON 데이터 요청 함수 
    def get_json(self, url):
        ret = requests.get(url)
        json = ret.json()        
        return json

    # xml 데이터 요청 함수 
    def get_xml(self, url, **kwargs):
        ret = requests.get(url, params=kwargs)

        # XML 추출 (오류 시 lxml 패키지 설치)
        xml = BeautifulSoup(ret.text,'xml')
        return xml
```

위의 클래스를 상속한 의안정보시스템 OpenAPI 클래스를 아래와 같이 작성한다.
```python
# 의안정보시스템 OpenAPI 클래스
class LIKMS(OpenAPI):
    def __init__(self, api_key):
        self._api_name = "의안정보시스템(LIKMS)"
        self._search_url = "https://open.assembly.go.kr/portal/openapi"
        self._url_delimeter = "/"
        super().__init__(self._api_name, api_key, self._search_url, self.url_delimeter) 

    # 클래스 변수
    def URL_BILL_DETAIL():
        return "https://likms.assembly.go.kr/bill/billDetail.do?billId="

    def URL_BILL_FILE_HWP():
        return "https://likms.assembly.go.kr/filegate/servlet/FileGate?type=0&bookId="

    def URL_BILL_FILE_PDF():
        return "https://likms.assembly.go.kr/filegate/servlet/FileGate?type=1&bookId="
```

### 1.3. 국회의원발의 법률안 데이터 요청하기
의안정보시스템 OpenAPI 클래스에 국회의원발의 법률안 API의 요청인자와 출력변수의 기본값을 잡아주는 함수를 추가한다. 인증키는 객체 생성 시에만 설정되도록 클래스 변수를 적용하였다.
```python
    # 조회요청 파라미터 딕셔너리
    def search_params_dic(self):
        params_dic = {
            "KEY" : self.api_key     # API 인증키 (필수)
            , "Type" : "xml"         # 파일형식: XML (필수)
            , "pIndex" : "1"         # 페이지 위치 (필수)
            , "pSize" : "100"        # 페이지당 건수 (필수)
            , "AGE" : "20"           # 대수 (필수)
            , "BILL_ID" : ""         # 의안ID
            , "BILL_NO" : ""         # 의안번호
            , "BILL_NAME" : ""       # 법률안명
            , "COMMITTEE" : ""       # 소관위원회
            , "COMMITTEE_ID" : ""    # 소관위원회ID
            , "PROC_RESULT" : ""     # 처리상태
            , "PROPOSER" : ""        # 제안자
        } 
        return params_dic

    # 조회결과 헤더 딕셔너리
    def search_headers_dic(self):
        headers_dic = {
            "BILL_ID"           : "의안ID"
            , "BILL_NO"	        : "의안번호"
            , "BILL_NAME"	    : "법률안명"
            , "COMMITTEE"	    : "소관위원회"
            , "COMMITTEE_ID"    : "소관위원회ID"
            , "PROC_RESULT"	    : "처리상태"
            , "PROPOSER"	    : "제안자"
            , "MEMBER_LIST"	    : "제안자목록링크"
            , "AGE"	            : "대수"
            , "PROPOSE_DT"	    : "제안일"
            , "DETAIL_LINK"	    : "상세페이지"
            , "RST_PROPOSER"    : "대표발의자"
            , "PUBL_PROPOSER"	: "공동발의자"
        } 
        return headers_dic
```

사용하는 API의 종류를 결정하는 서비스 코드는 향후 확장을 고려해 딕셔너리 함수로 의안정보시스템 OpenAPI 클래스에 추가하였다.
```python
    # 서비스코드 취득
    def get_svc_code(self, svc_name):
        svc_code_dic = {
            "SEARCH_BILL" : "nzmimeepazxkubdpn"
        }
        svc_code = svc_code_dic[svc_name]
        return svc_code
```

결과값이 XML로 출력되는 국회의원 발의법률안 조회함수를 아래와 같이 의안정보시스템 OpenAPI 클래스에 추가한다. 입력된 요청인자를 이용해 URL이 생성되면 부모함수에서 정의된 get_xml 함수에서 request HTTP 요청 처리가 진행된다.  
```
    # 국회의원 발의법률안 조회 함수(XML)
    def search_xml(self, svc_code, assembly_age, pIndex, pSize="100", bill_id="", bill_no="", bill_name="", committee="", committee_id="", proc_result="", proposer=""):
        params_dic = self.search_params_dic()

        # 필수인자 설정
        params_dic["pIndex"] = pIndex       # 페이지 위치 (필수)
        params_dic["pSize"] = pSize         # 페이지당 건수 (필수)
        params_dic["AGE"] = assembly_age    # 대수 (필수)    

        # 선택인자 설정
        params_dic["BILL_ID"] = bill_id             # 의안ID    
        params_dic["BILL_NO"] = bill_no             # 의안번호
        params_dic["BILL_NAME"] = bill_name         # 법률안명    
        params_dic["COMMITTEE"] = committee         # 소관위원회    
        params_dic["COMMITTEE_ID"] = committee_id   # 소관위원회ID            
        params_dic["PROC_RESULT"] = proc_result     # 처리상태        
        params_dic["PROPOSER"] = proposer           # 제안자    

        url = self.params_url(self.search_url, "/", [svc_code])
        xml = self.get_xml(url, **params_dic)
        return xml
```

아래와 같이 객체를 생성하고 테스트 해본다.
```python
# TEST 1
# 의안정보시스템 OpenAPI 클래스 객체 생성
api_likms = LIKMS("발급한_인증키_값")

# 요청인자 설정
svc_code = api_likms.get_svc_code("SEARCH_BILL")
assembly_age = "20"
pIndex = "1"
pSize = "10"

# 국회의원 발의법률안 조회함수 호출
xml = api_likms.search_xml(svc_code, assembly_age, pIndex, pSize)

# 결과 출력
print("xml:") 
print(xml)
```

### 1.4. 국회의원발의 법률안 데이터 CSV로 저장하기
특정 대수에 발의된 모든 국휘의원발의 법률안 데이터를 저장하기 위해 아래와 같이 while 반복문이 포함된 저장함수를 추가한다. API는 XML 형태로 결과를 출력하므로 이를 데이터프레임으로 변환해 페이지가 끝날 때까지 리스트에 저장하고. 리스트에 담긴 데이터는 pandas의 concat 함수를 이용해 병합한 뒤 CSV 파일로 저장한다. (경로를 따로 지정하지 않았으므로 동일 디렉토리에 저장됨)
```python
# 국회의원 발의법률안 저장 (대수별)
def save_bills(api_likms, assembly_age, index=1, size=1000):
    print("####### save_bills START ######")

    dfs = []
    rows = 0
    data_size = size
    svc_code = LIKMS.get_svc_code("SEARCH_BILL")

    # 페이징 처리 반복문
    while(data_size == size):
        try:
            # 국회의원 발의법률안 조회
            xml = api_likms.search_xml(svc_code, assembly_age, str(index), str(size))

            # 데이터 변환 처리
            df = xml_to_df(xml, headers_dic=api_likms.search_headers_dic())
            # print(df)

            # 데이터 개수 갱신
            data_size = len(df)
            rows += data_size
            print(data_size)

            # 데이터프레임 리스트에 추가
            dfs.append(df)

        except:
            # 에러 발생 시 루프 중지
            print("ERROR at page ", index)
            break

        # 페이지 인덱스 갱신
        index += 1

    print("PAGE:", index -1, ", ROWS:", rows, "DFS:", len(dfs))

    # CSV 파일로 저장
    csv_path = "data" + assembly_age + "_" + now_timestamp() + ".csv"
    df_total = pd.concat(dfs)
    df_total.to_csv(csv_path, mode="w")

    print("####### save_bills END ######")
```

아래 코드를 테스트 추가해 20대 국회에서 발의된 의원발의 법률안 정보를 저장해본다.
```python
# TEST 2
assembly_age = "20"
save_bills(api_likms, assembly_age)
```

아래와 같이 저장된 총 페이지수와 데이터 건수가 출력되고, CSV 파일이 생성되면 정상적으로 저장되었음을 알 수 있다.
```
$ PAGE: 22 , ROWS: 21594 DFS: 22
$ ####### save_bills END ######
```

## 2. 의안정보시스템 크롤링
국회의원발의 법률안 API에서 제공되는 정보는 한정적이기 때문에 추가적인 정보를 추출하려면 크롤링 작업이 필요하다. 특히 발의한 국회의원의 정보가 중심이 되는 "국회의원발의 법률안" 데이터임에도 불구하고, 의원의 ID값이 요청인자나 출력값에 빠져있는 다소 황당한 구조를 취하고 있어 하나의 법안당 2회의 크롤링이 필요하다. 

### 2.1. 발의의원 데이터 크롤링 함수 작성
아래와 같이 발의의원 목록 페이지에서 의원정보를 긁어오는 크롤링 함수를 의안정보시스템 OpenAPI 클래스에 추가한다. 
```python
    # 발의의원 데이터 크롤링 함수
    def search_proposers(self, bill_id):
        member_list = []

        # 발의의원 목록 페이지 조회
        url = "https://likms.assembly.go.kr/bill/coactorListPopup.do?billId=" + bill_id
        ret = requests.get(url)
        html = BeautifulSoup(ret.text,'html.parser')

        # 발의의원 목록 추출
        member_tags = html.find("div", {"id":"periodDiv"}).find_all("a")
        for item in member_tags:
            # 의원정보 추출
            member_name = item.string.split("(")[0]
            party_name = item.string.split("(")[1].split("/")[0]

            # 의원 소개 페이지 URL에서 의원ID 추출
            if item.has_attr("href"):
                member_url = item.get("href")
                member_id = parse_qs(urlparse(member_url).query)["dept_cd"][0]
            else:
                member_id = ""
                member_url = ""
                # print("NO HREF:", url)
                # print("item:", item)

            # 발의의원 목록에 추가
            member_dic = {}
            member_dic["의안ID"] = bill_id          # 의안ID
            member_dic["member_name"] = member_name  # 이름(한글)
            member_dic["party_name"] = party_name    # 정당
            member_dic["member_id"] = member_id      # 의원ID
            member_list.append(member_dic)
        return member_list
```

의원 목록은 periodDiv 라는 id의 div 태그에 담겨있고, 각 의원의 정보는 의원 소개 페이지로 연결되는 a 태그에 담겨있으므로 find_all 함수를 이용해 한꺼번에 추출할 수 있으며. 의원의 이름과 정당은 a 태그의 텍스트에서, 의원ID는 의원 소개 페이지 URL에서 추출할 수 있다. (참고: 대표발의 의원은 첫번째로 표시되며, 소개 페이지가 현직 의원의 경우에만 표시되어 빈 데이터 처리 추가 필요)
```html
	<div id="periodDiv" class="layerpop layer830x500">
		<p class="layti">
					제안자 목록
		</p>
		<div style="overflow:hidden">
		<p class="textType02 alignL coaTxt">&nbsp;&nbsp;<label class="bul01">의원 명단에서 성명을 클릭하시면 해당 국회의원의 소개정보를 보실 수 있습니다. 다만, 현직 국회의원에 대해서만 소개정보를 제공하고 있습니다.</label></p>
			</div>
		<div class="layerInScroll coaTxtScroll">
			<p class="textType01 mt30"><!-- 해양생태계의 보전 및 관리에 관한 법률 일부개정법률안 -->[2109784]신용협동조합법 일부개정법률안(배진교의원 등 10인)</p>
			<div class="links textType02 mt20">
						<p><strong>· 발의의원 명단</strong></p>
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771277" target="_blank">배진교(정의당/裵晋敎)</a>
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771276" target="_blank">강은미(정의당/姜恩美)</a>
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9770984" target="_blank">김병욱(더불어민주당/金炳旭)</a>
					<br />					
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771274" target="_blank">류호정(정의당/柳好貞)</a>
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771175" target="_blank">민병덕(더불어민주당/閔炳德)</a>
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771238" target="_blank">송재호(더불어민주당/宋在祜)</a>
					<br />
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9770869" target="_blank">심상정(정의당/沈相奵)</a>
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771278" target="_blank">이은주(정의당/李恩周)</a>
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771275" target="_blank">장혜영(정의당/張惠英)</a>
					<br />
                <a href="http://www.assembly.go.kr/assm/memPop/memPopup.do?dept_cd=9771149" target="_blank">허종식(더불어민주당/許琮植)</a>
			</div>
			<br/><br/>
		</div>
	</div>
```

아래 코드를 실행해 기존에 저장된 20대 국회의원발의 법률안 CSV에 발의의원 데이터를 추가하고 새로운 파일로 저장한다. (전체 데이터 추가 시 오랜 시간이 소요되므로 데이터를 축소해서 테스트 할 것)
```python
# TEST 3
csv_path = "TEST2에서_저장한_CSV_파일_축소본.csv"
df_total = pd.read_csv(csv_path)

# 반복처리
members = []
for i in df_total.index:
    bill_id = df_total.loc[i, "의안ID"]
    # 발의의원 데이터 크롤링
    member_dic = api_likms.search_proposers(bill_id)[0]
    # 발의의원 데이터를 데이터프레임 형식으로 추가 
    members.append(pd.DataFrame(member_dic, index=[i]))

    # 100건에 1초 sleep
    if i % 100 == 0:
        time.sleep(1)  

# 데이터 개수 확인
print("df_total:", len(df_total), "members:", len(members), " index:", i) 

# 데이터 병합
df_member = pd.concat(members)
df_merge = pd.merge(df_total, df_member)

# csv로 저장
merge_csv_path = "data" + assembly_age + "_result.csv"
df_merge.to_csv(merge_csv_path, mode="w")
```

### 2.2. 의안원문 및 비용추계서 다운로드
의안정보시스템 OpenAPI 클래스에 아래와 같이 의안원문, 비용추계서, 비용추계서 미첨부 사유서를 다운로드하는 코드를 추가한다. 비용추계서는 첨부되는 케이스가 아래와 같이 있으므로 로직 이해 시 참고하도록 한다.

|비용추계서 상태|예시 URL|
|---|---|
|미요구     |https://likms.assembly.go.kr/bill/billDetail.do?billId=PRC_M2W1S0I8A0G3D0B9M3Q7Q1C1R3C1Y7
|파일-첨부  |https://likms.assembly.go.kr/bill/billDetail.do?billId=PRC_W1Y9Y0U2X2B6F1Y5N3C1L1Z0C8T9S9
|파일-미첨부|http://likms.assembly.go.kr/bill/billDetail.do?billId=PRC_O2X1D0J6C1T4Q0C9C5X0S3N3B4X3B2
|비고-요구됨|http://likms.assembly.go.kr/bill/billDetail.do?billId=PRC_N2H1K0G6W2I2R1W6G3T6L2S8Z3N4N5

```python
    # 의안 파일 다운로드 함수
    def save_bill_files(self, bill_id, title_max=10, save_bill_flag=False, save_cost_estimate_flag=True, save_no_estimate_flag=False):
        # 파일명 및 경로 설정
        bill_no = "" # 페이지에서 취득
        bill_name = "" # 페이지에서 취득
        delimeter = "_" # 파일명 구분문자

        # 파일명 딕셔너리
        file_names_dic = {
            "bill_raw": {"bill_raw_hwp": "의안원문.hwp", "bill_raw_pdf": "의안원문.pdf"}
            , "cost_estimate": {"cost_estimate_hwp": "비용추계서.hwp", "cost_estimate_pdf": "비용추계서.pdf"}
            , "no_estimate": {"no_estimate_hwp": "비용추계서 미첨부 사유서.hwp", "no_estimate_pdf": "비용추계서 미첨부 사유서.pdf"}
        }

        # 파일 링크 딕셔너리
        file_links_dic = {
            "bill_raw_hwp": ""
            , "bill_raw_pdf": ""
            , "cost_estimate_hwp": ""
            , "cost_estimate_pdf": ""
            , "no_estimate_hwp": ""
            , "no_estimate_pdf": ""
        }

        # 의안 상세 정보 조회
        detail_url = self.URL_BILL_DETAIL + bill_id
        ret = requests.get(detail_url, timeout=3)
        html = BeautifulSoup(ret.text,'html.parser')
        print(detail_url)

        # 의안명 확인
        bill_name = html.find("h3", {"class":"titCont"}).get_text().split("]")[1].split("(")[0]
        # 의안명 길이 제한 적용
        if title_max >= 0:
            bill_name = bill_name[:title_max]

        # 각 정보 div 검색
        info_divs = html.find_all("div", {"class":"contIn"})
        for item in info_divs:
            # 의안접수정보 확인
            caption = item.find("caption")
            if caption is not None and caption.get_text() == "의안접수정보":
                # 의안번호
                bill_no = item.find_all("td")[0].get_text()
                # 문서
                td_files = item.find_all("td")[3]
                link_tags = td_files.find_all("a")
                for i, val in enumerate(link_tags):
                    # href 속성에서 파일 다운로드 스크립트 추출
                    link_script = link_tags[i].get("href")
                    link_parts = link_script.split(",")
                    if len(link_parts) >= 2:
                        book_id = link_parts[1].replace("\'", "")
                        if i < 2:
                            # 의안 원문 파일 링크 저장
                            file_links_dic["bill_raw_hwp"] = self.URL_BILL_FILE_HWP + book_id
                            file_links_dic["bill_raw_pdf"] = self.URL_BILL_FILE_PDF + book_id
                        else:
                            # 비용추계 첨부 여부 확인
                            td_text = str(td_files.get_text())
                            if td_text.find("비용추계서 미첨부 사유서") > 0:
                                # 비용추계 미첨부 사유서 링크 저장
                                file_links_dic["no_estimate_hwp"] = self.URL_BILL_FILE_HWP + book_id
                                file_links_dic["no_estimate_pdf"] = self.URL_BILL_FILE_PDF + book_id
                            elif td_text.find("비용추계서") > 0:
                                # 비용추계서 링크 저장
                                file_links_dic["cost_estimate_hwp"] = self.URL_BILL_FILE_HWP+ book_id
                                file_links_dic["cost_estimate_pdf"] = self.URL_BILL_FILE_PDF+ book_id

                # div 검색 중지
                break

        # 파일 다운로드
        file_prefix = bill_no + delimeter + bill_name + delimeter
        # 의안원문 다운로드
        if save_bill_flag:
            sub_dic = file_names_dic["bill_raw"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 비용추계서 다운로드
        if save_cost_estimate_flag and len(file_links_dic["cost_estimate_hwp"]) > 0:
            sub_dic = file_names_dic["cost_estimate"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 비용추계서 미첨부 사유서 다운로드        
        if save_no_estimate_flag and len(file_links_dic["no_estimate_hwp"]) > 0:
            sub_dic = file_names_dic["no_estimate"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)

        return file_links_dic
```

### 2.3. 검토보고서 다운로드
의안정보시스템 OpenAPI 클래스에 아래와 같이 검토보고서, 심사보고서, 위원회결의안을 다운로드 하는 함수를 추가한다.

```python
    # 검토보고서 다운로드
    def save_bill_report(self, bill_id, title_max=20, advisor_review_flag=True, committee_review_flag=False, committee_bill_flag=False):
        # 파일명 및 경로 설정
        bill_no = "" # 페이지에서 취득
        bill_name = "" # 페이지에서 취득
        delimeter = "_" # 파일명 구분문자

        # 파일명 딕셔너리
        file_names_dic = {
            "advisor_review": {"advisor_review_hwp": "검토보고서.hwp", "advisor_review_pdf": "검토보고서.pdf"}
            , "committee_review": {"committee_review_hwp": "심사보고서.hwp", "committee_review_pdf": "심사보고서.pdf"}
            , "committee_bill": {"committee_bill_hwp": "위원회의결안.hwp", "committee_bill_pdf": "위원회의결안.pdf"}
        }

        # 파일 링크 딕셔너리
        file_links_dic = {
            "advisor_review_hwp": ""
            , "advisor_review_pdf": ""
            , "committee_review_hwp": ""
            , "committee_review_pdf": ""
            , "committee_bill_hwp": ""
            , "committee_bill_pdf": ""
        }

        # 의안 상세 정보 조회
        detail_url = self.URL_BILL_DETAIL + bill_id
        ret = requests.get(detail_url, timeout=3)
        html = BeautifulSoup(ret.text,'html.parser')
        print(detail_url)

        # 의안명 확인
        bill_name = html.find("h3", {"class":"titCont"}).get_text().split("]")[1].split("(")[0]
        # 의안명 길이 제한 적용
        if title_max >= 0:
            bill_name = bill_name[:title_max]

        # 각 정보 div 검색
        info_divs = html.find_all("div", {"class":"contIn"})
        for item in info_divs:
            # 의안접수정보 확인
            caption = item.find("caption")
            if caption is not None and caption.get_text() == "의안접수정보":
                # 의안번호
                bill_no = item.find_all("td")[0].get_text()
                print("의안번호: ", bill_no)
                print("의안접수정보 확인 완료")

            # 소관위 심사정보 확인
            if caption is not None and caption.get_text().replace(" ", "") == "소관위심사정보":
            
                # 문서
                td_files = item.find_all("td")[5]
                link_tags = td_files.find_all("a")

                # 문서 존재 플래그 초기화 
                advisor_review_exists = False
                committee_review_exists = False
                committee_bill_exists = False

                # 파일링크 인덱스 초기화
                advisor_review_idx = 0
                committee_review_idx = 2
                committee_bill_idx = 4

                # 파일 종류 확인
                td_text = td_files.get_text().strip()
                if td_text.find("검토보고서") >= 0:
                    advisor_review_exists = True
                else:
                    committee_review_idx -= 2
                    committee_bill_idx -= 2

                if td_text.find("심사보고서") >= 0:
                    committee_review_exists = True
                else:
                    committee_bill_idx -= 2

                if td_text.find("위원회의결안") >= 0:
                    committee_bill_exists = True

                if advisor_review_flag and advisor_review_exists:
                    book_id = link_tags[advisor_review_idx].get("href").split(",")[1].replace("\'", "")
                    # 검토보고서 링크 저장
                    file_links_dic["advisor_review_hwp"] = self.URL_BILL_FILE_HWP + book_id
                    file_links_dic["advisor_review_pdf"] = self.URL_BILL_FILE_PDF + book_id

                if committee_review_flag and committee_review_exists:
                    book_id = link_tags[committee_review_idx].get("href").split(",")[1].replace("\'", "")
                    # 심사보고서 링크 저장
                    file_links_dic["committee_review_hwp"] = self.URL_BILL_FILE_HWP + book_id
                    file_links_dic["committee_review_pdf"] = self.URL_BILL_FILE_PDF + book_id

                if committee_bill_flag and committee_bill_exists:
                    book_id = link_tags[committee_bill_idx].get("href").split(",")[1].replace("\'", "")
                    # 위원회결의안 링크 저장
                    file_links_dic["committee_bill_hwp"] = self.URL_BILL_FILE_HWP+ book_id
                    file_links_dic["committee_bill_pdf"] = self.URL_BILL_FILE_PDF+ book_id

                # div 검색 중지
                break

        # 파일 다운로드
        file_prefix = bill_no + delimeter + bill_name + delimeter
        # 검토보고서 다운로드
        if advisor_review_flag and len(file_links_dic["advisor_review_hwp"]) > 0:
            sub_dic = file_names_dic["advisor_review"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 심사보고서 다운로드
        if committee_review_flag and len(file_links_dic["committee_review_hwp"]) > 0:
            sub_dic = file_names_dic["committee_review"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 비용추계서 미첨부 사유서 다운로드        
        if committee_bill_flag and len(file_links_dic["committee_bill_hwp"]) > 0:
            sub_dic = file_names_dic["committee_bill"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)

        return file_links_dic
```

### 2.4. 반복처리
우선 처리상태 확인을 쉽게 하기 위해 프로그레스 바 함수를 유틸리티 함수 영역에 추가한다. (출처: https://stackoverflow.com/questions/3173320/text-progress-bar-in-the-console/34325723)

```python
# 프로그레스바 출력 함수
def printProgressBar (iteration, total, prefix = '', suffix = '', decimals = 1, length = 100, fill = '█', printEnd = "\r"):
    """
    Call in a loop to create terminal progress bar
    @params:
        iteration   - Required  : current iteration (Int)
        total       - Required  : total iterations (Int)
        prefix      - Optional  : prefix string (Str)
        suffix      - Optional  : suffix string (Str)
        decimals    - Optional  : positive number of decimals in percent complete (Int)
        length      - Optional  : character length of bar (Int)
        fill        - Optional  : bar fill character (Str)
        printEnd    - Optional  : end character (e.g. "\r", "\r\n") (Str)
    """
    percent = ("{0:." + str(decimals) + "f}").format(100 * (iteration / float(total)))
    filledLength = int(length * iteration // total)
    bar = fill * filledLength + '-' * (length - filledLength)
    print(f'\r{prefix} |{bar}| {percent}% {suffix}', end = printEnd)
    # Print New Line on Complete
    if iteration == total: 
        print()
```

아래와 같이 반복문을 이용한 일괄 다운로드 함수 2가지를 추가한다.
```python
# 의안원문 및 비용추계서 일괄 다운로드
def save_cost_estimates(api_likms, assembly_age, save_bill_flag, save_cost_estimate_flag, save_no_estimate_flag):
    # CSV 파일 로드
    csv_name = "data" + assembly_age + "_result.csv"
    down_cnt = 0

    # 데이터 파일 로드
    df_total = pd.read_csv(csv_name)

    # 반복처리
    printProgressBar(0, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)
    for i in df_total.index:
        bill_id = df_total.loc[i, "의안ID"]

        # 실패한 부분부터 다운로드 하는 경우 설정
        skip_index = -1
        if i < skip_index:
            continue

        # 의안원문 및 비용추계서 다운로드
        api_likms.save_bill_files(bill_id, save_bill_flag=save_bill_flag, save_cost_estimate_flag=save_cost_estimate_flag, save_no_estimate_flag=save_no_estimate_flag)
        down_cnt += 1

        # 25건에 1초 sleep
        if i % 25 == 0:
            time.sleep(1)  

        # 프로그레스바 갱신
        printProgressBar(i + 1, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)

    # 데이터 개수 확인
    print("df_total:", len(df_total), " downloaded:", down_cnt) 

# 검토보고서 일괄 다운로드
def save_bill_reports(api_likms, assembly_age, advisor_review_flag, committee_review_flag, committee_bill_flag):
    # CSV 파일 로드
    csv_name = "data" + assembly_age + "_result.csv"
    down_cnt = 0

    # 데이터 파일 로드
    df_total = pd.read_csv(csv_name)
    print(df_total)

    # 반복처리
    printProgressBar(0, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)
    for i in df_total.index:
        # 다운로드 결과 초기화
        result = None

        # 실패한 부분부터 다운로드 하는 경우 설정
        skip_index = -1
        if i < skip_index:
            continue
        
        # 의안ID 취득
        bill_id = df_total.loc[i, "의안ID"]

        # 타임아웃 오류 발생 시 재시도
        num_retry = 4
        sleep_sec = 3
        for n in range(num_retry):
            try:
                # 검토보고서 등 저장
                result = api_likms.save_bill_report(bill_id, advisor_review_flag=advisor_review_flag, committee_review_flag=committee_review_flag, committee_bill_flag=committee_bill_flag)
                break
            except requests.exceptions.Timeout:
                print("TIME_OUT at index ", i)
                time.sleep(sleep_sec)
                continue

        # 정보 추출 실패 시 작업 정지         
        if result is None:
            print("******* TIMEOUT ERROR ********")
            break

        # 데이터 카운터 증가    
        down_cnt += 1

        # 10건에 1초 sleep
        if i % 10 == 0:
            time.sleep(1)  

        # 프로그레스바 갱신
        printProgressBar(i + 1, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)

    # 데이터 개수 확인
    print("df_total:", len(df_total), " downloaded:", down_cnt) 
```

실행 코드를 추가해 테스트를 진행 한다 (축소본 이용하지 않는 경우 장시간 소요되며, sleep 시간이 짧을 경우 타임아웃 발생할 수 있음에 유의)  
```python
# TEST 4
# 20대 국회의원발의 법률안 검토보고서 저장 테스트
save_cost_estimates(api_likms, "20", True, True, True)

# 20대 국회의원발의 법률안 검토보고서 저장 테스트
save_bill_reports(api_likms, "20", True, True, True)
```

## 3. 전체코드
사용된 전체코드는 다음과 같다.
<details markdown="1">
<summary>접기/펼치기</summary>

```python
# 모듈 로드
# 기본모듈
import datetime
import time
from enum import Enum, unique

# 라이브러리
import pandas as pd
import requests 
import urllib.request
from urllib.parse import urlparse, parse_qs, quote
from bs4 import BeautifulSoup 

# 데코레이터
def constant(func):
    def func_set(self, value):
        raise TypeError

    def func_get(self):
        return func()
    return property(func_get, func_set)

# 유틸리티 함수
# 타임스탬프(초단위)
def now_timestamp():
    return datetime.datetime.now().strftime("%y%m%d%H%M%S")

# XML -> 데이터프레임 변환
def xml_to_df(xml, row_tag="row", headers_dic={}):
    buf = []
    cols = headers_dic.values()

    # 헤더 설정
    headers = headers_dic.keys()

    # XML에서 데이터 추출 (헤더 기준)
    rows = xml.find_all(row_tag)
    for item in rows:
        item_data = []
        for header in headers:
            value = item.find(header).text
            item_data.append(value)
        buf.append(item_data)    

    # 데이터프레임 객체 생성
    df = pd.DataFrame(buf, columns=cols)
    return df

# 프로그레스바 출력 함수
def printProgressBar (iteration, total, prefix = '', suffix = '', decimals = 1, length = 100, fill = '█', printEnd = "\r"):
    """
    Call in a loop to create terminal progress bar
    @params:
        iteration   - Required  : current iteration (Int)
        total       - Required  : total iterations (Int)
        prefix      - Optional  : prefix string (Str)
        suffix      - Optional  : suffix string (Str)
        decimals    - Optional  : positive number of decimals in percent complete (Int)
        length      - Optional  : character length of bar (Int)
        fill        - Optional  : bar fill character (Str)
        printEnd    - Optional  : end character (e.g. "\r", "\r\n") (Str)
    """
    percent = ("{0:." + str(decimals) + "f}").format(100 * (iteration / float(total)))
    filledLength = int(length * iteration // total)
    bar = fill * filledLength + '-' * (length - filledLength)
    print(f'\r{prefix} |{bar}| {percent}% {suffix}', end = printEnd)
    # Print New Line on Complete
    if iteration == total: 
        print()

# OpenAPI 공통 클래스
class OpenAPI:
    # 초기화 함수
    def __init__(self, api_name, api_key, search_url, url_delimeter):
        self._api_name = api_name
        self._api_key = api_key
        self._search_url = search_url
        self._url_delimeter = url_delimeter

    # 문자열 출력 함수
    def __str__(self):
        return f'str : {self.api_name} - {self.search_url}'

    # 클래스 변수
    # API 명칭
    @property
    def api_name(self):
        return self._api_name

    # API 인증키
    @property
    def api_key(self):
        return self._api_key

    # 조회 URL
    @property
    def search_url(self):
        return self._search_url

    # URL 파라미터 구분자
    @property
    def url_delimeter(self):
        return self._url_delimeter

    # URL 생성 함수
    def params_url(self, base_url, delimeter="",  *args):
        url = base_url    
        for param in args[0]:
            url += delimeter + str(param)
        
        print("URL:", url)
        return url    

    # CSV 파일 요청 함수 
    def get_csv(self, path, url):
        urllib.request.urlretrieve(url, path)
        return path        

    # JSON 데이터 요청 함수 
    def get_json(self, url):
        ret = requests.get(url)
        json = ret.json()        
        return json

    # xml 데이터 요청 함수 
    def get_xml(self, url, **kwargs):
        ret = requests.get(url, params=kwargs)

        # XML 추출 (오류 시 lxml 패키지 설치)
        xml = BeautifulSoup(ret.text,'xml')
        return xml

# 의안정보시스템 OpenAPI 클래스
class LIKMS(OpenAPI):
    def __init__(self, api_key):
        self._api_name = "의안정보시스템(LIKMS)"
        self._search_url = "https://open.assembly.go.kr/portal/openapi"
        self._url_delimeter = "/"
        super().__init__(self._api_name, api_key, self._search_url, self.url_delimeter) 

    # 클래스 변수
    @constant
    def URL_BILL_DETAIL():
        return "https://likms.assembly.go.kr/bill/billDetail.do?billId="

    @constant
    def URL_BILL_FILE_HWP():
        return "https://likms.assembly.go.kr/filegate/servlet/FileGate?type=0&bookId="

    @constant
    def URL_BILL_FILE_PDF():
        return "https://likms.assembly.go.kr/filegate/servlet/FileGate?type=1&bookId="

    # 조회요청 파라미터 딕셔너리
    def search_params_dic(self):
        params_dic = {
            "KEY" : self.api_key     # API 인증키 (필수)
            , "Type" : "xml"         # 파일형식: XML (필수)
            , "pIndex" : "1"         # 페이지 위치 (필수)
            , "pSize" : "100"        # 페이지당 건수 (필수)
            , "AGE" : "20"           # 대수 (필수)
            , "BILL_ID" : ""         # 의안ID
            , "BILL_NO" : ""         # 의안번호
            , "BILL_NAME" : ""       # 법률안명
            , "COMMITTEE" : ""       # 소관위원회
            , "COMMITTEE_ID" : ""    # 소관위원회ID
            , "PROC_RESULT" : ""     # 처리상태
            , "PROPOSER" : ""        # 제안자
        } 
        return params_dic

    # 조회결과 헤더 딕셔너리
    def search_headers_dic(self):
        headers_dic = {
            "BILL_ID"           : "의안ID"
            , "BILL_NO"	        : "의안번호"
            , "BILL_NAME"	    : "법률안명"
            , "COMMITTEE"	    : "소관위원회"
            , "COMMITTEE_ID"    : "소관위원회ID"
            , "PROC_RESULT"	    : "처리상태"
            , "PROPOSER"	    : "제안자"
            , "MEMBER_LIST"	    : "제안자목록링크"
            , "AGE"	            : "대수"
            , "PROPOSE_DT"	    : "제안일"
            , "DETAIL_LINK"	    : "상세페이지"
            , "RST_PROPOSER"    : "대표발의자"
            , "PUBL_PROPOSER"	: "공동발의자"
        } 
        return headers_dic

    # 서비스코드 취득
    def get_svc_code(self, svc_name):
        svc_code_dic = {
            "SEARCH_BILL" : "nzmimeepazxkubdpn"
        }
        svc_code = svc_code_dic[svc_name]
        return svc_code

    # 국회의원 발의법률안 조회 함수(XML)
    def search_xml(self, svc_code, assembly_age, pIndex, pSize="100", bill_id="", bill_no="", bill_name="", committee="", committee_id="", proc_result="", proposer=""):
        params_dic = self.search_params_dic()

        # 필수인자 설정
        params_dic["pIndex"] = pIndex       # 페이지 위치 (필수)
        params_dic["pSize"] = pSize         # 페이지당 건수 (필수)
        params_dic["AGE"] = assembly_age    # 대수 (필수)    

        # 선택인자 설정
        params_dic["BILL_ID"] = bill_id             # 의안ID    
        params_dic["BILL_NO"] = bill_no             # 의안번호
        params_dic["BILL_NAME"] = bill_name         # 법률안명    
        params_dic["COMMITTEE"] = committee         # 소관위원회    
        params_dic["COMMITTEE_ID"] = committee_id   # 소관위원회ID            
        params_dic["PROC_RESULT"] = proc_result     # 처리상태        
        params_dic["PROPOSER"] = proposer           # 제안자    

        url = self.params_url(self.search_url, "/", [svc_code])
        xml = self.get_xml(url, **params_dic)
        return xml

    # 발의의원 데이터 크롤링 함수
    def search_proposers(self, bill_id):
        member_list = []

        # 발의의원 목록 페이지 조회
        url = "https://likms.assembly.go.kr/bill/coactorListPopup.do?billId=" + bill_id
        ret = requests.get(url)
        html = BeautifulSoup(ret.text,'html.parser')

        # 발의의원 목록 추출
        member_tags = html.find("div", {"id":"periodDiv"}).find_all("a")
        for item in member_tags:
            # 의원정보 추출
            member_name = item.string.split("(")[0]
            party_name = item.string.split("(")[1].split("/")[0]

            # 의원 소개 페이지 URL에서 의원ID 추출
            if item.has_attr("href"):
                member_url = item.get("href")
                member_id = parse_qs(urlparse(member_url).query)["dept_cd"][0]
            else:
                member_id = ""
                member_url = ""
                # print("NO HREF:", url)
                # print("item:", item)

            # 발의의원 목록에 추가
            member_dic = {}
            member_dic["의안ID"] = bill_id          # 의안ID
            member_dic["member_name"] = member_name  # 이름(한글)
            member_dic["party_name"] = party_name    # 정당
            member_dic["member_id"] = member_id      # 의원ID
            member_list.append(member_dic)
        return member_list

    def save_bill_files(self, bill_id, title_max=10, save_bill_flag=False, save_cost_estimate_flag=True, save_no_estimate_flag=False):
        # 파일명 및 경로 설정
        bill_no = "" # 페이지에서 취득
        bill_name = "" # 페이지에서 취득
        delimeter = "_" # 파일명 구분문자

        # 파일명 딕셔너리
        file_names_dic = {
            "bill_raw": {"bill_raw_hwp": "의안원문.hwp", "bill_raw_pdf": "의안원문.pdf"}
            , "cost_estimate": {"cost_estimate_hwp": "비용추계서.hwp", "cost_estimate_pdf": "비용추계서.pdf"}
            , "no_estimate": {"no_estimate_hwp": "비용추계서 미첨부 사유서.hwp", "no_estimate_pdf": "비용추계서 미첨부 사유서.pdf"}
        }

        # 파일 링크 딕셔너리
        file_links_dic = {
            "bill_raw_hwp": ""
            , "bill_raw_pdf": ""
            , "cost_estimate_hwp": ""
            , "cost_estimate_pdf": ""
            , "no_estimate_hwp": ""
            , "no_estimate_pdf": ""
        }

        # 의안 상세 정보 조회
        detail_url = self.URL_BILL_DETAIL + bill_id
        ret = requests.get(detail_url, timeout=3)
        html = BeautifulSoup(ret.text,'html.parser')
        print(detail_url)

        # 의안명 확인
        bill_name = html.find("h3", {"class":"titCont"}).get_text().split("]")[1].split("(")[0]
        # 의안명 길이 제한 적용
        if title_max >= 0:
            bill_name = bill_name[:title_max]

        # 각 정보 div 검색
        info_divs = html.find_all("div", {"class":"contIn"})
        for item in info_divs:
            # 의안접수정보 확인
            caption = item.find("caption")
            if caption is not None and caption.get_text() == "의안접수정보":
                # 의안번호
                bill_no = item.find_all("td")[0].get_text()
                # 문서
                td_files = item.find_all("td")[3]
                link_tags = td_files.find_all("a")
                for i, val in enumerate(link_tags):
                    # href 속성에서 파일 다운로드 스크립트 추출
                    link_script = link_tags[i].get("href")
                    link_parts = link_script.split(",")
                    if len(link_parts) >= 2:
                        book_id = link_parts[1].replace("\'", "")
                        if i < 2:
                            # 의안 원문 파일 링크 저장
                            file_links_dic["bill_raw_hwp"] = self.URL_BILL_FILE_HWP + book_id
                            file_links_dic["bill_raw_pdf"] = self.URL_BILL_FILE_PDF + book_id
                        else:
                            # 비용추계 첨부 여부 확인
                            td_text = str(td_files.get_text())
                            if td_text.find("비용추계서 미첨부 사유서") > 0:
                                # 비용추계 미첨부 사유서 링크 저장
                                file_links_dic["no_estimate_hwp"] = self.URL_BILL_FILE_HWP + book_id
                                file_links_dic["no_estimate_pdf"] = self.URL_BILL_FILE_PDF + book_id
                            elif td_text.find("비용추계서") > 0:
                                # 비용추계서 링크 저장
                                file_links_dic["cost_estimate_hwp"] = self.URL_BILL_FILE_HWP+ book_id
                                file_links_dic["cost_estimate_pdf"] = self.URL_BILL_FILE_PDF+ book_id

                # div 검색 중지
                break

        # 파일 다운로드
        file_prefix = bill_no + delimeter + bill_name + delimeter
        # 의안원문 다운로드
        if save_bill_flag:
            sub_dic = file_names_dic["bill_raw"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 비용추계서 다운로드
        if save_cost_estimate_flag and len(file_links_dic["cost_estimate_hwp"]) > 0:
            sub_dic = file_names_dic["cost_estimate"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 비용추계서 미첨부 사유서 다운로드        
        if save_no_estimate_flag and len(file_links_dic["no_estimate_hwp"]) > 0:
            sub_dic = file_names_dic["no_estimate"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)

        return file_links_dic

    # 검토보고서 다운로드
    def save_bill_report(self, bill_id, title_max=20, advisor_review_flag=True, committee_review_flag=False, committee_bill_flag=False):
        # 파일명 및 경로 설정
        bill_no = "" # 페이지에서 취득
        bill_name = "" # 페이지에서 취득
        delimeter = "_" # 파일명 구분문자

        # 파일명 딕셔너리
        file_names_dic = {
            "advisor_review": {"advisor_review_hwp": "검토보고서.hwp", "advisor_review_pdf": "검토보고서.pdf"}
            , "committee_review": {"committee_review_hwp": "심사보고서.hwp", "committee_review_pdf": "심사보고서.pdf"}
            , "committee_bill": {"committee_bill_hwp": "위원회의결안.hwp", "committee_bill_pdf": "위원회의결안.pdf"}
        }

        # 파일 링크 딕셔너리
        file_links_dic = {
            "advisor_review_hwp": ""
            , "advisor_review_pdf": ""
            , "committee_review_hwp": ""
            , "committee_review_pdf": ""
            , "committee_bill_hwp": ""
            , "committee_bill_pdf": ""
        }

        # 의안 상세 정보 조회
        detail_url = self.URL_BILL_DETAIL + bill_id
        ret = requests.get(detail_url, timeout=3)
        html = BeautifulSoup(ret.text,'html.parser')
        print(detail_url)

        # 의안명 확인
        bill_name = html.find("h3", {"class":"titCont"}).get_text().split("]")[1].split("(")[0]
        # 의안명 길이 제한 적용
        if title_max >= 0:
            bill_name = bill_name[:title_max]

        # 각 정보 div 검색
        info_divs = html.find_all("div", {"class":"contIn"})
        for item in info_divs:
            # 의안접수정보 확인
            caption = item.find("caption")
            if caption is not None and caption.get_text() == "의안접수정보":
                # 의안번호
                bill_no = item.find_all("td")[0].get_text()
                print("의안번호: ", bill_no)
                print("의안접수정보 확인 완료")

            # 소관위 심사정보 확인
            if caption is not None and caption.get_text().replace(" ", "") == "소관위심사정보":
            
                # 문서
                td_files = item.find_all("td")[5]
                link_tags = td_files.find_all("a")

                # 문서 존재 플래그 초기화 
                advisor_review_exists = False
                committee_review_exists = False
                committee_bill_exists = False

                # 파일링크 인덱스 초기화
                advisor_review_idx = 0
                committee_review_idx = 2
                committee_bill_idx = 4

                # 파일 종류 확인
                td_text = td_files.get_text().strip()
                if td_text.find("검토보고서") >= 0:
                    advisor_review_exists = True
                else:
                    committee_review_idx -= 2
                    committee_bill_idx -= 2

                if td_text.find("심사보고서") >= 0:
                    committee_review_exists = True
                else:
                    committee_bill_idx -= 2

                if td_text.find("위원회의결안") >= 0:
                    committee_bill_exists = True

                if advisor_review_flag and advisor_review_exists:
                    book_id = link_tags[advisor_review_idx].get("href").split(",")[1].replace("\'", "")
                    # 검토보고서 링크 저장
                    file_links_dic["advisor_review_hwp"] = self.URL_BILL_FILE_HWP + book_id
                    file_links_dic["advisor_review_pdf"] = self.URL_BILL_FILE_PDF + book_id

                if committee_review_flag and committee_review_exists:
                    book_id = link_tags[committee_review_idx].get("href").split(",")[1].replace("\'", "")
                    # 심사보고서 링크 저장
                    file_links_dic["committee_review_hwp"] = self.URL_BILL_FILE_HWP + book_id
                    file_links_dic["committee_review_pdf"] = self.URL_BILL_FILE_PDF + book_id

                if committee_bill_flag and committee_bill_exists:
                    book_id = link_tags[committee_bill_idx].get("href").split(",")[1].replace("\'", "")
                    # 위원회결의안 링크 저장
                    file_links_dic["committee_bill_hwp"] = self.URL_BILL_FILE_HWP+ book_id
                    file_links_dic["committee_bill_pdf"] = self.URL_BILL_FILE_PDF+ book_id

                # div 검색 중지
                break

        # 파일 다운로드
        file_prefix = bill_no + delimeter + bill_name + delimeter
        # 검토보고서 다운로드
        if advisor_review_flag and len(file_links_dic["advisor_review_hwp"]) > 0:
            sub_dic = file_names_dic["advisor_review"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 심사보고서 다운로드
        if committee_review_flag and len(file_links_dic["committee_review_hwp"]) > 0:
            sub_dic = file_names_dic["committee_review"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)
        # 비용추계서 미첨부 사유서 다운로드        
        if committee_bill_flag and len(file_links_dic["committee_bill_hwp"]) > 0:
            sub_dic = file_names_dic["committee_bill"]
            for key in sub_dic:
                file_url = file_links_dic[key]
                file_name = file_prefix + sub_dic[key]
                urllib.request.urlretrieve(file_url, file_name)

        return file_links_dic

# TEST 1
# 의안정보시스템 OpenAPI 클래스 객체 생성 
api_likms = LIKMS("발급한_인증키_값") 

# 요청인자 설정
svc_code = api_likms.get_svc_code("SEARCH_BILL")
assembly_age = "20"
pIndex = "1"
pSize = "10"

# 국회의원 발의법률안 조회함수 호출
xml = api_likms.search_xml(svc_code, assembly_age, pIndex, pSize)

# 결과 출력
print("xml:") 
print(xml)

# 국회의원 발의법률안 저장 (대수별)
def save_bills(api_likms, assembly_age, index=1, size=1000):
    print("####### save_bills START ######")

    dfs = []
    rows = 0
    data_size = size
    svc_code = api_likms.get_svc_code("SEARCH_BILL")

    # 페이징 처리 반복문
    while(data_size == size):
        try:
            # 국회의원 발의법률안 조회
            xml = api_likms.search_xml(svc_code, assembly_age, str(index), str(size))

            # 데이터 변환 처리
            df = xml_to_df(xml, headers_dic=api_likms.search_headers_dic())
            # print(df)

            # 데이터 개수 갱신
            data_size = len(df)
            rows += data_size
            print(data_size)

            # 데이터프레임 리스트에 추가
            dfs.append(df)

        except:
            # 에러 발생 시 루프 중지
            print("ERROR at page ", index)
            break

        # 페이지 인덱스 갱신
        index += 1

    print("PAGE:", index -1, ", ROWS:", rows, "DFS:", len(dfs))

    # CSV 파일로 저장
    csv_path = "data" + assembly_age + "_" + now_timestamp() + ".csv"
    df_total = pd.concat(dfs)
    df_total.to_csv(csv_path, mode="w")

    print("####### save_bills END ######")

# TEST 2
assembly_age = "20"
save_bills(api_likms, assembly_age)

# TEST 3
csv_path = "data20_211221183445.csv"
df_total = pd.read_csv(csv_path)

# 반복처리
members = []
for i in df_total.index:
    bill_id = df_total.loc[i, "의안ID"]
    # 발의의원 데이터 크롤링
    member_dic = api_likms.search_proposers(bill_id)[0]
    # 발의의원 데이터를 데이터프레임 형식으로 추가 
    members.append(pd.DataFrame(member_dic, index=[i]))

    # 100건에 1초 sleep
    if i % 100 == 0:
        time.sleep(1)  

# 데이터 개수 확인
print("df_total:", len(df_total), "members:", len(members), " index:", i) 

# 데이터 병합
df_member = pd.concat(members)
df_merge = pd.merge(df_total, df_member)

# csv로 저장
merge_csv_path = "data" + assembly_age + "_result.csv"
df_merge.to_csv(merge_csv_path, mode="w")

# 의안원문 및 비용추계서 일괄 다운로드
def save_cost_estimates(api_likms, assembly_age, save_bill_flag, save_cost_estimate_flag, save_no_estimate_flag):
    # CSV 파일 로드
    csv_name = "data" + assembly_age + "_result.csv"
    down_cnt = 0

    # 데이터 파일 로드
    df_total = pd.read_csv(csv_name)

    # 반복처리
    printProgressBar(0, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)
    for i in df_total.index:
        bill_id = df_total.loc[i, "의안ID"]

        # 실패한 부분부터 다운로드 하는 경우 설정
        skip_index = -1
        if i < skip_index:
            continue

        # 의안원문 및 비용추계서 다운로드
        api_likms.save_bill_files(bill_id, save_bill_flag=save_bill_flag, save_cost_estimate_flag=save_cost_estimate_flag, save_no_estimate_flag=save_no_estimate_flag)
        down_cnt += 1

        # 25건에 1초 sleep
        if i % 25 == 0:
            time.sleep(1)  

        # 프로그레스바 갱신
        printProgressBar(i + 1, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)

    # 데이터 개수 확인
    print("df_total:", len(df_total), " downloaded:", down_cnt) 

# 검토보고서 일괄 다운로드
def save_bill_reports(api_likms, assembly_age, advisor_review_flag, committee_review_flag, committee_bill_flag):
    # CSV 파일 로드
    csv_name = "data" + assembly_age + "_result.csv"
    down_cnt = 0

    # 데이터 파일 로드
    df_total = pd.read_csv(csv_name)
    print(df_total)

    # 반복처리
    printProgressBar(0, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)
    for i in df_total.index:
        # 다운로드 결과 초기화
        result = None

        # 실패한 부분부터 다운로드 하는 경우 설정
        skip_index = -1
        if i < skip_index:
            continue
        
        # 의안ID 취득
        bill_id = df_total.loc[i, "의안ID"]

        # 타임아웃 오류 발생 시 재시도
        num_retry = 4
        sleep_sec = 3
        for n in range(num_retry):
            try:
                # 검토보고서 등 저장
                result = api_likms.save_bill_report(bill_id, advisor_review_flag=advisor_review_flag, committee_review_flag=committee_review_flag, committee_bill_flag=committee_bill_flag)
                break
            except requests.exceptions.Timeout:
                print("TIME_OUT at index ", i)
                time.sleep(sleep_sec)
                continue

        # 정보 추출 실패 시 작업 정지         
        if result is None:
            print("******* TIMEOUT ERROR ********")
            break

        # 데이터 카운터 증가    
        down_cnt += 1

        # 10건에 1초 sleep
        if i % 10 == 0:
            time.sleep(1)  

        # 프로그레스바 갱신
        printProgressBar(i + 1, len(df_total), prefix = 'Progress:', suffix = 'Complete', length = 50)

    # 데이터 개수 확인
    print("df_total:", len(df_total), " downloaded:", down_cnt) 

# TEST 4
# 20대 국회의원발의 법률안 검토보고서 저장 테스트
save_cost_estimates(api_likms, "20", True, True, True)

# 20대 국회의원발의 법률안 검토보고서 저장 테스트
save_bill_reports(api_likms, "20", True, True, True)
```

</details>
