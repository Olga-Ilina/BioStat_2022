---
title: "Mult_Comp"
author: "Olga"
date: "2022-11-17"
output: 
  html_document:
    keep_md: true
    
---




```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```




```
##      Name                 Position       Nationality      Age      
##  Length:150         Defender  :55   Argentinian:15   Min.   :18.0  
##  Class :character   Forward   :30   English    :27   1st Qu.:23.0  
##  Mode  :character   Goalkeeper:21   German     :25   Median :26.0  
##                     Midfielder:44   Italian    :33   Mean   :26.5  
##                                     Spanish    :50   3rd Qu.:30.0  
##                                                      Max.   :38.0  
##                                                      NA's   :4     
##      Height     
##  Min.   :165.0  
##  1st Qu.:179.0  
##  Median :183.0  
##  Mean   :182.4  
##  3rd Qu.:186.8  
##  Max.   :201.0  
## 
```


# Есть ли разница между средним ростом футболистов, играющих на разных позициях?


![](HW_Multcomp_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
Исходя из графика box plot, есть разница между ростом футболистов на разных позициях. Далее проверим, в каких случаях она статистически значима.








# Постройте доверительные интервалы для попарных разниц между средними (без поправок и с поправкой Бонферрони). 
Проведем вручную попарные сравнения 6 групп без коррекции conf.level (alpha = 0.05) и с поправкой Бонферрони.
При сравнении 6 групп попарно conf.level составит (1 - 0.05) / 6 = 0.992


```
## 
## 	Welch Two Sample t-test
## 
## data:  Def$Height and Mid$Height
## t = 3.8878, df = 86.443, p-value = 0.0001979
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  2.061467 6.374897
## sample estimates:
## mean of x mean of y 
##  183.2182  179.0000
```

```
## [1] 2.06 6.37
## attr(,"conf.level")
## [1] 0.95
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  Def$Height and Mid$Height
## t = 3.8878, df = 86.443, p-value = 0.0001979
## alternative hypothesis: true difference in means is not equal to 0
## 99.2 percent confidence interval:
##  1.272400 7.163963
## sample estimates:
## mean of x mean of y 
##  183.2182  179.0000
```

```
## [1] 1.27 7.16
## attr(,"conf.level")
## [1] 0.992
```




```
## 
## 	Welch Two Sample t-test
## 
## data:  Def$Height and Forw$Height
## t = 1.0694, df = 41.576, p-value = 0.2911
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -1.525215  4.961579
## sample estimates:
## mean of x mean of y 
##  183.2182  181.5000
```

```
## [1] -1.53  4.96
## attr(,"conf.level")
## [1] 0.95
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  Def$Height and Forw$Height
## t = 1.0694, df = 41.576, p-value = 0.2911
## alternative hypothesis: true difference in means is not equal to 0
## 99.2 percent confidence interval:
##  -2.758334  6.194698
## sample estimates:
## mean of x mean of y 
##  183.2182  181.5000
```

```
## [1] -2.76  6.19
## attr(,"conf.level")
## [1] 0.992
```



```
## 
## 	Welch Two Sample t-test
## 
## data:  Def$Height and Goal$Height
## t = -4.0594, df = 35.624, p-value = 0.0002569
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -7.885903 -2.630114
## sample estimates:
## mean of x mean of y 
##  183.2182  188.4762
```

```
## [1] -7.89 -2.63
## attr(,"conf.level")
## [1] 0.95
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  Def$Height and Goal$Height
## t = -4.0594, df = 35.624, p-value = 0.0002569
## alternative hypothesis: true difference in means is not equal to 0
## 99.2 percent confidence interval:
##  -8.897478 -1.618540
## sample estimates:
## mean of x mean of y 
##  183.2182  188.4762
```

```
## [1] -8.90 -1.62
## attr(,"conf.level")
## [1] 0.992
```


```
## 
## 	Welch Two Sample t-test
## 
## data:  Mid$Height and Forw$Height
## t = -1.4791, df = 48.351, p-value = 0.1456
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -5.8976773  0.8976773
## sample estimates:
## mean of x mean of y 
##     179.0     181.5
```

```
## [1] -5.9  0.9
## attr(,"conf.level")
## [1] 0.95
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  Mid$Height and Forw$Height
## t = -1.4791, df = 48.351, p-value = 0.1456
## alternative hypothesis: true difference in means is not equal to 0
## 99.2 percent confidence interval:
##  -7.176105  2.176105
## sample estimates:
## mean of x mean of y 
##     179.0     181.5
```

```
## [1] -7.18  2.18
## attr(,"conf.level")
## [1] 0.992
```


```
## 
## 	Welch Two Sample t-test
## 
## data:  Mid$Height and Goal$Height
## t = -6.7809, df = 43.584, p-value = 2.508e-08
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -12.293402  -6.658979
## sample estimates:
## mean of x mean of y 
##  179.0000  188.4762
```

```
## [1] -12.29  -6.66
## attr(,"conf.level")
## [1] 0.95
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  Mid$Height and Goal$Height
## t = -6.7809, df = 43.584, p-value = 2.508e-08
## alternative hypothesis: true difference in means is not equal to 0
## 99.2 percent confidence interval:
##  -13.360836  -5.591545
## sample estimates:
## mean of x mean of y 
##  179.0000  188.4762
```

```
## [1] -13.36  -5.59
## attr(,"conf.level")
## [1] 0.992
```


```
## 
## 	Welch Two Sample t-test
## 
## data:  Forw$Height and Goal$Height
## t = -3.8074, df = 48.632, p-value = 0.000394
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -10.658982  -3.293399
## sample estimates:
## mean of x mean of y 
##  181.5000  188.4762
```

```
## [1] -10.66  -3.29
## attr(,"conf.level")
## [1] 0.95
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  Forw$Height and Goal$Height
## t = -3.8074, df = 48.632, p-value = 0.000394
## alternative hypothesis: true difference in means is not equal to 0
## 99.2 percent confidence interval:
##  -12.044179  -1.908202
## sample estimates:
## mean of x mean of y 
##  181.5000  188.4762
```

```
## [1] -12.04  -1.91
## attr(,"conf.level")
## [1] 0.992
```

По результатам попарных сравнений выявлены статистические значимые различия по росту между следующими группами футболистов: Defenders vs Midfielders, Defenders vs Goalkeepers, Midfielders vs Goalkeepers, Forwards vs Goalkeepers, во всех случаях p-value <<0.001.




# Покрывают ли интервалы реальную разницу между средним ростом? 




```
## [1] 3.318329
```

```
## [1] -5.256514
```

```
## [1] 4.083288
```

```
## [1] -8.574843
```

```
## [1] -0.7649594
```

```
## [1] -9.339802
```

Вышеуказанные доверительные интервалы для разницы роста футболистов в выборке покрывают реальную разницу между средним ростом футболистов в ГС

# Проведите попарные тесты для разниц между средними (без поправок, с поправкой Холма и поправкой Бенджамини-Хохберга). 


```
## 
## 	Pairwise comparisons using t tests with non-pooled SD 
## 
## data:  soccer_wrk$Height and soccer_wrk$Position 
## 
##            Defender Forward Goalkeeper
## Forward    0.29106  -       -         
## Goalkeeper 0.00026  0.00039 -         
## Midfielder 0.00020  0.14559 2.5e-08   
## 
## P value adjustment method: none
```


```
## 
## 	Pairwise comparisons using t tests with non-pooled SD 
## 
## data:  soccer_wrk$Height and soccer_wrk$Position 
## 
##            Defender Forward Goalkeeper
## Forward    1.0000   -       -         
## Goalkeeper 0.0015   0.0024  -         
## Midfielder 0.0012   0.8735  1.5e-07   
## 
## P value adjustment method: bonferroni
```


```
## 
## 	Pairwise comparisons using t tests with non-pooled SD 
## 
## data:  soccer_wrk$Height and soccer_wrk$Position 
## 
##            Defender Forward Goalkeeper
## Forward    0.29118  -       -         
## Goalkeeper 0.00103  0.00118 -         
## Midfielder 0.00099  0.29118 1.5e-07   
## 
## P value adjustment method: holm
```



```
## 
## 	Pairwise comparisons using t tests with non-pooled SD 
## 
## data:  soccer_wrk$Height and soccer_wrk$Position 
## 
##            Defender Forward Goalkeeper
## Forward    0.29106  -       -         
## Goalkeeper 0.00051  0.00059 -         
## Midfielder 0.00051  0.17471 1.5e-07   
## 
## P value adjustment method: BH
```

Всего 4 открытия. Ложных открытий нет.

