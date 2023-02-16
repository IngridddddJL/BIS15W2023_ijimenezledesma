---
title: "Lab 10 Homework"
author: "Ingrid Jimenez Ledesma"
date: "2023-02-15"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Rows: 34786 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  

```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,…
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, …
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16…
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, …
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2, …
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM", …
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F",…
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, NA…
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipod…
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merri…
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod…
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Rod…
```

```r
structure(deserts)
```

```
## # A tibble: 34,786 × 13
##    record…¹ month   day  year plot_id speci…² sex   hindf…³ weight genus species
##       <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>   <chr>   <dbl>  <dbl> <chr> <chr>  
##  1        1     7    16  1977       2 NL      M          32     NA Neot… albigu…
##  2        2     7    16  1977       3 NL      M          33     NA Neot… albigu…
##  3        3     7    16  1977       2 DM      F          37     NA Dipo… merria…
##  4        4     7    16  1977       7 DM      M          36     NA Dipo… merria…
##  5        5     7    16  1977       3 DM      M          35     NA Dipo… merria…
##  6        6     7    16  1977       1 PF      M          14     NA Pero… flavus 
##  7        7     7    16  1977       2 PE      F          NA     NA Pero… eremic…
##  8        8     7    16  1977       1 DM      M          37     NA Dipo… merria…
##  9        9     7    16  1977       1 DM      F          34     NA Dipo… merria…
## 10       10     7    16  1977       6 PF      F          20     NA Pero… flavus 
## # … with 34,776 more rows, 2 more variables: taxa <chr>, plot_type <chr>, and
## #   abbreviated variable names ¹​record_id, ²​species_id, ³​hindfoot_length
```


2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts %>% 
  summarise(n_distinct(species))
```

```
## # A tibble: 1 × 1
##   `n_distinct(species)`
##                   <int>
## 1                    40
```



```r
deserts %>% 
summarise(n_distinct(genus))
```

```
## # A tibble: 1 × 1
##   `n_distinct(genus)`
##                 <int>
## 1                  26
```


3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.

```r
deserts %>% 
  count(taxa)
```

```
## # A tibble: 4 × 2
##   taxa        n
##   <chr>   <int>
## 1 Bird      450
## 2 Rabbit     75
## 3 Reptile    14
## 4 Rodent  34247
```


```r
deserts %>% 
  ggplot(aes(x=taxa))+ geom_bar()+scale_y_log10()
```

![](lab10_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`

```r
deserts %>% 
  ggplot(aes(x=taxa, fill=plot_type))+ geom_bar()+scale_y_log10()
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.

```r
deserts %>% 
  ggplot(aes(x=species, y= weight))+geom_boxplot(na.rm = T)+coord_flip()+scale_y_log10()
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.

```r
deserts %>% 
  ggplot(aes(x=species, y= weight))+geom_boxplot(na.rm = T)+coord_flip()+scale_y_log10()+geom_point()
```

```
## Warning: Removed 2503 rows containing missing values (`geom_point()`).
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?

```r
deserts %>% 
  filter(species=="merriami") %>% 
  
  ggplot(aes(year))+geom_bar()
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.

```r
deserts %>% 
  ggplot(aes(x=weight, y=hindfoot_length))+geom_point()+scale_x_log10()
```

```
## Warning: Removed 4048 rows containing missing values (`geom_point()`).
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.

```r
deserts %>% 
  filter(weight!="NA") %>% 
  group_by(species) %>% 
  summarise(average_wt=mean(weight)) %>% 
  arrange(desc(average_wt))
```

```
## # A tibble: 22 × 2
##    species      average_wt
##    <chr>             <dbl>
##  1 albigula          159. 
##  2 spectabilis       120. 
##  3 spilosoma          93.5
##  4 hispidus           65.6
##  5 fulviventer        58.9
##  6 ochrognathus       55.4
##  7 ordii              48.9
##  8 merriami           43.2
##  9 baileyi            31.7
## 10 leucogaster        31.6
## # … with 12 more rows
```

```r
deserts_ratio<- deserts %>% 
  filter(species=="albigula" | species=="spectabilis") %>% 
  filter(weight!="NA") %>% 
  filter(hindfoot_length!="NA") %>% 
  mutate(hindfoot_weight_ratio=weight/ hindfoot_length)
deserts_ratio
```

```
## # A tibble: 3,072 × 14
##    record…¹ month   day  year plot_id speci…² sex   hindf…³ weight genus species
##       <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>   <chr>   <dbl>  <dbl> <chr> <chr>  
##  1      357    11    12  1977       9 DS      F          50    117 Dipo… specta…
##  2      362    11    12  1977       1 DS      F          51    121 Dipo… specta…
##  3      367    11    12  1977      20 DS      M          51    115 Dipo… specta…
##  4      377    11    12  1977       9 DS      F          48    120 Dipo… specta…
##  5      381    11    13  1977      17 DS      F          48    118 Dipo… specta…
##  6      383    11    13  1977      11 DS      F          52    126 Dipo… specta…
##  7      385    11    13  1977      17 DS      M          50    132 Dipo… specta…
##  8      392    11    13  1977      11 DS      F          53    122 Dipo… specta…
##  9      394    11    13  1977       4 DS      F          48    107 Dipo… specta…
## 10      398    11    13  1977       4 DS      F          50    115 Dipo… specta…
## # … with 3,062 more rows, 3 more variables: taxa <chr>, plot_type <chr>,
## #   hindfoot_weight_ratio <dbl>, and abbreviated variable names ¹​record_id,
## #   ²​species_id, ³​hindfoot_length
```

```r
deserts_ratio %>% 
  ggplot(aes(x=species, y=hindfoot_weight_ratio, fill=sex))+geom_boxplot()
```

![](lab10_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.

```r
deserts %>% 
  count(sex)
```

```
## # A tibble: 3 × 2
##   sex       n
##   <chr> <int>
## 1 F     15690
## 2 M     17348
## 3 <NA>   1748
```

```r
deserts %>% 
  ggplot(aes(x=sex, fill=sex))+geom_bar()+
  labs(title="Female Vs. Male",
       x="sex",
       y="amount")
```

![](lab10_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
