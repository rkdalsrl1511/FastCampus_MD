fastcampus\_R프로그래밍\_4
================
huimin
2019년 3월 18일

Data Handling
=============

Data Handling = Data Pre-processing = Data Wrangling ( 데이터 전처리 )<br> 데이터 핸들리은 데이터 분석 과정의 70~80%를 차지한다.

``` r
library("ggplot2")
library("DT")
library("dplyr")
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# 작업공간
setwd("d:/fastcampus/")
```

전체 데이터 보기
----------------

``` r
# 1.1 data.name
# console에 출력이 됨
# 데이터가 많으면 다 보여주지 못하고 일부만 보여줌
diamonds # 총 53940행
```

    ## # A tibble: 53,940 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ##  3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ##  4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ##  5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ##  6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
    ##  7 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
    ##  8 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
    ##  9 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 10 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39
    ## # ... with 53,930 more rows

``` r
# 1.2 View(data.name) -  V 대문자임 주의!
# editor window에 출력이 됨
# 데이터가 많이다 다 보여줌
# View(diamonds)

# 1.3 DT::datatable(data.name)
# web style로 출력이 됨
# 데이터가 많아도 다 보여줌
# 드래그하여, 엑셀에 쉽게 붙여넣을 수 있다.
# DT::datatable(diamonds)
```

데이터의 일부 보기
------------------

실무에서 데이터를 잘 읽어 왔는지 확인하는 측면. 데이터 일부를 보고 싶을 때 사용한다.

``` r
# 2.1 head(data.name, n =6)
# 데이터 중에서 1~6행을 console에 출력
head(diamonds)
```

    ## # A tibble: 6 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ## 2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ## 3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ## 4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ## 5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ## 6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48

``` r
head(diamonds, n = 20)
```

    ## # A tibble: 20 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ##  3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ##  4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ##  5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ##  6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
    ##  7 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
    ##  8 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
    ##  9 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 10 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39
    ## 11 0.3   Good      J     SI1      64      55   339  4.25  4.28  2.73
    ## 12 0.23  Ideal     J     VS1      62.8    56   340  3.93  3.9   2.46
    ## 13 0.22  Premium   F     SI1      60.4    61   342  3.88  3.84  2.33
    ## 14 0.31  Ideal     J     SI2      62.2    54   344  4.35  4.37  2.71
    ## 15 0.2   Premium   E     SI2      60.2    62   345  3.79  3.75  2.27
    ## 16 0.32  Premium   E     I1       60.9    58   345  4.38  4.42  2.68
    ## 17 0.3   Ideal     I     SI2      62      54   348  4.31  4.34  2.68
    ## 18 0.3   Good      J     SI1      63.4    54   351  4.23  4.29  2.7 
    ## 19 0.3   Good      J     SI1      63.8    56   351  4.23  4.26  2.71
    ## 20 0.3   Very Good J     SI1      62.7    59   351  4.21  4.27  2.66

``` r
# 2.2 tail(data.name, n = 6)
# 데이터 중에서 마지막행부터 console에 출력
tail(diamonds)
```

    ## # A tibble: 6 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  0.72 Premium   D     SI1      62.7    59  2757  5.69  5.73  3.58
    ## 2  0.72 Ideal     D     SI1      60.8    57  2757  5.75  5.76  3.5 
    ## 3  0.72 Good      D     SI1      63.1    55  2757  5.69  5.75  3.61
    ## 4  0.7  Very Good D     SI1      62.8    60  2757  5.66  5.68  3.56
    ## 5  0.86 Premium   H     SI2      61      58  2757  6.15  6.12  3.74
    ## 6  0.75 Ideal     D     SI2      62.2    55  2757  5.83  5.87  3.64

``` r
tail(diamonds, n = 20)
```

    ## # A tibble: 20 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.7  Very Good E     VS2      62.4    60  2755  5.57  5.61  3.49
    ##  2  0.7  Very Good E     VS2      62.8    60  2755  5.59  5.65  3.53
    ##  3  0.7  Very Good D     VS1      63.1    59  2755  5.67  5.58  3.55
    ##  4  0.73 Ideal     I     VS2      61.3    56  2756  5.8   5.84  3.57
    ##  5  0.73 Ideal     I     VS2      61.6    55  2756  5.82  5.84  3.59
    ##  6  0.79 Ideal     I     SI1      61.6    56  2756  5.95  5.97  3.67
    ##  7  0.71 Ideal     E     SI1      61.9    56  2756  5.71  5.73  3.54
    ##  8  0.79 Good      F     SI1      58.1    59  2756  6.06  6.13  3.54
    ##  9  0.79 Premium   E     SI2      61.4    58  2756  6.03  5.96  3.68
    ## 10  0.71 Ideal     G     VS1      61.4    56  2756  5.76  5.73  3.53
    ## 11  0.71 Premium   E     SI1      60.5    55  2756  5.79  5.74  3.49
    ## 12  0.71 Premium   F     SI1      59.8    62  2756  5.74  5.73  3.43
    ## 13  0.7  Very Good E     VS2      60.5    59  2757  5.71  5.76  3.47
    ## 14  0.7  Very Good E     VS2      61.2    59  2757  5.69  5.72  3.49
    ## 15  0.72 Premium   D     SI1      62.7    59  2757  5.69  5.73  3.58
    ## 16  0.72 Ideal     D     SI1      60.8    57  2757  5.75  5.76  3.5 
    ## 17  0.72 Good      D     SI1      63.1    55  2757  5.69  5.75  3.61
    ## 18  0.7  Very Good D     SI1      62.8    60  2757  5.66  5.68  3.56
    ## 19  0.86 Premium   H     SI2      61      58  2757  6.15  6.12  3.74
    ## 20  0.75 Ideal     D     SI2      62.2    55  2757  5.83  5.87  3.64

``` r
# 2.3 View(head() or tail())
# View(head(diamonds))
```

입력오류 체크하기
-----------------

데이터의 허용치, 범위, 좋고 나쁨 등을 파악해야한다. 실무에서 전문 분야 데이터를 이해하는 능력이 매우 중요하다.

``` r
# summary(data.name)
summary(diamonds)
```

    ##      carat               cut        color        clarity     
    ##  Min.   :0.2000   Fair     : 1610   D: 6775   SI1    :13065  
    ##  1st Qu.:0.4000   Good     : 4906   E: 9797   VS2    :12258  
    ##  Median :0.7000   Very Good:12082   F: 9542   SI2    : 9194  
    ##  Mean   :0.7979   Premium  :13791   G:11292   VS1    : 8171  
    ##  3rd Qu.:1.0400   Ideal    :21551   H: 8304   VVS2   : 5066  
    ##  Max.   :5.0100                     I: 5422   VVS1   : 3655  
    ##                                     J: 2808   (Other): 2531  
    ##      depth           table           price             x         
    ##  Min.   :43.00   Min.   :43.00   Min.   :  326   Min.   : 0.000  
    ##  1st Qu.:61.00   1st Qu.:56.00   1st Qu.:  950   1st Qu.: 4.710  
    ##  Median :61.80   Median :57.00   Median : 2401   Median : 5.700  
    ##  Mean   :61.75   Mean   :57.46   Mean   : 3933   Mean   : 5.731  
    ##  3rd Qu.:62.50   3rd Qu.:59.00   3rd Qu.: 5324   3rd Qu.: 6.540  
    ##  Max.   :79.00   Max.   :95.00   Max.   :18823   Max.   :10.740  
    ##                                                                  
    ##        y                z         
    ##  Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.: 4.720   1st Qu.: 2.910  
    ##  Median : 5.710   Median : 3.530  
    ##  Mean   : 5.735   Mean   : 3.539  
    ##  3rd Qu.: 6.540   3rd Qu.: 4.040  
    ##  Max.   :58.900   Max.   :31.800  
    ## 

``` r
# 변수가 가지는 값이 numeric이면, min 과 max를 보고,
# min, max가 가질 수 없는 값이면 입력오류.
# 변수가 가지는 값이 범주(category)일 때, 범주가 가질 수 없는 값이 있다면 입력오류.
# 단, summary()에서 범주가 가지는 값을 보여주는 것은 그 변수가 factor이기 때문이다.

diamonds$cut <- as.character(diamonds$cut)
summary(diamonds) # character의 경우, summary에서는 표현을 안 해준다.
```

    ##      carat            cut            color        clarity     
    ##  Min.   :0.2000   Length:53940       D: 6775   SI1    :13065  
    ##  1st Qu.:0.4000   Class :character   E: 9797   VS2    :12258  
    ##  Median :0.7000   Mode  :character   F: 9542   SI2    : 9194  
    ##  Mean   :0.7979                      G:11292   VS1    : 8171  
    ##  3rd Qu.:1.0400                      H: 8304   VVS2   : 5066  
    ##  Max.   :5.0100                      I: 5422   VVS1   : 3655  
    ##                                      J: 2808   (Other): 2531  
    ##      depth           table           price             x         
    ##  Min.   :43.00   Min.   :43.00   Min.   :  326   Min.   : 0.000  
    ##  1st Qu.:61.00   1st Qu.:56.00   1st Qu.:  950   1st Qu.: 4.710  
    ##  Median :61.80   Median :57.00   Median : 2401   Median : 5.700  
    ##  Mean   :61.75   Mean   :57.46   Mean   : 3933   Mean   : 5.731  
    ##  3rd Qu.:62.50   3rd Qu.:59.00   3rd Qu.: 5324   3rd Qu.: 6.540  
    ##  Max.   :79.00   Max.   :95.00   Max.   :18823   Max.   :10.740  
    ##                                                                  
    ##        y                z         
    ##  Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.: 4.720   1st Qu.: 2.910  
    ##  Median : 5.710   Median : 3.530  
    ##  Mean   : 5.735   Mean   : 3.539  
    ##  3rd Qu.: 6.540   3rd Qu.: 4.040  
    ##  Max.   :58.900   Max.   :31.800  
    ## 

``` r
diamonds$cut <- as.factor(diamonds$cut)
summary(diamonds)
```

    ##      carat               cut        color        clarity     
    ##  Min.   :0.2000   Fair     : 1610   D: 6775   SI1    :13065  
    ##  1st Qu.:0.4000   Good     : 4906   E: 9797   VS2    :12258  
    ##  Median :0.7000   Ideal    :21551   F: 9542   SI2    : 9194  
    ##  Mean   :0.7979   Premium  :13791   G:11292   VS1    : 8171  
    ##  3rd Qu.:1.0400   Very Good:12082   H: 8304   VVS2   : 5066  
    ##  Max.   :5.0100                     I: 5422   VVS1   : 3655  
    ##                                     J: 2808   (Other): 2531  
    ##      depth           table           price             x         
    ##  Min.   :43.00   Min.   :43.00   Min.   :  326   Min.   : 0.000  
    ##  1st Qu.:61.00   1st Qu.:56.00   1st Qu.:  950   1st Qu.: 4.710  
    ##  Median :61.80   Median :57.00   Median : 2401   Median : 5.700  
    ##  Mean   :61.75   Mean   :57.46   Mean   : 3933   Mean   : 5.731  
    ##  3rd Qu.:62.50   3rd Qu.:59.00   3rd Qu.: 5324   3rd Qu.: 6.540  
    ##  Max.   :79.00   Max.   :95.00   Max.   :18823   Max.   :10.740  
    ##                                                                  
    ##        y                z         
    ##  Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.: 4.720   1st Qu.: 2.910  
    ##  Median : 5.710   Median : 3.530  
    ##  Mean   : 5.735   Mean   : 3.539  
    ##  3rd Qu.: 6.540   3rd Qu.: 4.040  
    ##  Max.   :58.900   Max.   :31.800  
    ## 

데이터의 구조(Structure) 보기
-----------------------------

``` r
# str(data.name)
# tbl_df = dataframe, tbl = table, 53940 obs = 행의 개수
# variables = 열이자 변수
str(diamonds)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    53940 obs. of  10 variables:
    ##  $ carat  : num  0.23 0.21 0.23 0.29 0.31 0.24 0.24 0.26 0.22 0.23 ...
    ##  $ cut    : Factor w/ 5 levels "Fair","Good",..: 3 4 2 4 2 5 5 5 1 5 ...
    ##  $ color  : Ord.factor w/ 7 levels "D"<"E"<"F"<"G"<..: 2 2 2 6 7 7 6 5 2 5 ...
    ##  $ clarity: Ord.factor w/ 8 levels "I1"<"SI2"<"SI1"<..: 2 3 5 4 2 6 7 3 4 5 ...
    ##  $ depth  : num  61.5 59.8 56.9 62.4 63.3 62.8 62.3 61.9 65.1 59.4 ...
    ##  $ table  : num  55 61 65 58 58 57 57 55 61 61 ...
    ##  $ price  : int  326 326 327 334 335 336 336 337 337 338 ...
    ##  $ x      : num  3.95 3.89 4.05 4.2 4.34 3.94 3.95 4.07 3.87 4 ...
    ##  $ y      : num  3.98 3.84 4.07 4.23 4.35 3.96 3.98 4.11 3.78 4.05 ...
    ##  $ z      : num  2.43 2.31 2.31 2.63 2.75 2.48 2.47 2.53 2.49 2.39 ...

데이터의 속성(attribute)
------------------------

여기서 데이터의 속성이란, Data.Frame을 이야기함

``` r
# 5.1 행의 개수
# nrow(data.name) or NROW(data.name)
nrow(diamonds) # vector
```

    ## [1] 53940

``` r
# 5.2 열의 개수
# ncol(data.name) or NCOL(data.name)
ncol(diamonds) # vector
```

    ## [1] 10

``` r
# 5.3 행의 이름
# 거의 안 쓰인다.
# rownames(data.name)
# rownames(diamonds) # vector

# 5.4 열 = 변수의 이름
# 잘 쓰인다.
# colnames(data.name)
colnames(diamonds) # vector(character)
```

    ##  [1] "carat"   "cut"     "color"   "clarity" "depth"   "table"   "price"  
    ##  [8] "x"       "y"       "z"

``` r
# 5.5 차원(Dimension)
# 거의 안 쓴다.
# dim(data.name)
dim(diamonds) # vector
```

    ## [1] 53940    10

``` r
dim(diamonds)[1] # vector의 슬라이싱 : 행의 개수
```

    ## [1] 53940

``` r
dim(diamonds)[2] # vector의 슬라이싱 : 열의 개수
```

    ## [1] 10

``` r
# 5.6 차원의 이름
# 거의 안 쓴다.
# dimnames(data.name)
# dimnames(diamonds) # List
# dimnames(diamonds)[1] # List
# dimnames(diamonds)[[1]] # Vector

dimnames(diamonds)[2] # List
```

    ## [[1]]
    ##  [1] "carat"   "cut"     "color"   "clarity" "depth"   "table"   "price"  
    ##  [8] "x"       "y"       "z"

``` r
dimnames(diamonds)[[2]] # Vector
```

    ##  [1] "carat"   "cut"     "color"   "clarity" "depth"   "table"   "price"  
    ##  [8] "x"       "y"       "z"

데이터의 Slicing
----------------

여기서 데이터란, data.frame을 뜻한다.

### 열

``` r
# data.name[, column]

# (1) 열 = 변수의 위치를 알고 있을 때
diamonds[ , 1] # tibble안의 vector로 인식
```

    ## # A tibble: 53,940 x 1
    ##    carat
    ##    <dbl>
    ##  1 0.23 
    ##  2 0.21 
    ##  3 0.23 
    ##  4 0.290
    ##  5 0.31 
    ##  6 0.24 
    ##  7 0.24 
    ##  8 0.26 
    ##  9 0.22 
    ## 10 0.23 
    ## # ... with 53,930 more rows

``` r
diamonds[ , 2] # tibble안의 factor로 인식
```

    ## # A tibble: 53,940 x 1
    ##    cut      
    ##    <fct>    
    ##  1 Ideal    
    ##  2 Premium  
    ##  3 Good     
    ##  4 Premium  
    ##  5 Good     
    ##  6 Very Good
    ##  7 Very Good
    ##  8 Very Good
    ##  9 Fair     
    ## 10 Very Good
    ## # ... with 53,930 more rows

``` r
# 예제 1
# 1, 9 , 10 번째 열을 한 번에 가져오시오
diamonds[ , c(1, 9, 10)]
```

    ## # A tibble: 53,940 x 3
    ##    carat     y     z
    ##    <dbl> <dbl> <dbl>
    ##  1 0.23   3.98  2.43
    ##  2 0.21   3.84  2.31
    ##  3 0.23   4.07  2.31
    ##  4 0.290  4.23  2.63
    ##  5 0.31   4.35  2.75
    ##  6 0.24   3.96  2.48
    ##  7 0.24   3.98  2.47
    ##  8 0.26   4.11  2.53
    ##  9 0.22   3.78  2.49
    ## 10 0.23   4.05  2.39
    ## # ... with 53,930 more rows

``` r
# 예제 2
# 3~7번째 열을 한 번에 가져오시오
diamonds[ , 3:7]
```

    ## # A tibble: 53,940 x 5
    ##    color clarity depth table price
    ##    <ord> <ord>   <dbl> <dbl> <int>
    ##  1 E     SI2      61.5    55   326
    ##  2 E     SI1      59.8    61   326
    ##  3 E     VS1      56.9    65   327
    ##  4 I     VS2      62.4    58   334
    ##  5 J     SI2      63.3    58   335
    ##  6 J     VVS2     62.8    57   336
    ##  7 I     VVS1     62.3    57   336
    ##  8 H     SI1      61.9    55   337
    ##  9 E     VS2      65.1    61   337
    ## 10 H     VS1      59.4    61   338
    ## # ... with 53,930 more rows

``` r
# 예제 3
# 짝수 번째 열만 한 번에 가져오시오
diamonds[ , seq(from=2, to=ncol(diamonds), by=2)]
```

    ## # A tibble: 53,940 x 5
    ##    cut       clarity table     x     z
    ##    <fct>     <ord>   <dbl> <dbl> <dbl>
    ##  1 Ideal     SI2        55  3.95  2.43
    ##  2 Premium   SI1        61  3.89  2.31
    ##  3 Good      VS1        65  4.05  2.31
    ##  4 Premium   VS2        58  4.2   2.63
    ##  5 Good      SI2        58  4.34  2.75
    ##  6 Very Good VVS2       57  3.94  2.48
    ##  7 Very Good VVS1       57  3.95  2.47
    ##  8 Very Good SI1        55  4.07  2.53
    ##  9 Fair      VS2        61  3.87  2.49
    ## 10 Very Good VS1        61  4     2.39
    ## # ... with 53,930 more rows

``` r
# (2) 원하는 열(변수)의 이름을 알고 있을 때
diamonds[ , "carat"]
```

    ## # A tibble: 53,940 x 1
    ##    carat
    ##    <dbl>
    ##  1 0.23 
    ##  2 0.21 
    ##  3 0.23 
    ##  4 0.290
    ##  5 0.31 
    ##  6 0.24 
    ##  7 0.24 
    ##  8 0.26 
    ##  9 0.22 
    ## 10 0.23 
    ## # ... with 53,930 more rows

``` r
diamonds[ , "cut"]
```

    ## # A tibble: 53,940 x 1
    ##    cut      
    ##    <fct>    
    ##  1 Ideal    
    ##  2 Premium  
    ##  3 Good     
    ##  4 Premium  
    ##  5 Good     
    ##  6 Very Good
    ##  7 Very Good
    ##  8 Very Good
    ##  9 Fair     
    ## 10 Very Good
    ## # ... with 53,930 more rows

``` r
# 예제 4
# 변수명이 x,y,z인 세 개의 열을 한 번에 가져오시오
diamonds[ , c("x", "y", "z")]
```

    ## # A tibble: 53,940 x 3
    ##        x     y     z
    ##    <dbl> <dbl> <dbl>
    ##  1  3.95  3.98  2.43
    ##  2  3.89  3.84  2.31
    ##  3  4.05  4.07  2.31
    ##  4  4.2   4.23  2.63
    ##  5  4.34  4.35  2.75
    ##  6  3.94  3.96  2.48
    ##  7  3.95  3.98  2.47
    ##  8  4.07  4.11  2.53
    ##  9  3.87  3.78  2.49
    ## 10  4     4.05  2.39
    ## # ... with 53,930 more rows

``` r
# (3) 열 = 변수의 이름에 패턴이 있는 경우
# grep("pattern", 패턴을 찾을 character data, value = FALSE) : column index를 알려준다.
# grep("pattern", 패턴을 찾을 character data, value = TRUE) : column의 이름을 알려준다.

# 패턴 1 : 특정한 글자를 포함하고 있는 경우
# 예시 : 특정한 글자 : "c"
colnames(diamonds)
```

    ##  [1] "carat"   "cut"     "color"   "clarity" "depth"   "table"   "price"  
    ##  [8] "x"       "y"       "z"

``` r
grep(pattern = "c", colnames(diamonds), value = FALSE)
```

    ## [1] 1 2 3 4 7

``` r
grep(pattern = "c", colnames(diamonds), value = TRUE)
```

    ## [1] "carat"   "cut"     "color"   "clarity" "price"

``` r
diamonds[ , grep(pattern = "c", colnames(diamonds), value = FALSE)]
```

    ## # A tibble: 53,940 x 5
    ##    carat cut       color clarity price
    ##    <dbl> <fct>     <ord> <ord>   <int>
    ##  1 0.23  Ideal     E     SI2       326
    ##  2 0.21  Premium   E     SI1       326
    ##  3 0.23  Good      E     VS1       327
    ##  4 0.290 Premium   I     VS2       334
    ##  5 0.31  Good      J     SI2       335
    ##  6 0.24  Very Good J     VVS2      336
    ##  7 0.24  Very Good I     VVS1      336
    ##  8 0.26  Very Good H     SI1       337
    ##  9 0.22  Fair      E     VS2       337
    ## 10 0.23  Very Good H     VS1       338
    ## # ... with 53,930 more rows

``` r
diamonds[ , grep(pattern = "c", colnames(diamonds), value = TRUE)]
```

    ## # A tibble: 53,940 x 5
    ##    carat cut       color clarity price
    ##    <dbl> <fct>     <ord> <ord>   <int>
    ##  1 0.23  Ideal     E     SI2       326
    ##  2 0.21  Premium   E     SI1       326
    ##  3 0.23  Good      E     VS1       327
    ##  4 0.290 Premium   I     VS2       334
    ##  5 0.31  Good      J     SI2       335
    ##  6 0.24  Very Good J     VVS2      336
    ##  7 0.24  Very Good I     VVS1      336
    ##  8 0.26  Very Good H     SI1       337
    ##  9 0.22  Fair      E     VS2       337
    ## 10 0.23  Very Good H     VS1       338
    ## # ... with 53,930 more rows

``` r
# 패턴 2 : 특정한 글자로 시작하는 경우
# 예시 : 특정한 글자 : "c"
grep(pattern = "^c", colnames(diamonds), value = FALSE)
```

    ## [1] 1 2 3 4

``` r
grep(pattern = "^c", colnames(diamonds), value = TRUE)
```

    ## [1] "carat"   "cut"     "color"   "clarity"

``` r
diamonds[ , grep(pattern = "^c", colnames(diamonds), value = FALSE)]
```

    ## # A tibble: 53,940 x 4
    ##    carat cut       color clarity
    ##    <dbl> <fct>     <ord> <ord>  
    ##  1 0.23  Ideal     E     SI2    
    ##  2 0.21  Premium   E     SI1    
    ##  3 0.23  Good      E     VS1    
    ##  4 0.290 Premium   I     VS2    
    ##  5 0.31  Good      J     SI2    
    ##  6 0.24  Very Good J     VVS2   
    ##  7 0.24  Very Good I     VVS1   
    ##  8 0.26  Very Good H     SI1    
    ##  9 0.22  Fair      E     VS2    
    ## 10 0.23  Very Good H     VS1    
    ## # ... with 53,930 more rows

``` r
diamonds[ , grep(pattern = "^c", colnames(diamonds), value = TRUE)]
```

    ## # A tibble: 53,940 x 4
    ##    carat cut       color clarity
    ##    <dbl> <fct>     <ord> <ord>  
    ##  1 0.23  Ideal     E     SI2    
    ##  2 0.21  Premium   E     SI1    
    ##  3 0.23  Good      E     VS1    
    ##  4 0.290 Premium   I     VS2    
    ##  5 0.31  Good      J     SI2    
    ##  6 0.24  Very Good J     VVS2   
    ##  7 0.24  Very Good I     VVS1   
    ##  8 0.26  Very Good H     SI1    
    ##  9 0.22  Fair      E     VS2    
    ## 10 0.23  Very Good H     VS1    
    ## # ... with 53,930 more rows

``` r
# 패턴 3 : 특정한 글자로 끝나는 경우
# 예시 : 특정한 글자 : "e"
grep(pattern = "e$", colnames(diamonds), value = FALSE)
```

    ## [1] 6 7

``` r
grep(pattern = "e$", colnames(diamonds), value = TRUE)
```

    ## [1] "table" "price"

``` r
diamonds[ , grep(pattern = "e$", colnames(diamonds), value = FALSE)]
```

    ## # A tibble: 53,940 x 2
    ##    table price
    ##    <dbl> <int>
    ##  1    55   326
    ##  2    61   326
    ##  3    65   327
    ##  4    58   334
    ##  5    58   335
    ##  6    57   336
    ##  7    57   336
    ##  8    55   337
    ##  9    61   337
    ## 10    61   338
    ## # ... with 53,930 more rows

``` r
diamonds[ , grep(pattern = "e$", colnames(diamonds), value = TRUE)]
```

    ## # A tibble: 53,940 x 2
    ##    table price
    ##    <dbl> <int>
    ##  1    55   326
    ##  2    61   326
    ##  3    65   327
    ##  4    58   334
    ##  5    58   335
    ##  6    57   336
    ##  7    57   336
    ##  8    55   337
    ##  9    61   337
    ## 10    61   338
    ## # ... with 53,930 more rows

``` r
# 참고 : 글자 처리할 때 사용하는 부호들 ^, $ 등등을 정규 표현식이라고 한다. ( regular expression )

# "ca" 라고 하면, ca가 들어가는 글자 탐색, "^ca"은 ca로 시작하는 글자
# "[ca]" 라고 하면, "c" 또는 "a"를 포함하는 경우를 뜻한다.
# "^[ca]", "[ca]$" 등등 마치 수식처럼 원하는 택스트를 찾을 수 있다.
# "(ca)|(cd)" : ca 또는 cd를 포함하는 경우

# (4) dplyr::select() 를 이용하여 열을 추출하기
# dplyr::select(data.name, variable, ...)
dplyr::select(diamonds, carat)
```

    ## # A tibble: 53,940 x 1
    ##    carat
    ##    <dbl>
    ##  1 0.23 
    ##  2 0.21 
    ##  3 0.23 
    ##  4 0.290
    ##  5 0.31 
    ##  6 0.24 
    ##  7 0.24 
    ##  8 0.26 
    ##  9 0.22 
    ## 10 0.23 
    ## # ... with 53,930 more rows

``` r
dplyr::select(diamonds, carat, cut)
```

    ## # A tibble: 53,940 x 2
    ##    carat cut      
    ##    <dbl> <fct>    
    ##  1 0.23  Ideal    
    ##  2 0.21  Premium  
    ##  3 0.23  Good     
    ##  4 0.290 Premium  
    ##  5 0.31  Good     
    ##  6 0.24  Very Good
    ##  7 0.24  Very Good
    ##  8 0.26  Very Good
    ##  9 0.22  Fair     
    ## 10 0.23  Very Good
    ## # ... with 53,930 more rows

``` r
dplyr::select(diamonds, -carat) # carat을 제외한 변수들
```

    ## # A tibble: 53,940 x 9
    ##    cut       color clarity depth table price     x     y     z
    ##    <fct>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2 Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ##  3 Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ##  4 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ##  5 Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ##  6 Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
    ##  7 Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
    ##  8 Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
    ##  9 Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 10 Very Good H     VS1      59.4    61   338  4     4.05  2.39
    ## # ... with 53,930 more rows

``` r
dplyr::select(diamonds, -c(carat,cut))
```

    ## # A tibble: 53,940 x 8
    ##    color clarity depth table price     x     y     z
    ##    <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2 E     SI1      59.8    61   326  3.89  3.84  2.31
    ##  3 E     VS1      56.9    65   327  4.05  4.07  2.31
    ##  4 I     VS2      62.4    58   334  4.2   4.23  2.63
    ##  5 J     SI2      63.3    58   335  4.34  4.35  2.75
    ##  6 J     VVS2     62.8    57   336  3.94  3.96  2.48
    ##  7 I     VVS1     62.3    57   336  3.95  3.98  2.47
    ##  8 H     SI1      61.9    55   337  4.07  4.11  2.53
    ##  9 E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 10 H     VS1      59.4    61   338  4     4.05  2.39
    ## # ... with 53,930 more rows

``` r
dplyr::select(diamonds, depth:y) # depth부터 y까지
```

    ## # A tibble: 53,940 x 5
    ##    depth table price     x     y
    ##    <dbl> <dbl> <int> <dbl> <dbl>
    ##  1  61.5    55   326  3.95  3.98
    ##  2  59.8    61   326  3.89  3.84
    ##  3  56.9    65   327  4.05  4.07
    ##  4  62.4    58   334  4.2   4.23
    ##  5  63.3    58   335  4.34  4.35
    ##  6  62.8    57   336  3.94  3.96
    ##  7  62.3    57   336  3.95  3.98
    ##  8  61.9    55   337  4.07  4.11
    ##  9  65.1    61   337  3.87  3.78
    ## 10  59.4    61   338  4     4.05
    ## # ... with 53,930 more rows

``` r
dplyr::select(diamonds, contains("c")) # c를 포함하는 변수
```

    ## # A tibble: 53,940 x 5
    ##    carat cut       color clarity price
    ##    <dbl> <fct>     <ord> <ord>   <int>
    ##  1 0.23  Ideal     E     SI2       326
    ##  2 0.21  Premium   E     SI1       326
    ##  3 0.23  Good      E     VS1       327
    ##  4 0.290 Premium   I     VS2       334
    ##  5 0.31  Good      J     SI2       335
    ##  6 0.24  Very Good J     VVS2      336
    ##  7 0.24  Very Good I     VVS1      336
    ##  8 0.26  Very Good H     SI1       337
    ##  9 0.22  Fair      E     VS2       337
    ## 10 0.23  Very Good H     VS1       338
    ## # ... with 53,930 more rows

``` r
dplyr::select(diamonds, starts_with("c")) # c로 시작하는 변수 
```

    ## # A tibble: 53,940 x 4
    ##    carat cut       color clarity
    ##    <dbl> <fct>     <ord> <ord>  
    ##  1 0.23  Ideal     E     SI2    
    ##  2 0.21  Premium   E     SI1    
    ##  3 0.23  Good      E     VS1    
    ##  4 0.290 Premium   I     VS2    
    ##  5 0.31  Good      J     SI2    
    ##  6 0.24  Very Good J     VVS2   
    ##  7 0.24  Very Good I     VVS1   
    ##  8 0.26  Very Good H     SI1    
    ##  9 0.22  Fair      E     VS2    
    ## 10 0.23  Very Good H     VS1    
    ## # ... with 53,930 more rows

``` r
dplyr::select(diamonds, ends_with("e")) # e로 끝나는 변수
```

    ## # A tibble: 53,940 x 2
    ##    table price
    ##    <dbl> <int>
    ##  1    55   326
    ##  2    61   326
    ##  3    65   327
    ##  4    58   334
    ##  5    58   335
    ##  6    57   336
    ##  7    57   336
    ##  8    55   337
    ##  9    61   337
    ## 10    61   338
    ## # ... with 53,930 more rows

### 행

``` r
# (1) 위치를 아는 경우
# data[row, ]
diamonds[1, ]
```

    ## # A tibble: 1 x 10
    ##   carat cut   color clarity depth table price     x     y     z
    ##   <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43

``` r
diamonds[c(1,9,10), ]
```

    ## # A tibble: 3 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <fct>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  0.23 Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ## 2  0.22 Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 3  0.23 Very Good H     VS1      59.4    61   338  4     4.05  2.39

``` r
diamonds[3:10, ]
```

    ## # A tibble: 8 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <fct>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ## 2 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ## 3 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ## 4 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
    ## 5 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
    ## 6 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
    ## 7 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 8 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39

``` r
diamonds[seq(from = 1, to = nrow(diamonds), by=100), ]
```

    ## # A tibble: 540 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <fct>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.75 Very Good D     SI1      63.2    56  2760  5.8   5.75  3.65
    ##  3  0.7  Very Good E     SI1      59.9    63  2777  5.76  5.7   3.43
    ##  4  0.77 Ideal     I     VS1      61.5    59  2798  5.87  5.91  3.62
    ##  5  0.3  Good      H     SI1      63.8    55   554  4.26  4.2   2.7 
    ##  6  0.71 Ideal     D     SI1      60.2    56  2822  5.86  5.83  3.52
    ##  7  0.7  Premium   F     VS2      59.4    61  2838  5.83  5.79  3.45
    ##  8  0.7  Very Good E     VS1      62.4    56  2854  5.64  5.7   3.54
    ##  9  1.22 Premium   E     I1       60.9    57  2862  6.93  6.88  4.21
    ## 10  0.73 Premium   E     VS1      62.6    60  2876  5.68  5.75  3.58
    ## # ... with 530 more rows

``` r
# (2) 비교 연산자와 논리 연산자를 사용하는 경우
# 조건을 만족하는 데이터를 추출할 때 사용
# data[조건1, ]
# data[조건1 & 조건2 ... , ]
# data[조건1 | 조건2 ... , ]


# cut이 fair인 것을 추출하기
diamonds[diamonds$cut == "Fair", ]
```

    ## # A tibble: 1,610 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49
    ##  2  0.86 Fair  E     SI2      55.1    69  2757  6.45  6.33  3.52
    ##  3  0.96 Fair  F     SI2      66.3    62  2759  6.27  5.95  4.07
    ##  4  0.7  Fair  F     VS2      64.5    57  2762  5.57  5.53  3.58
    ##  5  0.7  Fair  F     VS2      65.3    55  2762  5.63  5.58  3.66
    ##  6  0.91 Fair  H     SI2      64.4    57  2763  6.11  6.09  3.93
    ##  7  0.91 Fair  H     SI2      65.7    60  2763  6.03  5.99  3.95
    ##  8  0.98 Fair  H     SI2      67.9    60  2777  6.05  5.97  4.08
    ##  9  0.84 Fair  G     SI1      55.1    67  2782  6.39  6.2   3.47
    ## 10  1.01 Fair  E     I1       64.5    58  2788  6.29  6.21  4.03
    ## # ... with 1,600 more rows

``` r
# price가 18000 이상인 것을 추출하기
diamonds[diamonds$price >= 18000, ]
```

    ## # A tibble: 312 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <fct>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  2.16 Ideal     G     SI2      62.5  54.2 18001  8.23  8.27  5.16
    ##  2  2.09 Premium   F     SI2      61.7  59   18002  8.23  8.21  5.07
    ##  3  2.18 Premium   G     SI2      61.9  60   18003  8.29  8.24  5.12
    ##  4  2.06 Very Good G     SI2      62.3  59   18005  8.07  8.2   5.07
    ##  5  2.25 Premium   D     SI2      60.4  59   18007  8.54  8.48  5.13
    ##  6  1.76 Very Good G     VS1      62.8  55.4 18014  7.7   7.74  4.85
    ##  7  2.05 Ideal     G     SI2      61.6  56   18017  8.11  8.16  5.01
    ##  8  5.01 Fair      J     I1       65.5  59   18018 10.7  10.5   6.98
    ##  9  2.51 Premium   J     VS2      62.2  58   18020  8.73  8.67  5.41
    ## 10  2    Good      H     VS2      63.8  59   18023  7.88  8.01  5.07
    ## # ... with 302 more rows

``` r
# cut은 "Fair" price는 18000 이상인 것만 추출하기
# 반드시 사이에 괄호 넣어주기
diamonds[(diamonds$cut == "Fair") & (diamonds$price >= 18000), ]
```

    ## # A tibble: 9 x 10
    ##   carat cut   color clarity depth table price     x     y     z
    ##   <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  5.01 Fair  J     I1       65.5    59 18018 10.7  10.5   6.98
    ## 2  2.32 Fair  H     SI1      62      62 18026  8.47  8.31  5.2 
    ## 3  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9 
    ## 4  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9 
    ## 5  2.51 Fair  H     SI2      64.7    57 18308  8.44  8.5   5.48
    ## 6  2    Fair  G     VS2      67.6    58 18515  7.65  7.61  5.16
    ## 7  4.5  Fair  J     I1       65.8    58 18531 10.2  10.2   6.72
    ## 8  2.02 Fair  H     VS2      64.5    57 18565  8     7.95  5.14
    ## 9  2.01 Fair  G     SI1      70.6    64 18574  7.43  6.64  4.69

``` r
# cut은 "Fair" 이거나 price는 18000 이상인 것만 추출하기
diamonds[(diamonds$cut == "Fair") | (diamonds$price >= 18000), ]
```

    ## # A tibble: 1,913 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49
    ##  2  0.86 Fair  E     SI2      55.1    69  2757  6.45  6.33  3.52
    ##  3  0.96 Fair  F     SI2      66.3    62  2759  6.27  5.95  4.07
    ##  4  0.7  Fair  F     VS2      64.5    57  2762  5.57  5.53  3.58
    ##  5  0.7  Fair  F     VS2      65.3    55  2762  5.63  5.58  3.66
    ##  6  0.91 Fair  H     SI2      64.4    57  2763  6.11  6.09  3.93
    ##  7  0.91 Fair  H     SI2      65.7    60  2763  6.03  5.99  3.95
    ##  8  0.98 Fair  H     SI2      67.9    60  2777  6.05  5.97  4.08
    ##  9  0.84 Fair  G     SI1      55.1    67  2782  6.39  6.2   3.47
    ## 10  1.01 Fair  E     I1       64.5    58  2788  6.29  6.21  4.03
    ## # ... with 1,903 more rows

``` r
# cut은 "Fair" 이거나 또는 "Ideal" 인 것을 추출하기
diamonds[(diamonds$cut == "Fair") | (diamonds$cut == "Ideal"), ]
```

    ## # A tibble: 23,161 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49
    ##  3  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
    ##  4  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
    ##  5  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
    ##  6  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
    ##  7  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
    ##  8  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
    ##  9  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
    ## 10  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
    ## # ... with 23,151 more rows

``` r
# cut은 "Fair" 이거나 또는 "Ideal" 인 것을 추출하기 2 
diamonds[diamonds$cut %in% c("Fair","Ideal"), ]
```

    ## # A tibble: 23,161 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49
    ##  3  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
    ##  4  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
    ##  5  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
    ##  6  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
    ##  7  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
    ##  8  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
    ##  9  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
    ## 10  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
    ## # ... with 23,151 more rows

``` r
# 원소 %in% 집합 : 원소가 집합에 포함되는 것을 반환한다.

# (3) dplyr::filter(data, 조건)
dplyr::filter(diamonds, cut=="Fair")
```

    ## # A tibble: 1,610 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49
    ##  2  0.86 Fair  E     SI2      55.1    69  2757  6.45  6.33  3.52
    ##  3  0.96 Fair  F     SI2      66.3    62  2759  6.27  5.95  4.07
    ##  4  0.7  Fair  F     VS2      64.5    57  2762  5.57  5.53  3.58
    ##  5  0.7  Fair  F     VS2      65.3    55  2762  5.63  5.58  3.66
    ##  6  0.91 Fair  H     SI2      64.4    57  2763  6.11  6.09  3.93
    ##  7  0.91 Fair  H     SI2      65.7    60  2763  6.03  5.99  3.95
    ##  8  0.98 Fair  H     SI2      67.9    60  2777  6.05  5.97  4.08
    ##  9  0.84 Fair  G     SI1      55.1    67  2782  6.39  6.2   3.47
    ## 10  1.01 Fair  E     I1       64.5    58  2788  6.29  6.21  4.03
    ## # ... with 1,600 more rows

``` r
dplyr::filter(diamonds, price>=18000)
```

    ## # A tibble: 312 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <fct>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  2.16 Ideal     G     SI2      62.5  54.2 18001  8.23  8.27  5.16
    ##  2  2.09 Premium   F     SI2      61.7  59   18002  8.23  8.21  5.07
    ##  3  2.18 Premium   G     SI2      61.9  60   18003  8.29  8.24  5.12
    ##  4  2.06 Very Good G     SI2      62.3  59   18005  8.07  8.2   5.07
    ##  5  2.25 Premium   D     SI2      60.4  59   18007  8.54  8.48  5.13
    ##  6  1.76 Very Good G     VS1      62.8  55.4 18014  7.7   7.74  4.85
    ##  7  2.05 Ideal     G     SI2      61.6  56   18017  8.11  8.16  5.01
    ##  8  5.01 Fair      J     I1       65.5  59   18018 10.7  10.5   6.98
    ##  9  2.51 Premium   J     VS2      62.2  58   18020  8.73  8.67  5.41
    ## 10  2    Good      H     VS2      63.8  59   18023  7.88  8.01  5.07
    ## # ... with 302 more rows

``` r
dplyr::filter(diamonds, cut=="Fair", price>=18000)
```

    ## # A tibble: 9 x 10
    ##   carat cut   color clarity depth table price     x     y     z
    ##   <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  5.01 Fair  J     I1       65.5    59 18018 10.7  10.5   6.98
    ## 2  2.32 Fair  H     SI1      62      62 18026  8.47  8.31  5.2 
    ## 3  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9 
    ## 4  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9 
    ## 5  2.51 Fair  H     SI2      64.7    57 18308  8.44  8.5   5.48
    ## 6  2    Fair  G     VS2      67.6    58 18515  7.65  7.61  5.16
    ## 7  4.5  Fair  J     I1       65.8    58 18531 10.2  10.2   6.72
    ## 8  2.02 Fair  H     VS2      64.5    57 18565  8     7.95  5.14
    ## 9  2.01 Fair  G     SI1      70.6    64 18574  7.43  6.64  4.69

``` r
dplyr::filter(diamonds, cut=="Fair" & price>=18000) # 위와 같은 결과
```

    ## # A tibble: 9 x 10
    ##   carat cut   color clarity depth table price     x     y     z
    ##   <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  5.01 Fair  J     I1       65.5    59 18018 10.7  10.5   6.98
    ## 2  2.32 Fair  H     SI1      62      62 18026  8.47  8.31  5.2 
    ## 3  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9 
    ## 4  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9 
    ## 5  2.51 Fair  H     SI2      64.7    57 18308  8.44  8.5   5.48
    ## 6  2    Fair  G     VS2      67.6    58 18515  7.65  7.61  5.16
    ## 7  4.5  Fair  J     I1       65.8    58 18531 10.2  10.2   6.72
    ## 8  2.02 Fair  H     VS2      64.5    57 18565  8     7.95  5.14
    ## 9  2.01 Fair  G     SI1      70.6    64 18574  7.43  6.64  4.69

``` r
dplyr::filter(diamonds, cut=="Fair" | cut=="Ideal")
```

    ## # A tibble: 23,161 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49
    ##  3  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
    ##  4  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
    ##  5  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
    ##  6  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
    ##  7  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
    ##  8  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
    ##  9  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
    ## 10  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
    ## # ... with 23,151 more rows

``` r
dplyr::filter(diamonds, cut %in% c("Fair","Ideal")) # 위와 같은 결과
```

    ## # A tibble: 23,161 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49
    ##  3  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
    ##  4  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
    ##  5  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
    ##  6  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
    ##  7  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
    ##  8  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
    ##  9  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
    ## 10  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
    ## # ... with 23,151 more rows

알고 있으면 유용한 함수들
-------------------------

새롭게 범주형 변수를 만들 때 유용한 함수<br> ifelse(조건,<br> 조건이 맞을 때에 새로운 변수가 가지는 값,<br> 조건이 맞지 않을 때에 새로운 변수가 가지는 값)

연속형 자료를 구간별로 나누어서 범주형으로 만들 때 유용한 함수<br> new.variable &lt;- cut(data.variable,<br> breaks = 구간의 정보,<br> right = FALSE or TRUE)<br> breaks : 각 구간별로 중복되는 숫자는 하나로 한다.<br> right : FALSE의 경우 하한 &lt;= 데이터 &lt; 상한 ( 이상 미만 구간 )<br> TRUE의 경우 하한 &lt; 데이터 &lt;= 상한 ( 초과 이하 구간 )

``` r
# ifelse 예시
diamonds$cut.group <- ifelse(diamonds$cut == "Ideal",
                             "Ideal",
                             "Non-Ideal")

# cut함수 예시
diamonds$price.group <- cut(diamonds$price,
                            breaks = seq(from = 0, to = 20000, by = 5000),
                            right = FALSE)
```

메모리의 내용을 외부 데이터로 저장하기
--------------------------------------

### text

write.table(data, file = "directory/filename.txt", sep = "",<br> row.names = FALSE)<br> row.names는 행의 이름이다.<br> sep의 경우 3가지를 배웠었다. " " "," "/t"

### csv

write.csv(data, file = "directory/filename.csv", row.names = FALSE)

### excel

writexl::write\_xlsx(data, path = "directory/filename.xlsx")

### RData

R자체를 하드에 저장하는 방법<br> 실전에서는 이렇게 저장하는 것이 가장 좋다.<br> save(data, file = "directory/filename.RData")<br> load(file = "directory/filename.RData")<br> 불러오면 바로 RAM(메모리)에 올린다.

### 메모리에 적재된 모든 데이터 삭제하기

rm(list = ls())

데이터의 정렬
-------------

### vector의 정렬

sort(vector, decreasing = )<br> decreasing : FALSE일 경우 오름차순, TRUE일 경우 내림차순<br> **정렬된 데이터를 반환한다.**

``` r
age <- c(24, 25, 27, 27, 16, 35)
sort(age, decreasing = FALSE) # default는 오름차순(FALSE)
```

    ## [1] 16 24 25 27 27 35

``` r
sort(age, decreasing = TRUE) # 내림차순(TRUE)
```

    ## [1] 35 27 27 25 24 16

### 데이터프레임의 정렬

dataframe은 sort로 정렬할 수 없다.<br> (1) order(data$variable, decreasing = FALSE or TRUE)<br> **인덱스를 반환**하기 때문에, 대괄호에서 사용한다.

1.  dplyr::arrange(data, variable, desc(variable))<br> desc의 괄호 안에는 내림차순을 할 편수가 들어간다.

``` r
# (1) order(data$variable, decreasing = FALSE or TRUE)

# cut을 기준으로 오름차순을 할 경우
diamonds[order(diamonds$cut, decreasing = FALSE), ]
```

    ## # A tibble: 53,940 x 12
    ##    carat cut   color clarity depth table price     x     y     z cut.group
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>    
    ##  1  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49 Non-Ideal
    ##  2  0.86 Fair  E     SI2      55.1    69  2757  6.45  6.33  3.52 Non-Ideal
    ##  3  0.96 Fair  F     SI2      66.3    62  2759  6.27  5.95  4.07 Non-Ideal
    ##  4  0.7  Fair  F     VS2      64.5    57  2762  5.57  5.53  3.58 Non-Ideal
    ##  5  0.7  Fair  F     VS2      65.3    55  2762  5.63  5.58  3.66 Non-Ideal
    ##  6  0.91 Fair  H     SI2      64.4    57  2763  6.11  6.09  3.93 Non-Ideal
    ##  7  0.91 Fair  H     SI2      65.7    60  2763  6.03  5.99  3.95 Non-Ideal
    ##  8  0.98 Fair  H     SI2      67.9    60  2777  6.05  5.97  4.08 Non-Ideal
    ##  9  0.84 Fair  G     SI1      55.1    67  2782  6.39  6.2   3.47 Non-Ideal
    ## 10  1.01 Fair  E     I1       64.5    58  2788  6.29  6.21  4.03 Non-Ideal
    ## # ... with 53,930 more rows, and 1 more variable: price.group <fct>

``` r
# cut을 기준으로 내림차순을 할 경우
diamonds[order(diamonds$cut, decreasing = TRUE), ]
```

    ## # A tibble: 53,940 x 12
    ##    carat cut   color clarity depth table price     x     y     z cut.group
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>    
    ##  1  0.24 Very~ J     VVS2     62.8    57   336  3.94  3.96  2.48 Non-Ideal
    ##  2  0.24 Very~ I     VVS1     62.3    57   336  3.95  3.98  2.47 Non-Ideal
    ##  3  0.26 Very~ H     SI1      61.9    55   337  4.07  4.11  2.53 Non-Ideal
    ##  4  0.23 Very~ H     VS1      59.4    61   338  4     4.05  2.39 Non-Ideal
    ##  5  0.3  Very~ J     SI1      62.7    59   351  4.21  4.27  2.66 Non-Ideal
    ##  6  0.23 Very~ E     VS2      63.8    55   352  3.85  3.92  2.48 Non-Ideal
    ##  7  0.23 Very~ H     VS1      61      57   353  3.94  3.96  2.41 Non-Ideal
    ##  8  0.31 Very~ J     SI1      59.4    62   353  4.39  4.43  2.62 Non-Ideal
    ##  9  0.31 Very~ J     SI1      58.1    62   353  4.44  4.47  2.59 Non-Ideal
    ## 10  0.23 Very~ G     VVS2     60.4    58   354  3.97  4.01  2.41 Non-Ideal
    ## # ... with 53,930 more rows, and 1 more variable: price.group <fct>

``` r
# cut을 오름차순, price: 내림차순일 경우는?
diamonds[order(diamonds$cut, -diamonds$price, decreasing = FALSE), ]
```

    ## # A tibble: 53,940 x 12
    ##    carat cut   color clarity depth table price     x     y     z cut.group
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>    
    ##  1  2.01 Fair  G     SI1      70.6    64 18574  7.43  6.64  4.69 Non-Ideal
    ##  2  2.02 Fair  H     VS2      64.5    57 18565  8     7.95  5.14 Non-Ideal
    ##  3  4.5  Fair  J     I1       65.8    58 18531 10.2  10.2   6.72 Non-Ideal
    ##  4  2    Fair  G     VS2      67.6    58 18515  7.65  7.61  5.16 Non-Ideal
    ##  5  2.51 Fair  H     SI2      64.7    57 18308  8.44  8.5   5.48 Non-Ideal
    ##  6  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9  Non-Ideal
    ##  7  3.01 Fair  I     SI2      65.8    56 18242  8.99  8.94  5.9  Non-Ideal
    ##  8  2.32 Fair  H     SI1      62      62 18026  8.47  8.31  5.2  Non-Ideal
    ##  9  5.01 Fair  J     I1       65.5    59 18018 10.7  10.5   6.98 Non-Ideal
    ## 10  1.93 Fair  F     VS1      58.9    62 17995  8.17  7.97  4.75 Non-Ideal
    ## # ... with 53,930 more rows, and 1 more variable: price.group <fct>

``` r
# 음수를 붙이면 된다.

# cut : 오름차순 color: 내림차순일 경우는? 둘 다 factor이기 때문에, order로 구현하는 것은 불가능하다.

# (2) dplyr::arrange(data, variable, desc(variable))
diamonds %>% dplyr::arrange(cut) # cut 오름차순
```

    ## # A tibble: 53,940 x 12
    ##    carat cut   color clarity depth table price     x     y     z cut.group
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>    
    ##  1  0.22 Fair  E     VS2      65.1    61   337  3.87  3.78  2.49 Non-Ideal
    ##  2  0.86 Fair  E     SI2      55.1    69  2757  6.45  6.33  3.52 Non-Ideal
    ##  3  0.96 Fair  F     SI2      66.3    62  2759  6.27  5.95  4.07 Non-Ideal
    ##  4  0.7  Fair  F     VS2      64.5    57  2762  5.57  5.53  3.58 Non-Ideal
    ##  5  0.7  Fair  F     VS2      65.3    55  2762  5.63  5.58  3.66 Non-Ideal
    ##  6  0.91 Fair  H     SI2      64.4    57  2763  6.11  6.09  3.93 Non-Ideal
    ##  7  0.91 Fair  H     SI2      65.7    60  2763  6.03  5.99  3.95 Non-Ideal
    ##  8  0.98 Fair  H     SI2      67.9    60  2777  6.05  5.97  4.08 Non-Ideal
    ##  9  0.84 Fair  G     SI1      55.1    67  2782  6.39  6.2   3.47 Non-Ideal
    ## 10  1.01 Fair  E     I1       64.5    58  2788  6.29  6.21  4.03 Non-Ideal
    ## # ... with 53,930 more rows, and 1 more variable: price.group <fct>

``` r
diamonds %>% dplyr::arrange(desc(cut)) # cut 내림차순
```

    ## # A tibble: 53,940 x 12
    ##    carat cut   color clarity depth table price     x     y     z cut.group
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>    
    ##  1  0.24 Very~ J     VVS2     62.8    57   336  3.94  3.96  2.48 Non-Ideal
    ##  2  0.24 Very~ I     VVS1     62.3    57   336  3.95  3.98  2.47 Non-Ideal
    ##  3  0.26 Very~ H     SI1      61.9    55   337  4.07  4.11  2.53 Non-Ideal
    ##  4  0.23 Very~ H     VS1      59.4    61   338  4     4.05  2.39 Non-Ideal
    ##  5  0.3  Very~ J     SI1      62.7    59   351  4.21  4.27  2.66 Non-Ideal
    ##  6  0.23 Very~ E     VS2      63.8    55   352  3.85  3.92  2.48 Non-Ideal
    ##  7  0.23 Very~ H     VS1      61      57   353  3.94  3.96  2.41 Non-Ideal
    ##  8  0.31 Very~ J     SI1      59.4    62   353  4.39  4.43  2.62 Non-Ideal
    ##  9  0.31 Very~ J     SI1      58.1    62   353  4.44  4.47  2.59 Non-Ideal
    ## 10  0.23 Very~ G     VVS2     60.4    58   354  3.97  4.01  2.41 Non-Ideal
    ## # ... with 53,930 more rows, and 1 more variable: price.group <fct>

``` r
# cut : 오름차순 color: 내림차순일 경우는?
diamonds %>% dplyr::arrange(cut, desc(color)) # cut 오름 color 내림
```

    ## # A tibble: 53,940 x 12
    ##    carat cut   color clarity depth table price     x     y     z cut.group
    ##    <dbl> <fct> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>    
    ##  1  1.05 Fair  J     SI2      65.8    59  2789  6.41  6.27  4.18 Non-Ideal
    ##  2  1    Fair  J     VS2      65.7    59  2811  6.14  6.07  4.01 Non-Ideal
    ##  3  0.99 Fair  J     SI1      55      61  2812  6.72  6.67  3.68 Non-Ideal
    ##  4  0.9  Fair  J     VS2      65      56  2815  6.08  6.04  3.94 Non-Ideal
    ##  5  0.91 Fair  J     VS2      64.4    62  2854  6.06  6.03  3.89 Non-Ideal
    ##  6  0.91 Fair  J     VS2      65.4    60  2854  6.04  6     3.94 Non-Ideal
    ##  7  1    Fair  J     VS1      65.5    55  2875  6.3   6.25  4.11 Non-Ideal
    ##  8  0.99 Fair  J     SI1      58      67  2949  6.57  6.5   3.79 Non-Ideal
    ##  9  0.9  Fair  J     VS1      65.4    60  2964  6.02  5.93  3.91 Non-Ideal
    ## 10  0.9  Fair  J     VS1      64.6    58  2964  6.12  6.06  3.93 Non-Ideal
    ## # ... with 53,930 more rows, and 1 more variable: price.group <fct>

데이터 합치기
-------------

### rbind()

데이터가 위/아래로 합쳐짐<br> 두 데이터가 데이터의 format이 같아야 함 ( matrix의 속성 )<br> 변수명도 같고, 변수명의 위치도 같아야 함

``` r
d1 <- data.frame(id = 1:3,
                 height = c(177, 167, 170),
                 weight = c(69, 70, 65))

d2 <- data.frame(id = 4:6,
                 height = c(178, 148, 160),
                 weight = c(67, 97, 50))

d3 <- rbind(d1, d2)
d3
```

    ##   id height weight
    ## 1  1    177     69
    ## 2  2    167     70
    ## 3  3    170     65
    ## 4  4    178     67
    ## 5  5    148     97
    ## 6  6    160     50

### cbind()

데이터가 왼쪽 / 오른쪽으로 합쳐짐<br> 왼쪽의 데이터 행, 오른쪽의 데이터 행이 동일한 개체의 데이터어야 한다.

``` r
d4 <- data.frame(id = 1:3,
                 names = c("강민기", "양용준", "이경민"),
                 ages = c(24,25,27))

d5 <- data.frame(income = c(40, 60, 50),
                 sight = c(0.7, 1.0, 0.2))

d6 <- cbind(d4,d5)
d6
```

    ##   id  names ages income sight
    ## 1  1 강민기   24     40   0.7
    ## 2  2 양용준   25     60   1.0
    ## 3  3 이경민   27     50   0.2

### merge()

데이터가 왼쪽/오른쪽으로 합쳐짐

``` r
d7 <- data.frame(id = c(1,2,4,5),
                 age = c(10,20,40,50),
                 bt = c("A", "A", "B", "O"))

d8 <- data.frame(id = c(1, 4, 7:8),
                 company = c("아마존","삼성","넷플릭스","SKT"),
                 income = c(10000, 6000, 5000, 6500))

# (1) inner join
# 두 데이터 간의 primary key가 동일한 것만 합쳐진다.
# merge(data1, data2, by = "primary key")
d9 <- merge(d7, d8, by = "id")
d9
```

    ##   id age bt company income
    ## 1  1  10  A  아마존  10000
    ## 2  4  40  B    삼성   6000

``` r
# (2) full join
# 두 데이터 간의 primary key의 합집합
# merge(data1, data2, by = "primary key", all = TRUE)
d10 <- merge(d7, d8, by = "id", all = TRUE)
d10
```

    ##   id age   bt  company income
    ## 1  1  10    A   아마존  10000
    ## 2  2  20    A     <NA>     NA
    ## 3  4  40    B     삼성   6000
    ## 4  5  50    O     <NA>     NA
    ## 5  7  NA <NA> 넷플릭스   5000
    ## 6  8  NA <NA>      SKT   6500

``` r
# (3) left join
# 먼저 들어가는 데이터의 primary key를 중심으로 합쳐진다.
# merge(data1, data2, by = "primary key", all.x = TRUE)
d11 <- merge(d7, d8, by = "id", all.x = TRUE)
d11
```

    ##   id age bt company income
    ## 1  1  10  A  아마존  10000
    ## 2  2  20  A    <NA>     NA
    ## 3  4  40  B    삼성   6000
    ## 4  5  50  O    <NA>     NA

``` r
# (4) right join
# 다음에 들어가는 데이터의 primary key를 중심으로 합쳐진다.
# merge(data1, data2, by = "primary key", all.y = TRUE)
d12 <- merge(d7, d8, by = "id", all.y = TRUE)
d12
```

    ##   id age   bt  company income
    ## 1  1  10    A   아마존  10000
    ## 2  4  40    B     삼성   6000
    ## 3  7  NA <NA> 넷플릭스   5000
    ## 4  8  NA <NA>      SKT   6500

### dplyr패키지

실전에서 가장 많이 사용되는 방법이다.

``` r
dplyr::inner_join(d7, d8, by = "id")
```

    ##   id age bt company income
    ## 1  1  10  A  아마존  10000
    ## 2  4  40  B    삼성   6000

``` r
dplyr::full_join(d7, d8, by = "id")
```

    ##   id age   bt  company income
    ## 1  1  10    A   아마존  10000
    ## 2  2  20    A     <NA>     NA
    ## 3  4  40    B     삼성   6000
    ## 4  5  50    O     <NA>     NA
    ## 5  7  NA <NA> 넷플릭스   5000
    ## 6  8  NA <NA>      SKT   6500

``` r
dplyr::left_join(d7, d8, by = "id")
```

    ##   id age bt company income
    ## 1  1  10  A  아마존  10000
    ## 2  2  20  A    <NA>     NA
    ## 3  4  40  B    삼성   6000
    ## 4  5  50  O    <NA>     NA

``` r
dplyr::right_join(d7, d8, by = "id")
```

    ##   id age   bt  company income
    ## 1  1  10    A   아마존  10000
    ## 2  4  40    B     삼성   6000
    ## 3  7  NA <NA> 넷플릭스   5000
    ## 4  8  NA <NA>      SKT   6500

``` r
# primary key를 2가지 이상으로 설정할 수 있다.
# by = c(x, y, ...)
```
