fastcampus\_R프로그래밍\_7
================
huimin
2019년 3월 21일

설정하기
========

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
library(naniar)
library(nortest)
library(purrr)

house.price <- read.csv(file = "d:/fastcampus/HousePrices.csv",
                        header = TRUE)
```

One - sample test
=================

일표본 검정<br> 언제(when)<br> 하나의 모집단의 **평균이 커졌는지, 작아졌는지, 달라졌는지**를 통계적으로 검정(분석)하는 방법

종류<br> 모수적 방법(Parametric Method) : One Sample t-test **(일표본 t 검정)**<br> 가정(Assumption)이 필요하다. 모집단이 정규분포를 따른다. **( 정규성 가정 )**

비모수적 방법(Non-Parametric Method) : Wilcoxon's signed rank test(월콕슨의 부호 순위 검정)

1단계 : 가설 세우기
-------------------

귀무가설(H0) : SalePrice의 평균은 150,000 달러이다. **( mu = 150000 )**<br> 대립가설(H1) : SalePrice의 평균은 150,000 달러보다 많다. **( mu &gt; 150000 )**<br> 유의수준 = **0.05**

2단계 : 정규성 검정 (Normality Test)
------------------------------------

귀무가설 : SalePrice는 정규분포를 따른다.<br> 대립가설 : SalePrice는 정규분포를 따르지 않는다.<br> Shapiro-Wilk의 Normality test : 표본의 크기(n) &lt; 5000개 일 때 사용한다.<br> Anderson-Darling의 Normality test : 표본의 크기(n) &gt;= 5000개 일 때 사용한다.

``` r
#우선, 데이터의 크기를 파악한다.
nrow(house.price)
```

    ## [1] 2919

``` r
# shapiro.test(data$variable)
shapiro.test(house.price$SalePrice)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  house.price$SalePrice
    ## W = 0.86967, p-value < 2.2e-16

``` r
# 정규성 검정의 결론은 유의확률(p-value)이 0.000 이므로, 대립가설을 따른다. 
# SalePrice는 정규분포를 따르지 않는다.


# nortest::ad.test(data$variable)
nortest::ad.test(house.price$LotArea)
```

    ## 
    ##  Anderson-Darling normality test
    ## 
    ## data:  house.price$LotArea
    ## A = 283.99, p-value < 2.2e-16

``` r
# 유의확률(p-value)이 0.000 이므로, 대립가설을 따른다.
# LotArea는 정규분포를 따르지 않는다.
```

3단계 : 모수적 방법 / 비모수적 방법 결정 ( 정규성 검정 결과를 통해서 )
----------------------------------------------------------------------

Shapiro-Wilk normality test<br> data: house.price$SalePrice<br> W = 0.86967, p-value &lt; 2.2e-16<br> 이므로, 비모수적 방법을 사용하여야 한다. 하지만, markdown에서는 일단 2가지 방법을 모두 사용해보기로 한다.

4단계 : 귀무가설 / 대립가설에 따른 결론 도출
--------------------------------------------

### one sample t-test

t.test(data$variable, mu = , alternative = )<br> mu : **귀무가설의 모평균**<br> alternative : 대립가설 **( "greater", "less", "two.sided")**<br> alternative의 default는 **"two.sided"** 이다.

``` r
t.test(house.price$SalePrice, mu = 150000,
       alternative = "greater")
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  house.price$SalePrice
    ## t = 14.872, df = 1459, p-value < 2.2e-16
    ## alternative hypothesis: true mean is greater than 150000
    ## 95 percent confidence interval:
    ##  177499.2      Inf
    ## sample estimates:
    ## mean of x 
    ##  180921.2

``` r
# n의 갯수 파악하기
# naniar::n_miss는 NA의 개수를 파악해주는 함수이다.
n <- length(house.price$SalePrice) - naniar::n_miss(house.price$SalePrice)
n
```

    ## [1] 1460

``` r
# x bar의 표준편차(S) 구하기
S <- sd(house.price$SalePrice, na.rm = TRUE)
S
```

    ## [1] 79442.5

#### 결론

data: house.price$SalePrice<br> t = 14.872, df = 1459, p-value &lt; 2.2e-16<br> alternative hypothesis: true mean is greater than 150000<br> 95 percent confidence interval:<br> 177499.2 Inf<br> sample estimates:<br> mean of x <br> 180921.2

**검정통계량(t)** : 14.872<br> **자유도(df)** : 1459<br> **유의확률(p-value)** : 0.000<br> **대립가설 채택**<br> **도출된 결론**: 유의수준 0.05에서 유의확률이 0.000이므로, SalePrice의 평균은 통계적으로 유의하게 150000보다 크다고 판단할 수 있다.

### Wilcoxon's signed rank test

``` r
wilcox.test(house.price$SalePrice, mu = 150000,
            alternative = "greater")
```

    ## 
    ##  Wilcoxon signed rank test with continuity correction
    ## 
    ## data:  house.price$SalePrice
    ## V = 720850, p-value < 2.2e-16
    ## alternative hypothesis: true location is greater than 150000

#### 결론

윌콕슨의 부호 순위 검정은, 각 데이터간의 순위를 결정하여 비모수적인 방법으로 검정하기 때문에, 검정통계량의 종류가 다르다.<br> Wilcoxon signed rank test with continuity correction<br> data: house.price$SalePrice<br> V = 720850, p-value &lt; 2.2e-16<br> alternative hypothesis: true location is greater than 150000

**검정통계량(V)** : 14.872<br> **유의확률(p-value)** : 0.000<br> **대립가설 채택**<br> **도출된 결론**: 유의수준 0.05에서 유의확률이 0.000이므로, SalePrice의 평균은 통계적으로 유의하게 150000보다 크다고 판단할 수 있다.

연습문제
========

Data type이 numeric<br> Id를 제외한 모든 numeric data type에 대해서 다음의 문제를 해결하시오<br> 정규성 검정을 하고, 그것의 p-value를 보고 p-value가 0.05보다 크면<br> one sample ttest 를 하고, 아니면 signed rank test를 하시오<br> 그리고, 최종적으로 **변수명, 분석방법의 이름, 검정통계량, 유의확률**을 저장해서 엑셀에 출력하시오.<br> 엑셀의 이름은 day7\_result1.xlsx

``` r
# numeric 타입의 변수들
house.price.numeric <- house.price %>% 
  purrr::keep(is.numeric) %>% 
  dplyr::select(-Id)

# NA값 확인하기
sapply(house.price.numeric, function(x) sum(is.na(x)))
```

    ##    MSSubClass   LotFrontage       LotArea   OverallQual   OverallCond 
    ##             0           486             0             0             0 
    ##     YearBuilt  YearRemodAdd    MasVnrArea    BsmtFinSF1    BsmtFinSF2 
    ##             0             0            23             1             1 
    ##     BsmtUnfSF   TotalBsmtSF     X1stFlrSF     X2ndFlrSF  LowQualFinSF 
    ##             1             1             0             0             0 
    ##     GrLivArea  BsmtFullBath  BsmtHalfBath      FullBath      HalfBath 
    ##             0             2             2             0             0 
    ##  BedroomAbvGr  KitchenAbvGr  TotRmsAbvGrd    Fireplaces   GarageYrBlt 
    ##             0             0             0             0           159 
    ##    GarageCars    GarageArea    WoodDeckSF   OpenPorchSF EnclosedPorch 
    ##             1             1             0             0             0 
    ##    X3SsnPorch   ScreenPorch      PoolArea       MiscVal        MoSold 
    ##             0             0             0             0             0 
    ##        YrSold     SalePrice 
    ##             0          1459

``` r
# 데이터의 길이 확인하기
nrow(house.price.numeric)
```

    ## [1] 2919

``` r
# 결과를 저장할 데이터 프레임
result.frame <- data.frame()

# 반복문을 통한, 결과 도출
for(i in 1:ncol(house.price.numeric)){
  
  # NA값 대체하기
  house.price.numeric[is.na(house.price.numeric[, i]), i] <- mean(house.price.numeric[, i], na.rm = TRUE)
  
  P <- shapiro.test(house.price.numeric[, i])
  
  # t.test
  if(P$p.value > 0.05){
    
    result <- t.test(house.price.numeric[, i],
                     alternative = "two.sided")
    
    # wilcox.test
  }else{
    
    result <- wilcox.test(house.price.numeric[, i],
                          alternative = "two.sided")
    
  }
  
  
  result.1row <- data.frame(Colname = colnames(house.price.numeric)[i],
                            Method = result$method,
                            Statistic = result$statistic,
                            P = result$p.value)
  
  result.frame <- rbind(result.frame, result.1row)
  
}
```

    ## Warning in wilcox.test.default(house.price.numeric[, i], alternative =
    ## "two.sided"): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(house.price.numeric[, i], alternative =
    ## "two.sided"): cannot compute exact p-value with zeroes

    ## Warning in wilcox.test.default(house.price.numeric[, i], alternative =
    ## "two.sided"): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(house.price.numeric[, i], alternative =
    ## "two.sided"): cannot compute exact p-value with zeroes

    ## Warning in wilcox.test.default(house.price.numeric[, i], alternative =
    ## "two.sided"): cannot compute exact p-value with zeroes

``` r
rownames(result.frame) <- NULL
write.csv(result.frame,
          file = "day7_result1.csv",
          row.names = FALSE)
```

Two-sample test
===============

2표본 검정<br> **두 개의 독립적인 모집단**의 평균이 한 쪽이 큰 지, 작은 지, 다른 지를 통계적으로 검정하는 방법<br>

종류<br> **모수적 방법** : 독립 2표본 t검정 <br> (1) 등분산이 가정된 Two sample t test<br> (2) 이분산이 가정된 Two sample t test

**비모수적 방법** : wilcoxon's rank sum test ( 윌콕슨의 순위합 검정 )<br> 두 모집단 중 **하나라도 정규성 가정이 깨지면**, 비모수적 방법을 이용한다.

자료<br> 양적 자료 : 1개<br> 질적 자료 : 1개, 두 개의 값으로 이루어져 있어야 함 ( factor의 레벨이 )

1단계 : 가설 세우기
-------------------

귀무가설 : RL과 Non-RL간의 SalePrice의 차이가 없다. **(mu1 = mu2)**<br> 대립가설 : RL의 SalePrice가 Non-RL보다 크다. **(mu1 &gt; mu2)**

2단계 : 정규성 검정 (Normality Test)
------------------------------------

귀무가설 : RL의 SalePrice는 정규분포를 따른다.<br> 대립가설 : RL의 SalePrice는 정규분포를 따르지 않는다.<br>

귀무가설 : Non-RL의 SalePrice는 정규분포를 따른다.<br> 대립가설 : Non-RL의 SalePrice는 정규분포를 따르지 않는다.

``` r
# 그룹을 RL과 Non-RL 두개로만 나누기
house.price$MSZoning.newgroup <- ifelse(house.price$MSZoning == "RL",
                                        "RL",
                                        "Non-RL")

# factor로 바꾸기
house.price$MSZoning.newgroup <- as.factor(house.price$MSZoning.newgroup)
house.price$MSZoning.newgroup <- relevel(house.price$MSZoning.newgroup,
                                         ref = "RL")

# by함수를 이용한 정규성 검정
by(house.price$SalePrice, house.price$MSZoning.newgroup, shapiro.test)
```

    ## house.price$MSZoning.newgroup: RL
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  dd[x, ]
    ## W = 0.86096, p-value < 2.2e-16
    ## 
    ## -------------------------------------------------------- 
    ## house.price$MSZoning.newgroup: Non-RL
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  dd[x, ]
    ## W = 0.88012, p-value = 8.049e-15

house.price$MSZoning.newgroup: **Non-RL**<br> Shapiro-Wilk normality test<br> data: dd\[x, \]<br> W = 0.88012, **p-value = 8.049e-15**

house.price$MSZoning.newgroup: **RL**<br> Shapiro-Wilk normality test<br> data: dd\[x, \]<br> W = 0.86096, **p-value &lt; 2.2e-16**

**결론**<br> 유의확률이 0.000 이므로 유의수준 0.05에서 정규성 가정이 깨진 것으로 추정한다.<br> 유의확률이 0.000 이므로 유의수준 0.05에서 정규성 가정이 깨진 것으로 추정한다.<br> 그러므로, **wilcoxon's rank sum test**를 사용해야한다. 하지만, 마크다운에서는 둘 다 사용해보겠다.

3단계 : 등분산 검정
-------------------

만약, **정규성 가정이 성립**하였을 경우, 진행한다.<br> 귀무가설 : 두 집단의 모분산에는 차이가 없다. (등분산)<br> 대립가설 : 두 집단의 모분산에는 차이가 있다. (이분산)

``` r
# var.test(양적 자료 ~ 질적 자료)
# var.test(data$variable ~ data$variable)
var.test(house.price$SalePrice ~ house.price$MSZoning.newgroup)
```

    ## 
    ##  F test to compare two variances
    ## 
    ## data:  house.price$SalePrice by house.price$MSZoning.newgroup
    ## F = 1.7422, num df = 1150, denom df = 308, p-value = 8.067e-09
    ## alternative hypothesis: true ratio of variances is not equal to 1
    ## 95 percent confidence interval:
    ##  1.451219 2.072162
    ## sample estimates:
    ## ratio of variances 
    ##           1.742178

**결론**<br> 유의확률이 0.000이므로, 유의수준 0.05에서 이분산으로 추정한다.

4단계 : 검정
------------

t.test(양적자료 ~ 질적자료, alternative = , var.equal = )<br> wilcox.test(양적자료 ~ 질적자료, alternative = )

``` r
# 이분산인 경우, var.equal = FALSE 이다.
t.test(house.price$SalePrice ~ house.price$MSZoning.newgroup,
       alternative = "greater",
       var.equal = FALSE)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  house.price$SalePrice by house.price$MSZoning.newgroup
    ## t = 11.298, df = 626.76, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means is greater than 0
    ## 95 percent confidence interval:
    ##  40698.18      Inf
    ## sample estimates:
    ##     mean in group RL mean in group Non-RL 
    ##             191005.0             143359.9

``` r
# 등분산인 경우, var.equal = TRUE 이다.
t.test(house.price$SalePrice ~ house.price$MSZoning.newgroup,
       alternative = "greater",
       var.equal = TRUE)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  house.price$SalePrice by house.price$MSZoning.newgroup
    ## t = 9.6518, df = 1458, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means is greater than 0
    ## 95 percent confidence interval:
    ##  39520.28      Inf
    ## sample estimates:
    ##     mean in group RL mean in group Non-RL 
    ##             191005.0             143359.9

``` r
# 정규성 가정이 깨진 경우, wilcox.test를 진행한다.
# wilcox.test(양적 자료 ~ 질적 자료, alternative = )
wilcox.test(house.price$SalePrice ~ house.price$MSZoning.newgroup,
            alternative = "greater")
```

    ## 
    ##  Wilcoxon rank sum test with continuity correction
    ## 
    ## data:  house.price$SalePrice by house.price$MSZoning.newgroup
    ## W = 253780, p-value < 2.2e-16
    ## alternative hypothesis: true location shift is greater than 0

### 결론

Wilcoxon rank sum test with continuity correction<br> data: house.price*S**a**l**e**P**r**i**c**e**b**y**h**o**u**s**e*.*p**r**i**c**e*MSZoning.newgroup<br> W = 253780, p-value &lt; 2.2e-16<br> alternative hypothesis: true location shift is greater than 0<br> 유의확률이 0.000이므로, 유의수준 0.05에서 RL의 SalePrice가 평균적으로 더 높다고 추정한다.
