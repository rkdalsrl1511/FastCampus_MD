fastcampus\_R프로그래밍\_1
================
huimin
2019년 3월 15일

R Studio 편리한 단축키 모음
---------------------------

|              방법              |                내용               |
|:------------------------------:|:---------------------------------:|
|              ----              |             책갈피달기            |
|        ctrl + shift + c        |           주석생성/삭제           |
|          ctrl + enter          |            명령어 실행            |
|          ctrl + 1 ~ 9          |             커서 이동             |
|     console 창에서 ctrl + L    |          콘솔화면 지우기          |
| console 창에서 방향키(위/아래) | history(이제까지 실행한 명령어들) |
|  alt + shift + 방향키(위/아래) |    커서가 위치한 라인 복사하기    |
|             alt + -            |           &lt;- 생성하기          |
|        ctrl + shift + n        |       새로운 스크립트 만들기      |

### 참고: R은 대소문자를 구분한다! ( case sensitive )<br><br><br>

1. Data Type
============

``` r
# 1.1 수치형 데이터(Numeric)
# 1.1.1 정수형(integer)
x1 <- 10
x2 <- 10L
# 1.1.2 실수형(double)
x3 <- 10.2
# 1.2. 문자형 데이터(character)
# 작은 따옴표, 큰 따옴표 둘 다 같다.
# 단, 여기서 따옴표 안에 따옴표를 넣고 싶다면? 작은 따옴표 안에 큰 따옴표를, 큰 따옴표 안에는 작은 따옴표를 넣으면 된다.
x4 <- 'Love is not feeling'
x5 <- "Love is choice"
# 1.3. 논리형 데이터(Logical)
x6 <- TRUE
x7 <- FALSE
```

2. Data의 유형 확인하기
=======================

``` r
# 2.1 mode(data) : c언어를 기반으로 한 함수, character로 반환
mode(x1)
```

    ## [1] "numeric"

``` r
mode(x2)
```

    ## [1] "numeric"

``` r
mode(x3)
```

    ## [1] "numeric"

``` r
mode(x4)
```

    ## [1] "character"

``` r
mode(x6)
```

    ## [1] "logical"

``` r
# 2.2 typeof(data) : s언어를 기반으로 한 함수
typeof(x1)
```

    ## [1] "double"

``` r
typeof(x2) # L이 붙은 것을 integer로 인식한다.
```

    ## [1] "integer"

``` r
typeof(x3)
```

    ## [1] "double"

``` r
typeof(x4)
```

    ## [1] "character"

``` r
typeof(x5)
```

    ## [1] "character"

``` r
typeof(x6)
```

    ## [1] "logical"

``` r
typeof(x7)
```

    ## [1] "logical"

``` r
# 2.3 is.xxxx(data) : logical로 반환
is.numeric(x1)
```

    ## [1] TRUE

``` r
is.character(x4)
```

    ## [1] TRUE

``` r
is.logical(x7)
```

    ## [1] TRUE

3. Data의 유형 변환하기
=======================

수치형 데이터 칼럼에 하나라도 문자가 있다면, 전체를 문자로 인식하는 경우가 발생하기도 한다. 따라서 이럴 경우 변환한다.<br> 우선순위 character &gt; numeric &gt; logical 이다. 전부 character로 바꿀 수 있다. character에서 numeric으로 변환 가능한 경우는 숫자인 문자만 가능하다.

``` r
# 3.1 as.xxxx(data)
x1 <- 10
x2 <- "10"
x3 <- "LEE"
x4 <- FALSE

as.numeric(x2)
```

    ## [1] 10

``` r
x2 <- as.numeric(x2)
as.numeric(x3) # 불가능
```

    ## Warning: 강제형변환에 의해 생성된 NA 입니다

    ## [1] NA

``` r
as.numeric(x4) # 0 = false 1 = true
```

    ## [1] 0

``` r
as.character(x1)
```

    ## [1] "10"

``` r
as.character(x4)
```

    ## [1] "FALSE"

``` r
as.logical(x1) 
```

    ## [1] TRUE

``` r
# numeric -> logical : 가능 0은 false, 0이 아닌 모든 숫자는 true.
as.logical(x2)
```

    ## [1] TRUE

``` r
as.logical(x3) # character -> logical : 불가능
```

    ## [1] NA

4.1 산술 연산자(Arithmetic Operator)
====================================

+, -, \*, /, \*\*, ^, %/%, %%

``` r
3 + 4
```

    ## [1] 7

``` r
3 + 4; 4 - 3 # 한 라인에 명령어 2개 이상 넣을 때는 ;을 사용한다. (비추)
```

    ## [1] 7

    ## [1] 1

``` r
3 * 4
```

    ## [1] 12

``` r
3 / 4
```

    ## [1] 0.75

``` r
3 ** 4 # 거듭제곱
```

    ## [1] 81

``` r
3 ^ 4 # 거듭제곱
```

    ## [1] 81

``` r
4 %/% 3 # 몫만 구하기
```

    ## [1] 1

``` r
13 %% 4 # 나머지만 구하기
```

    ## [1] 1

``` r
# 문제 1 : 루트3 구하기
3 ^ 1/2
```

    ## [1] 1.5

``` r
3 ^ (1/2) 
```

    ## [1] 1.732051

``` r
# R에서의 연산도 우선순위에 따라서 계산되기 때문에 괄호를 넣는다.
# 우선순위는 일반적인 산술과 같다. **,^ > *,/ > +,- 
```

4.2 할당 연산자(allocation Operator)
====================================

&lt;- , = : 저장시키는 기능<br> &lt;- : 일반적인 저장기능<br> = : 함수 안의 argument를 저장하는 기능 ex. rnorm( n = 100 )에서 n을 argument라고 한다.

``` r
x <- rnorm(n = 100)
```

4.3 비교 연산자(Comparison Operator)
====================================

( &gt;, &gt;=, &lt;, &lt;=, ==, !=, ! )<br> 주로 전체 데이터 중에서 일부의 데이터를 추출할 때 사용한다.

``` r
3 > 4 # greater than
```

    ## [1] FALSE

``` r
3 >= 4 # greater than equal to
```

    ## [1] FALSE

``` r
3 < 4 # less than
```

    ## [1] TRUE

``` r
3 <= 4 # less than equal to
```

    ## [1] TRUE

``` r
3 == 4 # equal to
```

    ## [1] FALSE

``` r
3 != 4 # not equal to
```

    ## [1] TRUE

``` r
!( 3==4 ) # false를 부정하므로 true가 된다.
```

    ## [1] TRUE

4.4 논리 연산자(Logical Operator)
=================================

조건을 두 개 이상 줄 때<br> 주로 전체 데이터 중에서 일부를 추출할 때 사용<br> &, |(vertical bar)<br><br> & : and 기능. 여러 개의 조건이 동시에 만족될 때 TRUE<br> | : or 기능. 여러 개의 조건 중에서 하나만 만족되도 TRUE

``` r
(3>4) & (3<4)
```

    ## [1] FALSE

``` r
(3>4) | (3<4)
```

    ## [1] TRUE
