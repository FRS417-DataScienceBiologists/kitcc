---
title: "Lab 2 Homework"
author: "Kit Nga Chou"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ───────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Mammals Sleep
For this assignment, we are going to use built-in data on mammal sleep patterns.  

```r
msleep
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee… Acin… carni Carn… lc                  12.1      NA        NA    
##  2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA    
##  3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA    
##  4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667
##  6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767
##  7 Nort… Call… carni Carn… vu                   8.7       1.4       0.383
##  8 Vesp… Calo… <NA>  Rode… <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn… domesticated        10.1       2.9       0.333
## 10 Roe … Capr… herbi Arti… lc                   3        NA        NA    
## # … with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```

1. From which publication are these data taken from? Don't do an internet search; show the code that you would use to find out in R.

```r
?msleep
```



2. Provide some summary information about the data to get you started; feel free to use the functions that you find most helpful.

```r
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```

```r
glimpse(msleep)
```

```
## Observations: 83
## Variables: 11
## $ name         <chr> "Cheetah", "Owl monkey", "Mountain beaver", "Greate…
## $ genus        <chr> "Acinonyx", "Aotus", "Aplodontia", "Blarina", "Bos"…
## $ vore         <chr> "carni", "omni", "herbi", "omni", "herbi", "herbi",…
## $ order        <chr> "Carnivora", "Primates", "Rodentia", "Soricomorpha"…
## $ conservation <chr> "lc", NA, "nt", "lc", "domesticated", NA, "vu", NA,…
## $ sleep_total  <dbl> 12.1, 17.0, 14.4, 14.9, 4.0, 14.4, 8.7, 7.0, 10.1, …
## $ sleep_rem    <dbl> NA, 1.8, 2.4, 2.3, 0.7, 2.2, 1.4, NA, 2.9, NA, 0.6,…
## $ sleep_cycle  <dbl> NA, NA, NA, 0.1333333, 0.6666667, 0.7666667, 0.3833…
## $ awake        <dbl> 11.9, 7.0, 9.6, 9.1, 20.0, 9.6, 15.3, 17.0, 13.9, 2…
## $ brainwt      <dbl> NA, 0.01550, NA, 0.00029, 0.42300, NA, NA, NA, 0.07…
## $ bodywt       <dbl> 50.000, 0.480, 1.350, 0.019, 600.000, 3.850, 20.490…
```

```r
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```



3. Make a new data frame focused on body weight, but be sure to indicate the common name and genus of each mammal. Sort the data in descending order by body weight.


```r
msleep %>% 
  select(name, genus, bodywt) %>% 
  arrange(desc(bodywt))
```

```
## # A tibble: 83 x 3
##    name                 genus         bodywt
##    <chr>                <chr>          <dbl>
##  1 African elephant     Loxodonta      6654 
##  2 Asian elephant       Elephas        2547 
##  3 Giraffe              Giraffa         900.
##  4 Pilot whale          Globicephalus   800 
##  5 Cow                  Bos             600 
##  6 Horse                Equus           521 
##  7 Brazilian tapir      Tapirus         208.
##  8 Donkey               Equus           187 
##  9 Bottle-nosed dolphin Tursiops        173.
## 10 Tiger                Panthera        163.
## # … with 73 more rows
```



4. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. For our study, we are interested in body weight and sleep total Make two new dataframes (large and small) based on these parameters. Sort the data in descending order by body weight.

```r
large<-msleep%>%
  select(name,bodywt,sleep_total)%>% 
  filter(bodywt<=1) %>% 
  arrange(desc(bodywt))
large
```

```
## # A tibble: 36 x 3
##    name                      bodywt sleep_total
##    <chr>                      <dbl>       <dbl>
##  1 African giant pouched rat  1             8.3
##  2 Arctic ground squirrel     0.92         16.6
##  3 Tenrec                     0.9          15.6
##  4 European hedgehog          0.77         10.1
##  5 Squirrel monkey            0.743         9.6
##  6 Guinea pig                 0.728         9.4
##  7 Desert hedgehog            0.55         10.3
##  8 Owl monkey                 0.48         17  
##  9 Chinchilla                 0.42         12.5
## 10 Thick-tailed opposum       0.37         19.4
## # … with 26 more rows
```

```r
small<-msleep%>%
  select(name,bodywt,sleep_total)%>%
  filter(bodywt>=200) %>%
  arrange(desc(bodywt))
small
```

```
## # A tibble: 7 x 3
##   name             bodywt sleep_total
##   <chr>             <dbl>       <dbl>
## 1 African elephant  6654          3.3
## 2 Asian elephant    2547          3.9
## 3 Giraffe            900.         1.9
## 4 Pilot whale        800          2.7
## 5 Cow                600          4  
## 6 Horse              521          2.9
## 7 Brazilian tapir    208.         4.4
```





5. Let's try to figure out if large mammals sleep, on average, longer than small mammals. What is the average sleep duration for large mammals as we have defined them?

```r
#mean(large$sleep_total)
msleep%>%
  select(sleep_total)
```

```
## # A tibble: 83 x 1
##    sleep_total
##          <dbl>
##  1        12.1
##  2        17  
##  3        14.4
##  4        14.9
##  5         4  
##  6        14.4
##  7         8.7
##  8         7  
##  9        10.1
## 10         3  
## # … with 73 more rows
```

```r
mean(large$sleep_total)
```

```
## [1] 12.65833
```


6. What is the average sleep duration for small mammals as we have defined them?

```r
mean(small$sleep_total)
```

```
## [1] 3.3
```



7. Which animals sleep at least 18 hours per day? Be sure to show the name, genus, order, and sleep total. Sort by order and sleep total.

```r
msleep %>%
  select(name,genus,order,sleep_total)%>%
  filter(sleep_total>=18)
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 North American Opossum Didelphis  Didelphimorphia        18  
## 2 Big brown bat          Eptesicus  Chiroptera             19.7
## 3 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
## 4 Little brown bat       Myotis     Chiroptera             19.9
## 5 Giant armadillo        Priodontes Cingulata              18.1
```




## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
