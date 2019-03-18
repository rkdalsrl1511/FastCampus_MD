fastcampus\_R프로그래밍\_3
================
huimin
2019년 3월 18일

Import Data ( 데이터 불러오기 )
===============================

(1) txt
-------

### separator : one blank ( 공백으로 데이터 구분 )<br>

``` r
# data.name <- read.table(file = "directory/filename.txt", 
#                         header = TRUE, 
#                         sep = " ",
#                         stringsAsFactors = FALSE)
# stringsAsFactors : string을 factor로 변환해준다는 뜻이다.
# header = TRUE : raw 데이터의 첫행을 R의 변수명으로 사용하겠다.

blank <- read.table(file = "C:/Users/Leehuimin/Desktop/프로그래밍 언어/R데이터 분석 집중완성 SCHOOL/01_17_R프로그래밍/day 3/day3.txt", 
                    header = TRUE, 
                    sep = " ",
                    stringsAsFactors = TRUE)

blank
```

    ##   id nation address ttime
    ## 1  1 베트남  역삼동    10
    ## 2  2 스위스  흑석동    60
    ## 3  3   미국    수원    90
    ## 4  4 그리스  돈암동    60

``` r
str(blank) # stringsasfactors를 통해서 string이 factor로 변환되었다.
```

    ## 'data.frame':    4 obs. of  4 variables:
    ##  $ id     : int  1 2 3 4
    ##  $ nation : Factor w/ 4 levels "그리스","미국",..: 3 4 2 1
    ##  $ address: Factor w/ 4 levels "돈암동","수원",..: 3 4 2 1
    ##  $ ttime  : int  10 60 90 60

``` r
blank2 <- read.table(file = "C:/Users/Leehuimin/Desktop/프로그래밍 언어/R데이터 분석 집중완성 SCHOOL/01_17_R프로그래밍/day 3/day3.txt", 
                     header = TRUE, 
                     sep = " ",
                     stringsAsFactors = FALSE)

str(blank2) # stringsasfactors를 false라고 했을 경우, chr이다.
```

    ## 'data.frame':    4 obs. of  4 variables:
    ##  $ id     : int  1 2 3 4
    ##  $ nation : chr  "베트남" "스위스" "미국" "그리스"
    ##  $ address: chr  "역삼동" "흑석동" "수원" "돈암동"
    ##  $ ttime  : int  10 60 90 60

### separator : comma

``` r
# data.name <- read.table(file = "directory/filename.txt", 
#                         header = TRUE, 
#                         sep = ",")

comma <- read.table(file = "C:/Users/Leehuimin/Desktop/프로그래밍 언어/R데이터 분석 집중완성 SCHOOL/01_17_R프로그래밍/day 3/day3_2.txt", 
                    header = TRUE, 
                    sep = ",")

comma
```

    ##   id company    department team
    ## 1  1     AQR quantAnalysis    4
    ## 2  2    삼성        품질과   10
    ## 3  3  아모레          기획    4
    ## 4  4     P&G  dataAnalysis    4
    ## 5  5    축협        마케팅    7

### separator : tab

``` r
# data.name <- read.table(file = "directory/filename.txt", 
#                         header = TRUE, 
#                         sep = "\t")

tab <- read.table(file = "C:/Users/Leehuimin/Desktop/프로그래밍 언어/R데이터 분석 집중완성 SCHOOL/01_17_R프로그래밍/day 3/day3_3.txt", 
                    header = TRUE, 
                    sep = "\t")

tab
```

    ##   id   food      drink
    ## 1  1   피자       콜라
    ## 2  2 파스타 아이스라떼
    ## 3  3   한식       맥주
    ## 4  4   치킨       콜라
    ## 5  5 삼계탕       소주
    ## 6  6 꽃게탕       맥주

(2) csv
-------

``` r
# comma separated value의 약자
# 엑셀의 특수한 형태
# kaggle.com 에서 csv 데이터를 얻어 올 수 있다.

# data.name <- read.csv(file = "directory/filename.csv",
#                       header = TRUE,
#                       stringsAsFactors = FALSE)

hope <- read.csv(file = "C:/Users/Leehuimin/Desktop/프로그래밍 언어/R데이터 분석 집중완성 SCHOOL/01_17_R프로그래밍/day 3/day3.csv",
                 header = TRUE)

hope
```

    ##   id     idol myear
    ## 1  1     휘인    35
    ## 2  2 다니엘헨    34
    ## 3  3   조보아    32

(3) excel
---------

excel : xls(2003 이하 버전), xlsx(2007 이상 버전)<br> R의 기본 기능으로는 못 읽어온다.<br> R의 readxl package를 받아서, 읽어올 수 있다.<br>

``` r
library(readxl)

# data.name <- readxl::read_excel(path = "directory/filename.xlsx",
#                                 sheet = "sheet.name" or sheet.index,
#                                 col_names = TRUE)
# col_names는 header와 역할이 동일하다.

# sheet의 이름으로 불러오기
reading <- readxl::read_excel(path = "C:/Users/Leehuimin/Desktop/프로그래밍 언어/R데이터 분석 집중완성 SCHOOL/01_17_R프로그래밍/day 3/reading.xlsx",
                              sheet = "Sheet1",
                              col_names = TRUE)

reading
```

    ## # A tibble: 5 x 3
    ##      id books author
    ##   <dbl> <dbl> <chr> 
    ## 1     1     8 <NA>  
    ## 2     2    20 무로아
    ## 3     3     5 <NA>  
    ## 4     4     5 강일중
    ## 5     5    32 김형경

``` r
# sheet의 위치를 지정해서 불러올 수도 있다.
reading2 <- readxl::read_excel(path = "C:/Users/Leehuimin/Desktop/프로그래밍 언어/R데이터 분석 집중완성 SCHOOL/01_17_R프로그래밍/day 3/reading.xlsx",
                               sheet = 1,
                               col_names = TRUE)

reading2
```

    ## # A tibble: 5 x 3
    ##      id books author
    ##   <dbl> <dbl> <chr> 
    ## 1     1     8 <NA>  
    ## 2     2    20 무로아
    ## 3     3     5 <NA>  
    ## 4     4     5 강일중
    ## 5     5    32 김형경

``` r
# tibble : 데이터 프레임과 비슷한 데이터 유형이다.
# dbl : 더블을 의미한다.
# chr : character 타입을 의미한다.
```

참고 : 인터넷이 연결되지 않았을 때의 패키지 설치하기
====================================================

1.  인터넷이 되는 컴퓨터로 간다.<br>
2.  www.r-project.org -&gt; CRAN -&gt; Korea -&gt; 사이트 하나에 들어가서 packages -&gt; table of available packages -&gt; os에 맞는 압축 파일 탐색<br>
3.  해당 패키지 자료를 외장하드에 저장한다.<br>
4.  해당 컴퓨터에 복사<br>
5.  패키지 R에 설치하기<br> install.packages("directory/xxxx.zip", repos = NULL)<br> repos : Local Computer를 의미한다. 패키지를 다운로드 받는 서버의 주소<br> repos의 default값은 www.rstudio.com 인데, 이 경우에는 직접 설치이므로, NULL로 지정하는 것이다.
