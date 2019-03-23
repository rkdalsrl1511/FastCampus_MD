fastcampus\_R프로그래밍\_8
================
huimin
2019년 3월 21일

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
library(purrr)
library(car)
```

    ## Loading required package: carData

    ## 
    ## Attaching package: 'car'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     recode

    ## The following object is masked from 'package:purrr':
    ## 
    ##     some

``` r
sat1 <- readxl::read_excel(path = "d:/fastcampus/sat.xlsx",
                           sheet = 1,
                           col_names = TRUE)
```

paired Test ( 대응 2표본 검정 )
===============================

**동일한 대상자**에게 **사전/사후** 간의 차이가 있는지를 통계적으로 검정(분석)하는 방법<br>

**종류**<br> 모수적 방법( paired t-test : 대응 2표본 t검정 )<br> 비모수적 방법( wilcoxon's signed rank test : 윌콕슨의부호 순위 결정법 )

**자료**<br> 양적 자료 : 2개<br> 사전 1개, 사후 1개

``` r
# 사전과 사후의 차이를 나타내주는 새로운 변수
sat1$diffrence <- sat1$pre - sat1$post
```

1단계 : 가설 세우기
-------------------

귀무가설 : 이부일 강사의 강의 효과는 없다<br> (사전 만족도의 평균 = 사후 만족도의 평균)<br> 대립가설 : 이부일 강사의 강의 효과는 있다.<br> (사전 만족도의 평균 &lt; 사후 만족도의 평균)

2단계 : 정규성 검정
-------------------

``` r
shapiro.test(sat1$diffrence)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  sat1$diffrence
    ## W = 0.88573, p-value = 0.01299

유의확률이 0.013이므로, 유의수준 0.05에서 정규성 가정이 성립하지 않는다고 추정할 수 있다.

3단계 : 가설검정 / 결론도출
---------------------------

``` r
# 정규성 가정이 성립하였을 경우
t.test(sat1$pre,
       sat1$post,
       alternative = "less",
       paired = TRUE)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  sat1$pre and sat1$post
    ## t = -3.7491, df = 22, p-value = 0.000555
    ## alternative hypothesis: true difference in means is less than 0
    ## 95 percent confidence interval:
    ##        -Inf -0.5419817
    ## sample estimates:
    ## mean of the differences 
    ##                      -1

``` r
# 정규성 가정이 성립하지 않았을 경우
wilcox.test(sat1$pre,
            sat1$post,
            alternative = "less",
            paired = TRUE)
```

    ## Warning in wilcox.test.default(sat1$pre, sat1$post, alternative = "less", :
    ## cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(sat1$pre, sat1$post, alternative = "less", :
    ## cannot compute exact p-value with zeroes

    ## 
    ##  Wilcoxon signed rank test with continuity correction
    ## 
    ## data:  sat1$pre and sat1$post
    ## V = 12, p-value = 0.0008661
    ## alternative hypothesis: true location shift is less than 0

**결론** : 유의확률이 0.001이므로, 유의수준 0.05에서 이부일 강사의 강의 효과는 있다고 추정할 수 있다.

참고 팁
=======

unlist() : tibble, list 형태의 데이터를 벡터로 전환시키는 함수.
---------------------------------------------------------------

shapiro test 나 ad.test, 등 벡터만을 받아야하는 함수를 사용하기 위해서 사용한다.

ANOVA
=====

**독립적인 3개 이상의 모집단**의 평균이 차이가 있는지를 통계적으로 검정하는 방법<br> **종류**<br> 모수적 방법(분산 분석) : ANOVA<br> 비모수적 방법 : kruskal-wallis test ( 크루스칼 - 왈리스 검정 )<br> **자료**<br> 질적 자료 1개 : 3개 이상의 집단으로 분류되어 있어야 함<br> 양적 자료 1개 이상

``` r
# 실습 데이터 불러오기
house.price <- read.csv(file = "d:/fastcampus/HousePrices.csv",
                        header = TRUE,
                        stringsAsFactors = TRUE)
```

1단계 : 가설 세우기
-------------------

귀무가설 : MSZoning(집단)에 따라 SalePrice(양적 자료)에 차이가 없다.<br> 대립가설 : MSZoning에 따라 SalePrice에 차이가 있다.

2단계 : 정규성 검정
-------------------

귀무가설 : 각 집단의 SalePrice는 정규분포를 따른다.<br> 대립가설 : 각 집단의 SalePrice는 정규분포를 따르지 않는다.

``` r
sha.mszoning <- by(house.price$SalePrice,
                   house.price$MSZoning,
                   shapiro.test)

# 하나라도 성립하지 않을경우, 비모수적 방법을 사용해야 한다.
sha.mszoning$`C (all)`
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  dd[x, ]
    ## W = 0.93901, p-value = 0.542

``` r
sha.mszoning$FV
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  dd[x, ]
    ## W = 0.92003, p-value = 0.0004478

``` r
sha.mszoning$RH
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  dd[x, ]
    ## W = 0.96297, p-value = 0.7158

``` r
sha.mszoning$RL
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  dd[x, ]
    ## W = 0.86096, p-value < 2.2e-16

``` r
sha.mszoning$RM
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  dd[x, ]
    ## W = 0.73799, p-value < 2.2e-16

3단계 : 등분산 검정
-------------------

귀무가설 : 각 집단들의 모분산은 등분산이다.<br> 대립가설 : 각 집단들의 모분산은 이분산이다.<br> **Levene's Test**<br> car::leveneTest(양적 자료 ~ 질적 자료, data = )

``` r
car::leveneTest(SalePrice ~ MSZoning, 
                 data = house.price)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##         Df F value    Pr(>F)    
    ## group    4  12.306 7.635e-10 ***
    ##       1455                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

유의확률이 0.000이므로, 유의수준 0.05에서 각 집단들의 모분산은 이분산이라고 추정할 수 있다.

4단계 : 가설검정 / 결론도출
---------------------------

마크다운에서는 ANOVA와 kruskal-wallis를 모두 사용해본다.

``` r
# ANOVA
aov.result1 <- oneway.test(SalePrice ~ MSZoning,
                           data = house.price,
                           var.equal = FALSE)

aov.result2 <- aov(SalePrice ~ MSZoning,
                   data = house.price)

# aov함수를 이용했을 경우, summary를 사용할 수 있다.
summary(aov.result2)
```

    ##               Df    Sum Sq   Mean Sq F value Pr(>F)    
    ## MSZoning       4 9.904e+11 2.476e+11   43.84 <2e-16 ***
    ## Residuals   1455 8.218e+12 5.648e+09                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 1459 observations deleted due to missingness

``` r
# kruskal-wallis
kruskal.test(SalePrice ~ MSZoning,
             data = house.price)
```

    ## 
    ##  Kruskal-Wallis rank sum test
    ## 
    ## data:  SalePrice by MSZoning
    ## Kruskal-Wallis chi-squared = 270.07, df = 4, p-value < 2.2e-16

**결론** : 유의확률이 0.000이므로, 유의수준 0.05에서 MSZoning 그룹간의 SalePrice는 유의하게 차이가 있다고 추정할 수 있다.

5단계 : 사후분석(Post-Hoc), 다중비교(Multiple comparison)
---------------------------------------------------------

우리는 분석을 통해서, 각 집단별의 SalePrice에 차이가 있음을 알았다.<br> 그렇다면, 어떤 것들이 얼마나 차이가 있길래 이러한 결과가 나왔는지 궁금할 것이다.<br> 이럴 때, 다중비교를 사용한다.<br>

``` r
# TukeyHSD(분산분석 결과)
TukeyHSD(aov.result2)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = SalePrice ~ MSZoning, data = house.price)
    ## 
    ## $MSZoning
    ##                  diff         lwr        upr     p adj
    ## FV-C (all) 139486.062   69764.660 209207.464 0.0000005
    ## RH-C (all)  57030.375  -25710.258 139771.008 0.3271880
    ## RL-C (all) 116476.995   51288.552 181665.437 0.0000116
    ## RM-C (all)  51788.830  -14590.266 118167.926 0.2074812
    ## RH-FV      -82455.687 -139737.663 -25173.710 0.0008397
    ## RL-FV      -23009.067  -49176.710   3158.576 0.1154151
    ## RM-FV      -87697.231 -116704.073 -58690.389 0.0000000
    ## RL-RH       59446.620    7777.634 111115.605 0.0147312
    ## RM-RH       -5241.545  -58404.835  47921.745 0.9988468
    ## RM-RL      -64688.165  -79849.169 -49527.160 0.0000000

``` r
# nparcomp::nparcomp(양적 자료 ~ 질적 자료, data = , type = "Tukey")
# nparcomp::nparcomp(SalePrice ~ MSZoning,
#                    data = house.price,
#                    type = "Tukey")
```

**사후분석 결과표**<br> 소수점은 일단 모두 지웠다. 유의확률은 소수점 3자리만 남겼다.

|    비교    |  차이  |   lwr   |   upr  | 유의확률 |
|:----------:|:------:|:-------:|:------:|:--------:|
| FV-C (all) | 139486 |  69764  | 209207 |   0.000  |
| RH-C (all) |  57030 |  -25710 | 139771 |   0.327  |
| RL-C (all) | 116476 |  51288  | 181665 |   0.000  |
| RM-C (all) |  51788 |  -14590 | 118167 |   0.207  |
|    RH-FV   | -82455 | -139737 | -25173 |   0.001  |
|    RL-FV   | -23009 |  -49176 |  3158  |   0.115  |
|    RM-FV   | -87697 | -116704 | -58690 |   0.000  |
|    RL-RH   |  59446 |   7777  | 111115 |   0.015  |
|    RM-RH   |  -5241 |  -58404 |  47921 |   0.999  |
|    RM-RL   | -64688 |  -79849 | -49527 |   0.000  |

결과를 해석해보자면, 표의 첫행은 FV와 C를 비교한 것이다.<br> 그 차이는 139486으로, 양수이다. 즉, FV가 C보다 크다는 뜻이다.<br> 하지만 여기서 끝나면 안된다. **유의확률** 또한 봐야한다.<br> 유의확률이 0.05를 넘긴다면, 그 말은 두 집단의 차이가 없다고 추정하는 것이기 때문이다.<br> 따라서 첫행을 해석해보자면, **FV &gt; C**이며, 두 집단은 유의한 차이가 존재하기 때문에 서로 다른 집단이라고 추정할 수 있다는 것이다.<br> 반면에, 2번째 행의 RH와 C를 본다면, 유의확률이 0.327이므로 동일한 집단으로 추정할 수 있을 것이다.
