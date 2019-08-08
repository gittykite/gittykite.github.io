# ADsP :: 과목1. 데이터 이해

# 1장 데이터의 이해

## 1.1 데이터와 정보

### 데이터  
  : 추론과 추정의 근거를 이루는 객관적 사실

### 데이터의 종류 
  + 정성적 데이터 (qualitative data)  
  : 언어, 문자 형태의 데이터 => 저장/검색/분석에 **고비용/기술** 필요
  + 정량적 데이터 (quantitative data)  
  : 수치, 도형, 기호 형태의 데이터

### 지식의 종류
* 암묵지 (Tacit Knowledge)  
  : 사람/조직 내에 체득된 무형의 지식
* 형식지 (Expliit Knowledge)  
  : 문서/매체 등으로 형상화된 지식

### 지식의 상호작용
1) 내면화 (Internalization):  시행착오/경험 -> 암묵지(개인)   
2) 표출화 (Externalization): 암묵지 -> 형식지
3) 연결화 (Combination): 형식지 + 지식/경험* -> 형식지*   
4) 공통화 (Socialization): 형식지* 내면화 반복/순환 -> 암묵지(조직) 증대  
  

### DIKW 지식의 피라미드
- **Data**: 단순 수치, 기호
- **Info**: 가공/패턴/상관관계 이해해 의미 도출
- **Knowledge**: 여러 정보 구조화 + 경험과 결합해 내재화 
- **Wisdom**: 축적된 지식 + 창의적 idea => **가치창출**

## 1.2 DB의 정의와 특징
### DB 의 정의 
  + 여러 콘텐츠를 정보처리/통신 기기로 체계적 수집/축적해 다양한 활용 가능하도록 정리한 정보 집합  
  + 독립된 저작물(EU)  
  + 소재 체계적 배열/구성한 검색 가능한 편집물 (저작권법)

** 콘텐츠: 문자, 음성, 영상 등 의미전달 매체로 표현된 모든 자료

### DB의 특성
  + **통합성 Integrated**: 중복되지 않는 통합된 자료
  + **저장성 Stored**: 정보 저장
  + **공통성 Shared**: 여러 유저 공유, 복잡함
  + **변화성**: 변화하지만 정확해야(무결성, CRUD)
  + 기계가독성
  + 검색가능성
  + 원격조작성
  + 신속/경제적
  + 체계적 축적/관리
  + 정보/네트워크/인프라 기술 선도  
  
** CRUD: Create, Read, Update, Delete  
** DBMS: DB 관리하는 유저 인터페이스 시스템

## 1.3 DB의 활용

### 기업의 DB 활용
: 기업내부 DB(In-House DB)는 OLTP 에서 OLAP로,   2000년 부터는 CRM & SCM 중심으로 발전  
* OLTP(단순/자동화 수집 시스템: Online Transaction Processing)  
* OLAP(분석중심 시스템: Online Analytical Processing)  
* CRM(고객관계관리) + SCM(공급망관리)   

=> 유통/판매/고객 데이터 분석 및 연계 증가

### 부문별 DB 활용 방식 (기업)
+ 제조부문  

  |기존|현재|
  |---|---|
  |부품/재고 | 모든 영역|
  |기업별S/W |솔루션|
  |내부서버 시스템| 웹|
  |ERP |SCM|
  |대기업| 중소기업(RTE)|

+ 금융부문  
 
 |시기|활용방식|
  |---|---|
  |2000년대|EAI, ERP, e-CRM으로 정보통합 & 고객정보 전략적 활용|
  |2000 중반|인터넷뱅킹/방카슈랑스 도입후, 대규모 DW 위한 BI 기반 시스템 구축|
  |현재|다운사이징 & 바젤2(최저자기자본규제) 등으로 EDW 확장 예상|

+ 유통부문  

  |시기|활용방식|
  |---|---|
  |2000년대|CRM + SCM 으로 지역/고객중심 운영| 
  |2000중반|전자문서/상거래 인프라 & KMS(지식관리시스템) 백업 구축|
  |현재| RFID(전자태그) 이용 증가 => 대용량 DB 지원 필요 예상|
  |현재|고객분석 툴 이용 증가 <br> : BSC(균형성과관리), KPI(핵심성과지표), 웹 리포팅|

** BI: Bus. Intelligence  
** DW: Data Warehouse  
** EAI: Enterprise Apps Integration  
** EDW: Enterprise Data Warehouse  

### 사회기반구조로서의 DB 활용
|시기|활용방식|
|---|---|
|90년대|SOC(사회간접자본)의 EDI(전자문서교환)증가로 VAN/DB 구축|
|90중반|EDI/CALS 벗어나 지리/교통DB 구축|
|2000년대|기존 DB 고도화, 공공 DB 확대, 인터넷 보편화|
|현재| "사회 전반의 기간재", 공공데이터 개방, 민간용 증가|

** CALS: Commcerce at Light Speed  
** EDI: Electronic Data Interchange    
** SOC: Social Overhead Capital  

### 부문별 DB 활용 방식 (사회기반구조)
+ 물류부문  

  |시기|활용방식|상세|
  |---|---|---|
  |98년|종합물류정보망 구축|CVO(화물운송정보) + EDI + DB서비스 + 부가서비스|
  |2000년대|유관전산망 연계|종합물류정보망 + 항만/철도/항공/터미널망 + 무역/통관자동화망 + 민간물류VAN|
  |현재| 종합물류정보망 이용 확대/활성화|물류거점 정보화, 인터넷 기반 DB제공, 전자태그 사업|

  ** CVO: Commercial Vehicle Operation System  

+ 지리부문

  |시기|시스템|활용방식|
  |---|---|---|
  |95년|NGIS(국가지리정보체계)| 국가지형도/공통주제도/지하매설물도 전산화|
  |2000년|국가 수치지형도|4S통합기술 (GIS+RS+GPS+ITS), LBS, SIM|
  |2005년|LMIS(토지종합정보망)|수치지형도 수정/갱신, 국가기준점 정비, 지적도면 전산화|
  |현재|지리정보통합관리소|기관/기업/국민 대상 정보유통사업 관리/확장, 대국민서비스 강화| 

  ** GPS: Global Positioning System  
  ** ITS: Intelligent Transport System  
  ** LBS: Location Based Service  
  ** NGIS: National Geographic Info System    
  ** RS: Resmote Sensing  
  ** SIM: Spatial Info System

+ 교통부문

  |종류|활용방식|
  |---|---|
  |동적(실시간)교통정보| ITS(지능형교통시스템), 방송매체의 교통정보|
  |정적(비실시간)교통정보| 전국교통DB(기초자료, 통계 등) <br> => 정책수립, 교통조사/분석 중복방지|

+ 의료부문

  |시기|활용방식|상세|
  |---|---|---|
  |96년| 의료정보망 구축 |의료EDI 상용서비스|
  |2000년대| 의료정보시스템 본격화| 처방전달시스템 + 전자의무기록 + PACS(영상처리시스템) + 원격의료 등 |
  |2005년| HL7 표준화| 전국적 진료정보 공유체계 구축 (계획)|
  |05년 이후| u-헬스 |의료정보DB 기반 서비스 |
  |현재| 환자중심 서비스 | 환자 중심 병원, ABC, BSC, 6시그마 도입 등 |

  ** ABC: Activity Based Costing  
  ** BSC: Balanced Score Card  
  ** HL7: [국제 의료정보 전송 표준 (Health Level 7)](https://www.ibm.com/support/knowledgecenter/ko/SSMKHH_10.0.0/com.ibm.etools.mft.doc/ad09565_.htm)    
  ** PACS: Picture Archiving & Comm System  
  ** uー헬스: ubiquitous-Health

+ 교육부문

  |시기|활용방식|상세|
  |---|---|---|
  |97~00년|교육정보화종합계획(1단계)|정보소양교육|
  |01~05년|교육정보화종합계획(2단계)|교육정보개발/보급, 정보활용교육, 대학/교육행정정보화|
  |2002년|전국교육정보공유체제| 교육청/산하기관/학교 보유 교육자료 표준/체계화 후 공동활용|
  |2003년|NEIS(교육행정정보시스템)|학사/인사/물품/회계 등 교육행정 전 업무처리|

  ** NEIS: National Education Info System
