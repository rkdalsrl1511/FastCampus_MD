fastcampus\_R프로그래밍\_2
================
huimin
2019년 3월 16일

R에서의 데이터 형식
===================

Vector : 벡터<br> Factor : 펙터<br> Matrix : 행렬(메트릭스)<br> Array : Matrix와 vector의 확장 형태인 다차원 구조이다.(3차원 이상)<br> Data.Frame : 데이터 프레임. 실제로 만나는 대부분의 데이터의 형식이다.<br> List : 모든 데이터 형식을 연결할 수 있는, 1차원 구조의 형식이다.<br>

Vector
======

하나의 열(column)로 구성되어 있음. 1차원 구조.<br> 데이터 분석의 기본 단위<br> 하나의 데이터 유형만 가짐**(수치형, 문자형, 논리형)**

``` r
# 하나의 값을 가지는 벡터 만들기
name     <- "강민기"
age      <- 24
marriage <- FALSE


# 두 개 이상의 요소를 가지는 벡터 만들기
our.names <- c("안재현", "오준승", "조인정", "임동신", "윤휘영", "박현")
our.marriage <- c(FALSE,FALSE,FALSE,FALSE,FALSE,FALSE)


# :(콜론)을 이용하면 numeric vector를 만들 수 있음
# 1씩 증가하거나 감소하는 규칙이 있는 숫자로 이루어진 vector
1:5
```

    ## [1] 1 2 3 4 5

``` r
5:1
```

    ## [1] 5 4 3 2 1

``` r
10:1
```

    ##  [1] 10  9  8  7  6  5  4  3  2  1

``` r
# 값은 없거나 초기값으로 벡터 만들기
# vector(mode = , length = )
# mode : "numeric", "character", "logical"
# 주로 초기화할 때 사용함
vector(mode = "numeric", length = 10)
```

    ##  [1] 0 0 0 0 0 0 0 0 0 0

``` r
vector(mode = "character", length = 10)
```

    ##  [1] "" "" "" "" "" "" "" "" "" ""

``` r
vector(mode = "logical", length = 10)
```

    ##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE

벡터의 속성(Attributes)

``` r
height <- c(170L, 163L, 150L)
height
```

    ## [1] 170 163 150

``` r
# (1) element의 이름 : names(vector)
names(height)
```

    ## NULL

``` r
names(height) <- c("박문일", "김주우", "박현")
names(height) <- c("강민기","이희민","교아")
height
```

    ## 강민기 이희민   교아 
    ##    170    163    150

``` r
# NULL : Object가 없다는 의미
names(height) <- NULL
height
```

    ## [1] 170 163 150

``` r
# (2) element의 개수 : length(vector)
length(height)
```

    ## [1] 3

``` r
length(height)
```

    ## [1] 3

``` r
# (3) data type      : mode(vector), typeof(vector)
mode(height)
```

    ## [1] "numeric"

``` r
mode(height)
```

    ## [1] "numeric"

``` r
typeof(height)
```

    ## [1] "integer"

``` r
typeof(height)
```

    ## [1] "integer"

``` r
# (4) is.vector(data)
is.vector(height)
```

    ## [1] TRUE

``` r
is.vector(height)
```

    ## [1] TRUE

``` r
# (5) as.vector(data)
```

벡터의 인덱스(index)의 경우, **1부터 시작한다.**<br> R에서는 **슬라이싱**이 가능하다.( 벡터 중에서 일부의 element를 잘라내기 )

``` r
# vector[index]
weight <- c(70, 67, 52, 45, 65, 73, 82, 45, 70, 60, 53)
weight[1]
```

    ## [1] 70

``` r
weight[3]
```

    ## [1] 52

``` r
weight[5:4]
```

    ## [1] 65 45

``` r
# 1, 4, 11번째 element를 한 번에 가져오기
weight[c(1, 4, 11)]
```

    ## [1] 70 45 53

``` r
# 2 ~ 9번째 element를 한 번에 가져오기
weight[2:9]
```

    ## [1] 67 52 45 65 73 82 45 70

``` r
# 짝수 번째 element를 한 번에 가져오기
weight[seq(from = 2, to = 11, by = 2)]
```

    ## [1] 67 45 73 45 60

``` r
weight[seq(from = 2, to = length(weight), by = 2)]
```

    ## [1] 67 45 73 45 60

``` r
weight[seq(from = 2, to = length(weight), by = 2)]
```

    ## [1] 67 45 73 45 60

유용한 함수

``` r
# 관련 함수 : seq
# 시작과 끝, 그리고 증감 단위를 지정한다.
seq(from = 1, to = 5, by = 0.1)
```

    ##  [1] 1.0 1.1 1.2 1.3 1.4 1.5 1.6 1.7 1.8 1.9 2.0 2.1 2.2 2.3 2.4 2.5 2.6
    ## [18] 2.7 2.8 2.9 3.0 3.1 3.2 3.3 3.4 3.5 3.6 3.7 3.8 3.9 4.0 4.1 4.2 4.3
    ## [35] 4.4 4.5 4.6 4.7 4.8 4.9 5.0

``` r
seq(from = 1, to = 1000, by = 10)
```

    ##   [1]   1  11  21  31  41  51  61  71  81  91 101 111 121 131 141 151 161
    ##  [18] 171 181 191 201 211 221 231 241 251 261 271 281 291 301 311 321 331
    ##  [35] 341 351 361 371 381 391 401 411 421 431 441 451 461 471 481 491 501
    ##  [52] 511 521 531 541 551 561 571 581 591 601 611 621 631 641 651 661 671
    ##  [69] 681 691 701 711 721 731 741 751 761 771 781 791 801 811 821 831 841
    ##  [86] 851 861 871 881 891 901 911 921 931 941 951 961 971 981 991

``` r
seq(from = 2, to = 5000, by = 100)
```

    ##  [1]    2  102  202  302  402  502  602  702  802  902 1002 1102 1202 1302
    ## [15] 1402 1502 1602 1702 1802 1902 2002 2102 2202 2302 2402 2502 2602 2702
    ## [29] 2802 2902 3002 3102 3202 3302 3402 3502 3602 3702 3802 3902 4002 4102
    ## [43] 4202 4302 4402 4502 4602 4702 4802 4902

``` r
# 관련 함수 : rep
# 주어진 벡터를 복사해서 하나의 벡터를 만듬
# numeric / character / logical vector를 만들 수 있음
rep(1, times = 10)
```

    ##  [1] 1 1 1 1 1 1 1 1 1 1

``` r
rep(1, times = 10)
```

    ##  [1] 1 1 1 1 1 1 1 1 1 1

``` r
rep(1, each = 10)
```

    ##  [1] 1 1 1 1 1 1 1 1 1 1

``` r
rep(1, each = 10)
```

    ##  [1] 1 1 1 1 1 1 1 1 1 1

벡터의 두 가지 중요한 법칙<br> Vectorization : for와 같은 반복문을 사용하지 않고도 결과를 얻어내는 성질<br> for문을 사용하면 속도가 느려짐<br><br>

Recycling Rule : 재사용 규칙. 요소의 수가 적은 쪽의 벡터를 큰 쪽의 벡터의 데이터 개수와 동일하게 맞춰줌. 즉, 추가적으로 데이터를 임시로 만듬 임시로 만든 곳은 자신 자신의 값으로 채운다.<br><br>

Factor
======

하나의 열로 구성되어 있음. 1차원 구조.<br> 하나의 데이터 유형만 가짐<br> 데이터 분석의 기본 단위<br> 질적 자료( 범주형 자료 )가 됨

``` r
# factor(vector, labels = , levels = , ordered = )
# ordered = FALSE : 질적 자료, 명목형(nominal) 자료
# ordered = TRUE  : 질적 자료, 순서형(ordinal) 자료
at <- c("A","B","C","A","B")
at <- factor(at, levels = c("B","A","C"), labels = c('a','b','c'), ordered = TRUE)
at
```

    ## [1] b a c b a
    ## Levels: a < b < c

Factor의 **인덱스와 슬라이싱은 Vector와 동일**하다.<br> levels : 펙터의 집단<br> labels : levels의 이름<br> 이라고 이해하면 쉽다.<br><br> Factor의 속성

``` r
# (1) 집단의 개수       : nlevels(factor)
nlevels(at)
```

    ## [1] 3

``` r
# (2) 집단의 이름, 순서 : levels(factor)
levels(at)
```

    ## [1] "a" "b" "c"

``` r
# (3) data type         : mode(factor), typeof(factor)
mode(at)
```

    ## [1] "numeric"

``` r
typeof(at)
```

    ## [1] "integer"

``` r
# 눈에 보이기는 character 보이지만
# 실질적으로는 numeric으로 인식하고 있음

# (4) is.factor(data)
is.factor(at)
```

    ## [1] TRUE

``` r
# (5) as.factor(data)
as.factor(at)
```

    ## [1] b a c b a
    ## Levels: a < b < c

``` r
# (6) element의개수 : length(factor)
length(at)
```

    ## [1] 5

Matrix
======

행(row)과 열(column)로 구성되어 있음. 2차원 구조<br> 하나의 데이터 유형만 가짐.<br> Vector의 확장이 됨 : Vectorization, Recycling Rule이 적용<br> **머신러닝과 여러가지 통계 수식**에 많이 쓰인다.

``` r
v1 <- 1:3
v2 <- 4:6
v3 <- 1:6

# Matrix 데이터 형식 만들기
# (1) rbind(vector1, vector2, ...)
# bind by row
# 여러 개의 벡터를 행으로 묶어줌
A1 <- rbind(v1, v2) 
A1
```

    ##    [,1] [,2] [,3]
    ## v1    1    2    3
    ## v2    4    5    6

``` r
A2 <- rbind(v1, v2, v3)
A2
```

    ##    [,1] [,2] [,3] [,4] [,5] [,6]
    ## v1    1    2    3    1    2    3
    ## v2    4    5    6    4    5    6
    ## v3    1    2    3    4    5    6

``` r
# (2) cbind(vector1, vector2, ...)
# bind by column
B1 <- cbind(v1, v2)
B1
```

    ##      v1 v2
    ## [1,]  1  4
    ## [2,]  2  5
    ## [3,]  3  6

``` r
B2 <- cbind(v1, v2, v3)
B2
```

    ##      v1 v2 v3
    ## [1,]  1  4  1
    ## [2,]  2  5  2
    ## [3,]  3  6  3
    ## [4,]  1  4  4
    ## [5,]  2  5  5
    ## [6,]  3  6  6

``` r
# (3) matrix(vector, nrow = , ncol = , byrow = , dimnames = )
matrix(1:4, nrow = 2, ncol = 2)               # 열부터 입력
```

    ##      [,1] [,2]
    ## [1,]    1    3
    ## [2,]    2    4

``` r
matrix(1:4, nrow = 2, ncol = 2)
```

    ##      [,1] [,2]
    ## [1,]    1    3
    ## [2,]    2    4

``` r
matrix(1:4, nrow = 2, ncol = 2, byrow = TRUE) # 행부터 입력
```

    ##      [,1] [,2]
    ## [1,]    1    2
    ## [2,]    3    4

``` r
matrix(1:4, nrow = 2, ncol = 2, byrow = TRUE)
```

    ##      [,1] [,2]
    ## [1,]    1    2
    ## [2,]    3    4

``` r
matrix(1:4, nrow = 3, ncol = 2)               # recycling rule
```

    ## Warning in matrix(1:4, nrow = 3, ncol = 2): 데이터의 길이[4]가 행의 개수[3]
    ## 의 배수가 되지 않습니다

    ##      [,1] [,2]
    ## [1,]    1    4
    ## [2,]    2    1
    ## [3,]    3    2

``` r
matrix(1:4, nrow = 3, ncol = 2)
```

    ## Warning in matrix(1:4, nrow = 3, ncol = 2): 데이터의 길이[4]가 행의 개수[3]
    ## 의 배수가 되지 않습니다

    ##      [,1] [,2]
    ## [1,]    1    4
    ## [2,]    2    1
    ## [3,]    3    2

``` r
matrix(1:9, nrow = 3, ncol = 2)
```

    ## Warning in matrix(1:9, nrow = 3, ncol = 2): 데이터의 길이[9]가 열의 개수[2]
    ## 의 배수가 되지 않습니다

    ##      [,1] [,2]
    ## [1,]    1    4
    ## [2,]    2    5
    ## [3,]    3    6

Matrix의 속성

``` r
M <- matrix(1:4, nrow = 2, ncol = 2)
M
```

    ##      [,1] [,2]
    ## [1,]    1    3
    ## [2,]    2    4

``` r
# (1) 행의 개수   : nrow(matrix)
nrow(M)
```

    ## [1] 2

``` r
# (2) 열의 개수   : ncol(matrix)
ncol(M)
```

    ## [1] 2

``` r
# (3) 행의 이름   : rownames(matrix)
rownames(M)
```

    ## NULL

``` r
rownames(M) <- c("R1", "R2")

# (4) 열의 이름   : colnames(matrix)
colnames(M)
```

    ## NULL

``` r
colnames(M) <- c("C1", "C2")

M
```

    ##    C1 C2
    ## R1  1  3
    ## R2  2  4

``` r
# (5) 차원        : dim(matrix), 행의 개수와 열의 개수를 동시에 알려줌
dim(M)
```

    ## [1] 2 2

``` r
dim(M)[2]
```

    ## [1] 2

``` r
# (6) 차원의 이름 : dimnames(matrix), 행의 열의 이름을 동시에 알려줌 
dimnames(M)
```

    ## [[1]]
    ## [1] "R1" "R2"
    ## 
    ## [[2]]
    ## [1] "C1" "C2"

``` r
dimnames(M)[1]
```

    ## [[1]]
    ## [1] "R1" "R2"

``` r
dimnames(M)[2]
```

    ## [[1]]
    ## [1] "C1" "C2"

Matrix의 인덱스의 경우, vector, factor와 마찬가지로 1부터 시작한다.<br> Matrix의 슬라이싱

``` r
# matrix[row, column]
M1 <- matrix(1:9, nrow = 3, ncol = 3)
M1
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9

``` r
M1[1 , ]              # 첫 행, Vector
```

    ## [1] 1 4 7

``` r
M1[1, , drop = FALSE] # 첫 행, Matrix
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7

``` r
M1[1:2 , ]
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8

``` r
M1[ , 1]               # 첫 열, Vector
```

    ## [1] 1 2 3

``` r
M1[ , 1, drop = FALSE] # 첫 열, Matrix
```

    ##      [,1]
    ## [1,]    1
    ## [2,]    2
    ## [3,]    3

``` r
M1[ , c(1, 3)]
```

    ##      [,1] [,2]
    ## [1,]    1    7
    ## [2,]    2    8
    ## [3,]    3    9

``` r
M1[1, 1]                # 1행, 1열, Vector
```

    ## [1] 1

``` r
M1[1, 1, drop = FALSE]  # 1행, 1열, Matrix
```

    ##      [,1]
    ## [1,]    1

``` r
M1[1:2, 1:2]
```

    ##      [,1] [,2]
    ## [1,]    1    4
    ## [2,]    2    5

Matrix 관련 연산과 함수

``` r
# 1. matrix의 곱셉
# A%*%B
# 조건: A행렬의 열의 개수와 B행렬의 행의 개수가 같아야 한다.
# 교환법칙 성립하지 않는다.

A <- matrix(c(1,2), nrow = 1, ncol = 2)
B <- matrix(c(3,4), nrow = 2, ncol = 1)

A %*% B
```

    ##      [,1]
    ## [1,]   11

``` r
# 참고: A * B 는 수학에서의 행렬의 곱셈이 아니다. 같은 인덱스끼리 곱하는 식이다. 


# 2. matrix의 역행렬 (inverse matrix) ----
# solve(matrix) 함수를 이용해서 간단하게 구할 수 있다.
C <- matrix(1:4, nrow = 2, ncol = 2)
solve(C)
```

    ##      [,1] [,2]
    ## [1,]   -2  1.5
    ## [2,]    1 -0.5

``` r
C %*% solve(C) # 단위행렬이 결과로 나옴.
```

    ##      [,1] [,2]
    ## [1,]    1    0
    ## [2,]    0    1

Array(다차원 구조)
==================

하나의 데이터 유형만 가진다.<br> vector, matrix의 확장이다. 기본 성질은 비슷하다.

``` r
# array(vector, dim = )
array(1:5, dim = 10) # 1차원 array 생성 ( array 이면서, vector가 된다 )
```

    ##  [1] 1 2 3 4 5 1 2 3 4 5

``` r
array(1:5, dim = c(3,3)) # 2차원 array 생성 ( matrix 의 형식이 된다.)
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    2
    ## [2,]    2    5    3
    ## [3,]    3    1    4

``` r
array(1:5, dim = c(5,5))
```

    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,]    1    1    1    1    1
    ## [2,]    2    2    2    2    2
    ## [3,]    3    3    3    3    3
    ## [4,]    4    4    4    4    4
    ## [5,]    5    5    5    5    5

``` r
array(1:5, dim = c(2,2,2)) # 3차원 array
```

    ## , , 1
    ## 
    ##      [,1] [,2]
    ## [1,]    1    3
    ## [2,]    2    4
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2]
    ## [1,]    5    2
    ## [2,]    1    3

``` r
array(1:5, dim = c(3,3,3))
```

    ## , , 1
    ## 
    ##      [,1] [,2] [,3]
    ## [1,]    1    4    2
    ## [2,]    2    5    3
    ## [3,]    3    1    4
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2] [,3]
    ## [1,]    5    3    1
    ## [2,]    1    4    2
    ## [3,]    2    5    3
    ## 
    ## , , 3
    ## 
    ##      [,1] [,2] [,3]
    ## [1,]    4    2    5
    ## [2,]    5    3    1
    ## [3,]    1    4    2

DataFrame
=========

행과 열로 구성되어 있다. ( 2차원 구조 )<br> 여러 개의 데이터 유형을 가질 수 있다는 점에서 matrix와 다르다.<br> 단, 하나의 열은 하나의 데이터 유형만 가진다.<br> recycling rule이 작동하지 않는다.<br> 우리들이 실제로 만나는 데이터는 대부분 dataframe이다. 여러가지 데이터 유형을 가지고 있기 때문이다.

``` r
id <- 1:4
major <- c("경영학","컴퓨터공학","전자공학","문화콘텐츠학")
major <- factor(major)
car <- c("포나", NA, "아우디", "G80")
income <- c(3000,5000,5000,3000)

survey <- data.frame(id,major,car,income)
survey
```

    ##   id        major    car income
    ## 1  1       경영학   포나   3000
    ## 2  2   컴퓨터공학   <NA>   5000
    ## 3  3     전자공학 아우디   5000
    ## 4  4 문화콘텐츠학    G80   3000

List
====

1차원 구조<br> 가장 유연한 형태의 데이터 유형이다.<br> 회귀분석 등의 결과를 저장하는 데이터<br> 데이터의 유형들의 요소들은 동일한 메모리 크기를 차지하지만, List는 element의 사이즈를 다르게 할 수 있다.<br> 따라서 리스트안에 vector, factor, matrix, array, dataframe을 포함할 수 있다.

``` r
food <- c("회덮밥","한식부페", "스테이크덮밥", "목살스테이크", "한식부페")

result <- list(food, A, survey)
result # 이중 대괄호가 나오면, 리스트다.
```

    ## [[1]]
    ## [1] "회덮밥"       "한식부페"     "스테이크덮밥" "목살스테이크"
    ## [5] "한식부페"    
    ## 
    ## [[2]]
    ##      [,1] [,2]
    ## [1,]    1    2
    ## 
    ## [[3]]
    ##   id        major    car income
    ## 1  1       경영학   포나   3000
    ## 2  2   컴퓨터공학   <NA>   5000
    ## 3  3     전자공학 아우디   5000
    ## 4  4 문화콘텐츠학    G80   3000

리스트의 슬라이싱<br> list\[index\] : 리스트의 1번째 요소를 가져온다. ( 리스트를 반환함 )<br> list\[\[index\]\] : 동일. ( 단, 요소의 데이터 유형을 반환함 )

``` r
result[1] # List
```

    ## [[1]]
    ## [1] "회덮밥"       "한식부페"     "스테이크덮밥" "목살스테이크"
    ## [5] "한식부페"

``` r
result[[1]] # Vector
```

    ## [1] "회덮밥"       "한식부페"     "스테이크덮밥" "목살스테이크"
    ## [5] "한식부페"

``` r
# 응용
result[[1]][c(1,5)] # Vector의 슬라이싱
```

    ## [1] "회덮밥"   "한식부페"

``` r
result[[1]][c(1,5)][1] # Vector의 슬라이싱의 슬라이싱
```

    ## [1] "회덮밥"

``` r
result[2] # list
```

    ## [[1]]
    ##      [,1] [,2]
    ## [1,]    1    2

``` r
result[[2]]
```

    ##      [,1] [,2]
    ## [1,]    1    2

``` r
result[[2]][1, ]
```

    ## [1] 1 2

``` r
result[[2]][1, ,drop=FALSE] # matrix
```

    ##      [,1] [,2]
    ## [1,]    1    2

``` r
result[3] # list
```

    ## [[1]]
    ##   id        major    car income
    ## 1  1       경영학   포나   3000
    ## 2  2   컴퓨터공학   <NA>   5000
    ## 3  3     전자공학 아우디   5000
    ## 4  4 문화콘텐츠학    G80   3000

``` r
result[[3]] # data.frame
```

    ##   id        major    car income
    ## 1  1       경영학   포나   3000
    ## 2  2   컴퓨터공학   <NA>   5000
    ## 3  3     전자공학 아우디   5000
    ## 4  4 문화콘텐츠학    G80   3000
