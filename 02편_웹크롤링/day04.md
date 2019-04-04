fastcampus\_웹크롤링\_4
================
huimin
2019년 4월 3일

기초 설정하기
=============

``` r
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
library(httr)
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

``` r
library(readr)
library(urltools)
```

    ## 
    ## Attaching package: 'urltools'

    ## The following object is masked from 'package:xml2':
    ## 
    ##     url_parse

RSelenium 패키지
================

Selenium은 웹 브라우저를 직접 제어하여, 웹 애플리케이션 테스트를 자동화하려는 목적으로 주로 사용된다.<br> RSelenium 패키지는 R에서 Selenium을 동작시킬 때 사용한다.<br> **cmd 또는 terminal에서 Selenium Server가 제대로 작동하는지 확인해야한다.**<br> **단, 최근에는 그럴 필요가 없어졌다.** 웹 드라이버 설정이 가능하기 때문이다.<br> **웹 드라이버**의 종류로는, **크롬과 팬텀 드라이버** 등이 있다.<br>

**실행이 안되길래 더 자세히 알아본 결과 다음과 같다.**<br> 셀레니엄을 실행하는 방법은 2가지가 있는데,<br> (1) 수동으로 직접 selenium server standalone과 chrome driver의 버전을 선택하여 설치하고, cmd에서 selenium server를 실행하는 방법<br> (2) RSelenium 패키지의 rsDriver를 사용하여 웹 드라이브 객체를 만들어서 실행하는 방법<br> 이 있다. (1)번의 장점은 자신의 컴퓨터에 맞는 버전들을 직접 선택하고 설치할 수 있다는 장점이 있고, (2)번의 경우는 번거로운 설치가 필요 없다는 장점이 있다.<br> 다만, (2)번 또한 인자에 현재 **자신의 크롬 버전과 컴퓨터 사양에 맞는 옵션**들을 집어 넣어야 한다.

실습1 - 리모트 드라이버 설정하기
--------------------------------

``` r
library(rJava)
library(RSelenium) # rsDriver를 사용하기 위한 패키지
library(binman) # list_versions() 함수를 사용하기 위한 패키지


# 방법(1) : remoteDriver 사용하기
chrome <- wdman::chrome(port = 4445L,
                        version = "73.0.3683.68")
```

    ## checking chromedriver versions:

    ## BEGIN: PREDOWNLOAD

    ## BEGIN: DOWNLOAD

    ## BEGIN: POSTDOWNLOAD

``` r
# 리모트 드라이버를 설정하기
remote <- remoteDriver(port = 4445L, browserName = 'chrome')


# 방법(2) : rsDriver 사용하기 - 잘 튕겨서 비추천함
# 자신에게 맞는 크롬 드라이버 버전을 찾아야한다.
# remote <- rsDriver(port = 4445L, 
#                    browser = "chrome",
#                    chromever = "73.0.3683.68") %>% `[[`("client")
```

실습2 - 기초적인 리모트 컨트롤하기
----------------------------------

``` r
# 웹 브라우저 열기
remote$open()
```

    ## [1] "Connecting to remote server"
    ## $acceptInsecureCerts
    ## [1] FALSE
    ## 
    ## $acceptSslCerts
    ## [1] FALSE
    ## 
    ## $applicationCacheEnabled
    ## [1] FALSE
    ## 
    ## $browserConnectionEnabled
    ## [1] FALSE
    ## 
    ## $browserName
    ## [1] "chrome"
    ## 
    ## $chrome
    ## $chrome$chromedriverVersion
    ## [1] "73.0.3683.68 (47787ec04b6e38e22703e856e101e840b65afe72)"
    ## 
    ## $chrome$userDataDir
    ## [1] "C:\\Users\\LEEHUI~1\\AppData\\Local\\Temp\\scoped_dir29556_13337"
    ## 
    ## 
    ## $cssSelectorsEnabled
    ## [1] TRUE
    ## 
    ## $databaseEnabled
    ## [1] FALSE
    ## 
    ## $`goog:chromeOptions`
    ## $`goog:chromeOptions`$debuggerAddress
    ## [1] "localhost:58210"
    ## 
    ## 
    ## $handlesAlerts
    ## [1] TRUE
    ## 
    ## $hasTouchScreen
    ## [1] FALSE
    ## 
    ## $javascriptEnabled
    ## [1] TRUE
    ## 
    ## $locationContextEnabled
    ## [1] TRUE
    ## 
    ## $mobileEmulationEnabled
    ## [1] FALSE
    ## 
    ## $nativeEvents
    ## [1] TRUE
    ## 
    ## $networkConnectionEnabled
    ## [1] FALSE
    ## 
    ## $pageLoadStrategy
    ## [1] "normal"
    ## 
    ## $platform
    ## [1] "Windows NT"
    ## 
    ## $proxy
    ## named list()
    ## 
    ## $rotatable
    ## [1] FALSE
    ## 
    ## $setWindowRect
    ## [1] TRUE
    ## 
    ## $strictFileInteractability
    ## [1] FALSE
    ## 
    ## $takesHeapSnapshot
    ## [1] TRUE
    ## 
    ## $takesScreenshot
    ## [1] TRUE
    ## 
    ## $timeouts
    ## $timeouts$implicit
    ## [1] 0
    ## 
    ## $timeouts$pageLoad
    ## [1] 300000
    ## 
    ## $timeouts$script
    ## [1] 30000
    ## 
    ## 
    ## $unexpectedAlertBehaviour
    ## [1] "ignore"
    ## 
    ## $version
    ## [1] "73.0.3683.86"
    ## 
    ## $webStorageEnabled
    ## [1] TRUE
    ## 
    ## $id
    ## [1] "5f2de652aec6395c93819b0e7c886aeb"

``` r
# 페이지 이동하기
remote$navigate(url = "https://www.naver.com")

# 페이지 HTML 문서 가져오기
res <- remote$getPageSource() %>% `[[`(1)

# 가져온 페이지에서 실시간 검색어 부분 뽑아보기
example <- res %>%
  read_html() %>% 
  html_nodes(css = "span.ah_k") %>% 
  html_text()

# 출력해보기
example[1:10]
```

    ##  [1] "속초"          "속초 산불"     "고성산불"      "산불"         
    ##  [5] "속초 불"       "케이케이"      "고성"          "강원도 산불"  
    ##  [9] "김양"          "영화배우 신씨"

``` r
# 네이버 검색어창 지정하기
query <- remote$findElement(using = "xpath", value = "//*[@id='query']")

# 검색어 창에 사과 입력해보기
query$sendKeysToElement(sendKeys = list('사과'))

# 검색 버튼 지정하기
queryBtn <- remote$findElement(using = "xpath", 
                               value = "//*[@id='search_btn']")

# 검색 버튼 클릭하기
queryBtn$clickElement()

# 뒤로가기
remote$goBack()
# 앞으로가기
remote$goForward()
# 새로고침
remote$refresh()
```

실습3 - 교보문고 로그인하기
---------------------------

``` r
# 교보문고로 이동한다.
remote$navigate(url = "http://www.kyobobook.co.kr")

# 현재 컨트롤 중인 윈도우 창들의 아이디를 얻는다.
window.id <- remote$getWindowHandles()
print(window.id)
```

    ## [[1]]
    ## [1] "CDwindow-B904A6B94058C823420A75E82792128D"
    ## 
    ## [[2]]
    ## [1] "CDwindow-3F1FB230166B5DA747E700419E713691"

``` r
# 끄고싶은 윈도우 창으로 스위칭하기
remote$switchToWindow(windowId = window.id[[2]])

# 해당 원도우 창 닫기
remote$closeWindow()

# 다시 조작할 윈도우 창으로 이동하기
remote$switchToWindow(windowId = window.id[[1]])

# 로그인 버튼의 위치 지정하기
login <- remote$findElement(using = "xpath", value = "//*[@id='gnbLoginInfoList']/li[1]/a")

# 로그인 클릭
login$clickElement()

# 경고 문구 출력하기
# remote$getAlertText()

# 경고 수용하기
# remote$acceptAlert()

# 브라우져 닫기
remote$close()
```

로그인 쿠키 활용 방법
=====================

쿠키란 웹 사이트에 방문했을 때 웹 서버가 클라이언트 컴퓨터에 저장해놓는 작은 파일을 의미한다.<br> 웹 서버가 저장해놓은 쿠키를 얻으면 **로그인 상태로 HTTP요청**이 가능해진다.<br>

일반적으로 **POST 방식**이 사용된다.<br> res &lt;- POST(url = "로그인 URL",<br> body = list("로그인 아이디 파라미터명" = "로그인 아이디",<br> "로그인 비밀번호 파라미터명" = "로그인 비밀번호"))<br>

로그인에 성공하면 응답 헤더에서 쿠키를 얻을 수 있다.<br> 쿠키를 저장 후 다음 HTTP 요청 시 추가하면 로그인 상태의 HTML을 얻을 수 있다.<br> **cookies(res)**<br>

cookies()함수는 결과로 **리스트형 객체**를 반환한다.<br> **unlist()**를 통해서 벡터로 변환해준다.<br> mycookies &lt;- cookies(res) %&gt;% unlist()<br>

쿠키를 저장하였다면 다음과 같이 사용한다.<br> res &lt;- GET("관심 URL", set\_cookies(.cookies = mycookies))

실습4 - 로그인 쿠키를 이용하여 로그인 상태로 데이터 수집하기
------------------------------------------------------------

``` r
# 일단 잡플레닛에 들어가서 로그인 페이지로 간다.
# 그후, presereve log를 체크하고 로그인을 하면 sign_in이라는 문서가 나오는데,
# 이것이 바로 로그인 쿠키이다. 이것의 형식을 post 방식으로 request하면 된다.
res <- POST(url = "https://www.jobplanet.co.kr/users/sign_in",
            body = list("user[email]" = "rkdalsrl1511@naver.com",
                        "user[password]" = "s1352313"))

# 결과보기
print(res)
```

    ## Response [https://www.jobplanet.co.kr/users/sign_in]
    ##   Date: 2019-04-04 15:16
    ##   Status: 200
    ##   Content-Type: application/json; charset=utf-8
    ##   Size: 62 B

``` r
# 로그인 쿠키 객체 설정하기
my.cookies <- set_cookies(.cookies = unlist(cookies(res)))

# 로그인을 해야만 들어갈 수 있는 페이지
res <- GET(url = "https://www.jobplanet.co.kr/profile/settings",
           config = list(cookies = my.cookies))

# 결과보기
print(res)
```

    ## Response [https://www.jobplanet.co.kr/profile/settings]
    ##   Date: 2019-04-04 15:16
    ##   Status: 200
    ##   Content-Type: text/html; charset=utf-8
    ##   Size: 78.7 kB
    ## <!DOCTYPE html>
    ## <!--[if lt IE 7]>
    ## <html class="no-js lt-ie9 lt-ie8 lt-ie7 win ko-KR" lang="ko">
    ## <![endif]-->
    ## <!--[if IE 7]>
    ## <html class="no-js lt-ie9 lt-ie8 lt-ie7 win ko-KR" lang="ko">
    ## <![endif]-->
    ## <!--[if IE 8]>
    ## <html class="no-js lt-ie9 lt-ie8 win ko-KR" lang="ko">
    ## <![endif]-->
    ## ...

``` r
# 확인해보기
res %>%
  content(as = "text", encoding = "UTF-8") %>% 
  str_detect(pattern = "rkdalsrl1511")
```

    ## [1] TRUE

stringr 패키지 주요 함수 사용법
===============================

stringr 패키지는 문자 데이터를 다루는 데 필요한 주요 함수를 담고 있다.<br>

|                        함수                       |                         기능                         |
|:-------------------------------------------------:|:----------------------------------------------------:|
|             str\_detect(pattern = "")             |        찾고자 하는 패턴이 포함되어 있는지 확인       |
|             str\_remove(pattern = "")             |                해당 패턴을 한 번 삭제                |
|           str\_remove\_all(pattern = "")          |                 해당 패턴을 모두 삭제                |
|    str\_replace(pattern = "", replacement = "")   |                해당 패턴을 한 번 교체                |
| str\_replace\_all(pattern = "", replacement = "") |                  해당 패턴 전부 교체                 |
|             str\_extract(pattern = "")            |                해당 패턴을 한 번 추출                |
|          str\_extract\_all(pattern = "")          |                 해당 패턴을 전부 추출                |
|         str\_sub(start = 숫자, end = 숫자)        |              start부터 end까지 가져온다.             |
|               str\_c(args, sep = "")              | 두 개 이상의 문자열을 묶는다. sep을 통해 구분자 설정 |
|              str\_split(pattern = "")             |        하나의 문자열을 구분자를 기준으로 분리        |
|                    str\_trim()                    |          양 옆의 불필요한 공백들을 제거한다.         |

실습 - stringr 패키지 실습하기
------------------------------

``` r
library(stringr)
library(magrittr)
```

    ## 
    ## Attaching package: 'magrittr'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     set_names

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     extract

``` r
# 실습할 문장
string <- "동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세"



# 빠르게 한 번 씩

string %>% str_detect(pattern = "백두산")
```

    ## [1] TRUE

``` r
string %>% str_remove(pattern = "백두산")
```

    ## [1] "동해물과 이 마르고 닳도록 하느님이 보우하사 우리나라 만세"

``` r
string %>% str_replace_all(pattern = " ", replacement = "/")
```

    ## [1] "동해물과/백두산이/마르고/닳도록/하느님이/보우하사/우리나라/만세"

``` r
string %>% str_extract_all(pattern = "이")
```

    ## [[1]]
    ## [1] "이" "이"

``` r
string %>% str_sub(start = 1, end = 10)
```

    ## [1] "동해물과 백두산이 "

``` r
str_c("안녕","하세요", sep = " ")
```

    ## [1] "안녕 하세요"

``` r
string %>% str_split(pattern = " ")
```

    ## [[1]]
    ## [1] "동해물과" "백두산이" "마르고"   "닳도록"   "하느님이" "보우하사"
    ## [7] "우리나라" "만세"

``` r
str_trim("                 아            ")
```

    ## [1] "아"

정규표현식
==========

**stringr 패키지 필요하다!**<br> 정규표현식은 **패턴을 갖는 문자열**의 집합을 표현하는 데 사용하는 언어이다.<br> 다른 언어에도 정규표현식이 있다. R에서는 **escape 문자를 두 번 사용해야 한다**는 특징이 있다.<br> 정규표현식만을 따로 정리한 책이 있을 정도로 그 내용이 복잡하고 방대하다.<br> **정규표현식은 일반적인 패턴 사용법과 혼용**해서 사용해도 문제가 없다.

**정규표현식 기본 문법 1**

|   정규표현식  |                 의미                 |        정규표현식        |                의미               |
|:-------------:|:------------------------------------:|:------------------------:|:---------------------------------:|
|     \\\\w     | 영소문자, 영대문자, 숫자, \_(언더바) |             .            |  모든 문자(공백포함\\r, \\n 제외) |
|     \\\\d     |                 숫자                 |        파이프문자        | 앞 뒤 문자열들을 or 조건으로 지정 |
|     \\\\s     |      공백, \\r\\n(개행), \\t(탭)     |           \[ \]          | \[ \]안 문자들을 or 조건으로 지정 |
|     \\\\W     |             \\\\w의 반대             |     \[a-z\], \[A-Z\]     |      영어 소문자, 영어 대문자     |
|     \\\\D     |             \\\\d의 반대             |          \[0-9\]         |                숫자               |
|     \\\\S     |             \\\\s의 반대             | \[ㄱ - ㅣ\], \[가 - 힣\] |      한글 자음/모음, 한글문자     |
| \\\\p{Hangul} |                 한글                 |           \[^\]          |      대괄호 안 패턴 제외(not)     |

**정규표현식 기본 문법 2**

|   정규표현식  |             의미             | 정규표현식 |               의미              |
|:-------------:|:----------------------------:|:----------:|:-------------------------------:|
|       +       | 앞 패턴이 1~무한대 연속 일치 |      ^     |    문자열의 시작 위치를 지정    |
|       \*      | 앞 패턴이 0~무한대 연속 일치 |      $     |     문자열의 끝 위치를 지정     |
|       ?       |  앞 패턴이 없거나 1번만 일치 |    \\\\b   | 문자열 중 음절의 시작 위치 지정 |
|      {n}      |    앞 패턴이 n번 연속 일치   |    \\\\B   |           \\\\b의 반대          |
|      {n,}     | 앞 패턴이 n~무한대 연속 일치 |     ( )    |  ( ) 안 문자열을 그룹으로 지정  |
|     {n,m}     |   앞 패턴이 n~m번 연속 일치  |  \\\\숫자  |   그룹의 번호를 가리킴(역참조)  |
| 게으른 수량자 |  탐욕적 수량자 뒤에 ?를 붙임 |   A(?=B)   |      B패턴 앞의 A패턴 지정      |
|  \\\\메타문자 |   메타문자 모습 그대로 지정  | (?&lt;=A)B |      A패턴 뒤의 B패턴 지정      |

실습 - 정규표현식
-----------------

``` r
# 실습용 문자열
string <- "abCD가나123 \r\n\t-_,./?\\"

# 문자열 길이 확인
nchar(string)
```

    ## [1] 20

``` r
# \\w : 영어 대소문자, 한글, 숫자 및 _
string %>% str_extract_all(pattern = "\\w")
```

    ## [[1]]
    ##  [1] "a"  "b"  "C"  "D"  "가" "나" "1"  "2"  "3"  "_"

``` r
# \\d : 숫자
string %>% str_extract_all(pattern = "\\d")
```

    ## [[1]]
    ## [1] "1" "2" "3"

``` r
# \\s : 공백과 개행 탭을 지정
string %>% str_extract_all(pattern = "\\s")
```

    ## [[1]]
    ## [1] " "  "\r" "\n" "\t"

``` r
# \\p{Hangul} : 한글만
string %>% str_extract_all(pattern = "\\p{Hangul}")
```

    ## [[1]]
    ## [1] "가" "나"

``` r
# . : 모든 문자
string %>% str_extract_all(pattern = ".")
```

    ## [[1]]
    ##  [1] "a"  "b"  "C"  "D"  "가" "나" "1"  "2"  "3"  " "  "\t" "-"  "_"  "," 
    ## [15] "."  "/"  "?"  "\\"

``` r
# 사실 대문자들은 큰 의미가 없다.
# \\W : \\w의 반대
string %>% str_extract_all(pattern = "\\W")
```

    ## [[1]]
    ##  [1] " "  "\r" "\n" "\t" "-"  ","  "."  "/"  "?"  "\\"

``` r
# \\D : \\d의 반대
string %>% str_extract_all(pattern = "\\D")
```

    ## [[1]]
    ##  [1] "a"  "b"  "C"  "D"  "가" "나" " "  "\r" "\n" "\t" "-"  "_"  ","  "." 
    ## [15] "/"  "?"  "\\"

``` r
# \\S : \\s의 반대
string %>% str_extract_all(pattern = "\\S")
```

    ## [[1]]
    ##  [1] "a"  "b"  "C"  "D"  "가" "나" "1"  "2"  "3"  "-"  "_"  ","  "."  "/" 
    ## [15] "?"  "\\"

실습2 - or조건으로 패턴 지정
----------------------------

``` r
# 실습용 문자열
string1 <- c("abc","bcd","cde","def")
string2 <- "abcdEFGH0123ㄱㅏㄴㅑ가나다라"

# 앞과 뒤의 문자를 or로 지정한다.
string1 %>% str_extract(pattern = "ab|cd")
```

    ## [1] "ab" "cd" "cd" NA

``` r
# 대괄호 안 문자들을 or로 지정한다.
string1 %>% str_extract(pattern = "[af]")
```

    ## [1] "a" NA  NA  "f"

``` r
# 영어 소문자 지정
string2 %>% str_extract_all(pattern = "[a-z]")
```

    ## [[1]]
    ## [1] "a" "b" "c" "d"

``` r
# 영어 대문자 지정
string2 %>% str_extract_all(pattern = "[A-Z]")
```

    ## [[1]]
    ## [1] "E" "F" "G" "H"

``` r
# 숫자만 지정
string2 %>% str_extract_all(pattern = "[0-9]")
```

    ## [[1]]
    ## [1] "0" "1" "2" "3"

``` r
# 한글 자음/모음 지정
string2 %>% str_extract_all(pattern = "[ㄱ-ㅣ]")
```

    ## [[1]]
    ## [1] "ㄱ" "ㅏ" "ㄴ" "ㅑ"

``` r
# 한글 지정
string2 %>% str_extract_all(pattern = "[가-힣]")
```

    ## [[1]]
    ## [1] "가" "나" "다" "라"

``` r
# 대괄호 안의 문자 제외하기 (not)
string2 %>% str_extract_all(pattern = "[^ㄱ-ㅣ가-힣]")
```

    ## [[1]]
    ##  [1] "a" "b" "c" "d" "E" "F" "G" "H" "0" "1" "2" "3"

실습3 - 탐욕적 수량자와 게으른 수량자
-------------------------------------

``` r
# 탐욕적 수량자
string1 <- c("12","345","가나","다라마")

# + : 앞 패턴이 1~무한대 연속 일치
string1 %>% str_extract(pattern = "\\d+")
```

    ## [1] "12"  "345" NA    NA

``` r
# * : 앞 패턴이 0~무한대 연속 일치
string1 %>% str_extract(pattern = "\\d*")
```

    ## [1] "12"  "345" ""    ""

``` r
# ? : 앞 패턴이 0~1회 일치
string1 %>% str_extract(pattern = "\\d?")
```

    ## [1] "1" "3" ""  ""

``` r
# {n} : 앞 패턴이 n번 일치
string1 %>% str_extract(pattern = "\\d{2}")
```

    ## [1] "12" "34" NA   NA

``` r
# {n,} : 앞 패턴이 n~무한대 연속 일치
string1 %>% str_extract(pattern = "\\p{Hangul}{3,}")
```

    ## [1] NA       NA       NA       "다라마"

``` r
# {n,m} : 앞 패턴이 n~m번 일치
string1 %>% str_extract(pattern = "\\d{1,2}")
```

    ## [1] "12" "34" NA   NA

``` r
# 게으른 수량자
# 탐욕적 수량자 뒤에 ?를 붙이면 최소 단위로 일치할 때마다 패턴을 찾는다.
string2 <- "<p>이것은<br>HTML<br>입니다</p>"

# 탐욕적 수량자를 추가하지 않았을 경우
string2 %>% str_extract_all(pattern = "<.+>")
```

    ## [[1]]
    ## [1] "<p>이것은<br>HTML<br>입니다</p>"

``` r
# 게으른 수량자를 추가하였을 경우
string2 %>% str_extract_all(pattern = "<.+?>")
```

    ## [[1]]
    ## [1] "<p>"  "<br>" "<br>" "</p>"

실습4 - 기타 정규표현식
-----------------------

``` r
string1 <- "우리집 강아지는 (복슬강아지)입니다."
string2 <- c("가나다","나다라","가다라","라가나","다라가")
string3 <- "가방에 가위와 종이 쪼가리를 넣어라"
string4 <- "매경부동산과 한경부동산보다 KB부동산이 더 자세합니다."
string5 <- c("100원","300달러","450엔","800위안")

# escape 문자 : \\를 앞에 붙이면 된다.
string1 %>% str_extract(pattern = "\\(\\w+\\)")
```

    ## [1] "(복슬강아지)"

``` r
# ^ : 문자열의 시작 위치를 지정하며, [^]와 헷갈리지 말자.
string2 %>% str_extract(pattern = "^가")
```

    ## [1] "가" NA   "가" NA   NA

``` r
# $ : 문자열의 끝 위치를 지정한다.
string2 %>% str_extract(pattern = "가$")
```

    ## [1] NA   NA   NA   NA   "가"

``` r
# \\b : 문자열 중 음절의 시작 위치를 지정한다.
string3 %>% str_remove_all(pattern = "\\b가")
```

    ## [1] "방에 위와 종이 쪼가리를 넣어라"

``` r
# \\B : \\b가 아닌 위치를 지정한다.
string3 %>% str_remove_all(pattern = "\\B가")
```

    ## [1] "가방에 가위와 종이 쪼리를 넣어라"

``` r
# ( ) : 소괄호 안 문자열을 그룹으로 지정한다.
string4 %>% str_remove_all(pattern = "(부동산)")
```

    ## [1] "매경과 한경보다 KB이 더 자세합니다."

``` r
# \\숫자 : 그룹의 번호를 가리킨다. 문자열에서의 그룹 설정
# 패턴에 포함되는 것들을 전부 그룹으로 해버리는 모양이다.
string4 %>% str_extract(pattern = "(부동산).+\\1")
```

    ## [1] "부동산과 한경부동산보다 KB부동산"

``` r
string4 %>% str_extract(pattern = "(부동산).+?\\1")
```

    ## [1] "부동산과 한경부동산"

``` r
# A(?=B) : B패턴 앞의 A패턴을 지정한다. (전방탐색)
string5 %>% str_extract(pattern = "\\d+(?=\\p{Hangul}+)")
```

    ## [1] "100" "300" "450" "800"

``` r
# (?<=A)B : A패턴 뒤의 B패턴을 지정한다. (후방탐색)
string5 %>% str_extract(pattern = "(?<=\\d)\\p{Hangul}+")
```

    ## [1] "원"   "달러" "엔"   "위안"
