fastcampus\_웹크롤링\_1
================
huimin
2019년 3월 26일

``` r
library(httr)
library(tidyverse)
```

    ## -- Attaching packages --------------- tidyverse 1.2.1 --

    ## √ ggplot2 3.1.0       √ purrr   0.3.1  
    ## √ tibble  2.0.1       √ dplyr   0.8.0.1
    ## √ tidyr   0.8.3       √ stringr 1.4.0  
    ## √ readr   1.3.1       √ forcats 0.4.0

    ## -- Conflicts ------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## Loading required package: xml2

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     pluck

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

웹크롤링이란?
=============

**웹크롤링(crawling)**은 웹페이지에서 보이는 데이터를 필요한 부분만 선택하여 수집하는 행위를 말한다.<br> 스크레이핑이라고도 한다. 웹크롤링에 사용되는 프로그램을 크롤러라고 부른다.<br>

IE, 크롬과 같은 웹 브라우저 상에서 보이는 데이터는 크롤링이 가능하다고 할 수 있다.<br> 그러므로 필요로 하는 데이터를 포함하고 있는 웹 사이트를 발견하는 것이 웹 크롤링의 시작이 된다.<br>

**대략적인 웹크롤링 과정**<br> (1) 클라이언트에서 웹 서버로 데이터를 요청(URI) - **HTTP Request**<br> (2) HTTP 요청에 대해 웹 서버가 결과를 발송하는 것(HTML) - **HTML Response**<br> (3) HTML을 가공하여 저장하거나 출력한다.<br>

**필요 패키지**<br> HTTP Request에 필요한 패키지들 : httr, urltools, RSelenium 등<br> HTML 추출에 필요한 패키지 : rvest<br> 텍스트 전처리 및 저장에 필요한 패키지 : stringr, dplyr 등<br>

**GET 방식과 POST 방식**<br> HTTP Request를 할 때 사용하는 방식으로, GET과 POST가 있다.<br> 요청할 때 필요한 양식이 각각 다르다.<br> GET : 요청 라인 + 요청 헤더<br> POST : 요청 라인 + 요청 헤더 + 메시지 바디<br> GET의 경우는 URI 입력만으로 간단하게 요청할 수 있지만, POST는 URL과 Parameters를 찾아내야 하는 다소 복잡한 방법이다.<br>

**URL vs URI**<br> URI는 Uniform Resource Indicator의 머리글자, 리소스를 식별하는 문자열들을 차례대로 배열한 것이다.<br> URL은 Uniform Resource Locator의 머리글자, 리소스가 포함되어 있는 위치를 의미한다. URL은 URI의 부분집합이다.<br>

예시<br> <https://section.blog.naver.com/BlogHome.nhn?directoryNo=0&currentPage=1><br>

|                      URL                      |         Query String        |
|:---------------------------------------------:|:---------------------------:|
| <https://section.blog.naver.com/BlogHome.nhn> | directoryNo=0&currentPage=1 |

여기에서 URL과 Query String사이의 **?**는 쿼리 스트링의 시작을 알리는 기호이다.<br> 파라미터들(Parameters) 사이의 **&**는 Query String Separator이다.<br>

**HTTP Response 상태**<br> 웹서버는 클라이언트의 요청에 대해 응답메시지를 발송한다.<br> 응답메시지는 응답 헤더와 바디로 구성되어 있다.<br> 응답 헤더 : HTTP 버전, 상태코드, 일시, 콘텐츠 형태, 인코딩 방식, 크기 등<br> 응답 바디 : HTML<br>

| 상태코드 |                내용                |
|:--------:|:----------------------------------:|
|    1XX   |              정보 교환             |
|    2XX   |      데이터 전송 성공 / 수락됨     |
|    3XX   |    방향 바꿈 - 데이터 전송 성공    |
|    4XX   | 클라이언트 오류 - 주로 자신의 이유 |
|    5XX   |   서버 오류 - 주로 웹서버의 거부   |

httr 패키지 주요 함수 사용법
============================

**주요함수**<br> HTTP 요청에 관한 함수들 : GET(), POST()<br> HTTP 응답에 관한 함수들 : status\_code(), content()<br> HTTP 응답에 성공하지 못했을 때 사용하는 함수들 : warn\_for\_status(), stop\_for\_status()<br>

GET
---

**응답코드가 200인 경우**

``` r
# GET 방식
# res <- GET(url = "요청할 웹 페이지 URL",
#            query = list(a = "a에 할당된 값",
#                         b = "b에 할당된 값",
#                         ...))

res <- GET(url = "https://www.naver.com")

# 응답 상태 확인하기
print(res)
```

    ## Response [https://www.naver.com/]
    ##   Date: 2019-03-27 06:31
    ##   Status: 200
    ##   Content-Type: text/html; charset=UTF-8
    ##   Size: 176 kB
    ## <!doctype html>
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## ...

``` r
status_code(res) # 상태코드 확인하기
```

    ## [1] 200

``` r
# 너무 길어서 실행하진 않겠음
# content(x = res, as = "text", encoding = "UTF-8") # HTML 텍스트로 출력
# content(x = res, as = "text", encoding = "UTF-8") %>% cat() # HTML 계층 구조로 출력하기
```

**응답코드가 400대인 경우**

``` r
# 네이버 부동산의 주소
# https://land.naver.com/article/divisionInfo.nhn?cortarNo=1168000000&rletTypeCd=A01&hscpTypeCd=A01%3AA03%3AA04

res <- GET(url = "https://land.naver.com/article/divisionInfo.nhn",
           query = list(cortarNo="1168000000",
                        rletTypeCd="A01",
                        hscpTypeCd="A01%3AA03%3AA04"))

print(res)
```

    ## Response [https://land.naver.com/article/divisionInfo.nhn?cortarNo=1168000000&rletTypeCd=A01&hscpTypeCd=A01%253AA03%253AA04]
    ##   Date: 2019-03-27 06:31
    ##   Status: 403
    ##   Content-Type: text/plain; charset=utf-8
    ## <EMPTY BODY>

    ## NULL

``` r
status_code(res)
```

    ## [1] 403

CSS와 XPath
===========

HTML은 문서의 구조를 나타내는 마크업 언어이다.<br> HTML은 꺽쇠 괄호 &lt;&gt;안에 태그로 되어 있는 HTML 요소 형태로 작성된다.<br> 크롬 개발자도구에서 **Elements와 Network**를 중심으로 사용한다.<br>

**원하는 HTML요소 지정하기**<br> (1) Elements탭에서 필요한 HTML요소에서 마우스 오른쪽 버튼을 클릭하고, Copy를 선택한다.<br> (2) **Copy selector 또는 Copy XPath**를 선택한다.<br>

**CSS** : HTML의 디자인을 담당하는 요소이다. 이것을 중심으로 원하는 내용을 찾아낼 수 있다.<br> **XPath** : XML Path Language를 나타내며, 계층 구조를 갖는 XML문서에서 노드(HTML의 태그)를 탐색하는 경로로 사용된다.<br> 일반적인 크롤러에서는 CSS Selector를 사용하고, RSelenium에서는 XPath를 사용한다.<br>

**CSS**<br>

|   표현식   |                                                        의미                                                        |
|:----------:|:------------------------------------------------------------------------------------------------------------------:|
|   태그명   |                                           태그명이 같은 모든 태그를 선택                                           |
|    &gt;    | 앞 태그의 직계 자손 태그에서 선택 (이것없이 태그만 입력하면 자손 이외의 동일한 태그를 사용한 모든 내용들을 선택함) |
|     \#     |                                                 속성명이 id인 태그                                                 |
|      .     |                                                속성명이 class인 태그                                               |
|    \[\]    |                                                속성을 지정할 때 사용                                               |
| :nth-child |                                             n번째 태그를 선택할 때 사용                                            |

**XPath**<br>

| 표현식 |                    의미                   |
|:------:|:-----------------------------------------:|
| 노드명 |       노드명이 같은 모든 노드를 선택      |
|    /   | 루트노드부터 탐색. 부모노드/자식노드 경로 |
|   //   |    위치와 상관없이 지정된 노드부터 탐색   |
|    .   |     현재노드 선택 (..은 상위노드 선택)    |
|    @   |               속성노드 선택               |
|  \[\]  |  속성 지정 및 n번째 노드를 선택할 때 사용 |

**CSS Selector와 XPath 표기법 예시**<br>

|                목표               |      CSS Selector     |          XPath          |
|:---------------------------------:|:---------------------:|:-----------------------:|
|             모든 요소             |           \*          |            -            |
|             모든 요소             |           -           |           //\*          |
|     p태그를 포함하는 모든 요소    |           p           |           //p           |
|       p태그의 모든 자식 요소      |        p&gt;\*        |            -            |
|       p태그의 모든 자식 요소      |           -           |          //p/\*         |
|       id가 "foo"인 모든 요소      |         \#foo         |    //\*\[@id='foo'\]    |
|     class가 "foo"인 모든 요소     |          .foo         |   //\*\[@class="foo"\]  |
| title="header"속성 포함 모든 요소 |  \*\[title="header"\] |            -            |
| title="header"속성 포함 모든 요소 |           -           | //\*\[@title="header"\] |
|      li 태그 중 n번째인 요소      |    li:nth-child(n)    |        //li\[n\]        |
|     ul의 자식 li 중 n번째 요소    | ul&gt;li:nth-child(n) |       //ul/li\[n\]      |

rvest 패키지
============

웹페이지로부터 데이터를 수집할 때 사용하는 패키지이다.<br>

응답 객체를 HTML로 변환하는 함수 : **read\_html()**<br> HTML 요소를 추출하는 함수 : **html\_node()**, html\_nodes()<br> HTML 속성에 관련된 함수 : **html\_attr()**, html\_attrs(), html\_name()<br> 데이터를 추출하는 함수 : **html\_text()**, **html\_table()**<br>

read\_html(x = res, encoding = "UTF-8")<br> 읽어온 html의 한글 인코딩 방식에 따라서 encoding을 설정한다.<br>

html\_nodes(x = html,)<br> css = "개발자도구에서 복사해온 CSS Selector",<br> xpath = "개발자도구에서 복사해온 XPath")<br>

html\_text(x = item,<br> trim = FALSE)<br> trim = TRUE일 경우, 문자열 양 옆의 불필요한 여백을 제거<br>

html\_table(x = item,<br> fill = FALSE)<br> html의 table태그로 이루어져있는 데이터를 데이터 프레임 형식으로 수집하고자 할 때 사용한다.<br>

실습 - 네이버 실시간 검색어 수집
================================

``` r
# HTTP request
res <- GET(url = "https://www.naver.com/")
print(res)
```

    ## Response [https://www.naver.com/]
    ##   Date: 2019-03-27 06:31
    ##   Status: 200
    ##   Content-Type: text/html; charset=UTF-8
    ##   Size: 191 kB
    ## <!doctype html>
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## ...

``` r
html <- read_html(res)
span <- html_nodes(html,
                   css = "span.ah_k")
searchword <- html_text(span)
searchword <- searchword[1:20]

print(searchword)
```

    ##  [1] "히든프라이스"     "이매리"           "야옹이 작가"     
    ##  [4] "조양호"           "여신강림 작가"    "박영선"          
    ##  [7] "론"               "파키라"           "이사강"          
    ## [10] "홍일표"           "감스트"           "위자"            
    ## [13] "장동윤"           "타인은 지옥이다"  "이원진"          
    ## [16] "진영"             "팔카오"           "gi지수 낮은 음식"
    ## [19] "조현우"           "임시완"

실습 - 네이버뉴스에서 정치 - 가장많이본뉴스 10위까지 가져오기
=============================================================

``` r
res <- GET(url = "https://news.naver.com/main/ranking/popularDay.nhn",
           query = list(mid="etc",
                        sid1="111"))
print(res)
```

    ## Response [https://news.naver.com/main/ranking/popularDay.nhn?mid=etc&sid1=111]
    ##   Date: 2019-03-27 06:31
    ##   Status: 200
    ##   Content-Type: text/html;charset=EUC-KR
    ##   Size: 130 kB
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## <!DOCTYPE HTML>
    ## <html lang="ko">
    ## <head>
    ##     <meta charset="euc-kr">
    ## ...

``` r
text <- res %>%
  read_html() %>% 
  html_nodes(css = ".section_list_ranking > li > a") %>% 
  html_text()

text <- text[1:10]

print(text)
```

    ##  [1] "시작부터 고성 박영선 청문회…진영·조동호는 '검증' 착착"                 
    ##  [2] "박영선 청문회 자료 제출 공방…與 \"망신주기\" vs 野 \"깜깜이\"(종합)"    
    ##  [3] "'적진' 한국당으로 직진한 박영선, 이언주 \"우리가 청문회 당하는 거냐\""   
    ##  [4] "신보라 \"아이 안고 본회의 가겠다\"…文의장, 허가 여부 '고심'"            
    ##  [5] "[데일리안 여론조사] 민주당 지지율 7.3%p 하락…정의당·무당층으로 이탈"   
    ##  [6] "피우진 '영부인 친구' 발언에 발끈…야당 \"사퇴하라\""                     
    ##  [7] "“이메일 주소 틀려” “콩나물값 영수증 끊나” 공수 바꿔 다 받아친 박영선"
    ##  [8] " 김원봉의 월북 이유, 나경원은 알고 있을까 "                              
    ##  [9] "김학의 사건 역공 펴는 한국당 …\"최순실 특검보했던 A변호사 수사하라\""   
    ## [10] "美FBI, 김한솔 보호하며 '자유조선'과 공조 강화한 듯"
