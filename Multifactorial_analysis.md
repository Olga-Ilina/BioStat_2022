---
title: "Multifactorial_analysis"
author: "Olga"
date: "2022-12-22"
output:
  html_document:
    keep_md: true
---






1. Известно, что креатинин k и мочевина m являются сильно коррелированными величинами, распределения которых близки к нормальным. При этом μk = 88.5, σk = 13.25, μm = 5.4, σm = 1.45, а коэффициент корреляции ρ(k, m) = 0.6.

Смоделируйте выборку S, содержащий информацию о 100 пациентах, у которых замерили эти два показателя. 

Предположим, что вы хотите построить линейную модель, связывающую их. 
(Воспользуйтесь пакетом mvtnorm чтобы смоделировать соответствующее многомерное нормальное распределение.)




```r
# Standard deviations and correlation
sig_k <- 13.25
sig_m <- 1.45
rho_km <- 0.6

# Covariance between k and m
sig_km <- rho_km * sig_k * sig_m

# Covariance matrix
Sigma_km <- matrix(c(sig_k ^ 2, sig_km, sig_km, sig_m ^ 2), nrow = 2, ncol = 2)


#means
mu_k <- 88.5
mu_m <- 5.4

# simulate 100 observations
set.seed(123) # for reproducibility
km_vals <- rmvnorm(100, mean = c(mu_k, mu_m), sigma = Sigma_km) # multivariate normal distribution

# Have a look at the first observations
head(km_vals)
```

```
##           [,1]     [,2]
## [1,]  80.90340 4.673865
## [2,] 109.17165 6.729984
## [3,]  91.57945 7.579019
## [4,]  93.58587 4.236915
## [5,]  79.05988 4.312146
## [6,] 104.97692 6.812934
```

```r
# convert matrix to data frame
km_vals <- as.data.frame(km_vals)
```


```r
# rename columns

km_vals <- km_vals %>% 
  dplyr::rename(
    Kreatinine = V1,
    Urea = V2
    )
```


(a) (1 балл) Как выглядит модель линейной регрессии креатинина k на мочевину m? Воспользуйтесь функцией lm, чтобы оценить параметры в этой модели. 
Как полученные оценки связаны с выборочными характеристиками (средним, дисперсией, корреляцией) выборки S?


```r
lm_km <- lm(Kreatinine~Urea, data = km_vals) #Create the linear regression
summary(lm_km) #Review the results
```

```
## 
## Call:
## lm(formula = Kreatinine ~ Urea, data = km_vals)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -26.5217  -6.7218   0.9362   7.0692  26.0898 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  65.9056     4.5372  14.526  < 2e-16 ***
## Urea          4.2130     0.8208   5.133 1.45e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 10.52 on 98 degrees of freedom
## Multiple R-squared:  0.2119,	Adjusted R-squared:  0.2038 
## F-statistic: 26.34 on 1 and 98 DF,  p-value: 1.449e-06
```
Модель линейной регрессии креатинина на мочевину выглядит следующим образом:
Kreatinine = 65.91*Urea + 4.21


(b) (1 балл) Проверьте гипотезу о нормальном распределении остатков в вашей модели.
Распределение остатков в модели выглядит следующим образом:
Residuals:
     Min       1Q   Median       3Q      Max 
-26.5217  -6.7218   0.9362   7.0692  26.0898 

Т.о. сумма остатков примерно равна нулю.


```r
# residual normality test
ols_plot_resid_qq(lm_km) # residual QQ plot
```

![](Multifactorial_analysis_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
ols_test_normality(lm_km) # residual normality test
```

```
## -----------------------------------------------
##        Test             Statistic       pvalue  
## -----------------------------------------------
## Shapiro-Wilk              0.9902         0.6768 
## Kolmogorov-Smirnov        0.0594         0.8720 
## Cramer-von Mises          7.9125         0.0000 
## Anderson-Darling          0.3453         0.4778 
## -----------------------------------------------
```

```r
ols_plot_resid_hist(lm_km) # residual histogram
```

![](Multifactorial_analysis_files/figure-html/unnamed-chunk-5-2.png)<!-- -->
Тест на нормальность реального распределения остатков показывает отсутствие статистически значимых отличий с предполагаемым распределением остатков. Т.о. остатки в данной модели распределены нормально.

Коэффициент детерминации R² данной модели составил 0.2119, adjusted  R² 0.2038 
20.38% дисперсии в популяции объясняется уравнением линейной регрессии.

(c) (1 балл) Добавим в модель еще один признак W, никак не связанный с k и m. Смоделируйте одномерную выборку (с произвольным распределением), и, считая ее одним из признаков в выборке S, добавьте этот признак в вашу модель линейной регрессии. Как изменился коэффициент детерминации в новой модели? Как изменился модифицированный коэффициент детерминации?


```r
# sample with random distribution

set.seed (3)
Kalium <- round (runif(100, 3, 6), 2) 

# convert matrix to data frame
Kalium <- as.data.frame(Kalium)
```


```r
# merge data frames
df <-cbind(km_vals, Kalium)
```


```r
lm_km2 = lm(Kreatinine~Urea + Kalium, data = df) #Create a linear regression with two variables
summary(lm_km2) #Review the results
```

```
## 
## Call:
## lm(formula = Kreatinine ~ Urea + Kalium, data = df)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -26.4816  -6.6787   0.9246   7.1024  26.0964 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 66.19694    7.40869   8.935 2.67e-14 ***
## Urea         4.21007    0.82718   5.090 1.76e-06 ***
## Kalium      -0.06187    1.23996  -0.050     0.96    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 10.57 on 97 degrees of freedom
## Multiple R-squared:  0.2119,	Adjusted R-squared:  0.1956 
## F-statistic: 13.04 on 2 and 97 DF,  p-value: 9.656e-06
```
Модель линейной регрессии описывается формулой:
Kreatinine = 66,19 + Urea * 4.21 - Kalium * 0.06

Однако уровень значимость p для Kalium превышает 0.05 и составляет 0.96. 
Т.о. в 96% случаев данный предиктор не является статистически значимым в данной модели.



```r
# residual normality test
ols_plot_resid_qq(lm_km2) # residual QQ plot
```

![](Multifactorial_analysis_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
ols_test_normality(lm_km2) # residual normality test
```

```
## -----------------------------------------------
##        Test             Statistic       pvalue  
## -----------------------------------------------
## Shapiro-Wilk              0.9902         0.6823 
## Kolmogorov-Smirnov        0.0568         0.9038 
## Cramer-von Mises          7.9154         0.0000 
## Anderson-Darling          0.3397         0.4922 
## -----------------------------------------------
```

```r
ols_plot_resid_hist(lm_km2) # residual histogram
```

![](Multifactorial_analysis_files/figure-html/unnamed-chunk-9-2.png)<!-- -->
Распределение остатков не отличается от нормального.

Коэффициент детерминации R² данной модели составил 0.2133, adjusted R² 0.197. Это значит, что 19.7% дисперсии в популяции объясняется уравнением линейной регрессии. По сравнению с предыдущей моделью коэффициент детерминации уменьшился, но не значимо.


## ** Задание 2**

Смоделируйте такую выборку объема 50 c 6 признаками, чтобы в модели линейной регрессии первого признака на остальные 5 каждый из коэффициентов регрессии в отдельности не значимо отличался от 0, но все параметры в совокупности — значимо. 



```r
set.seed(123)
x1 <- rnorm(50, 0, 0.1)

set.seed(234)
x2 <- rnorm(50, 0, 0.3)

set.seed(345)
x3 <- rnorm(50, 0, 0.25)

set.seed(456)
x4 <- rnorm(50, 0, 0.1)

set.seed(567)
x5 <- rnorm(50, 0, 0.2)

y <- x1 + x2 +x3 + x4 + x5 + rnorm(50, 0, 1)
```



```r
lm_km5 <- lm(y~x1 + x2 + x3 + x4 + x5) #Create the linear regression

summary(lm_km5) %>%
  tidy() %>%
  mutate_if(is.numeric, round, digits = 3) %>%
  kable() %>%
  kableExtra::kable_styling(full_width = FALSE, position = "left")
```

<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -0.063 </td>
   <td style="text-align:right;"> 0.168 </td>
   <td style="text-align:right;"> -0.377 </td>
   <td style="text-align:right;"> 0.708 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> x1 </td>
   <td style="text-align:right;"> -1.032 </td>
   <td style="text-align:right;"> 1.910 </td>
   <td style="text-align:right;"> -0.540 </td>
   <td style="text-align:right;"> 0.592 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> x2 </td>
   <td style="text-align:right;"> 1.514 </td>
   <td style="text-align:right;"> 0.607 </td>
   <td style="text-align:right;"> 2.492 </td>
   <td style="text-align:right;"> 0.017 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> x3 </td>
   <td style="text-align:right;"> 0.748 </td>
   <td style="text-align:right;"> 0.722 </td>
   <td style="text-align:right;"> 1.036 </td>
   <td style="text-align:right;"> 0.306 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> x4 </td>
   <td style="text-align:right;"> -0.504 </td>
   <td style="text-align:right;"> 1.604 </td>
   <td style="text-align:right;"> -0.314 </td>
   <td style="text-align:right;"> 0.755 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> x5 </td>
   <td style="text-align:right;"> 0.063 </td>
   <td style="text-align:right;"> 0.948 </td>
   <td style="text-align:right;"> 0.067 </td>
   <td style="text-align:right;"> 0.947 </td>
  </tr>
</tbody>
</table>


Воспользуйтесь пакетом glmnet, чтобы построить для такой модели lasso-оценку c параметром r = 0.2. Как изменились при этом оценки коэффициентов линейной регрессии? Придумайте из своей практики набор сильно зависимых показателей, которые могут быть связаны регрессионным соотношением.



```r
set.seed(123)

xvars <- matrix(c(x1, x2, x3, x4, x5), ncol = 5)

la.eq <- glmnet(xvars, y, family = 'gaussian', alpha = 1)

matplot(log(la.eq$lambda), t(la.eq$beta), type = 'l', main='Lasso', lwd=2)
```

![](Multifactorial_analysis_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

```r
la.eq <- glmnet(xvars,y, family = 'gaussian', alpha = 1, lambda = 0.2)
predict(la.eq, xvars, type = "coefficients")
```

```
## 6 x 1 sparse Matrix of class "dgCMatrix"
##                      s0
## (Intercept) -0.08971878
## V1           .         
## V2           0.66276797
## V3           .         
## V4           .         
## V5           .
```

При лассо-оценке с параметром lambda = 0.2 оценки коэффициентов регрессии изменились: первый, третий, четвертый и пятый обнулились, второй уменьшился в 2,3 раза и составил 0,66. 


## ** Задание 3**
У пациента, попавшего в реанимацию с риском сепсиса, измеряют уровень нейтрофилов (Neu) и лимфоцитов (Ly). При определенном заболевании уровень лимфоцитов является нормальной случайной величиной с распределением N(20; 5), а уровень нейтрофилов — N(80; 5). 
Пусть при нейтрофильно-лимфоцитарном соотношении NLR < 3 угроза сепсиса отсутствует, NLR > 9 - сепсис неизбежен, при остальных значениях сепсис возникает с вероятностью p = (NLR−3) / 6.

Смоделируйте трёхмерную выборку S объема 201, состоящую из наблюдений за (Neu, Ly, Sepsis). 
Воспользуйтесь пакетом glmnet и функцией glm, чтобы построить модель логистической регрессии признака Sepsis на Neu и Ly. 
Какая вероятность для случайной величины Sepsis быть равной 1, если Neu = 90, а Ly = 15? 
Какую вероятность предсказывает ваша модель (воспользуйтесь функцией predict)?


```r
# generate a sample of Neu with normal distribution N(80; 5)

set.seed(100)

#generate a sample with normal distribution of 201 obs. that follows normal dist. with mean=80 and sd=5

Neu <- round(rnorm(201,80,5),2)

# trasform to data frame
Neu <- as.data.frame(Neu)
```


```r
# generate a sample of Lym with normal distribution N(20; 5)

set.seed(10)

#generate sample of 201 obs. that follows normal dist. with mean=20 and sd=3

Lym <- round(rnorm(201,20,5),2)

# trasform to data frame
Lym <- as.data.frame(Lym)
```


```r
# merge Lym und Neu to a new data frame and transform to tibble
S <-cbind(Lym, Neu)
```


```r
# create a new column for NLR
S$NLR <- S$Neu / S$Lym
```


```r
# create a new column for sepsis probability

S <- S %>% 
  mutate(p = (NLR - 3) / 6)
```


```r
# create a new column for sepsis with a condition
S$Sepsis <- ifelse(S$NLR < 3, 0,
                  ifelse(S$NLR > 9, 1,
                         S$p))
```

Воспользуйтесь пакетом glmnet и функцией glm, чтобы построить модель логистической регрессии признака Sepsis на Neu и Ly.


```r
glm <- glm(Sepsis ~ Lym + Neu, family=binomial, data=S)
```

```
## Warning in eval(family$initialize): non-integer #successes in a binomial glm!
```

```r
summary (glm)
```

```
## 
## Call:
## glm(formula = Sepsis ~ Lym + Neu, family = binomial, data = S)
## 
## Deviance Residuals: 
##      Min        1Q    Median        3Q       Max  
## -0.24790  -0.06329   0.00161   0.05276   0.64107  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept) -1.35725    3.45384  -0.393    0.694    
## Lym         -0.29024    0.05014  -5.789 7.08e-09 ***
## Neu          0.06730    0.04291   1.568    0.117    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 52.4809  on 200  degrees of freedom
## Residual deviance:  2.3517  on 198  degrees of freedom
## AIC: 103.42
## 
## Number of Fisher Scoring iterations: 5
```


Уравнение логистической регрессии выглядит следующим образом:
Sepsis = -1.36 - 0.29 * Lym + 0.07 * Neu

Какая вероятность для случайной величины Sepsis быть равной 1, если Neu = 90, а Ly = 15? 
Какую вероятность предсказывает ваша модель (воспользуйтесь функцией predict)?


```r
#define new observation
newdata = data.frame(Neu=90, Lym = 15)

#use model to predict value of Sepsis
predict(glm, newdata, type="response")
```

```
##         1 
## 0.5856832
```
Т.о. при Neu = 90 и Lym = 15  вероятность развития сепсиса составляет 58,6%.
