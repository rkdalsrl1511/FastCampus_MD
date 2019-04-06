fastcampus\_웹크롤링\_5
================
huimin
2019년 4월 6일

기초 설정하기
=============

``` r
library(httr)
library(rvest)
```

    ## Loading required package: xml2

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------- tidyverse 1.2.1 --

    ## √ ggplot2 3.1.0       √ purrr   0.3.1  
    ## √ tibble  2.0.1       √ dplyr   0.8.0.1
    ## √ tidyr   0.8.3       √ stringr 1.4.0  
    ## √ readr   1.3.1       √ forcats 0.4.0

    ## -- Conflicts ------------------ tidyverse_conflicts() --
    ## x dplyr::filter()         masks stats::filter()
    ## x readr::guess_encoding() masks rvest::guess_encoding()
    ## x dplyr::lag()            masks stats::lag()
    ## x purrr::pluck()          masks rvest::pluck()

``` r
library(stringr)
library(jsonlite)
```

    ## 
    ## Attaching package: 'jsonlite'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     flatten

``` r
library(readr)
library(urltools)
```

    ## 
    ## Attaching package: 'urltools'

    ## The following object is masked from 'package:xml2':
    ## 
    ##     url_parse

Open API를 활용한 웹 크롤링
===========================

**RSelenium의 경우에는 javaScript 등의 여러가지 웹 기술들이 활용된 페이지의 데이터도 쉽게 크롤링할 수 있다는 장점이 있지만, 느리다.**<br> **API**의 경우에는 에초부터 데이터를 주고 받을 목적으로 만들어지고, 사용되기 때문에 크롤링하기 편하도록 정리되어 있다는 것이 특징이다.<br> 응용프로그램과 운영체제 간 통신을 위한 인터페이스라고 할 수 있다.

**API 서비스 사용자가 사용 권한을 신청**하면, API 제공자는 **API 인증키**를 발급한다.<br> 사용자는 **API 오퍼레이션에서 요구하는 방법**으로 서비스 요청하고, 제공자는 XML 또는 JSON 형식으로 데이터를 전송하여 응답한다.

**(1) 공공데이터 포털에서 오픈 API에 접속한다.**

![Caption](img/day05_1.jpg)

**(2)** API 활용 신청 후, 상세보기를 하면, **인증키와 기타 요청변수**를 확인할 수 있다.

![Caption](img/day05_2.jpg)

XML과 JSON의 비교
=================

**XML**은 마크업 언어로, 인터넷으로 연결된 **서로 다른 시스템**끼리 데이터를 주고 받으려는 목적으로 만들어졌다.

**활용법**<br> read\_xml(x, encoding)<br> xml\_node(x, css, xpath)<br> xml\_nodes(x, css, xpath)<br> xml\_text(x, trim = FALSE)

**JSON**은 javascript object notation의 줄임말로, **javascript**를 이용하여 데이터를 주고 받을 때 사용되는 교환 형식이다.

**활용법**<br> fromJSON()<br> fromJSON에는 txt인자를 넣는다. 따라서 **as.character()** 또는 **content(as="text",encoding)**함수를 활용해서 문자열 벡터로 변환하면 된다.

실습1 - 대기오염정보 조회 서비스 인증키 저장하기
================================================

``` r
# R 환경변수 확인하기
usethis::edit_r_environ()
```

    ## ● Edit C:/Users/Leehuimin/Documents/.Renviron
    ## ● Restart R for changes to take effect

``` r
# 활용가이드를 본다면, 공공데이터 포털에서는 
# DATAGOKR_TOKEN = "인증키"
# 형식을 지정하고 있다.

# 따라서 R환경변수 창에서 위의 형식을 저장하면 앞으로 사용할 때 편리하다.

# 환경변수에 저장한 내용 출력해보기
Sys.getenv("DATAGOKR_TOKEN")
```

    ## [1] "%2FCfYlHHo4pI%2Fiz6ahALH10GN8aIpwO70D7%2Bv7dZCTC%2Bd%2BUWuEIdn1CLTqHhCDJI%2FCbGWFquUr6VaAKfs71AP4Q%3D%3D"

``` r
# 인증키 객체 만들기
my.apikey <- Sys.getenv("DATAGOKR_TOKEN")
```

실습2 - 대기오염정보 조회 서비스 응답받기
=========================================

``` r
# 미리보기를 통해서 가져온 url
# http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getMsrstnAcctoRltmMesureDnsty?serviceKey=%2FCfYlHHo4pI%2Fiz6ahALH10GN8aIpwO70D7%2Bv7dZCTC%2Bd%2BUWuEIdn1CLTqHhCDJI%2FCbGWFquUr6VaAKfs71AP4Q%3D%3D&numOfRows=10&pageNo=1&stationName=%EC%A2%85%EB%A1%9C%EA%B5%AC&dataTerm=DAILY&ver=1.3


# 인코딩 형식 확인하기
guess_encoding("http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getMsrstnAcctoRltmMesureDnsty?serviceKey=%2FCfYlHHo4pI%2Fiz6ahALH10GN8aIpwO70D7%2Bv7dZCTC%2Bd%2BUWuEIdn1CLTqHhCDJI%2FCbGWFquUr6VaAKfs71AP4Q%3D%3D&numOfRows=10&pageNo=1&stationName=%EC%A2%85%EB%A1%9C%EA%B5%AC&dataTerm=DAILY&ver=1.3")
```

    ## # A tibble: 3 x 2
    ##   encoding     confidence
    ##   <chr>             <dbl>
    ## 1 UTF-8              1   
    ## 2 windows-1252       0.56
    ## 3 windows-1250       0.34

``` r
# 가이드를 통해서 인자들 확인하기
api.url <- "http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getMsrstnAcctoRltmMesureDnsty"

# 종로구를 UTF-8 형식으로 바꾸고 url인코딩
api.stationname <- "%EC%A2%85%EB%A1%9C%EA%B5%AC"

# 설명을 보면 daily, month, 3month가 있음
api.dataterm <- "MONTH"

# 한 페이지당 보여주는 행의 수. 9999로 설정하면 반복문 쓸 필요 없음
api.numofraws <- 9999

# 페이지 번호
api.pageno <- 1

# API 버전으로 추정됨. 그냥 나와 있는 걸로 쓰면 된다.
api.ver <- 1.3



# http request
res <- GET(url = api.url,
           query = list(serviceKey = my.apikey %>% I(),
                        numOfRows = api.numofraws,
                        pageNo = api.pageno,
                        stationName = api.stationname %>% I(),
                        dataTerm = api.dataterm %>% I(),
                        ver = api.ver))


# totalCount 확인하기
res %>%
  read_xml() %>% 
  xml_nodes(css = "totalCount") %>% 
  xml_text()
```

    ## [1] "743"

``` r
# 이 중에서 미세먼지 한달치 자료들 데이터프레임에 저장하기
response.result <- res %>% read_xml()

result <- data.frame(dataTime = response.result %>% 
                       xml_nodes(css = "dataTime") %>% 
                       xml_text(),
                     pm10Value = response.result %>% 
                       xml_nodes(css = "pm10Value") %>% 
                       xml_text(),
                     pm10Value24 = response.result %>% 
                       xml_nodes(css = "pm10Value24") %>% 
                       xml_text(),
                     pm25Value = response.result %>% 
                       xml_nodes(css = "pm25Value") %>% 
                       xml_text(),
                     pm25Value24 = response.result %>% 
                       xml_nodes(css = "pm25Value24") %>% 
                       xml_text(),
                     pm10Grade = response.result %>% 
                       xml_nodes(css = "pm10Grade") %>% 
                       xml_text(),
                     pm25Grade = response.result %>% 
                       xml_nodes(css = "pm25Grade") %>% 
                       xml_text(),
                     pm10Grade1h = response.result %>% 
                       xml_nodes(css = "pm10Grade1h") %>% 
                       xml_text(),
                     pm25Grade1h = response.result %>% 
                       xml_nodes(css = "pm25Grade1h") %>% 
                       xml_text())


# 결과 출력해보기
head(result, n = 10)
```

    ##            dataTime pm10Value pm10Value24 pm25Value pm25Value24 pm10Grade
    ## 1  2019-04-06 19:00        48          55        17          19         2
    ## 2  2019-04-06 18:00        50          57        19          19         2
    ## 3  2019-04-06 17:00        58          57        21          19         2
    ## 4  2019-04-06 16:00        60          56        20          18         2
    ## 5  2019-04-06 15:00        58          55        19          18         2
    ## 6  2019-04-06 14:00        56          55        19          18         2
    ## 7  2019-04-06 13:00        57          55        20          17         2
    ## 8  2019-04-06 12:00        61          55        19          16         2
    ## 9  2019-04-06 11:00        63          53        21          15         2
    ## 10 2019-04-06 10:00        59          51        17          13         2
    ##    pm25Grade pm10Grade1h pm25Grade1h
    ## 1          2           2           2
    ## 2          2           2           2
    ## 3          2           2           2
    ## 4          2           2           2
    ## 5          2           2           2
    ## 6          2           2           2
    ## 7          2           2           2
    ## 8          2           2           2
    ## 9          1           2           2
    ## 10         1           2           2
