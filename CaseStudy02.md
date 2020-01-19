---
title: "Case Study 02: Wealth and Life Expectancy"
author: "Josh Newey"
date: "1/18/2020"
output: 
  html_document:
    keep_md: true
---


Background:

I learned quite a lot doing this assignment. Most of what I learned was about how to use the ggplot library to customize the plots exactly. I also learned a lot about how to use the group_by function with the weighted.mean function to create the mean data. I feel a lot more confident in my ability to code in R. I think that this assignment will make more of my assignments easier in the future.



Images:


```r
library(gapminder)
library(ggplot2)
library(dplyr)
```

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




```r
dat <-  filter(gapminder, gdpPercap < 55000)
ggplot(data = dat,
       aes(x = lifeExp, 
           y = gdpPercap,
           size = pop / 100000,
           color = continent)) +
  theme_bw() +
  geom_point() +
  ylab("GDP per capita") +
  xlab("Life Expectancy") +
  scale_y_continuous(limits = c(0,50000),
                     trans = "sqrt") +
  facet_grid(~year) +
  guides(color = guide_legend("Continent"), 
         size = guide_legend("Population(100k)"))
```

![](CaseStudy02_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
ggsave("CS_02Plot_1.png", width = 15, units = "in")
```

```
## Saving 15 x 5 in image
```



```r
dat2 <- filter(gapminder, gdpPercap < 50000)

mean_data <- 
  dat2 %>% 
  group_by(year) %>%
  group_by(continent, add = TRUE) %>%  
  summarise(gdpPercap = weighted.mean(gdpPercap,pop), pop = sum(as.numeric(pop)))

View(mean_data)

ggplot(data = dat2) +
  aes(x = year, y = gdpPercap) +
  geom_line(aes(color = continent, line = country)) +
  geom_point(aes(size = pop / 100000, color = continent)) +
  geom_line(data = mean_data) +
  geom_point(data = mean_data, aes(size = pop / 100000)) +
  guides(color = guide_legend("Continent"),
         size = guide_legend("Population(100k)")) +
  ylab("GDP per capita") +
  xlab("Year") +
  theme_bw() +
  facet_grid(~continent)
```

```
## Warning: Ignoring unknown aesthetics: line
```

![](CaseStudy02_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
ggsave("CS_02Plot_2.png", width = 15, units = "in")
```

```
## Saving 15 x 5 in image
```

