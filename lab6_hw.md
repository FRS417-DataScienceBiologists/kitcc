---
title: "Lab 6 Homework"
author: "Kit Nga Chou"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the libraries

```r
library(tidyverse)
library(skimr)
library("RColorBrewer")
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get lost go have a look. Please do not copy and paste her code!  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use.

```r
#install.packages("gapminder")
```


```r
library("gapminder")
```

Please load the data into a new object called gapminder.

```r
gapminder <- 
  gapminder::gapminder
```

1. Explore the data using the various function you have learned. Is it tidy, are there any NA's, what are its dimensions, what are the column names, etc.

```r
glimpse(gapminder)
```

```
## Observations: 1,704
## Variables: 6
## $ country   <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, Af…
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 148803…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.…
```


```r
skimr::skim(gapminder)
```

```
## Skim summary statistics
##  n obs: 1704 
##  n variables: 6 
## 
## ── Variable type:factor ────────────────────────────────────────────────────────────────────────────────────────────────────────────────
##   variable missing complete    n n_unique
##  continent       0     1704 1704        5
##    country       0     1704 1704      142
##                              top_counts ordered
##  Afr: 624, Asi: 396, Eur: 360, Ame: 300   FALSE
##      Afg: 12, Alb: 12, Alg: 12, Ang: 12   FALSE
## 
## ── Variable type:integer ───────────────────────────────────────────────────────────────────────────────────────────────────────────────
##  variable missing complete    n    mean       sd    p0        p25     p50
##       pop       0     1704 1704 3e+07    1.1e+08 60011 2793664    7e+06  
##      year       0     1704 1704  1979.5 17.27     1952    1965.75  1979.5
##       p75       p100     hist
##  2e+07       1.3e+09 ▇▁▁▁▁▁▁▁
##   1993.25 2007       ▇▃▇▃▃▇▃▇
## 
## ── Variable type:numeric ───────────────────────────────────────────────────────────────────────────────────────────────────────────────
##   variable missing complete    n    mean      sd     p0     p25     p50
##  gdpPercap       0     1704 1704 7215.33 9857.45 241.17 1202.06 3531.85
##    lifeExp       0     1704 1704   59.47   12.92  23.6    48.2    60.71
##      p75      p100     hist
##  9325.46 113523.13 ▇▁▁▁▁▁▁▁
##    70.85     82.6  ▁▂▅▅▅▅▇▃
```


```r
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```



```r
gapminder %>% 
  summarize(number_nas = sum(is.na(.)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```


2. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer on average. Make a quick plot below to visualize this relationship.

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap, y=lifeExp))+
  geom_point()+
  labs(title = "Relationship between per capita GDP and Life Expectancy",
       x = "per capita GDP",
       y= "Life Expectancy")
```

![](lab6_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

3. There is extreme disparity in per capita GDP. Rescale the x axis to make this easier to interpret. How would you characterize the relationship?

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap, y=lifeExp))+
  geom_point()+
  scale_x_log10()+
  labs(title = "Relationship between per capita GDP and Life Expectancy",
       x = "per capita GDP",
       y= "Life Expectancy")
```

![](lab6_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

4. This should look pretty dense to you with significant overplotting. Try using a faceting approach to break this relationship down by year.

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap, y=lifeExp))+
  geom_point()+
  scale_x_log10()+
  facet_wrap(~year)+
  labs(title = "Relationship between per capita GDP and Life Expectancy",
       x = "per capita GDP",
       y= "Life Expectancy")
```

![](lab6_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

5. Simplify the comparison by comparing only 1952 and 2007. Can you come to any conclusions?

```r
gapminder %>% 
  filter(year==1952|year==2007) %>% 
  ggplot(aes(x=gdpPercap, y=lifeExp))+
  geom_point()+
  scale_x_log10()+
  facet_grid(~year)+
  labs(title = "Relationship between per capita GDP and Life Expectancy",
       x = "per capita GDP",
       y= "Life Expectancy")
```

![](lab6_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

6. Let's stick with the 1952 and 2007 comparison but make some aesthetic adjustments. First try to color by continent and adjust the size of the points by population. Add `+ scale_size(range = c(0.1, 10), guide = "none")` as a layer to clean things up a bit.

```r
gapminder %>%
  filter(year==1952 | year==2007) %>% 
  ggplot(aes(x=gdpPercap,y=lifeExp, color=continent, size=pop))+
  geom_point()+
  scale_x_log10()+
  facet_grid(~year)+
  labs(title = "Relationship between per capita GDP and Life Expectancy",
       x = "per capita GDP",
       y= "Life Expectancy")+
  scale_size(range = c(0.1, 10), guide = "none")
```

![](lab6_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

7. Although we did not introduce them in lab, ggplot has a number of built-in themes that make things easier. I like the light theme for these data, but you can see lots of options. Apply one of these to your plot above.

```r
?theme_light
```

8. What is the population for all countries on the Asian continent in 2007? Show this as a barplot.

```r
gapminder %>%
  filter(continent=="Asia",year==2007) %>% 
  ggplot(aes(x=country,y=pop))+
  geom_bar(stat = "identity")+
  coord_flip()+
  labs(title = "Population of Asian Countries in 2007",
       x="Population",
       y="Country")+
  theme_light()
```

![](lab6_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

9. You should see that China's population is the largest with India a close second. Let's focus on China only. Make a plot that shows how population has changed over the years.

```r
gapminder %>%
  filter(country=="China") %>% 
  ggplot(aes(x=year, y=pop))+
  geom_point()+
  geom_smooth(method=lm, se=TRUE)+
  labs(title = "Population in China over time",
       x="Year",
       y="Population")+
  theme_light()
```

![](lab6_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


10. Let's compare China and India. Make a barplot that shows population growth by year using `position=dodge`. Apply a custom color theme using RColorBrewer.

```r
gapminder %>% 
  filter(country=="China"|country=="India") %>% 
  ggplot(aes(x=year,y=pop,fill=country))+
  geom_bar(stat = "identity", position = "dodge")+
  scale_fill_brewer(palette = "Pastel1")+
  labs(title="Population Growth",
       x= "Year",
       y= "Population")+
  theme_light()
```

![](lab6_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
