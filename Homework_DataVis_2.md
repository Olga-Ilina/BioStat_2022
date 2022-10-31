    devtools::install_github("ropensci/plotly")

    ## Downloading GitHub repo ropensci/plotly@HEAD

    ## 
    ##      checking for file ‘/private/var/folders/md/d3tjc99n06z74xxpmw6c9_y40000gn/T/RtmpQRtR5x/remotes179733ae5de57/plotly-plotly.R-2db78b7/DESCRIPTION’ ...  ✔  checking for file ‘/private/var/folders/md/d3tjc99n06z74xxpmw6c9_y40000gn/T/RtmpQRtR5x/remotes179733ae5de57/plotly-plotly.R-2db78b7/DESCRIPTION’
    ##   ─  preparing ‘plotly’:
    ##    checking DESCRIPTION meta-information ...  ✔  checking DESCRIPTION meta-information
    ##   ─  checking for LF line-endings in source and make files and shell scripts
    ##   ─  checking for empty or unneeded directories
    ##   ─  building ‘plotly_4.10.0.9001.tar.gz’
    ##      
    ## 

    library(plotly)

    ## Loading required package: ggplot2

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(magrittr)
    library(tidyr)

    ## 
    ## Attaching package: 'tidyr'

    ## The following object is masked from 'package:magrittr':
    ## 
    ##     extract

    library(ggplot2)
    library(ggpubr)
    library(skimr)
    library(tibble)
    install.packages (c('dplyr', 'magrittr', 'tidyr', 'ggplot2', 'ggpubr', "skimr", "plotly", "tibble", 'fastDummies', "pheatmap", "FactoMineR",
    "ggbiplot"), repos = "http://cran.us.r-project.org")

    ## Warning: package 'ggbiplot' is not available for this version of R
    ## 
    ## A version of this package for your version of R might be available elsewhere,
    ## see the ideas at
    ## https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/md/d3tjc99n06z74xxpmw6c9_y40000gn/T//RtmpQRtR5x/downloaded_packages

# 1. Загрузите датасет insurance\_cost.csv.

    df <-read.csv ("insurance_cost.csv", stringsAsFactors = TRUE)

    summary(df)

    ##       age            sex           bmi           children     smoker    
    ##  Min.   :18.00   female:662   Min.   :15.96   Min.   :0.000   no :1064  
    ##  1st Qu.:27.00   male  :676   1st Qu.:26.30   1st Qu.:0.000   yes: 274  
    ##  Median :39.00                Median :30.40   Median :1.000             
    ##  Mean   :39.21                Mean   :30.66   Mean   :1.095             
    ##  3rd Qu.:51.00                3rd Qu.:34.69   3rd Qu.:2.000             
    ##  Max.   :64.00                Max.   :53.13   Max.   :5.000             
    ##        region       charges     
    ##  northeast:324   Min.   : 1122  
    ##  northwest:325   1st Qu.: 4740  
    ##  southeast:364   Median : 9382  
    ##  southwest:325   Mean   :13270  
    ##                  3rd Qu.:16640  
    ##                  Max.   :63770

    sum(is.na(df))

    ## [1] 0

# 2. Сделайте интерактивный plotly график отношения индекса массы тела и трат на страховку. Раскрасьте его по колонке smoker

Plotly не всегда корректно ведёт себя во время knit – для него нужно
настраивать .Rmd документ. Если вы столкнулись с тем, что у вас
не-“нитится” из-за plotly – просто отмените выполнение чанка при
сохранении кода в его настройках (eval=FALSE)

    pal <- c("#CC99FF", "#99CCFF")
    pal <- setNames(pal, levels(df$smoker))

    plot_ly(
      data = df, 
      x = ~bmi, 
      y = ~charges, 
      color = ~smoker,# Развивка по цвету по smoker
      text = ~charges,
      size = ~charges,
      hoverinfo = "text",
      colors = pal, alpha = 0.7, #Новая палетка и прозрачность
      marker = list(
        line = list(color = '#999999',  # Цвет окружности
                    width = 1))) %>%
        layout(
        title = 'Отношение индекса массы тела и трат на страховку в зависимости от курения',
        yaxis = list(title = 'Индекс массы тела',
                     zeroline = FALSE), 
        xaxis = list(title = 'Траты на страховку',
                     zeroline = FALSE)) 

\#3.Сделайте тоже самое через ggplotly Plotly не всегда корректно ведёт
себя во время knit – для него нужно настраивать .Rmd документ. Если вы
столкнулись с тем, что у вас не-“нитится” из-за plotly – просто отмените
выполнение чанка при сохранении кода в его настройках (eval=FALSE)

    plot <- df %>% 
      ggplot(aes(x=bmi, y=charges)) + 
      geom_point(color='#999999', shape=21, size=4, alpha = 0.7,
                 aes(fill=factor(smoker))) + 
      scale_fill_manual(values=c('#CC99FF', '#99CCFF')) +
      xlab("BMI") + 
      ylab("Charges") +
      theme_light()+
      theme(plot.title = element_text(size = 14, hjust = 0.5),
            axis.text.x = element_text(size = 14))

    ggplotly(plot)

\#4.Кратко сделайте корреляционный анализ данных insurance\_cost.
Посмотрите документацию пакетов, которые мы проходили на занятии и,
исходя из этого,постройте минимум два новых типа графика (которые мы не
строили на занятии).

    library(corrplot)

    ## corrplot 0.92 loaded

    # Удаляем ошибочные значения
    df_clear <- df %>% 
      filter(children != 0 & charges != 0) %>% 
      select(is.integer | is.numeric) 

    ## Warning: Use of bare predicate functions was deprecated in tidyselect 1.1.0.
    ## ℹ Please use wrap predicates in `where()` instead.
    ##   # Was:
    ##   data %>% select(is.integer)
    ## 
    ##   # Now:
    ##   data %>% select(where(is.integer))

    ## Warning: Use of bare predicate functions was deprecated in tidyselect 1.1.0.
    ## ℹ Please use wrap predicates in `where()` instead.
    ##   # Was:
    ##   data %>% select(is.numeric)
    ## 
    ##   # Now:
    ##   data %>% select(where(is.numeric))

    head(df_clear)

    ##   age children   bmi  charges
    ## 1  18        1 33.77 1725.552
    ## 2  28        3 33.00 4449.462
    ## 3  46        1 33.44 8240.590
    ## 4  37        3 27.74 7281.506
    ## 5  37        2 29.83 6406.411
    ## 6  19        1 24.60 1837.237

    df_cor <- cor(df_clear)
    df_cor

    ##                 age     children          bmi    charges
    ## age      1.00000000 0.0136491484 0.1310212912 0.27606861
    ## children 0.01364915 1.0000000000 0.0001989708 0.03642234
    ## bmi      0.13102129 0.0001989708 1.0000000000 0.21441658
    ## charges  0.27606861 0.0364223400 0.2144165797 1.00000000

    corrplot(df_cor, method = 'circle', order = 'alphabet')

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-9-1.png)

    corrplot(df_cor, method = 'pie')

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    corrplot(df_cor, method = 'square', order = 'FPC', type = 'lower', diag = FALSE)

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-11-1.png)

    corrplot(df_cor, method = 'square', diag = FALSE, order = 'hclust',
             addrect = 3, rect.col = 'blue', rect.lwd = 3, tl.pos = 'd')

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-12-1.png)

    # Наибольшая корреляция отмечается между charges и age, charges и bmi

    library(magrittr)


    df_aggr <- df %>% 
      group_by(smoker, sex) %>% 
      summarise(N = n())

    ## `summarise()` has grouped output by 'smoker'. You can override using the
    ## `.groups` argument.

    df_aggr %>% 
      ggplot(aes(x = smoker, y = sex, fill = N)) +
      geom_tile(color = "black") +
      geom_text(aes(label = N), color = "white", size = 4) +
      coord_fixed()

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-15-1.png)

\#5.Превратите все номинативные переменные в бинарные/дамми. Т.е. sex и
smoker должны стать бинарными (1/0), а каждое уникальное значение region
– отдельной колонкой, где 1 говорит о наличии этого признака для
наблюдения, а 0 – об отсутствии (В работе с данными эта операция
называется one hot-encoding). Создайте новый датафрейм, где вы оставите
только нумерические переменные.

    str(df)

    ## 'data.frame':    1338 obs. of  7 variables:
    ##  $ age     : int  19 18 28 33 32 31 46 37 37 60 ...
    ##  $ sex     : Factor w/ 2 levels "female","male": 1 2 2 2 2 1 1 1 2 1 ...
    ##  $ bmi     : num  27.9 33.8 33 22.7 28.9 ...
    ##  $ children: int  0 1 3 0 0 0 1 3 2 0 ...
    ##  $ smoker  : Factor w/ 2 levels "no","yes": 2 1 1 1 1 1 1 1 1 1 ...
    ##  $ region  : Factor w/ 4 levels "northeast","northwest",..: 4 3 3 2 2 3 3 2 1 2 ...
    ##  $ charges : num  16885 1726 4449 21984 3867 ...

    # создадим dummy variables
    # из переменых sex и smoker с помощью функции mutate (case_when), из переменной region с помощью функции dummy_cols

    library('fastDummies')

    df_numeric <- df %>%
    mutate(sex_cat = case_when(
      sex == 'female' ~0,
      sex == 'male' ~ 1
    )) %>%
    mutate(smoker_cat = case_when(
      smoker == 'no' ~ 0,
      smoker == 'yes' ~ 1
    )) %>%
     dummy_cols(select_columns = 'region') %>%
      select(is.integer | is.numeric)

    head(df_numeric)

    ##   age children region_northeast region_northwest region_southeast
    ## 1  19        0                0                0                0
    ## 2  18        1                0                0                1
    ## 3  28        3                0                0                1
    ## 4  33        0                0                1                0
    ## 5  32        0                0                1                0
    ## 6  31        0                0                0                1
    ##   region_southwest    bmi   charges sex_cat smoker_cat
    ## 1                1 27.900 16884.924       0          1
    ## 2                0 33.770  1725.552       1          0
    ## 3                0 33.000  4449.462       1          0
    ## 4                0 22.705 21984.471       1          0
    ## 5                0 28.880  3866.855       1          0
    ## 6                0 25.740  3756.622       0          0

\#6.Постройте иерархическую кластеризацию на этом датафрейме

    library(factoextra)

    ## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

    library(magrittr)
    library(dplyr)

    # Получаем объект матрицы. Избавляемся от ошибочных значений
    new_df_clear <- df_numeric %>% 
      filter(age != 0 & bmi != 0 & charges != 0) 

    head(new_df_clear)

    ##   age children region_northeast region_northwest region_southeast
    ## 1  19        0                0                0                0
    ## 2  18        1                0                0                1
    ## 3  28        3                0                0                1
    ## 4  33        0                0                1                0
    ## 5  32        0                0                1                0
    ## 6  31        0                0                0                1
    ##   region_southwest    bmi   charges sex_cat smoker_cat
    ## 1                1 27.900 16884.924       0          1
    ## 2                0 33.770  1725.552       1          0
    ## 3                0 33.000  4449.462       1          0
    ## 4                0 22.705 21984.471       1          0
    ## 5                0 28.880  3866.855       1          0
    ## 6                0 25.740  3756.622       0          0

    # Стандартизуем значения
    new_df_clear_scaled <- scale(new_df_clear)

    # Создаём матрицу дистанций по методу Euclidian
    new_df_clear_dist <- dist(new_df_clear_scaled, method = "euclidean")
    as.matrix(new_df_clear_dist)[1:8,1:8]

    ##          1        2        3        4        5        6        7        8
    ## 1 0.000000 4.878506 5.382420 4.785072 4.799844 4.318076 4.725628 5.046541
    ## 2 4.878506 0.000000 1.823634 4.289327 3.582563 2.702546 2.874267 4.499604
    ## 3 5.382420 1.823634 0.000000 4.663256 4.148789 3.414209 2.914534 3.960545
    ## 4 4.785072 4.289327 4.663256 0.000000 1.807952 4.124373 4.517528 3.525260
    ## 5 4.799844 3.582563 4.148789 1.807952 0.000000 3.840206 4.104574 3.229818
    ## 6 4.318076 2.702546 3.414209 4.124373 3.840206 0.000000 1.886631 4.128836
    ## 7 4.725628 2.874267 2.914534 4.517528 4.104574 1.886631 0.000000 3.810807
    ## 8 5.046541 4.499604 3.960545 3.525260 3.229818 4.128836 3.810807 0.000000

    # Создаем дендрограмму кластера
    new_df_clear_hc <- hclust(d = new_df_clear_dist, 
                            method = "ward.D2")

    # Визуализируем
    fviz_dend(new_df_clear_hc, 
              cex = 0.1, # cex() - размер лейблов
              main = "Dendrogram", # Название
              xlab = "Objects",
              ylab = "Distance")

    ## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
    ## "none")` instead.

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-23-1.png)

    # Verify the cluster tree
    new_df_clear_coph <-cophenetic(new_df_clear_hc) # compute cophenetic distance
    cor (new_df_clear_dist, new_df_clear_coph ) # correlation between cophenetic distance and the original distance. Correlation coefficient 0.729 for a strong correlantion

    ## [1] 0.7244742

\#7.(это задание засчитывается за два) Используя документацию или
предложенный учебник (С. 64-117)сделайте ещё несколько возможных
графиков по иерархической кластеризации. Попробуйте раскрасить кластеры
разными цветами (с. 75)

    fviz_dend(new_df_clear_hc, k = 5,
              cex = 0.5,
              k_colors = "jco",
              color_labels_by_k = TRUE, 
              rect = TRUE, 
              rect_border = "jco",
              rect_fill = TRUE)

    ## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
    ## "none")` instead.

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-25-1.png)

    df_grp <- cutree(new_df_clear_hc, k = 4)

    table (df_grp)

    ## df_grp
    ##   1   2   3   4 
    ## 274 273 524 267

    fviz_cluster(list(data = new_df_clear_dist, cluster = df_grp),
      ellipse.type = "convex", 
      repel = TRUE,
      show.clust.cent = FALSE, 
      ggtheme = theme_minimal())

    ## Warning: ggrepel: 1315 unlabeled data points (too many overlaps). Consider
    ## increasing max.overlaps

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-26-1.png)

    # Если взять другой метод вычисления дистанций  - например, manhattan

    # Создаём матрицу дистанций по методу Manhattan
    df_dist_m <- dist(new_df_clear_scaled, method = "manhattan")
    as.matrix(df_dist_m)[1:8,1:8]

    ##           1         2         3         4         5        6         7
    ## 1  0.000000 12.168888 14.046160 11.407978 11.299497 9.346815 11.427963
    ## 2 12.168888  0.000000  2.722016  9.961841  7.381985 5.238671  4.584352
    ## 3 14.046160  2.722016  0.000000 10.557971  8.074333 5.949225  5.324787
    ## 4 11.407978  9.961841 10.557971  0.000000  2.579856 8.721894 11.226752
    ## 5 11.299497  7.381985  8.074333  2.579856  0.000000 7.171852  9.511578
    ## 6  9.346815  5.238671  5.949225  8.721894  7.171852 0.000000  3.530095
    ## 7 11.427963  4.584352  5.324787 11.226752  9.511578 3.530095  0.000000
    ## 8 11.728182 11.035666  8.313650  6.812441  5.312754 8.112000  7.890851
    ##           8
    ## 1 11.728182
    ## 2 11.035666
    ## 3  8.313650
    ## 4  6.812441
    ## 5  5.312754
    ## 6  8.112000
    ## 7  7.890851
    ## 8  0.000000

    # Создаем дендрограмму кластера с помощью метода "median"
    df_hc_complete <- hclust(d = new_df_clear_dist, 
                            method = "median")

    df_grp_2 <- cutree(df_hc_complete, k = 4)

    table(df_grp_2)

    ## df_grp_2
    ##    1    2    3    4 
    ## 1330    6    1    1

    fviz_cluster(list(data = df_dist_m, cluster = df_grp_2),
      ellipse.type = "convex", 
      repel = TRUE,
      show.clust.cent = FALSE, ggtheme = theme_minimal())

    ## Warning: ggrepel: 1320 unlabeled data points (too many overlaps). Consider
    ## increasing max.overlaps

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-27-1.png)

    # Результаты кластерного анализа будут отличаться при использовании разных методов определения дистанций и постооения дендрограммы 

    # Из первого датафрейма создадим датафрейм из наиболее коррелирующих переменных: age, charges, bmi

    df_new <- df_numeric %>% 
      select('charges', 'age', 'bmi')

    df_new_clear <- df_new %>% 
      filter(age != 0 & bmi != 0 & charges != 0) 

    df_new_clear_scaled <- scale(df_new_clear)

    df_new_clear_dist <- dist(df_new_clear_scaled, method = "euclidean")
    as.matrix(new_df_clear_dist)[1:8,1:8]

    ##          1        2        3        4        5        6        7        8
    ## 1 0.000000 4.878506 5.382420 4.785072 4.799844 4.318076 4.725628 5.046541
    ## 2 4.878506 0.000000 1.823634 4.289327 3.582563 2.702546 2.874267 4.499604
    ## 3 5.382420 1.823634 0.000000 4.663256 4.148789 3.414209 2.914534 3.960545
    ## 4 4.785072 4.289327 4.663256 0.000000 1.807952 4.124373 4.517528 3.525260
    ## 5 4.799844 3.582563 4.148789 1.807952 0.000000 3.840206 4.104574 3.229818
    ## 6 4.318076 2.702546 3.414209 4.124373 3.840206 0.000000 1.886631 4.128836
    ## 7 4.725628 2.874267 2.914534 4.517528 4.104574 1.886631 0.000000 3.810807
    ## 8 5.046541 4.499604 3.960545 3.525260 3.229818 4.128836 3.810807 0.000000

    df_new_clear_hc <- hclust(d = df_new_clear_dist, 
                            method = "ward.D2")

    fviz_dend(new_df_clear_hc, k = 5, 
      cex = 0.1,
      horiz = TRUE,
      k_colors = "jco",
              color_labels_by_k = TRUE, 
              rect = TRUE, 
              rect_border = "jco",
              rect_fill = TRUE,
              ggtheme = theme_gray())

    ## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
    ## "none")` instead.

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-28-1.png)

    # Zooming the dendrogram
    fviz_dend(df_new_clear_hc, xlim = c(1, 200), ylim = c(1, 20))

    ## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
    ## "none")` instead.

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-29-1.png)

\#Circular dendrogram

    fviz_dend(df_new_clear_hc, cex = 0.5, k = 4,
              k_colors = 'jco', type = "circular" )

    ## Warning: `guides(<scale> = FALSE)` is deprecated. Please use `guides(<scale> =
    ## "none")` instead.

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-30-1.png)

\#8. Сделайте одновременный график heatmap и иерархической кластеризации

    library(pheatmap)

    pheatmap(new_df_clear_scaled)

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-32-1.png)

\#9.Проведите анализ данных полученных в задании 5 методом PCA. Кратко
проинтерпретируйте полученные результаты.

    library(FactoMineR)
    library(ggbiplot)

    ## Loading required package: plyr

    ## ------------------------------------------------------------------------------

    ## You have loaded plyr after dplyr - this is likely to cause problems.
    ## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
    ## library(plyr); library(dplyr)

    ## ------------------------------------------------------------------------------

    ## 
    ## Attaching package: 'plyr'

    ## The following object is masked from 'package:ggpubr':
    ## 
    ##     mutate

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     arrange, count, desc, failwith, id, mutate, rename, summarise,
    ##     summarize

    ## The following objects are masked from 'package:plotly':
    ## 
    ##     arrange, mutate, rename, summarise

    ## Loading required package: scales

    ## Loading required package: grid

    df_pca <- prcomp(df_numeric,
                            scale = T)
    summary(df_pca)

    ## Importance of components:
    ##                           PC1    PC2    PC3    PC4    PC5    PC6    PC7     PC8
    ## Standard deviation     1.3939 1.2182 1.1510 1.1496 1.0403 1.0018 0.9767 0.86822
    ## Proportion of Variance 0.1943 0.1484 0.1325 0.1321 0.1082 0.1004 0.0954 0.07538
    ## Cumulative Proportion  0.1943 0.3427 0.4752 0.6073 0.7156 0.8159 0.9113 0.98669
    ##                            PC9      PC10
    ## Standard deviation     0.36478 1.139e-15
    ## Proportion of Variance 0.01331 0.000e+00
    ## Cumulative Proportion  1.00000 1.000e+00

    fviz_eig(df_pca, 
             addlabels = T, 
             ylim = c(0, 40))

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-34-1.png)

    # Первые 4 компоненты объясняют 60.6% вариации данных. 7 первых компонент объясняют 90.9% вариации данных

    fviz_pca_var(df_pca, col.var = "contrib")

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-35-1.png)

    #На графике выделяются 3 группы переменных: 1) region_southeast, bmi; 2) charges, smokers_cat; age; 3) остальные. Отрицательно скоррелированы region_southeast, bmi и region_nothwest, region_northwest

    fviz_pca_var(df_pca, 
                 select.var = list(contrib = 5), # Число компонент  
                 col.var = "contrib")

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-36-1.png)

    fviz_contrib(df_pca, choice = "var", axes = 1, top = 24) 

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-37-1.png)

    # 1 компонента - charges & smoker

    fviz_contrib(df_pca, choice = "var", axes = 2, top = 24) 

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-38-1.png)

    # 2 компонента - region southeast, region northeast и bmi

    fviz_contrib(df_pca, choice = "var", axes = 3, top = 24) 

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-39-1.png)

    # 3 компонента - region southwest

    fviz_contrib(df_pca, choice = "var", axes = 4, top = 24) 

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-40-1.png)

    # 4 компонента - region norhtwest & region northeast

    fviz_contrib(df_pca, choice = "var", axes = 5, top = 24) 

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-41-1.png)

    # 5 компонента - age

    ggbiplot(df_pca, 
             scale=0, alpha = 0.1) + 
      theme_minimal()

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-42-1.png)

    #PC1 объясняет 19.4% вариации, PC2 14.8%

    #PC1 объясняет 19.4% дисперсии. В PC1 входят charges и smoker. PC2 объясняет 14.8% дисперсии. В PC2 входят region и bmi.

\#10.В финале вы получите график PCA по наблюдениям и переменным.
Сделайте кластеризацию (В значении кода на строке 909 файла
R\_pro\_work\_with\_graphics.Rmd) данных на нём по возрастным группам
(создайте их сами на ваш вкус, но их количество должно быть не меньше
3).

    # Создадим категориальную переменную по возрасту 
    df_2 <-df_numeric %>%
      mutate(age_group = case_when(age < 40 ~ "1",
                                   age >= 40 & age < 60 ~ "2",
                                   age >= 60 ~ "3")) 

    ggbiplot(df_pca, 
             scale=0, 
             groups = as.factor(df_2$age_group), 
             ellipse = T,
             alpha = 0.2) +
      theme_minimal()

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-44-1.png)

\#11. Подумайте и создайте ещё две номинативные переменные, которые бы
гипотетически могли хорошо разбить данные на кластеры. Сделайте две
соответствующие визуализации.

    #Создадим переменную Obesity

    df_2 <-df_2 %>%
      mutate(bmi_group = case_when(bmi < 35 ~ "0",
                                   bmi >= 35 ~ "1")) 

    df_2$bmi_group <- factor(df_2$bmi_group, labels = c("No obesity", "Obesity"))

    ggbiplot(df_pca, 
             scale=0, 
             groups = df_2$bmi_group, 
             ellipse = T,
             alpha = 0.2) +
      theme_minimal()

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-45-1.png)

    #Создадим переменную Charges 
    df_2 <-df_2 %>%
      mutate(charges_group = case_when(charges< 20000 ~ "0",
                                   charges >= 20000 ~ "1"))
    df_2$charges_group <- factor(df_2$charges_group, labels = c("< 20000", ">= 20000"))

    ggbiplot(df_pca, 
             scale=0, 
             groups = df_2$charges_group, 
             ellipse = T,
             alpha = 0.2) +
      theme_minimal()

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-46-1.png)

\#12. (это задание засчитывается за три) Давайте самостоятельно увидим,
что снижение размерности – это группа методов, славящаяся своей
неустойчивостью. Попробуйте самостоятельно поизменять дафрейм – удалить
какие-либо переменные или создать их (создавайте только дамми
переменные). Ваша задача –резко поднять качество вашего анализа PCA (при
этом, фактически, оперируя всё теми же данными). Кратко опишите, почему
добавление той или иной дамми-переменной так улучшает PCA.

    str(df_numeric)

    ## 'data.frame':    1338 obs. of  10 variables:
    ##  $ age             : int  19 18 28 33 32 31 46 37 37 60 ...
    ##  $ children        : int  0 1 3 0 0 0 1 3 2 0 ...
    ##  $ region_northeast: int  0 0 0 0 0 0 0 0 1 0 ...
    ##  $ region_northwest: int  0 0 0 1 1 0 0 1 0 1 ...
    ##  $ region_southeast: int  0 1 1 0 0 1 1 0 0 0 ...
    ##  $ region_southwest: int  1 0 0 0 0 0 0 0 0 0 ...
    ##  $ bmi             : num  27.9 33.8 33 22.7 28.9 ...
    ##  $ charges         : num  16885 1726 4449 21984 3867 ...
    ##  $ sex_cat         : num  0 1 1 1 1 0 0 0 1 0 ...
    ##  $ smoker_cat      : num  1 0 0 0 0 0 0 0 0 0 ...

    # Дамми-переменные - region, sex, smoker. Удалим по очереди эти переменные
    # Удалим region

    df_pca_r <- df_numeric %>%
      select(-contains("region_")) %>%
      prcomp(
              scale = T) 
      
    df_pca1 <- fviz_eig(df_pca_r,
             addlabels = T, 
             ylim = c(0, 40))

    df_pca1

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-48-1.png)

    # Удалим smoker
    df_pca_sm <- df_numeric %>%
      select(-contains("smoker")) %>%
      prcomp(
              scale = T) 
      
    df_pca2 <- fviz_eig(df_pca_sm,
             addlabels = T, 
             ylim = c(0, 40))

    df_pca2

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-49-1.png)

    # Удалим sex
    df_pca_sex <- df_numeric %>%
      select(-contains("sex")) %>%
      prcomp(
              scale = T) 
      
    df_pca3 <- fviz_eig(df_pca_sex,
             addlabels = T, 
             ylim = c(0, 40))

    df_pca3

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-50-1.png)

    #При удалении переменной regions качество модели повышается. При этом выяввляется ведущая компонента, которая объясняет 31.4% дисперсии

    # Добавим дамми-переменные age_group и bmi_group после удаления переменной region

    #Удалим переменную region
    df_pca_region <- df_numeric %>%
      select(-contains("region_"))

    # Добавим дамми-переменную bmi_group
    df_pca4 <-df_pca_region %>%
      mutate(bmi_group = case_when(bmi < 35 ~ "0",
                                   bmi >= 35 ~ "1")) 

    df_pca4$bmi_group <- as.numeric(as.character(df_pca4$bmi_group))

    pca4 <- prcomp(df_pca4,
                            scale = T)

    fviz_eig(pca4, 
             addlabels = T, 
             ylim = c(0, 40))

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-52-1.png)

    # После удаления переменной region и добавления переменной bmi_group  качество модели повышается и выделяется ведущая компонента, которая объясняет 29.3% дисперсии

    # Добавим дамми-переменную age_group после удаления переменной region
    df_pca5 <-df_pca_region %>%
      mutate(age_group = case_when(age < 40 ~ "1",
                                   age >= 40 & age < 60 ~ "2",
                                   age >= 60 ~ "3"))

    df_pca5$age_group <- as.numeric(as.character(df_pca5$age_group))

    pca5 <- prcomp(df_pca5,
                            scale = T)

    fviz_eig(pca5, 
             addlabels = T, 
             ylim = c(0, 40))

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-54-1.png)

    # После удаления переменной region и добавления переменной age_group  качество модели резко повышается и выделяется ведущая компонента, которая объясняет 31.4% дисперсии

    # Добавим дамми-переменные age_group и bmi_group после удаления переменной region
    df_pca6 <-df_pca_region %>%
      mutate(age_group = case_when(age < 40 ~ "1",
                                   age >= 40 & age < 60 ~ "2",
                                   age >= 60 ~ "3")) %>%
      mutate(bmi_group = case_when(bmi < 35 ~ "0",
                                   bmi >= 35 ~ "1")) 

    df_pca6$age_group <- as.numeric(as.character(df_pca6$age_group))
    df_pca6$bmi_group <- as.numeric(as.character(df_pca6$bmi_group))

    pca6 <- prcomp(df_pca6,
                            scale = T)

    fviz_eig(pca6, 
             addlabels = T, 
             ylim = c(0, 40))

![](Homework_DataVis_2_files/figure-markdown_strict/unnamed-chunk-56-1.png)

    # При добавлении дамми-переменных age_group и bmi_group после удаления переменной region качество модели меняется незначительно по сравнению только с переменной bmi. Наилучшую модель мы получаем после удаления переменной region и добавлении переменной age_group.
