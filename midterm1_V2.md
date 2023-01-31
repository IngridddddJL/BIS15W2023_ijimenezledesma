---
title: "Midterm 1"
author: "Ingrid Jimenez Ledesma"
date: "2023-01-31"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above.  

After the first 50 minutes, please upload your code (5 points). During the second 50 minutes, you may get help from each other- but no copy/paste. Upload the last version at the end of this time, but be sure to indicate it as final. If you finish early, you are free to leave.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Use the tidyverse and pipes unless otherwise indicated. This exam is worth a total of 35 points. 

Please load the following libraries.

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ecs21351-sup-0003-SupplementS1.csv`. These data are from Soykan, C. U., J. Sauer, J. G. Schuetz, G. S. LeBaron, K. Dale, and G. M. Langham. 2016. Population trends for North American winter birds based on hierarchical models. Ecosphere 7(5):e01351. 10.1002/ecs2.1351.  

Please load these data as a new object called `ecosphere`. In this step, I am providing the code to load the data, clean the variable names, and remove a footer that the authors used as part of the original publication.

```r
ecosphere <- read_csv("data/ecs21351-sup-0003-SupplementS1.csv", skip=2) %>% 
  clean_names() %>%
  slice(1:(n() - 18)) # this removes the footer
```

Problem 1. (1 point) Let's start with some data exploration. What are the variable names?


```r
names(ecosphere)
```

```
##  [1] "order"                       "family"                     
##  [3] "common_name"                 "scientific_name"            
##  [5] "diet"                        "life_expectancy"            
##  [7] "habitat"                     "urban_affiliate"            
##  [9] "migratory_strategy"          "log10_mass"                 
## [11] "mean_eggs_per_clutch"        "mean_age_at_sexual_maturity"
## [13] "population_size"             "winter_range_area"          
## [15] "range_in_cbc"                "strata"                     
## [17] "circles"                     "feeder_bird"                
## [19] "median_trend"                "lower_95_percent_ci"        
## [21] "upper_95_percent_ci"
```


Problem 2. (1 point) Use the function of your choice to summarize the data.

```r
structure(ecosphere)
```

```
## # A tibble: 551 × 21
##    order    family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##    <chr>    <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
##  1 Anserif… Anati… "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09
##  2 Anserif… Anati… "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88
##  3 Anserif… Anati… "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96
##  4 Anserif… Anati… "Black… Branta… Vege… Long    Wetland No      Modera…    3.11
##  5 Anserif… Anati… "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02
##  6 Anserif… Anati… "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88
##  7 Anserif… Anati… "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56
##  8 Anserif… Anati… "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6 
##  9 Anserif… Anati… "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45
## 10 Anserif… Anati… "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08
## # … with 541 more rows, 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

Problem 3. (2 points) How many distinct orders of birds are represented in the data?

```r
ecosphere %>% 
  summarize(d_order=n_distinct(order))  
```

```
## # A tibble: 1 × 1
##   d_order
##     <int>
## 1      19
```

Problem 4. (2 points) Which habitat has the highest diversity (number of species) in the data?

```r
ecosphere %>% 
  group_by(habitat) %>% 
  summarise(highest_diversity=n_distinct(scientific_name))
```

```
## # A tibble: 7 × 2
##   habitat   highest_diversity
##   <chr>                 <int>
## 1 Grassland                36
## 2 Ocean                    44
## 3 Shrubland                82
## 4 Various                  45
## 5 Wetland                 153
## 6 Woodland                177
## 7 <NA>                     14
```

Run the code below to learn about the `slice` function. Look specifically at the examples (at the bottom) for `slice_max()` and `slice_min()`. If you are still unsure, try looking up examples online (https://rpubs.com/techanswers88/dplyr-slice). Use this new function to answer question 5 below.

```r
?slice_max
```

Problem 5. (4 points) Using the `slice_max()` or `slice_min()` function described above which species has the largest and smallest winter range?

```r
ecosphere %>%
  slice_max(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Procella… Proce… Sooty … Puffin… Vert… Long    Ocean   No      Long        2.9
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```


```r
ecosphere %>% 
  slice_min(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Passerif… Alaud… Skylark Alauda… Seed  Short   Grassl… No      Reside…    1.57
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

Problem 6. (2 points) The family Anatidae includes ducks, geese, and swans. Make a new object `ducks` that only includes species in the family Anatidae. Restrict this new dataframe to include all variables except order and family.


```r
ducks <- data.frame(ecosphere)
ducks
```

```
##                 order            family
## 1        Anseriformes          Anatidae
## 2        Anseriformes          Anatidae
## 3        Anseriformes          Anatidae
## 4        Anseriformes          Anatidae
## 5        Anseriformes          Anatidae
## 6        Anseriformes          Anatidae
## 7        Anseriformes          Anatidae
## 8        Anseriformes          Anatidae
## 9        Anseriformes          Anatidae
## 10       Anseriformes          Anatidae
## 11       Anseriformes          Anatidae
## 12       Anseriformes          Anatidae
## 13       Anseriformes          Anatidae
## 14       Anseriformes          Anatidae
## 15       Anseriformes          Anatidae
## 16       Anseriformes          Anatidae
## 17       Anseriformes          Anatidae
## 18       Anseriformes          Anatidae
## 19       Anseriformes          Anatidae
## 20       Anseriformes          Anatidae
## 21       Anseriformes          Anatidae
## 22       Anseriformes          Anatidae
## 23       Anseriformes          Anatidae
## 24       Anseriformes          Anatidae
## 25       Anseriformes          Anatidae
## 26       Anseriformes          Anatidae
## 27       Anseriformes          Anatidae
## 28       Anseriformes          Anatidae
## 29       Anseriformes          Anatidae
## 30       Anseriformes          Anatidae
## 31       Anseriformes          Anatidae
## 32       Anseriformes          Anatidae
## 33       Anseriformes          Anatidae
## 34       Anseriformes          Anatidae
## 35       Anseriformes          Anatidae
## 36       Anseriformes          Anatidae
## 37       Anseriformes          Anatidae
## 38       Anseriformes          Anatidae
## 39       Anseriformes          Anatidae
## 40       Anseriformes          Anatidae
## 41       Anseriformes          Anatidae
## 42       Anseriformes          Anatidae
## 43       Anseriformes          Anatidae
## 44       Anseriformes          Anatidae
## 45        Apodiformes          Apodidae
## 46        Apodiformes          Apodidae
## 47        Apodiformes       Trochilidae
## 48        Apodiformes       Trochilidae
## 49        Apodiformes       Trochilidae
## 50        Apodiformes       Trochilidae
## 51        Apodiformes       Trochilidae
## 52        Apodiformes       Trochilidae
## 53        Apodiformes       Trochilidae
## 54        Apodiformes       Trochilidae
## 55        Apodiformes       Trochilidae
## 56        Apodiformes       Trochilidae
## 57        Apodiformes       Trochilidae
## 58        Apodiformes       Trochilidae
## 59        Apodiformes       Trochilidae
## 60   Caprimulgiformes     Caprimulgidae
## 61   Caprimulgiformes     Caprimulgidae
## 62   Caprimulgiformes     Caprimulgidae
## 63   Caprimulgiformes     Caprimulgidae
## 64   Caprimulgiformes     Caprimulgidae
## 65    Charadriiformes           Alcidae
## 66    Charadriiformes           Alcidae
## 67    Charadriiformes           Alcidae
## 68    Charadriiformes           Alcidae
## 69    Charadriiformes           Alcidae
## 70    Charadriiformes           Alcidae
## 71    Charadriiformes           Alcidae
## 72    Charadriiformes           Alcidae
## 73    Charadriiformes           Alcidae
## 74    Charadriiformes           Alcidae
## 75    Charadriiformes           Alcidae
## 76    Charadriiformes      Charadriidae
## 77    Charadriiformes      Charadriidae
## 78    Charadriiformes      Charadriidae
## 79    Charadriiformes      Charadriidae
## 80    Charadriiformes      Charadriidae
## 81    Charadriiformes      Charadriidae
## 82    Charadriiformes      Charadriidae
## 83    Charadriiformes      Charadriidae
## 84    Charadriiformes      Charadriidae
## 85    Charadriiformes      Charadriidae
## 86    Charadriiformes      Charadriidae
## 87    Charadriiformes      Charadriidae
## 88    Charadriiformes           Laridae
## 89    Charadriiformes           Laridae
## 90    Charadriiformes           Laridae
## 91    Charadriiformes           Laridae
## 92    Charadriiformes           Laridae
## 93    Charadriiformes           Laridae
## 94    Charadriiformes           Laridae
## 95    Charadriiformes           Laridae
## 96    Charadriiformes           Laridae
## 97    Charadriiformes           Laridae
## 98    Charadriiformes           Laridae
## 99    Charadriiformes           Laridae
## 100   Charadriiformes           Laridae
## 101   Charadriiformes           Laridae
## 102   Charadriiformes           Laridae
## 103   Charadriiformes           Laridae
## 104   Charadriiformes           Laridae
## 105   Charadriiformes           Laridae
## 106   Charadriiformes           Laridae
## 107   Charadriiformes           Laridae
## 108   Charadriiformes       Rynchopidae
## 109   Charadriiformes      Scolopacidae
## 110   Charadriiformes      Scolopacidae
## 111   Charadriiformes      Scolopacidae
## 112   Charadriiformes      Scolopacidae
## 113   Charadriiformes      Scolopacidae
## 114   Charadriiformes      Scolopacidae
## 115   Charadriiformes      Scolopacidae
## 116   Charadriiformes      Scolopacidae
## 117   Charadriiformes      Scolopacidae
## 118   Charadriiformes      Scolopacidae
## 119   Charadriiformes      Scolopacidae
## 120   Charadriiformes      Scolopacidae
## 121   Charadriiformes      Scolopacidae
## 122   Charadriiformes      Scolopacidae
## 123   Charadriiformes      Scolopacidae
## 124   Charadriiformes      Scolopacidae
## 125   Charadriiformes      Scolopacidae
## 126   Charadriiformes      Scolopacidae
## 127   Charadriiformes      Scolopacidae
## 128   Charadriiformes      Scolopacidae
## 129   Charadriiformes      Scolopacidae
## 130   Charadriiformes      Scolopacidae
## 131   Charadriiformes      Scolopacidae
## 132   Charadriiformes      Scolopacidae
## 133   Charadriiformes      Scolopacidae
## 134   Charadriiformes      Scolopacidae
## 135   Charadriiformes      Scolopacidae
## 136   Charadriiformes      Scolopacidae
## 137   Charadriiformes      Scolopacidae
## 138   Charadriiformes    Stercorariidae
## 139   Charadriiformes    Stercorariidae
## 140   Charadriiformes         Sternidae
## 141   Charadriiformes         Sternidae
## 142   Charadriiformes         Sternidae
## 143   Charadriiformes         Sternidae
## 144   Charadriiformes         Sternidae
## 145   Charadriiformes         Sternidae
## 146     Ciconiiformes        Anhingidae
## 147     Ciconiiformes          Ardeidae
## 148     Ciconiiformes          Ardeidae
## 149     Ciconiiformes          Ardeidae
## 150     Ciconiiformes          Ardeidae
## 151     Ciconiiformes          Ardeidae
## 152     Ciconiiformes          Ardeidae
## 153     Ciconiiformes          Ardeidae
## 154     Ciconiiformes          Ardeidae
## 155     Ciconiiformes          Ardeidae
## 156     Ciconiiformes          Ardeidae
## 157     Ciconiiformes          Ardeidae
## 158     Ciconiiformes          Ardeidae
## 159     Ciconiiformes        Ciconiidae
## 160     Ciconiiformes        Fregatidae
## 161     Ciconiiformes       Pelecanidae
## 162     Ciconiiformes       Pelecanidae
## 163     Ciconiiformes Phalacrocoracidae
## 164     Ciconiiformes Phalacrocoracidae
## 165     Ciconiiformes Phalacrocoracidae
## 166     Ciconiiformes Phalacrocoracidae
## 167     Ciconiiformes Phalacrocoracidae
## 168     Ciconiiformes Phalacrocoracidae
## 169     Ciconiiformes           Sulidae
## 170     Ciconiiformes           Sulidae
## 171     Ciconiiformes Threskiornithidae
## 172     Ciconiiformes Threskiornithidae
## 173     Ciconiiformes Threskiornithidae
## 174     Ciconiiformes Threskiornithidae
## 175     Columbiformes        Columbidae
## 176     Columbiformes        Columbidae
## 177     Columbiformes        Columbidae
## 178     Columbiformes        Columbidae
## 179     Columbiformes        Columbidae
## 180     Columbiformes        Columbidae
## 181     Columbiformes        Columbidae
## 182     Columbiformes        Columbidae
## 183     Columbiformes        Columbidae
## 184     Columbiformes        Columbidae
## 185     Columbiformes        Columbidae
## 186     Coraciiformes       Alcedinidae
## 187     Coraciiformes       Alcedinidae
## 188     Coraciiformes       Alcedinidae
## 189      Cuculiformes     Crotophagidae
## 190      Cuculiformes     Crotophagidae
## 191      Cuculiformes         Cuculidae
## 192     Falconiformes      Accipitridae
## 193     Falconiformes      Accipitridae
## 194     Falconiformes      Accipitridae
## 195     Falconiformes      Accipitridae
## 196     Falconiformes      Accipitridae
## 197     Falconiformes      Accipitridae
## 198     Falconiformes      Accipitridae
## 199     Falconiformes      Accipitridae
## 200     Falconiformes      Accipitridae
## 201     Falconiformes      Accipitridae
## 202     Falconiformes      Accipitridae
## 203     Falconiformes      Accipitridae
## 204     Falconiformes      Accipitridae
## 205     Falconiformes      Accipitridae
## 206     Falconiformes      Accipitridae
## 207     Falconiformes      Accipitridae
## 208     Falconiformes      Accipitridae
## 209     Falconiformes      Accipitridae
## 210     Falconiformes      Accipitridae
## 211     Falconiformes      Accipitridae
## 212     Falconiformes       Cathartidae
## 213     Falconiformes       Cathartidae
## 214     Falconiformes       Cathartidae
## 215     Falconiformes        Falconidae
## 216     Falconiformes        Falconidae
## 217     Falconiformes        Falconidae
## 218     Falconiformes        Falconidae
## 219     Falconiformes        Falconidae
## 220     Falconiformes        Falconidae
## 221     Falconiformes        Falconidae
## 222     Falconiformes       Pandionidae
## 223       Galliformes          Cracidae
## 224       Galliformes     Meleagrididae
## 225       Galliformes    Odontophoridae
## 226       Galliformes    Odontophoridae
## 227       Galliformes    Odontophoridae
## 228       Galliformes    Odontophoridae
## 229       Galliformes    Odontophoridae
## 230       Galliformes    Odontophoridae
## 231       Galliformes       Phasianidae
## 232       Galliformes       Phasianidae
## 233       Galliformes       Phasianidae
## 234       Galliformes     Tetraodonidae
## 235       Galliformes     Tetraodonidae
## 236       Galliformes     Tetraodonidae
## 237       Galliformes     Tetraodonidae
## 238       Galliformes     Tetraodonidae
## 239       Galliformes     Tetraodonidae
## 240       Galliformes     Tetraodonidae
## 241       Galliformes     Tetraodonidae
## 242       Galliformes     Tetraodonidae
## 243       Galliformes     Tetraodonidae
## 244       Gaviiformes          Gaviidae
## 245       Gaviiformes          Gaviidae
## 246       Gaviiformes          Gaviidae
## 247       Gaviiformes          Gaviidae
## 248        Gruiformes          Aramidae
## 249        Gruiformes           Gruidae
## 250        Gruiformes           Gruidae
## 251        Gruiformes          Rallidae
## 252        Gruiformes          Rallidae
## 253        Gruiformes          Rallidae
## 254        Gruiformes          Rallidae
## 255        Gruiformes          Rallidae
## 256        Gruiformes          Rallidae
## 257        Gruiformes          Rallidae
## 258        Gruiformes          Rallidae
## 259        Gruiformes          Rallidae
## 260     Passeriformes      Aegithalidae
## 261     Passeriformes         Alaudidae
## 262     Passeriformes         Alaudidae
## 263     Passeriformes     Bombycillidae
## 264     Passeriformes     Bombycillidae
## 265     Passeriformes        Calcaridae
## 266     Passeriformes      Cardinalidae
## 267     Passeriformes      Cardinalidae
## 268     Passeriformes      Cardinalidae
## 269     Passeriformes      Cardinalidae
## 270     Passeriformes      Cardinalidae
## 271     Passeriformes      Cardinalidae
## 272     Passeriformes      Cardinalidae
## 273     Passeriformes      Cardinalidae
## 274     Passeriformes      Cardinalidae
## 275     Passeriformes      Cardinalidae
## 276     Passeriformes      Cardinalidae
## 277     Passeriformes      Cardinalidae
## 278     Passeriformes        Certhiidae
## 279     Passeriformes         Cinclidae
## 280     Passeriformes          Corvidae
## 281     Passeriformes          Corvidae
## 282     Passeriformes          Corvidae
## 283     Passeriformes          Corvidae
## 284     Passeriformes          Corvidae
## 285     Passeriformes          Corvidae
## 286     Passeriformes          Corvidae
## 287     Passeriformes          Corvidae
## 288     Passeriformes          Corvidae
## 289     Passeriformes          Corvidae
## 290     Passeriformes          Corvidae
## 291     Passeriformes          Corvidae
## 292     Passeriformes          Corvidae
## 293     Passeriformes          Corvidae
## 294     Passeriformes          Corvidae
## 295     Passeriformes          Corvidae
## 296     Passeriformes          Corvidae
## 297     Passeriformes       Emberizidae
## 298     Passeriformes       Emberizidae
## 299     Passeriformes       Emberizidae
## 300     Passeriformes       Emberizidae
## 301     Passeriformes       Emberizidae
## 302     Passeriformes       Emberizidae
## 303     Passeriformes       Emberizidae
## 304     Passeriformes       Emberizidae
## 305     Passeriformes       Emberizidae
## 306     Passeriformes       Emberizidae
## 307     Passeriformes       Emberizidae
## 308     Passeriformes       Emberizidae
## 309     Passeriformes       Emberizidae
## 310     Passeriformes       Emberizidae
## 311     Passeriformes       Emberizidae
## 312     Passeriformes       Emberizidae
## 313     Passeriformes       Emberizidae
## 314     Passeriformes       Emberizidae
## 315     Passeriformes       Emberizidae
## 316     Passeriformes       Emberizidae
## 317     Passeriformes       Emberizidae
## 318     Passeriformes       Emberizidae
## 319     Passeriformes       Emberizidae
## 320     Passeriformes       Emberizidae
## 321     Passeriformes       Emberizidae
## 322     Passeriformes       Emberizidae
## 323     Passeriformes       Emberizidae
## 324     Passeriformes       Emberizidae
## 325     Passeriformes       Emberizidae
## 326     Passeriformes       Emberizidae
## 327     Passeriformes       Emberizidae
## 328     Passeriformes       Emberizidae
## 329     Passeriformes       Emberizidae
## 330     Passeriformes       Emberizidae
## 331     Passeriformes       Emberizidae
## 332     Passeriformes       Emberizidae
## 333     Passeriformes       Emberizidae
## 334     Passeriformes       Emberizidae
## 335     Passeriformes       Emberizidae
## 336     Passeriformes       Emberizidae
## 337     Passeriformes       Emberizidae
## 338     Passeriformes       Emberizidae
## 339     Passeriformes       Emberizidae
## 340     Passeriformes      Fringillidae
## 341     Passeriformes      Fringillidae
## 342     Passeriformes      Fringillidae
## 343     Passeriformes      Fringillidae
## 344     Passeriformes      Fringillidae
## 345     Passeriformes      Fringillidae
## 346     Passeriformes      Fringillidae
## 347     Passeriformes      Fringillidae
## 348     Passeriformes      Fringillidae
## 349     Passeriformes      Fringillidae
## 350     Passeriformes      Fringillidae
## 351     Passeriformes      Fringillidae
## 352     Passeriformes      Fringillidae
## 353     Passeriformes      Fringillidae
## 354     Passeriformes      Hirundinidae
## 355     Passeriformes      Hirundinidae
## 356     Passeriformes      Hirundinidae
## 357     Passeriformes      Hirundinidae
## 358     Passeriformes      Hirundinidae
## 359     Passeriformes         Icteridae
## 360     Passeriformes         Icteridae
## 361     Passeriformes         Icteridae
## 362     Passeriformes         Icteridae
## 363     Passeriformes         Icteridae
## 364     Passeriformes         Icteridae
## 365     Passeriformes         Icteridae
## 366     Passeriformes         Icteridae
## 367     Passeriformes         Icteridae
## 368     Passeriformes         Icteridae
## 369     Passeriformes         Icteridae
## 370     Passeriformes         Icteridae
## 371     Passeriformes         Icteridae
## 372     Passeriformes         Icteridae
## 373     Passeriformes         Icteridae
## 374     Passeriformes         Icteridae
## 375     Passeriformes         Icteridae
## 376     Passeriformes         Icteridae
## 377     Passeriformes         Icteridae
## 378     Passeriformes         Icteridae
## 379     Passeriformes          Laniidae
## 380     Passeriformes          Laniidae
## 381     Passeriformes           Mimidae
## 382     Passeriformes           Mimidae
## 383     Passeriformes           Mimidae
## 384     Passeriformes           Mimidae
## 385     Passeriformes           Mimidae
## 386     Passeriformes           Mimidae
## 387     Passeriformes           Mimidae
## 388     Passeriformes           Mimidae
## 389     Passeriformes           Mimidae
## 390     Passeriformes           Mimidae
## 391     Passeriformes      Motacillidae
## 392     Passeriformes      Motacillidae
## 393     Passeriformes           Paridae
## 394     Passeriformes           Paridae
## 395     Passeriformes           Paridae
## 396     Passeriformes           Paridae
## 397     Passeriformes           Paridae
## 398     Passeriformes           Paridae
## 399     Passeriformes           Paridae
## 400     Passeriformes           Paridae
## 401     Passeriformes           Paridae
## 402     Passeriformes           Paridae
## 403     Passeriformes         Parulidae
## 404     Passeriformes         Parulidae
## 405     Passeriformes         Parulidae
## 406     Passeriformes         Parulidae
## 407     Passeriformes         Parulidae
## 408     Passeriformes         Parulidae
## 409     Passeriformes         Parulidae
## 410     Passeriformes         Parulidae
## 411     Passeriformes         Parulidae
## 412     Passeriformes         Parulidae
## 413     Passeriformes         Parulidae
## 414     Passeriformes         Parulidae
## 415     Passeriformes         Parulidae
## 416     Passeriformes         Parulidae
## 417     Passeriformes         Parulidae
## 418     Passeriformes         Parulidae
## 419     Passeriformes         Parulidae
## 420     Passeriformes         Parulidae
## 421     Passeriformes         Parulidae
## 422     Passeriformes         Parulidae
## 423     Passeriformes         Parulidae
## 424     Passeriformes         Parulidae
## 425     Passeriformes         Parulidae
## 426     Passeriformes         Parulidae
## 427     Passeriformes         Parulidae
## 428     Passeriformes         Parulidae
## 429     Passeriformes         Parulidae
## 430     Passeriformes         Parulidae
## 431     Passeriformes         Parulidae
## 432     Passeriformes         Parulidae
## 433     Passeriformes         Parulidae
## 434     Passeriformes        Passeridae
## 435     Passeriformes        Passeridae
## 436     Passeriformes     Peucedramidae
## 437     Passeriformes     Polioptilidae
## 438     Passeriformes     Polioptilidae
## 439     Passeriformes    Ptilogonatidae
## 440     Passeriformes         Regulidae
## 441     Passeriformes         Regulidae
## 442     Passeriformes          Sittidae
## 443     Passeriformes          Sittidae
## 444     Passeriformes          Sittidae
## 445     Passeriformes          Sittidae
## 446     Passeriformes         Sturnidae
## 447     Passeriformes         Sturnidae
## 448     Passeriformes         Sturnidae
## 449     Passeriformes         Sturnidae
## 450     Passeriformes        Thraupidae
## 451     Passeriformes        Timaliidae
## 452     Passeriformes     Troglodytidae
## 453     Passeriformes     Troglodytidae
## 454     Passeriformes     Troglodytidae
## 455     Passeriformes     Troglodytidae
## 456     Passeriformes     Troglodytidae
## 457     Passeriformes     Troglodytidae
## 458     Passeriformes     Troglodytidae
## 459     Passeriformes     Troglodytidae
## 460     Passeriformes     Troglodytidae
## 461     Passeriformes          Turdidae
## 462     Passeriformes          Turdidae
## 463     Passeriformes          Turdidae
## 464     Passeriformes          Turdidae
## 465     Passeriformes          Turdidae
## 466     Passeriformes          Turdidae
## 467     Passeriformes          Turdidae
## 468     Passeriformes          Turdidae
## 469     Passeriformes          Turdidae
## 470     Passeriformes        Tyrannidae
## 471     Passeriformes        Tyrannidae
## 472     Passeriformes        Tyrannidae
## 473     Passeriformes        Tyrannidae
## 474     Passeriformes        Tyrannidae
## 475     Passeriformes        Tyrannidae
## 476     Passeriformes        Tyrannidae
## 477     Passeriformes        Tyrannidae
## 478     Passeriformes        Tyrannidae
## 479     Passeriformes        Tyrannidae
## 480     Passeriformes        Tyrannidae
## 481     Passeriformes        Tyrannidae
## 482     Passeriformes        Tyrannidae
## 483     Passeriformes        Tyrannidae
## 484     Passeriformes        Tyrannidae
## 485     Passeriformes        Tyrannidae
## 486     Passeriformes        Tyrannidae
## 487     Passeriformes        Tyrannidae
## 488     Passeriformes        Tyrannidae
## 489     Passeriformes        Tyrannidae
## 490     Passeriformes        Tyrannidae
## 491     Passeriformes        Tyrannidae
## 492     Passeriformes        Vireonidae
## 493     Passeriformes        Vireonidae
## 494     Passeriformes        Vireonidae
## 495     Passeriformes        Vireonidae
## 496     Passeriformes        Vireonidae
## 497        Piciformes           Picidae
## 498        Piciformes           Picidae
## 499        Piciformes           Picidae
## 500        Piciformes           Picidae
## 501        Piciformes           Picidae
## 502        Piciformes           Picidae
## 503        Piciformes           Picidae
## 504        Piciformes           Picidae
## 505        Piciformes           Picidae
## 506        Piciformes           Picidae
## 507        Piciformes           Picidae
## 508        Piciformes           Picidae
## 509        Piciformes           Picidae
## 510        Piciformes           Picidae
## 511        Piciformes           Picidae
## 512        Piciformes           Picidae
## 513        Piciformes           Picidae
## 514        Piciformes           Picidae
## 515        Piciformes           Picidae
## 516        Piciformes           Picidae
## 517        Piciformes           Picidae
## 518        Piciformes           Picidae
## 519  Podicipediformes     Podicipedidae
## 520  Podicipediformes     Podicipedidae
## 521  Podicipediformes     Podicipedidae
## 522  Podicipediformes     Podicipedidae
## 523  Podicipediformes     Podicipedidae
## 524  Podicipediformes     Podicipedidae
## 525 Procellariiformes       Diomedeidae
## 526 Procellariiformes    Procellariidae
## 527 Procellariiformes    Procellariidae
## 528 Procellariiformes    Procellariidae
## 529    Psittaciformes       Psittacidae
## 530    Psittaciformes       Psittacidae
## 531    Psittaciformes       Psittacidae
## 532    Psittaciformes       Psittacidae
## 533    Psittaciformes       Psittacidae
## 534    Psittaciformes       Psittacidae
## 535      Strigiformes         Strigidae
## 536      Strigiformes         Strigidae
## 537      Strigiformes         Strigidae
## 538      Strigiformes         Strigidae
## 539      Strigiformes         Strigidae
## 540      Strigiformes         Strigidae
## 541      Strigiformes         Strigidae
## 542      Strigiformes         Strigidae
## 543      Strigiformes         Strigidae
## 544      Strigiformes         Strigidae
## 545      Strigiformes         Strigidae
## 546      Strigiformes         Strigidae
## 547      Strigiformes         Strigidae
## 548      Strigiformes         Strigidae
## 549      Strigiformes         Strigidae
## 550      Strigiformes         Tytonidae
## 551     Trogoniformes        Trogonidae
##                                                   common_name
## 1                                         American Black Duck
## 2                                             American Wigeon
## 3                                          Barrow's Goldeneye
## 4                                                 Black Brant
## 5                                                Black Scoter
## 6                                Black-bellied Whistling-Duck
## 7                                            Blue-winged Teal
## 8                                                  Bufflehead
## 9                              Cackling and Canada Goose \xa0
## 10                                                 Canvasback
## 11                                              Cinnamon Teal
## 12                                               Common Eider
## 13                                           Common Goldeneye
## 14                                           Common Merganser
## 15                                              Emperor Goose
## 16                                            Eurasian Wigeon
## 17                                     Fulvous Whistling-Duck
## 18                                                    Gadwall
## 19                                              Greater Scaup
## 20                                Greater White-fronted Goose
## 21                                          Green-winged Teal
## 22                                             Harlequin Duck
## 23                                           Hooded Merganser
## 24                                                 King Eider
## 25                                               Lesser Scaup
## 26                                           Long-tailed Duck
## 27                                                    Mallard
## 28                                               Mottled Duck
## 29                                                  Mute Swan
## 30                                           Northern Pintail
## 31                                          Northern Shoveler
## 32                                     Red-breasted Merganser
## 33                                                    Redhead
## 34                                           Ring-necked Duck
## 35                                               Ross's Goose
## 36                                                 Ruddy Duck
## 37                                                 Snow Goose
## 38                                            Steller's Eider
## 39                                                Surf Scoter
## 40                                             Trumpeter Swan
## 41                                                Tufted Duck
## 42                                                Tundra Swan
## 43                                        White-winged Scoter
## 44                                                  Wood Duck
## 45                                               Vaux's Swift
## 46                                       White-throated Swift
## 47                                        Allen's Hummingbird
## 48                                         Anna's Hummingbird
## 49                                  Black-chinned Hummingbird
## 50                                  Blue-throated Hummingbird
## 51                                   Broad-billed Hummingbird
## 52                                   Broad-tailed Hummingbird
## 53                                   Buff-bellied Hummingbird
## 54                                       Calliope Hummingbird
## 55                                        Costa's Hummingbird
## 56                                    Magnificent Hummingbird
## 57                                  Ruby-throated Hummingbird
## 58                                         Rufous Hummingbird
## 59                                 Violet-crowned Hummingbird
## 60                                         Chuck-will's-widow
## 61                                            Common Pauraque
## 62                                            Common Poorwill
## 63                                           Lesser Nighthawk
## 64                    Eastern and Mexican Whip-poor-will \xe0
## 65                                           Ancient Murrelet
## 66                                            Atlantic Puffin
## 67                                            Black Guillemot
## 68                                            Cassin's Auklet
## 69                                               Common Murre
## 70                                                    Dovekie
## 71                                           Marbled Murrelet
## 72                                           Pigeon Guillemot
## 73                                                  Razorbill
## 74                                          Rhinoceros Auklet
## 75                                         Thick-billed Murre
## 76                                            American Avocet
## 77                                     American Oystercatcher
## 78                                        Black Oystercatcher
## 79                                       Black-bellied Plover
## 80                                         Black-necked Stilt
## 81                                                   Killdeer
## 82                                            Mountain Plover
## 83                                      Pacific Golden-Plover
## 84                                              Piping Plover
## 85                                        Semipalmated Plover
## 86                                               Snowy Plover
## 87                                            Wilson's Plover
## 88                                          Black-headed Gull
## 89                                     Black-legged Kittiwake
## 90                                           Bonaparte's Gull
## 91                                            California Gull
## 92                                               Elegant Tern
## 93                                            Franklin's Gull
## 94                                              Glaucous Gull
## 95                                       Glaucous-winged Gull
## 96                                    Great Black-backed Gull
## 97                                            Heermann's Gull
## 98                                               Herring Gull
## 99                                              Laughing Gull
## 100                                                Least Tern
## 101                                  Lesser Black-backed Gull
## 102                                               Little Gull
## 103                                                  Mew Gull
## 104                                          Ring-billed Gull
## 105                            Iceland and Thayer's Gull \xa4
## 106                                              Western Gull
## 107                                        Yellow-footed Gull
## 108                                             Black Skimmer
## 109                                         American Woodcock
## 110                                           Black Turnstone
## 111                                                    Dunlin
## 112                                        Greater Yellowlegs
## 113                                           Least Sandpiper
## 114                                         Lesser Yellowlegs
## 115                                        Long-billed Curlew
## 116                                     Long-billed Dowitcher
## 117                                            Marbled Godwit
## 118                                        Pectoral Sandpiper
## 119                                          Purple Sandpiper
## 120                                                  Red Knot
## 121                                      Red-necked Phalarope
## 122                                            Rock Sandpiper
## 123                                           Ruddy Turnstone
## 124                                                Sanderling
## 125                                    Semipalmated Sandpiper
## 126                                    Short-billed Dowitcher
## 127                                        Solitary Sandpiper
## 128                                         Spotted Sandpiper
## 129                                           Stilt Sandpiper
## 130                                                  Surfbird
## 131                                         Wandering Tattler
## 132                                         Western Sandpiper
## 133                                                  Whimbrel
## 134                                    White-rumped Sandpiper
## 135                                                    Willet
## 136                                        Wilson's Phalarope
## 137                                            Wilson's Snipe
## 138                                          Parasitic Jaeger
## 139                                           Pomarine Jaeger
## 140                                              Caspian Tern
## 141                                               Common Tern
## 142                                            Forster's Tern
## 143                                          Gull-billed Tern
## 144                                                Royal Tern
## 145                                             Sandwich Tern
## 146                                                   Anhinga
## 147                                          American Bittern
## 148                                 Black-crowned Night-Heron
## 149                                              Cattle Egret
## 150                                          Great Blue Heron
## 151                                               Great Egret
## 152                                               Green Heron
## 153                                             Least Bittern
## 154                                         Little Blue Heron
## 155                                             Reddish Egret
## 156                                               Snowy Egret
## 157                                          Tricolored Heron
## 158                                Yellow-crowned Night-Heron
## 159                                                Wood Stork
## 160                                   Magnificent Frigatebird
## 161                                    American White Pelican
## 162                                             Brown Pelican
## 163                                        Brandt's Cormorant
## 164                                  Double-crested Cormorant
## 165                                           Great Cormorant
## 166                                       Neotropic Cormorant
## 167                                         Pelagic Cormorant
## 168                                       Red-faced Cormorant
## 169                                               Brown Booby
## 170                                           Northern Gannet
## 171                                               Glossy Ibis
## 172                                         Roseate Spoonbill
## 173                                                White Ibis
## 174                                          White-faced Ibis
## 175                                        Band-tailed Pigeon
## 176                                        Common Ground-Dove
## 177                                    Eurasian Collared-Dove
## 178                                                 Inca Dove
## 179                                             Mourning Dove
## 180                                         Red-billed Pigeon
## 181                                         Ruddy Ground-dove
## 182                                              Spotted Dove
## 183                                      White-crowned Pigeon
## 184                                         White-tipped Dove
## 185                                         White-winged Dove
## 186                                         Belted Kingfisher
## 187                                          Green Kingfisher
## 188                                         Ringed Kingfisher
## 189                                         Groove-billed Ani
## 190                                         Smooth-billed Ani
## 191                                        Greater Roadrunner
## 192                                                Bald Eagle
## 193                                         Broad-winged Hawk
## 194                                             Cooper's Hawk
## 195                                          Ferruginous Hawk
## 196                                              Golden Eagle
## 197                                                 Gray Hawk
## 198                                             Harris's Hawk
## 199                                          Hook-billed Kite
## 200                                          Northern Goshawk
## 201                                          Northern Harrier
## 202                                       Red-shouldered Hawk
## 203                                           Red-tailed Hawk
## 204                                         Rough-legged Hawk
## 205                                        Sharp-shinned Hawk
## 206                                         Short-tailed Hawk
## 207                                                Snail Kite
## 208                                           Swainson's Hawk
## 209                                         White-tailed Hawk
## 210                                         White-tailed Kite
## 211                                          Zone-tailed Hawk
## 212                                             Black Vulture
## 213                                         California Condor
## 214                                            Turkey Vulture
## 215                                          American Kestrel
## 216                                           Aplomado Falcon
## 217                                          Crested Caracara
## 218                                                 Gyrfalcon
## 219                                                    Merlin
## 220                                          Peregrine Falcon
## 221                                            Prairie Falcon
## 222                                                    Osprey
## 223                                          Plain Chachalaca
## 224                                               Wild Turkey
## 225                                          California Quail
## 226                                            Gambel's Quail
## 227                                           Montezuma Quail
## 228                                            Mountain Quail
## 229                                         Northern Bobwhite
## 230                                              Scaled Quail
## 231                                                    Chukar
## 232                                            Gray Partridge
## 233                                      Ring-necked Pheasant
## 234                                              Dusky Grouse
## 235                                   Greater Prairie-Chicken
## 236                                       Greater Sage-Grouse
## 237                                    Lesser Prairie-Chicken
## 238                                            Rock Ptarmigan
## 239                                             Ruffed Grouse
## 240                                       Sharp-tailed Grouse
## 241                                             Spruce Grouse
## 242                                    White-tailed Ptarmigan
## 243                                          Willow Ptarmigan
## 244                                               Common Loon
## 245                              Arctic and Pacific Loon \xa6
## 246                                         Red-throated Loon
## 247                                        Yellow-billed Loon
## 248                                                   Limpkin
## 249                                            Sandhill Crane
## 250                                            Whooping Crane
## 251                                             American Coot
## 252                                                Black Rail
## 253                                              Clapper Rail
## 254                                            Common Moorhen
## 255                                                 King Rail
## 256                                          Purple Gallinule
## 257                                                      Sora
## 258                                             Virginia Rail
## 259                                               Yellow Rail
## 260                                                   Bushtit
## 261                                               Horned Lark
## 262                                                   Skylark
## 263                                          Bohemian Waxwing
## 264                                             Cedar Waxwing
## 265                                         McCown's Longspur
## 266                                     Black-headed Grosbeak
## 267                                             Blue Grosbeak
## 268                                                Dickcissel
## 269                                           Hepatic Tanager
## 270                                            Indigo Bunting
## 271                                            Lazuli Bunting
## 272                                         Northern Cardinal
## 273                                           Painted Bunting
## 274                                               Pyrrhuloxia
## 275                                    Rose-breasted Grosbeak
## 276                                            Summer Tanager
## 277                                           Western Tanager
## 278                                             Brown Creeper
## 279                                           American Dipper
## 280                                             American Crow
## 281                                       Black-billed Magpie
## 282                                                  Blue Jay
## 283                                                 Brown Jay
## 284                                          Chihuahuan Raven
## 285                                        Clark's Nutcracker
## 286                                              Common Raven
## 287                                                 Fish Crow
## 288                                         Florida Scrub-Jay
## 289                                                  Gray Jay
## 290                                                 Green Jay
## 291                                               Mexican Jay
## 292                                         Northwestern Crow
## 293                                                Pinyon Jay
## 294                                             Steller's Jay
## 295                                         Western Scrub-Jay
## 296                                      Yellow-billed Magpie
## 297                                            Abert's Towhee
## 298                                     American Tree Sparrow
## 299                                         Bachman's Sparrow
## 300                                           Baird's Sparrow
## 301                                     Black-chinned Sparrow
## 302                                    Black-throated Sparrow
## 303                                          Brewer's Sparrow
## 304                      California and Canyon/Brown Towhee #
## 305                                          Cassin's Sparrow
## 306                                Chestnut-collared Longspur
## 307                                          Chipping Sparrow
## 308                                      Clay-colored Sparrow
## 309                                           Dark-eyed Junco
## 310                              Eastern and Spotted Towhee _
## 311                                             Field Sparrow
## 312                                      Five-striped Sparrow
## 313                                               Fox Sparrow
## 314                                    Golden-crowned Sparrow
## 315                                       Grasshopper Sparrow
## 316                                       Green-tailed Towhee
## 317                                          Harris's Sparrow
## 318                                         Henslow's Sparrow
## 319                                          Lapland Longspur
## 320                                              Lark Bunting
## 321                                              Lark Sparrow
## 322                                        Le Conte's Sparrow
## 323                                         Lincoln's Sparrow
## 324                                           McKay's Bunting
## 325                                             Olive Sparrow
## 326                                    Rufous-crowned Sparrow
## 327                                     Rufous-winged Sparrow
## 328                     Bell's and Sagebrush Sparrow \xa0\xa0
## 329                                          Savannah Sparrow
## 330                                           Seaside Sparrow
## 331                   Nelson's and Saltmarsh Sparrow \xe0\xe0
## 332                                          Smith's Longspur
## 333                                              Snow Bunting
## 334                                              Song Sparrow
## 335                                             Swamp Sparrow
## 336                                            Vesper Sparrow
## 337                                     White-crowned Sparrow
## 338                                    White-throated Sparrow
## 339                                         Yellow-eyed Junco
## 340                                        American Goldfinch
## 341 Black, Brown-capped, and Gray-crowned Rosy-Finch \xa4\xa4
## 342                                            Cassin's Finch
## 343                                            Common Redpoll
## 344                                          Evening Grosbeak
## 345                                             Hoary Redpoll
## 346                                               House Finch
## 347                                      Lawrence's Goldfinch
## 348                                          Lesser Goldfinch
## 349                                             Pine Grosbeak
## 350                                               Pine Siskin
## 351                                              Purple Finch
## 352                                             Red Crossbill
## 353                                    White-winged Crossbill
## 354                                              Barn Swallow
## 355                                              Cave Swallow
## 356                             Northern Rough-winged Swallow
## 357                                              Tree Swallow
## 358                                      Violet-green Swallow
## 359                                           Altamira Oriole
## 360                                          Audubon's Oriole
## 361                                          Baltimore Oriole
## 362             Boat-tailed and Great-tailed Grackle \xa6\xa6
## 363                                        Brewer's Blackbird
## 364                                           Bronzed Cowbird
## 365                                      Brown-headed Cowbird
## 366                                          Bullock's Oriole
## 367                                            Common Grackle
## 368                                        Eastern Meadowlark
## 369                                             Hooded Oriole
## 370                                            Orchard Oriole
## 371                                      Red-winged Blackbird
## 372                                           Rusty Blackbird
## 373                                            Scott's Oriole
## 374                                             Shiny Cowbird
## 375                                      Spot-breasted Oriole
## 376                                      Tricolored Blackbird
## 377                                        Western Meadowlark
## 378                                   Yellow-headed Blackbird
## 379                                         Loggerhead Shrike
## 380                                           Northern Shrike
## 381                                        Bendire's Thrasher
## 382                                            Brown Thrasher
## 383                                       California Thrasher
## 384                                          Crissal Thrasher
## 385                                     Curve-billed Thrasher
## 386                                              Gray Catbird
## 387                                       Le Conte's Thrasher
## 388                                      Long-billed Thrasher
## 389                                      Northern Mockingbird
## 390                                             Sage Thrasher
## 391                                            American Pipit
## 392                                           Sprague's Pipit
## 393                                    Black-capped Chickadee
## 394                                          Boreal Chickadee
## 395                                          Bridled Titmouse
## 396                                        Carolina Chickadee
## 397                                 Chestnut-backed Chickadee
## 398                                         Mexican Chickadee
## 399                                        Mountain Chickadee
## 400                               Juniper and Oak Titmouse ##
## 401                      Black-crested and Tufted Titmouse __
## 402                                                    Verdin
## 403                                         American Redstart
## 404                                   Black-and-white Warbler
## 405                               Black-throated Blue Warbler
## 406                               Black-throated Gray Warbler
## 407                              Black-throated Green Warbler
## 408                                          Cape May Warbler
## 409                                    Chestnut-sided Warbler
## 410                                       Common Yellowthroat
## 411                                            Hermit Warbler
## 412                                     Louisiana Waterthrush
## 413                                    MacGillivray's Warbler
## 414                                          Magnolia Warbler
## 415                                         Nashville Warbler
## 416                                           Northern Parula
## 417                                      Northern Waterthrush
## 418                                    Orange-crowned Warbler
## 419                                                  Ovenbird
## 420                                          Painted Redstart
## 421                                              Palm Warbler
## 422                                              Pine Warbler
## 423                                           Prairie Warbler
## 424                                      Prothonotary Warbler
## 425                                         Tennessee Warbler
## 426                                        Townsend's Warbler
## 427                                           Tropical Parula
## 428                                          Wilson's Warbler
## 429                                       Worm-eating Warbler
## 430                                            Yellow Warbler
## 431                                      Yellow-breasted Chat
## 432                                     Yellow-rumped Warbler
## 433                                   Yellow-throated Warbler
## 434                                     Eurasian Tree Sparrow
## 435                                             House Sparrow
## 436                                             Olive Warbler
## 437                                     Blue-gray Gnatcatcher
## 438                                    California Gnatcatcher
## 439                                               Phainopepla
## 440                                    Golden-crowned Kinglet
## 441                                      Ruby-crowned Kinglet
## 442                                     Brown-headed Nuthatch
## 443                                            Pygmy Nuthatch
## 444                                     Red-breasted Nuthatch
## 445                                   White-breasted Nuthatch
## 446                                               Common Myna
## 447                                              Crested Myna
## 448                                         European Starling
## 449                                                 Hill Myna
## 450                                  White-collared Seedeater
## 451                                                   Wrentit
## 452                                             Bewick's Wren
## 453                                               Cactus Wren
## 454                                               Canyon Wren
## 455                                             Carolina Wren
## 456                                                House Wren
## 457                                                Marsh Wren
## 458                                                 Rock Wren
## 459                                                Sedge Wren
## 460                                               Winter Wren
## 461                                            American Robin
## 462                                       Clay-colored Thrush
## 463                                          Eastern Bluebird
## 464                                             Hermit Thrush
## 465                                         Mountain Bluebird
## 466                                      Townsend's Solitaire
## 467                                             Varied Thrush
## 468                                          Western Bluebird
## 469                                               Wood Thrush
## 470                                   Ash-throated Flycatcher
## 471                                              Black Phoebe
## 472                                  Brown-crested Flycatcher
## 473                                         Cassin's Kingbird
## 474                                          Dusky Flycatcher
## 475                                   Dusky-capped Flycatcher
## 476                                            Eastern Phoebe
## 477                                        Eastern Wood-Pewee
## 478                                           Gray Flycatcher
## 479                                  Great Crested Flycatcher
## 480                                            Great Kiskadee
## 481                                             Greater Pewee
## 482                                      Hammond's Flycatcher
## 483                                          Least Flycatcher
## 484                             Northern Beardless-Tyrannulet
## 485                                  Pacific-slope Flycatcher
## 486                                              Say's Phoebe
## 487                                 Scissor-tailed Flycatcher
## 488                                     Thick-billed Kingbird
## 489                Couch's and Tropical Kingbird \xa0\xa0\xa0
## 490                                      Vermilion Flycatcher
## 491                                          Western Kingbird
## 492                                              Bell's Vireo
## 493                                            Hutton's Vireo
## 494   Blue-headed, Cassin's, and Plumbeous Vireo \xe0\xe0\xe0
## 495                                          White-eyed Vireo
## 496                                     Yellow-throated Vireo
## 497                                          Acorn Woodpecker
## 498                            American Three-toed Woodpecker
## 499                                        Arizona Woodpecker
## 500                                   Black-backed Woodpecker
## 501                                          Downy Woodpecker
## 502                                           Gila Woodpecker
## 503                                            Gilded Flicker
## 504                                 Golden-fronted Woodpecker
## 505                                          Hairy Woodpecker
## 506                                  Ladder-backed Woodpecker
## 507                                        Lewis's Woodpecker
## 508                                          Northern Flicker
## 509                                      Nuttall's Woodpecker
## 510                                       Pileated Woodpecker
## 511                                    Red-bellied Woodpecker
## 512                                    Red-breasted Sapsucker
## 513                                   Red-cockaded Woodpecker
## 514                                     Red-headed Woodpecker
## 515                                       Red-naped Sapsucker
## 516                                   White-headed Woodpecker
## 517                                    Williamson's Sapsucker
## 518                                  Yellow-bellied Sapsucker
## 519                    Clark's and Western Grebe \xa4\xa4\xa4
## 520                                               Eared Grebe
## 521                                              Horned Grebe
## 522                                               Least Grebe
## 523                                         Pied-billed Grebe
## 524                                          Red-necked Grebe
## 525                                    Black-footed Albatross
## 526                                           Northern Fulmar
## 527                                   Short-tailed Shearwater
## 528                                          Sooty Shearwater
## 529                                                Budgerigar
## 530                                            Green Parakeet
## 531                                             Monk Parakeet
## 532                                        Red-crowned Parrot
## 533                                      Rose-ringed Parakeet
## 534                                     White-winged Parakeet
## 535                                                Barred Owl
## 536                                                Boreal Owl
## 537                                             Burrowing Owl
## 538              Eastern and Western Screech-Owl \xa6\xa6\xa6
## 539                                     Ferruginous Pygmy-Owl
## 540                                            Great Gray Owl
## 541                                          Great Horned Owl
## 542                                            Long-eared Owl
## 543                                         Northern Hawk Owl
## 544                                        Northern Pygmy-Owl
## 545                                     Northern Saw-whet Owl
## 546                                           Short-eared Owl
## 547                                                 Snowy Owl
## 548                                               Spotted Owl
## 549                                     Whiskered Screech-Owl
## 550                                                  Barn Owl
## 551                                            Elegant Trogon
##                                          scientific_name          diet
## 1                                          Anas rubripes    Vegetation
## 2                                         Anas americana    Vegetation
## 3                                    Bucephala islandica Invertebrates
## 4                                        Branta bernicla    Vegetation
## 5                                    Melanitta americana Invertebrates
## 6                                 Dendrocygna autumnalis    Vegetation
## 7                                           Anas discors    Vegetation
## 8                                      Bucephala albeola Invertebrates
## 9                    Branta hutchinsii and B. canadensis    Vegetation
## 10                                    Aythya valisineria    Vegetation
## 11                                       Anas cyanoptera    Vegetation
## 12                                  Somateria mollissima Invertebrates
## 13                                    Bucephala clangula Invertebrates
## 14                                      Mergus merganser   Vertebrates
## 15                                         Chen canagica      Omnivore
## 16                                         Anas penelope    Vegetation
## 17                                   Dendrocygna bicolor    Vegetation
## 18                                         Anas strepera    Vegetation
## 19                                         Aythya marila Invertebrates
## 20                                       Anser albifrons    Vegetation
## 21                                           Anas crecca    Vegetation
## 22                             Histrionicus histrionicus Invertebrates
## 23                                 Lophodytes cucullatus Invertebrates
## 24                                 Somateria spectabilis Invertebrates
## 25                                        Aythya affinis    Vegetation
## 26                                     Clangula hyemalis Invertebrates
## 27                                    Anas platyrhynchos      Omnivore
## 28                                        Anas fulvigula      Omnivore
## 29                                           Cygnus olor    Vegetation
## 30                                            Anas acuta      Omnivore
## 31                                         Anas clypeata Invertebrates
## 32                                       Mergus serrator   Vertebrates
## 33                                      Aythya americana    Vegetation
## 34                                       Aythya collaris    Vegetation
## 35                                           Chen rossii    Vegetation
## 36                                    Oxyura jamaicensis Invertebrates
## 37                                     Chen caerulescens    Vegetation
## 38                                   Polysticta stelleri Invertebrates
## 39                               Melanitta perspicillata Invertebrates
## 40                                     Cygnus buccinator    Vegetation
## 41                                       Aythya fuligula    Vegetation
## 42                                    Cygnus columbianus    Vegetation
## 43                                       Melanitta fusca Invertebrates
## 44                                            Aix sponsa          Seed
## 45                                        Chaetura vauxi Invertebrates
## 46                                  Aeronautes saxatalis Invertebrates
## 47                                     Selasphorus sasin        Nectar
## 48                                          Calypte anna        Nectar
## 49                                 Archilochus alexandri        Nectar
## 50                                  Lampornis clemenciae        Nectar
## 51                                 Cynanthus latirostris        Nectar
## 52                               Selasphorus platycercus        Nectar
## 53                                 Amazilia yucatanensis        Nectar
## 54                                     Stellula calliope        Nectar
## 55                                        Calypte costae        Nectar
## 56                                       Eugenes fulgens        Nectar
## 57                                  Archilochus colubris        Nectar
## 58                                     Selasphorus rufus        Nectar
## 59                                    Amazilia violiceps        Nectar
## 60                              Caprimulgus carolinensis Invertebrates
## 61                                Nyctidromus albicollis Invertebrates
## 62                              Phalaenoptilus nuttallii Invertebrates
## 63                                Chordeiles acutipennis Invertebrates
## 64                 Caprimulgus vociferus and C. arizonae Invertebrates
## 65                             Synthliboramphus antiquus Invertebrates
## 66                                    Fratercula arctica   Vertebrates
## 67                                        Cepphus grylle   Vertebrates
## 68                               Ptychoramphus aleuticus Invertebrates
## 69                                            Uria aalge   Vertebrates
## 70                                             Alle alle Invertebrates
## 71                              Brachyramphus marmoratus Invertebrates
## 72                                       Cepphus columba   Vertebrates
## 73                                            Alca torda   Vertebrates
## 74                                 Cerorhinca monocerata   Vertebrates
## 75                                           Uria lomvia   Vertebrates
## 76                               Recurvirostra americana Invertebrates
## 77                                  Haematopus palliatus Invertebrates
## 78                                   Haematopus bachmani Invertebrates
## 79                                  Pluvialis squatarola Invertebrates
## 80                                  Himantopus mexicanus Invertebrates
## 81                                  Charadrius vociferus Invertebrates
## 82                                   Charadrius montanus Invertebrates
## 83                                       Pluvialis fulva Invertebrates
## 84                                    Charadrius melodus Invertebrates
## 85                               Charadrius semipalmatus Invertebrates
## 86                               Charadrius alexandrinus Invertebrates
## 87                                   Charadrius wilsonia Invertebrates
## 88                            Chroicocephalus ridibundus Invertebrates
## 89                                      Rissa tridactyla Invertebrates
## 90                          Chroicocephalus philadelphia      Omnivore
## 91                                    Larus californicus      Omnivore
## 92                                    Thalasseus elegans   Vertebrates
## 93                                  Leucophaeus pipixcan Invertebrates
## 94                                     Larus hyperboreus      Omnivore
## 95                                     Larus glaucescens      Omnivore
## 96                                         Larus marinus      Omnivore
## 97                                       Larus heermanni   Vertebrates
## 98                                      Larus argentatus      Omnivore
## 99                                 Leucophaeus atricilla Invertebrates
## 100                                  Sternula antillarum   Vertebrates
## 101                                         Larus fuscus   Vertebrates
## 102                                 Hydrocoloeus minutus Invertebrates
## 103                                          Larus canus Invertebrates
## 104                                   Larus delawarensis   Vertebrates
## 105                      Larus glaucoides and L. thayeri   Vertebrates
## 106                                   Larus occidentalis      Omnivore
## 107                                         Larus livens   Vertebrates
## 108                                       Rynchops niger   Vertebrates
## 109                                       Scolopax minor Invertebrates
## 110                               Arenaria melanocephala Invertebrates
## 111                                      Calidris alpina Invertebrates
## 112                                   Tringa melanoleuca Invertebrates
## 113                                   Calidris minutilla Invertebrates
## 114                                      Tringa flavipes Invertebrates
## 115                                  Numenius americanus Invertebrates
## 116                              Limnodromus scolopaceus Invertebrates
## 117                                         Limosa fedoa Invertebrates
## 118                                   Calidris melanotos Invertebrates
## 119                                    Calidris maritima Invertebrates
## 120                                     Calidris canutus Invertebrates
## 121                                   Phalaropus lobatus Invertebrates
## 122                                 Calidris ptilocnemis Invertebrates
## 123                                   Arenaria interpres Invertebrates
## 124                                        Calidris alba Invertebrates
## 125                                     Calidris pusilla Invertebrates
## 126                                  Limnodromus griseus Invertebrates
## 127                                     Tringa solitaria Invertebrates
## 128                                    Actitis macularia Invertebrates
## 129                                  Calidris himantopus      Omnivore
## 130                                      Aphriza virgata Invertebrates
## 131                                        Tringa incana Invertebrates
## 132                                       Calidris mauri Invertebrates
## 133                                    Numenius phaeopus Invertebrates
## 134                                 Calidris fuscicollis Invertebrates
## 135                                   Tringa semipalmata Invertebrates
## 136                                  Phalaropus tricolor Invertebrates
## 137                                  Gallinago gallinago Invertebrates
## 138                             Stercorarius parasiticus   Vertebrates
## 139                               Stercorarius pomarinus   Vertebrates
## 140                                   Hydroprogne caspia   Vertebrates
## 141                                       Sterna hirundo   Vertebrates
## 142                                      Sterna forsteri   Vertebrates
## 143                                Gelochelidon nilotica      Omnivore
## 144                                   Thalasseus maximus   Vertebrates
## 145                              Thalasseus sandvicensis      Omnivore
## 146                                      Anhinga anhinga   Vertebrates
## 147                                Botaurus lentiginosus   Vertebrates
## 148                                Nycticorax nycticorax   Vertebrates
## 149                                        Bubulcus ibis Invertebrates
## 150                                       Ardea herodias   Vertebrates
## 151                                           Ardea alba   Vertebrates
## 152                                  Butorides virescens   Vertebrates
## 153                                    Ixobrychus exilis   Vertebrates
## 154                                     Egretta caerulea   Vertebrates
## 155                                    Egretta rufescens   Vertebrates
## 156                                        Egretta thula Invertebrates
## 157                                     Egretta tricolor   Vertebrates
## 158                                  Nyctanassa violacea Invertebrates
## 159                                   Mycteria americana   Vertebrates
## 160                                  Fregata magnificens   Vertebrates
## 161                            Pelecanus erythrorhynchos   Vertebrates
## 162                               Pelecanus occidentalis   Vertebrates
## 163                           Phalacrocorax penicillatus   Vertebrates
## 164                                Phalacrocorax auritus   Vertebrates
## 165                                  Phalacrocorax carbo   Vertebrates
## 166                            Phalacrocorax brasilianus   Vertebrates
## 167                              Phalacrocorax pelagicus   Vertebrates
## 168                                  Phalacrocorax urile   Vertebrates
## 169                                     Sula leucogaster   Vertebrates
## 170                                       Morus bassanus   Vertebrates
## 171                                 Plegadis falcinellus Invertebrates
## 172                                          Ajaia ajaja   Vertebrates
## 173                                      Eudocimus albus Invertebrates
## 174                                       Plegadis chihi Invertebrates
## 175                                 Patagioenas fasciata          Seed
## 176                                  Columbina passerina          Seed
## 177                                Streptopelia decaocto          Seed
## 178                                       Columbina inca          Seed
## 179                                     Zenaida macroura          Seed
## 180                             Patagioenas flavirostris         Fruit
## 181                                  Columbina talpacoti          Seed
## 182                               Streptopelia chinensis          Seed
## 183                             Patagioenas leucocephala          Seed
## 184                                  Leptotila verreauxi          Seed
## 185                                     Zenaida asiatica          Seed
## 186                                    Megaceryle alcyon   Vertebrates
## 187                               Chloroceryle americana   Vertebrates
## 188                                  Megaceryle torquata   Vertebrates
## 189                              Crotophaga sulcirostris Invertebrates
## 190                                       Crotophaga ani Invertebrates
## 191                              Geococcyx californianus   Vertebrates
## 192                             Haliaeetus leucocephalus   Vertebrates
## 193                                    Buteo platypterus   Vertebrates
## 194                                   Accipiter cooperii   Vertebrates
## 195                                        Buteo regalis   Vertebrates
## 196                                    Aquila chrysaetos   Vertebrates
## 197                                        Buteo nitidus   Vertebrates
## 198                                 Parabuteo unicinctus   Vertebrates
## 199                              Chondrohierax uncinatus Invertebrates
## 200                                   Accipiter gentilis   Vertebrates
## 201                                       Circus cyaneus   Vertebrates
## 202                                       Buteo lineatus Invertebrates
## 203                                    Buteo jamaicensis   Vertebrates
## 204                                        Buteo lagopus   Vertebrates
## 205                                   Accipiter striatus   Vertebrates
## 206                                     Buteo brachyurus   Vertebrates
## 207                                Rostrhamus sociabilis Invertebrates
## 208                                      Buteo swainsoni   Vertebrates
## 209                                   Buteo albicaudatus   Vertebrates
## 210                                      Elanus leucurus   Vertebrates
## 211                                    Buteo albonotatus   Vertebrates
## 212                                     Coragyps atratus   Vertebrates
## 213                              Gymnogyps californianus   Vertebrates
## 214                                       Cathartes aura   Vertebrates
## 215                                     Falco sparverius Invertebrates
## 216                                      Falco femoralis   Vertebrates
## 217                                    Caracara cheriway   Vertebrates
## 218                                     Falco rusticolus   Vertebrates
## 219                                    Falco columbarius   Vertebrates
## 220                                     Falco peregrinus   Vertebrates
## 221                                      Falco mexicanus   Vertebrates
## 222                                    Pandion haliaetus   Vertebrates
## 223                                       Ortalis vetula          Seed
## 224                                  Meleagris gallopavo      Omnivore
## 225                               Callipepla californica      Omnivore
## 226                                  Callipepla gambelii          Seed
## 227                                  Cyrtonyx montezumae      Omnivore
## 228                                      Oreortyx pictus          Seed
## 229                                  Colinus virginianus          Seed
## 230                                  Callipepla squamata          Seed
## 231                                     Alectoris chukar          Seed
## 232                                        Perdix perdix          Seed
## 233                                  Phasianus colchicus    Vegetation
## 234                                 Dendragapus obscurus    Vegetation
## 235                                   Tympanuchus cupido          Seed
## 236                            Centrocercus urophasianus    Vegetation
## 237                           Tympanuchus pallidicinctus      Omnivore
## 238                                        Lagopus mutus    Vegetation
## 239                                      Bonasa umbellus    Vegetation
## 240                             Tympanuchus phasianellus    Vegetation
## 241                               Falcipennis canadensis      Omnivore
## 242                                      Lagopus leucura    Vegetation
## 243                                      Lagopus lagopus    Vegetation
## 244                                          Gavia immer   Vertebrates
## 245                        Gavia arctica and G. pacifica   Vertebrates
## 246                                       Gavia stellata   Vertebrates
## 247                                        Gavia adamsii   Vertebrates
## 248                                      Aramus guarauna Invertebrates
## 249                                      Grus canadensis      Omnivore
## 250                                       Grus americana Invertebrates
## 251                                     Fulica americana    Vegetation
## 252                               Laterallus jamaicensis Invertebrates
## 253                                  Rallus longirostris      Omnivore
## 254                                  Gallinula chloropus      Omnivore
## 255                                       Rallus elegans Invertebrates
## 256                                 Porphyrio martinicus      Omnivore
## 257                                     Porzana carolina      Omnivore
## 258                                      Rallus limicola Invertebrates
## 259                           Coturnicops noveboracensis Invertebrates
## 260                                 Psaltriparus minimus Invertebrates
## 261                                 Eremophila alpestris          Seed
## 262                                      Alauda arvensis          Seed
## 263                                  Bombycilla garrulus         Fruit
## 264                                  Bombycilla cedrorum         Fruit
## 265                               Rhynchophanes mccownii          Seed
## 266                            Pheucticus melanocephalus      Omnivore
## 267                                   Passerina caerulea      Omnivore
## 268                                      Spiza americana          Seed
## 269                                        Piranga flava      Omnivore
## 270                                     Passerina cyanea          Seed
## 271                                     Passerina amoena          Seed
## 272                                Cardinalis cardinalis          Seed
## 273                                      Passerina ciris          Seed
## 274                                  Cardinalis sinuatus      Omnivore
## 275                              Pheucticus ludovicianus         Fruit
## 276                                        Piranga rubra      Omnivore
## 277                                  Piranga ludoviciana      Omnivore
## 278                                    Certhia americana Invertebrates
## 279                                    Cinclus mexicanus Invertebrates
## 280                                Corvus brachyrhynchos      Omnivore
## 281                                        Pica hudsonia      Omnivore
## 282                                  Cyanocitta cristata      Omnivore
## 283                                    Psilorhinus morio      Omnivore
## 284                                  Corvus cryptoleucus      Omnivore
## 285                                 Nucifraga columbiana          Seed
## 286                                         Corvus corax      Omnivore
## 287                                    Corvus ossifragus      Omnivore
## 288                              Aphelocoma coerulescens      Omnivore
## 289                                Perisoreus canadensis      Omnivore
## 290                                     Cyanocorax yncas      Omnivore
## 291                               Aphelocoma ultramarina      Omnivore
## 292                                      Corvus caurinus      Omnivore
## 293                            Gymnorhinus cyanocephalus      Omnivore
## 294                                  Cyanocitta stelleri      Omnivore
## 295                               Aphelocoma californica      Omnivore
## 296                                        Pica nuttalli      Omnivore
## 297                                      Melozone aberti Invertebrates
## 298                                     Spizella arborea      Omnivore
## 299                                   Peucaea aestivalis      Omnivore
## 300                                   Ammodramus bairdii          Seed
## 301                                 Spizella atrogularis      Omnivore
## 302                                 Amphispiza bilineata      Omnivore
## 303                                     Spizella breweri      Omnivore
## 304                     Melozone crissalis and M. fuscus          Seed
## 305                                     Peucaea cassinii      Omnivore
## 306                                    Calcarius ornatus      Omnivore
## 307                                   Spizella passerina          Seed
## 308                                     Spizella pallida          Seed
## 309                                       Junco hyemalis          Seed
## 310            Pipilo erythrophthalmus  and P. maculatus      Omnivore
## 311                                     Spizella pusilla          Seed
## 312                            Amphispiza quinquestriata Invertebrates
## 313                                    Passerella iliaca      Omnivore
## 314                              Zonotrichia atricapilla      Omnivore
## 315                                Ammodramus savannarum Invertebrates
## 316                                     Pipilo chlorurus      Omnivore
## 317                                  Zonotrichia querula          Seed
## 318                                 Ammodramus henslowii Invertebrates
## 319                                 Calcarius lapponicus          Seed
## 320                              Calamospiza melanocorys          Seed
## 321                                 Chondestes grammacus      Omnivore
## 322                                 Ammodramus leconteii      Omnivore
## 323                                  Melospiza lincolnii      Omnivore
## 324                            Plectrophenax hyperboreus      Omnivore
## 325                              Arremonops rufivirgatus      Omnivore
## 326                                   Aimophila ruficeps      Omnivore
## 327                                     Peucaea carpalis      Omnivore
## 328                   Amphispiza belli and A. nevadensis          Seed
## 329                            Passerculus sandwichensis      Omnivore
## 330                                 Ammodramus maritimus Invertebrates
## 331                 Ammodramus nelsoni and A. caudacutus      Omnivore
## 332                                     Calcarius pictus      Omnivore
## 333                                Plectrophenax nivalis          Seed
## 334                                    Melospiza melodia      Omnivore
## 335                                  Melospiza georgiana      Omnivore
## 336                                  Pooecetes gramineus          Seed
## 337                               Zonotrichia leucophrys          Seed
## 338                               Zonotrichia albicollis          Seed
## 339                                     Junco phaeonotus      Omnivore
## 340                                       Spinus tristis          Seed
## 341 Leucosticte atrata, L. australis, and L. tephrocotis      Omnivore
## 342                                  Carpodacus cassinii          Seed
## 343                                     Acanthis flammea          Seed
## 344                           Coccothraustes vespertinus      Omnivore
## 345                                  Acanthis hornemanni          Seed
## 346                                 Carpodacus mexicanus          Seed
## 347                                     Spinus lawrencei          Seed
## 348                                      Spinus psaltria          Seed
## 349                                  Pinicola enucleator          Seed
## 350                                         Spinus pinus          Seed
## 351                                 Carpodacus purpureus          Seed
## 352                                    Loxia curvirostra          Seed
## 353                                     Loxia leucoptera          Seed
## 354                                      Hirundo rustica Invertebrates
## 355                                  Petrochelidon fulva Invertebrates
## 356                           Stelgidopteryx serripennis Invertebrates
## 357                                  Tachycineta bicolor Invertebrates
## 358                               Tachycineta thalassina Invertebrates
## 359                                      Icterus gularis Invertebrates
## 360                                  Icterus graduacauda      Omnivore
## 361                                      Icterus galbula      Omnivore
## 362                     Quiscalus major and Q. mexicanus      Omnivore
## 363                               Euphagus cyanocephalus Invertebrates
## 364                                     Molothrus aeneus      Omnivore
## 365                                       Molothrus ater      Omnivore
## 366                                    Icterus bullockii      Omnivore
## 367                                   Quiscalus quiscula      Omnivore
## 368                                      Sturnella magna Invertebrates
## 369                                   Icterus cucullatus Invertebrates
## 370                                      Icterus spurius      Omnivore
## 371                                  Agelaius phoeniceus      Omnivore
## 372                                   Euphagus carolinus      Omnivore
## 373                                    Icterus parisorum Invertebrates
## 374                                Molothrus bonariensis      Omnivore
## 375                                   Icterus pectoralis      Omnivore
## 376                                    Agelaius tricolor Invertebrates
## 377                                   Sturnella neglecta Invertebrates
## 378                        Xanthocephalus xanthocephalus      Omnivore
## 379                                  Lanius ludovicianus Invertebrates
## 380                                     Lanius excubitor   Vertebrates
## 381                                   Toxostoma bendirei Invertebrates
## 382                                      Toxostoma rufum      Omnivore
## 383                                  Toxostoma redivivum Invertebrates
## 384                                   Toxostoma crissale Invertebrates
## 385                                Toxostoma curvirostre Invertebrates
## 386                               Dumetella carolinensis      Omnivore
## 387                                   Toxostoma lecontei Invertebrates
## 388                                Toxostoma longirostre Invertebrates
## 389                                    Mimus polyglottos      Omnivore
## 390                                 Oreoscoptes montanus Invertebrates
## 391                                     Anthus rubescens      Omnivore
## 392                                     Anthus spragueii      Omnivore
## 393                                 Poecile atricapillus Invertebrates
## 394                                   Poecile hudsonicus      Omnivore
## 395                                Baeolophus wollweberi Invertebrates
## 396                                 Poecile carolinensis Invertebrates
## 397                                    Poecile rufescens      Omnivore
## 398                                       Parus sclateri Invertebrates
## 399                                      Poecile gambeli Invertebrates
## 400                 Baeolophus ridgwayi and B. inornatus      Omnivore
## 401              Baeolophus atricristatus and B. bicolor Invertebrates
## 402                                  Auriparus flaviceps Invertebrates
## 403                                  Setophaga ruticilla Invertebrates
## 404                                      Mniotilta varia Invertebrates
## 405                               Dendroica caerulescens Invertebrates
## 406                                 Dendroica nigrescens Invertebrates
## 407                                     Dendroica virens Invertebrates
## 408                                    Dendroica tigrina Invertebrates
## 409                               Dendroica pensylvanica Invertebrates
## 410                                   Geothlypis trichas Invertebrates
## 411                               Dendroica occidentalis Invertebrates
## 412                                   Parkesia motacilla Invertebrates
## 413                                    Oporornis tolmiei Invertebrates
## 414                                   Dendroica magnolia Invertebrates
## 415                              Oreothlypis ruficapilla Invertebrates
## 416                                     Parula americana Invertebrates
## 417                              Parkesia noveboracensis Invertebrates
## 418                                   Oreothlypis celata Invertebrates
## 419                                 Seiurus aurocapillus Invertebrates
## 420                                     Myioborus pictus Invertebrates
## 421                                   Dendroica palmarum Invertebrates
## 422                                      Dendroica pinus Invertebrates
## 423                                   Dendroica discolor Invertebrates
## 424                                  Protonotaria citrea Invertebrates
## 425                                Oreothlypis peregrina Invertebrates
## 426                                  Dendroica townsendi Invertebrates
## 427                                     Parula pitiayumi Invertebrates
## 428                                     Wilsonia pusilla Invertebrates
## 429                               Helmitheros vermivorus Invertebrates
## 430                                   Dendroica petechia Invertebrates
## 431                                       Icteria virens      Omnivore
## 432                                   Dendroica coronata Invertebrates
## 433                                   Dendroica dominica Invertebrates
## 434                                      Passer montanus          Seed
## 435                                    Passer domesticus          Seed
## 436                                Peucedramus taeniatus Invertebrates
## 437                                  Polioptila caerulea Invertebrates
## 438                               Polioptila californica Invertebrates
## 439                                   Phainopepla nitens         Fruit
## 440                                      Regulus satrapa Invertebrates
## 441                                    Regulus calendula Invertebrates
## 442                                        Sitta pusilla          Seed
## 443                                        Sitta pygmaea          Seed
## 444                                     Sitta canadensis          Seed
## 445                                   Sitta carolinensis          Seed
## 446                                 Acridotheres tristis      Omnivore
## 447                            Acridotheres cristatellus      Omnivore
## 448                                     Sturnus vulgaris      Omnivore
## 449                                    Gracula religiosa         Fruit
## 450                                 Sporophila torqueola          Seed
## 451                                     Chamaea fasciata      Omnivore
## 452                                  Thryomanes bewickii Invertebrates
## 453                      Campylorhynchus brunneicapillus Invertebrates
## 454                                  Catherpes mexicanus Invertebrates
## 455                             Thryothorus ludovicianus Invertebrates
## 456                                    Troglodytes aedon Invertebrates
## 457                                Cistothorus palustris Invertebrates
## 458                                 Salpinctes obsoletus Invertebrates
## 459                                Cistothorus platensis Invertebrates
## 460                              Troglodytes troglodytes Invertebrates
## 461                                   Turdus migratorius         Fruit
## 462                                         Turdus grayi Invertebrates
## 463                                        Sialia sialis Invertebrates
## 464                                    Catharus guttatus      Omnivore
## 465                                   Sialia currucoides Invertebrates
## 466                                  Myadestes townsendi      Omnivore
## 467                                      Ixoreus naevius      Omnivore
## 468                                      Sialia mexicana Invertebrates
## 469                                 Hylocichla mustelina Invertebrates
## 470                                Myiarchus cinerascens Invertebrates
## 471                                   Sayornis nigricans Invertebrates
## 472                                 Myiarchus tyrannulus Invertebrates
## 473                                  Tyrannus vociferans Invertebrates
## 474                                Empidonax oberholseri Invertebrates
## 475                               Myiarchus tuberculifer Invertebrates
## 476                                      Sayornis phoebe Invertebrates
## 477                                      Contopus virens Invertebrates
## 478                                   Empidonax wrightii Invertebrates
## 479                                   Myiarchus crinitus Invertebrates
## 480                                 Pitangus sulphuratus Invertebrates
## 481                                    Contopus pertinax Invertebrates
## 482                                  Empidonax hammondii Invertebrates
## 483                                    Empidonax minimus Invertebrates
## 484                                  Camptostoma imberbe Invertebrates
## 485                                 Empidonax difficilis Invertebrates
## 486                                        Sayornis saya Invertebrates
## 487                                  Tyrannus forficatus Invertebrates
## 488                               Tyrannus crassirostris Invertebrates
## 489                Tyrannus couchii and T. melancholicus Invertebrates
## 490                                 Pyrocephalus rubinus Invertebrates
## 491                                  Tyrannus verticalis Invertebrates
## 492                                         Vireo bellii Invertebrates
## 493                                        Vireo huttoni Invertebrates
## 494        Vireo solitarius, V. cassini, and V. plumbeus Invertebrates
## 495                                        Vireo griseus Invertebrates
## 496                                     Vireo flavifrons Invertebrates
## 497                              Melanerpes formicivorus          Seed
## 498                                    Picoides dorsalis Invertebrates
## 499                                    Picoides arizonae Invertebrates
## 500                                    Picoides arcticus Invertebrates
## 501                                   Picoides pubescens      Omnivore
## 502                               Melanerpes uropygialis      Omnivore
## 503                                  Colaptes chrysoides Invertebrates
## 504                                 Melanerpes aurifrons      Omnivore
## 505                                    Picoides villosus      Omnivore
## 506                                    Picoides scalaris Invertebrates
## 507                                     Melanerpes lewis      Omnivore
## 508                                     Colaptes auratus Invertebrates
## 509                                   Picoides nuttallii Invertebrates
## 510                                   Dryocopus pileatus Invertebrates
## 511                                 Melanerpes carolinus      Omnivore
## 512                                    Sphyrapicus ruber      Omnivore
## 513                                    Picoides borealis Invertebrates
## 514                           Melanerpes erythrocephalus      Omnivore
## 515                                 Sphyrapicus nuchalis      Omnivore
## 516                                Picoides albolarvatus      Omnivore
## 517                               Sphyrapicus thyroideus      Omnivore
## 518                                   Sphyrapicus varius      Omnivore
## 519             Aechmophorus clarkii and A. occidentalis   Vertebrates
## 520                                 Podiceps nigricollis Invertebrates
## 521                                     Podiceps auritus   Vertebrates
## 522                                Tachybaptus dominicus Invertebrates
## 523                                  Podilymbus podiceps Invertebrates
## 524                                   Podiceps grisegena Invertebrates
## 525                                 Phoebastria nigripes   Vertebrates
## 526                                   Fulmarus glacialis   Vertebrates
## 527                                Puffinus tenuirostris   Vertebrates
## 528                                     Puffinus griseus   Vertebrates
## 529                              Melopsittacus undulatus          Seed
## 530                                  Aratinga holochlora         Fruit
## 531                                  Myiopsitta monachus          Seed
## 532                                Amazona viridigenalis         Fruit
## 533                                   Psittacula krameri         Fruit
## 534                              Brotogeris versicolurus         Fruit
## 535                                          Strix varia   Vertebrates
## 536                                    Aegolius funereus   Vertebrates
## 537                                   Athene cunicularia   Vertebrates
## 538                    Megascops asio and M. kennicottii   Vertebrates
## 539                               Glaucidium brasilianum Invertebrates
## 540                                       Strix nebulosa   Vertebrates
## 541                                     Bubo virginianus   Vertebrates
## 542                                            Asio otus   Vertebrates
## 543                                         Surnia ulula   Vertebrates
## 544                              Glaucidium californicum Invertebrates
## 545                                    Aegolius acadicus   Vertebrates
## 546                                        Asio flammeus   Vertebrates
## 547                                      Bubo scandiacus   Vertebrates
## 548                                   Strix occidentalis   Vertebrates
## 549                                 Megascops trichopsis   Vertebrates
## 550                                            Tyto alba   Vertebrates
## 551                                       Trogon elegans      Omnivore
##     life_expectancy   habitat urban_affiliate migratory_strategy log10_mass
## 1              Long   Wetland              No              Short       3.09
## 2            Middle   Wetland              No              Short       2.88
## 3            Middle   Wetland              No           Moderate       2.96
## 4              Long   Wetland              No           Moderate       3.11
## 5            Middle   Wetland              No           Moderate       3.02
## 6             Short   Wetland              No         Withdrawal       2.88
## 7            Middle   Wetland              No           Moderate       2.56
## 8            Middle   Wetland              No              Short       2.60
## 9            Middle   Wetland             Yes              Short       3.45
## 10           Middle   Wetland              No              Short       3.08
## 11           Middle   Wetland              No              Short       2.58
## 12           Middle     Ocean              No              Short       3.31
## 13           Middle   Wetland              No              Short       2.96
## 14           Middle   Wetland              No              Short       3.16
## 15           Middle   Wetland              No           Moderate       3.33
## 16             Long   Wetland              No               Long       2.89
## 17            Short   Wetland              No         Withdrawal       2.88
## 18           Middle   Wetland              No              Short       2.96
## 19           Middle   Wetland              No           Moderate       3.00
## 20             Long   Wetland              No           Moderate       3.40
## 21             Long   Wetland              No              Short       2.53
## 22           Middle   Wetland              No           Moderate       2.75
## 23           Middle   Wetland              No              Short       2.79
## 24           Middle   Wetland              No           Moderate       3.21
## 25           Middle   Wetland              No              Short       2.91
## 26           Middle   Wetland              No           Moderate       2.94
## 27             Long   Wetland             Yes              Short       2.93
## 28            Short   Wetland              No         Withdrawal       2.99
## 29           Middle   Wetland             Yes           Resident       4.03
## 30             Long   Wetland              No              Short       2.98
## 31           Middle   Wetland              No              Short       2.79
## 32            Short   Wetland              No           Moderate       3.01
## 33           Middle   Wetland              No              Short       3.03
## 34           Middle   Wetland              No              Short       2.85
## 35           Middle   Wetland              No           Moderate       3.16
## 36            Short   Wetland              No              Short       2.78
## 37           Middle   Wetland              No           Moderate       3.42
## 38           Middle   Wetland              No           Moderate       2.91
## 39            Short   Wetland              No           Moderate       3.00
## 40           Middle   Wetland              No              Short       4.04
## 41           Middle   Wetland             Yes               Long       2.85
## 42           Middle   Wetland              No           Moderate       3.81
## 43           Middle   Wetland              No           Moderate       3.26
## 44           Middle   Wetland              No              Short       2.82
## 45            Short  Woodland              No           Moderate       1.28
## 46            Short   Various              No              Short       1.49
## 47            Short Shrubland             Yes           Moderate       0.53
## 48            Short  Woodland             Yes              Short       0.63
## 49           Middle  Woodland             Yes              Short       0.48
## 50            Short  Woodland              No              Short       0.88
## 51            Short  Woodland             Yes         Withdrawal       0.48
## 52           Middle  Woodland             Yes           Moderate       0.55
## 53           Middle  Woodland             Yes           Resident       0.60
## 54            Short  Woodland              No           Moderate       0.48
## 55            Short Shrubland              No              Short       0.48
## 56            Short  Woodland              No              Short       0.90
## 57            Short  Woodland             Yes           Moderate       0.49
## 58            Short  Woodland              No           Moderate       0.65
## 59            Short  Woodland              No              Short       0.70
## 60           Middle  Woodland              No              Short       2.04
## 61           Middle Shrubland              No           Resident       1.76
## 62            Short Shrubland              No              Short       1.68
## 63            Short Shrubland              No              Short       1.69
## 64           Middle  Woodland              No           Moderate       1.72
## 65            Short     Ocean              No              Short       2.34
## 66             Long     Ocean              No              Short       2.77
## 67             Long     Ocean              No         Withdrawal       2.58
## 68           Middle     Ocean              No              Short       2.27
## 69             Long     Ocean              No              Short       3.00
## 70           Middle     Ocean              No         Withdrawal       2.26
## 71            Short     Ocean              No         Withdrawal       2.34
## 72           Middle     Ocean              No         Withdrawal       2.72
## 73             Long     Ocean              No              Short       2.86
## 74            Short     Ocean              No         Withdrawal       2.68
## 75             Long     Ocean              No              Short       2.98
## 76            Short   Wetland              No              Short       2.48
## 77           Middle     Ocean              No         Withdrawal       2.74
## 78           Middle     Ocean              No           Resident       2.74
## 79           Middle   Wetland              No           Moderate       2.40
## 80           Middle   Wetland              No              Short       2.22
## 81           Middle   Various             Yes              Short       1.98
## 82            Short Grassland              No           Moderate       1.98
## 83            Short Grassland              No               Long       2.18
## 84           Middle     Ocean              No           Moderate       1.72
## 85            Short   Wetland              No               Long       1.67
## 86           Middle     Ocean              No              Short       1.63
## 87           Middle     Ocean              No              Short       1.74
## 88             Long   Wetland              No           Moderate       2.45
## 89             Long     Ocean              No           Moderate       2.62
## 90           Middle   Wetland              No           Moderate       2.32
## 91             Long   Wetland             Yes              Short       2.84
## 92           Middle     Ocean              No               Long       2.41
## 93            Short   Wetland              No               Long       2.45
## 94           Middle   Wetland              No           Moderate       3.18
## 95             Long     Ocean             Yes              Short       3.02
## 96             Long   Wetland             Yes              Short       3.22
## 97            Short     Ocean              No         Withdrawal       2.70
## 98             Long   Wetland             Yes              Short       3.04
## 99           Middle     Ocean             Yes         Withdrawal       2.49
## 100          Middle     Ocean             Yes           Moderate       1.67
## 101            Long     Ocean             Yes           Moderate       2.88
## 102          Middle   Wetland              No           Moderate       2.07
## 103            Long     Ocean              No              Short       2.62
## 104          Middle   Wetland             Yes              Short       2.71
## 105          Middle   Wetland              No           Moderate       2.97
## 106            Long     Ocean             Yes         Withdrawal       2.97
## 107          Middle   Wetland              No              Short       3.12
## 108          Middle     Ocean              No         Withdrawal       2.47
## 109           Short   Wetland              No              Short       2.29
## 110           Short   Wetland              No           Moderate       2.10
## 111            Long   Wetland              No           Moderate       1.72
## 112           Short   Wetland              No           Moderate       2.21
## 113          Middle   Wetland              No           Moderate       1.36
## 114           Short   Wetland              No           Moderate       1.89
## 115          Middle Grassland              No           Moderate       2.77
## 116          Middle   Wetland              No           Moderate       2.02
## 117          Middle   Wetland              No           Moderate       2.55
## 118           Short   Wetland              No               Long       1.90
## 119          Middle   Wetland              No               Long       1.81
## 120            Long   Wetland              No               Long       2.15
## 121           Short   Wetland              No               Long       1.56
## 122           Short   Wetland              No           Moderate       1.97
## 123          Middle   Wetland              No           Moderate       2.13
## 124          Middle   Wetland              No               Long       1.71
## 125          Middle   Wetland              No               Long       1.44
## 126          Middle   Wetland              No           Moderate       2.04
## 127          Middle   Wetland              No               Long       1.69
## 128          Middle   Wetland              No           Moderate       1.62
## 129          Middle   Wetland              No               Long       1.80
## 130           Short   Wetland              No           Moderate       2.30
## 131          Middle   Wetland              No               Long       2.07
## 132           Short   Wetland              No               Long       1.44
## 133          Middle   Wetland              No           Moderate       2.56
## 134           Short   Wetland              No               Long       1.63
## 135          Middle   Wetland              No           Moderate       2.39
## 136           Short   Wetland              No           Moderate       1.78
## 137          Middle   Wetland              No              Short       2.05
## 138            Long     Ocean              No           Moderate       2.65
## 139          Middle     Ocean              No           Moderate       2.84
## 140            Long   Wetland              No           Moderate       2.81
## 141           Short   Wetland              No           Moderate       2.11
## 142          Middle   Wetland              No           Moderate       2.17
## 143          Middle     Ocean              No           Moderate       2.23
## 144          Middle     Ocean              No              Short       2.65
## 145          Middle     Ocean              No              Short       2.30
## 146          Middle   Wetland              No         Withdrawal       3.13
## 147           Short   Wetland              No              Short       2.85
## 148          Middle   Wetland             Yes              Short       2.91
## 149          Middle   Wetland             Yes              Short       2.58
## 150          Middle   Wetland              No              Short       3.40
## 151          Middle   Wetland              No              Short       2.98
## 152          Middle   Wetland              No              Short       2.38
## 153           Short   Wetland              No              Short       1.94
## 154          Middle   Wetland              No              Short       2.53
## 155          Middle   Wetland              No         Withdrawal       2.79
## 156          Middle   Wetland              No              Short       2.57
## 157          Middle   Wetland              No              Short       2.50
## 158           Short   Wetland              No         Withdrawal       2.83
## 159            Long   Wetland              No         Withdrawal       3.41
## 160            Long     Ocean              No         Withdrawal       3.17
## 161            Long   Wetland              No           Moderate       3.75
## 162            Long     Ocean             Yes         Withdrawal       3.54
## 163          Middle     Ocean              No         Withdrawal       3.35
## 164          Middle   Wetland             Yes              Short       3.26
## 165          Middle     Ocean              No              Short       3.40
## 166          Middle   Wetland              No         Withdrawal       3.15
## 167          Middle     Ocean              No              Short       3.27
## 168          Middle     Ocean              No           Resident       3.33
## 169            Long     Ocean              No           Resident       3.11
## 170          Middle     Ocean              No              Short       3.48
## 171            Long   Wetland              No         Withdrawal       2.80
## 172            Long   Wetland              No         Withdrawal       3.17
## 173            Long   Wetland             Yes         Withdrawal       2.95
## 174          Middle   Wetland              No              Short       2.79
## 175          Middle  Woodland             Yes         Withdrawal       2.55
## 176           Short Shrubland              No           Resident       1.55
## 177          Middle      <NA>             Yes           Resident       2.17
## 178           Short Shrubland             Yes           Resident       1.71
## 179          Middle   Various             Yes         Withdrawal       2.08
## 180          Middle  Woodland              No           Resident       2.40
## 181           Short      <NA>             Yes           Resident       1.66
## 182           Short      <NA>             Yes           Resident       2.22
## 183          Middle  Woodland              No           Resident       2.38
## 184           Short  Woodland              No           Resident       2.17
## 185          Middle   Various             Yes         Withdrawal       2.19
## 186           Short   Wetland              No              Short       2.23
## 187           Short   Wetland              No           Resident       1.57
## 188           Short   Wetland              No           Resident       2.51
## 189           Short Shrubland              No         Withdrawal       1.91
## 190           Short Shrubland             Yes           Resident       2.04
## 191           Short Shrubland             Yes           Resident       2.57
## 192            Long   Wetland              No              Short       3.67
## 193          Middle  Woodland              No               Long       2.66
## 194          Middle  Woodland             Yes              Short       2.63
## 195          Middle Grassland              No              Short       3.17
## 196            Long   Various              No              Short       3.63
## 197          Middle  Woodland              No         Withdrawal       2.63
## 198          Middle Shrubland             Yes           Resident       2.93
## 199          Middle  Woodland              No           Resident       2.49
## 200          Middle  Woodland              No         Withdrawal       2.94
## 201          Middle   Various              No              Short       2.59
## 202          Middle  Woodland              No              Short       2.78
## 203            Long  Woodland             Yes              Short       3.04
## 204          Middle Grassland              No           Moderate       2.98
## 205          Middle  Woodland             Yes              Short       2.12
## 206          Middle  Woodland              No         Withdrawal       2.71
## 207          Middle   Wetland              No           Resident       2.57
## 208          Middle   Various              No               Long       2.98
## 209          Middle Grassland              No           Resident       3.01
## 210           Short   Various              No         Withdrawal       2.54
## 211          Middle  Woodland              No              Short       2.88
## 212            Long   Various             Yes           Resident       3.28
## 213            Long   Various              No           Resident       3.93
## 214          Middle   Various             Yes         Withdrawal       3.18
## 215          Middle   Various              No              Short       2.06
## 216          Middle Grassland              No           Resident       2.53
## 217            Long Shrubland              No           Resident       3.07
## 218          Middle Grassland              No          Irruptive       3.16
## 219          Middle  Woodland             Yes              Short       2.28
## 220          Middle   Various             Yes              Short       2.88
## 221           Short Grassland              No              Short       2.85
## 222          Middle   Wetland             Yes              Short       3.22
## 223           Short  Woodland             Yes           Resident       2.69
## 224          Middle  Woodland             Yes           Resident       3.76
## 225           Short Shrubland             Yes           Resident       2.27
## 226           Short Shrubland             Yes           Resident       2.23
## 227           Short  Woodland              No           Resident       2.27
## 228           Short Shrubland              No           Resident       2.36
## 229           Short Shrubland              No           Resident       2.24
## 230           Short Shrubland             Yes           Resident       2.27
## 231           Short   Various              No           Resident       2.70
## 232           Short Grassland              No           Resident       2.61
## 233           Short Grassland              No           Resident       3.05
## 234          Middle  Woodland              No           Resident       3.02
## 235           Short Grassland              No           Resident       2.94
## 236           Short Shrubland              No           Resident       3.28
## 237          Middle Shrubland              No           Resident       2.86
## 238           Short Grassland              No         Withdrawal       2.73
## 239           Short  Woodland              No           Resident       2.73
## 240           Short Shrubland              No           Resident       2.95
## 241          Middle  Woodland              No           Resident       3.05
## 242          Middle Grassland              No           Resident       2.55
## 243           Short Shrubland              No         Withdrawal       2.75
## 244           Short   Wetland              No           Moderate       3.70
## 245          Middle   Wetland              No           Moderate       3.35
## 246          Middle   Wetland              No           Moderate       3.17
## 247          Middle   Wetland              No           Moderate       3.69
## 248          Middle   Wetland              No           Resident       3.03
## 249          Middle   Wetland              No           Moderate       3.63
## 250          Middle   Wetland              No           Moderate       3.77
## 251           Short   Wetland              No              Short       2.75
## 252           Short   Wetland              No              Short       1.50
## 253           Short   Wetland              No           Resident       2.41
## 254          Middle   Wetland              No         Withdrawal       2.53
## 255           Short   Wetland              No              Short       2.53
## 256          Middle   Wetland              No              Short       2.32
## 257           Short   Wetland              No              Short       1.87
## 258           Short   Wetland              No              Short       1.90
## 259           Short   Wetland              No           Moderate       1.75
## 260           Short   Various             Yes           Resident       0.70
## 261           Short Grassland              No           Moderate       1.52
## 262           Short Grassland              No           Resident       1.57
## 263          Middle  Woodland             Yes          Irruptive       1.74
## 264           Short  Woodland             Yes          Irruptive       1.50
## 265           Short Grassland              No           Moderate       1.41
## 266          Middle  Woodland              No              Short       1.62
## 267           Short Shrubland              No              Short       1.47
## 268           Short Grassland              No           Moderate       1.42
## 269          Middle  Woodland              No         Withdrawal       1.57
## 270           Short  Woodland              No           Moderate       1.17
## 271           Short Shrubland              No           Moderate       1.19
## 272          Middle Shrubland             Yes           Resident       1.63
## 273          Middle Shrubland              No           Moderate       1.19
## 274           Short Shrubland              No           Resident       1.55
## 275          Middle  Woodland              No           Moderate       1.62
## 276           Short  Woodland              No           Moderate       1.45
## 277          Middle  Woodland              No           Moderate       1.48
## 278           Short  Woodland              No              Short       0.90
## 279           Short   Wetland             Yes           Resident       1.75
## 280          Middle   Various             Yes         Withdrawal       2.65
## 281          Middle  Woodland             Yes           Resident       2.34
## 282          Middle  Woodland             Yes         Withdrawal       1.94
## 283          Middle  Woodland              No           Resident       2.31
## 284          Middle Shrubland              No         Withdrawal       2.73
## 285          Middle  Woodland              No           Resident       2.11
## 286          Middle   Various             Yes           Resident       2.97
## 287          Middle   Wetland             Yes         Withdrawal       2.45
## 288          Middle Shrubland             Yes           Resident       1.89
## 289          Middle  Woodland              No           Resident       1.85
## 290          Middle  Woodland             Yes           Resident       1.90
## 291          Middle  Woodland              No           Resident       2.12
## 292          Middle   Wetland             Yes           Resident       2.59
## 293          Middle  Woodland              No           Resident       2.02
## 294          Middle  Woodland             Yes           Resident       2.11
## 295          Middle Shrubland             Yes           Resident       1.94
## 296           Short  Woodland              No           Resident       2.19
## 297           Short Shrubland             Yes           Resident       1.68
## 298          Middle Shrubland              No           Moderate       1.26
## 299           Short  Woodland              No         Withdrawal       1.29
## 300           Short Grassland              No           Moderate       1.26
## 301           Short Shrubland              No              Short       1.10
## 302           Short Shrubland              No         Withdrawal       1.11
## 303           Short Shrubland              No           Moderate       1.02
## 304          Middle Shrubland             Yes           Resident       1.69
## 305          Middle Grassland              No              Short       1.26
## 306           Short Grassland              No           Moderate       1.30
## 307           Short  Woodland             Yes              Short       1.09
## 308           Short Shrubland              No           Moderate       1.05
## 309          Middle  Woodland             Yes              Short       1.29
## 310          Middle Shrubland              No              Short       1.61
## 311           Short Shrubland              No              Short       1.10
## 312           Short Shrubland              No           Resident       1.28
## 313           Short Shrubland              No              Short       1.54
## 314           Short Shrubland              No           Moderate       1.50
## 315           Short   Various              No              Short       1.25
## 316           Short Shrubland              No              Short       1.46
## 317          Middle Shrubland              No           Moderate       1.55
## 318           Short Grassland              No           Moderate       1.11
## 319           Short Grassland              No           Moderate       1.45
## 320           Short Grassland              No              Short       1.58
## 321           Short   Various              No              Short       1.46
## 322          Middle   Wetland              No           Moderate       1.11
## 323           Short Shrubland              No           Moderate       1.26
## 324           Short Grassland              No           Moderate       1.63
## 325           Short Shrubland              No           Resident       1.34
## 326           Short Shrubland              No           Resident       1.28
## 327           Short Shrubland              No           Resident       1.18
## 328           Short Shrubland              No              Short       1.23
## 329           Short Grassland              No              Short       1.33
## 330           Short   Wetland              No         Withdrawal       1.35
## 331           Short   Wetland              No              Short       1.23
## 332           Short Grassland              No           Moderate       1.45
## 333           Short Grassland              No           Moderate       1.63
## 334          Middle   Various             Yes              Short       1.38
## 335           Short   Wetland              No              Short       1.24
## 336           Short Grassland              No           Moderate       1.41
## 337          Middle   Various             Yes           Moderate       1.45
## 338           Short  Woodland              No              Short       1.39
## 339           Short  Woodland              No           Resident       1.30
## 340          Middle   Various             Yes              Short       1.19
## 341           Short   Various              No              Short       1.40
## 342           Short  Woodland              No              Short       1.42
## 343           Short Shrubland              No          Irruptive       1.15
## 344          Middle  Woodland              No          Irruptive       1.79
## 345           Short Shrubland              No          Irruptive       1.18
## 346          Middle   Various             Yes           Resident       1.33
## 347           Short   Various              No          Irruptive       1.04
## 348           Short   Various             Yes              Short       0.95
## 349           Short  Woodland              No          Irruptive       1.75
## 350          Middle  Woodland             Yes          Irruptive       1.10
## 351          Middle  Woodland             Yes              Short       1.37
## 352           Short  Woodland              No           Resident       1.58
## 353           Short  Woodland              No         Withdrawal       1.46
## 354           Short   Various             Yes           Moderate       1.25
## 355           Short   Various             Yes              Short       1.28
## 356           Short   Various             Yes           Moderate       1.16
## 357          Middle   Various              No           Moderate       1.29
## 358           Short  Woodland              No           Moderate       1.15
## 359           Short  Woodland              No           Resident       1.74
## 360           Short  Woodland              No           Resident       1.62
## 361          Middle  Woodland             Yes           Moderate       1.54
## 362          Middle   Wetland             Yes           Resident       2.18
## 363          Middle   Various             Yes              Short       1.80
## 364           Short   Various             Yes         Withdrawal       1.80
## 365          Middle   Various             Yes              Short       1.61
## 366           Short  Woodland             Yes           Moderate       1.56
## 367          Middle   Various             Yes              Short       2.02
## 368           Short Grassland              No         Withdrawal       1.96
## 369           Short  Woodland             Yes         Withdrawal       1.39
## 370           Short  Woodland              No           Moderate       1.28
## 371          Middle   Various             Yes              Short       1.71
## 372           Short   Wetland              No           Moderate       1.78
## 373           Short Shrubland              No              Short       1.56
## 374           Short   Various             Yes              Short       1.60
## 375           Short      <NA>             Yes           Resident       1.69
## 376          Middle   Wetland              No              Short       1.76
## 377           Short Grassland              No              Short       2.00
## 378          Middle   Wetland              No              Short       1.87
## 379          Middle Shrubland              No         Withdrawal       1.71
## 380          Middle Shrubland              No              Short       1.80
## 381           Short Shrubland              No         Withdrawal       1.79
## 382          Middle Shrubland              No              Short       1.84
## 383           Short Shrubland             Yes           Resident       1.93
## 384           Short Shrubland              No           Resident       1.80
## 385          Middle Shrubland             Yes           Resident       1.91
## 386          Middle Shrubland             Yes              Short       1.55
## 387           Short Shrubland              No           Resident       1.79
## 388           Short Shrubland              No           Resident       1.83
## 389           Short Shrubland             Yes         Withdrawal       1.69
## 390           Short Shrubland              No              Short       1.65
## 391           Short Grassland              No           Moderate       1.35
## 392           Short Grassland              No           Moderate       1.38
## 393          Middle  Woodland             Yes           Resident       1.03
## 394           Short  Woodland              No           Resident       0.98
## 395           Short  Woodland              No           Resident       1.00
## 396          Middle  Woodland             Yes           Resident       1.00
## 397           Short  Woodland             Yes           Resident       1.00
## 398          Middle  Woodland              No           Resident       1.02
## 399          Middle  Woodland             Yes           Resident       1.05
## 400           Short  Woodland              No           Resident       1.20
## 401           Short  Woodland              No           Resident       1.33
## 402           Short Shrubland             Yes           Resident       0.83
## 403           Short  Woodland              No           Moderate       0.92
## 404          Middle  Woodland              No           Moderate       1.04
## 405           Short  Woodland              No               Long       1.01
## 406           Short   Various              No           Moderate       0.90
## 407           Short  Woodland              No           Moderate       0.94
## 408           Short  Woodland              No               Long       1.00
## 409           Short Shrubland              No               Long       0.97
## 410           Short Shrubland              No              Short       0.98
## 411           Short  Woodland              No           Moderate       1.00
## 412          Middle  Woodland              No           Moderate       1.30
## 413           Short Shrubland              No           Moderate       1.00
## 414           Short  Woodland              No               Long       0.91
## 415          Middle  Woodland              No           Moderate       0.91
## 416           Short  Woodland              No           Moderate       0.90
## 417           Short  Woodland              No           Moderate       1.24
## 418           Short   Various              No           Moderate       0.96
## 419          Middle  Woodland              No           Moderate       1.27
## 420           Short  Woodland              No         Withdrawal       1.00
## 421           Short   Various              No              Short       1.01
## 422           Short  Woodland              No              Short       1.07
## 423           Short Shrubland              No              Short       0.88
## 424           Short   Wetland              No           Moderate       1.16
## 425           Short  Woodland              No               Long       0.95
## 426           Short  Woodland              No           Moderate       0.95
## 427           Short  Woodland              No         Withdrawal       0.85
## 428           Short Shrubland              No           Moderate       0.88
## 429           Short  Woodland              No           Moderate       1.15
## 430           Short Shrubland              No           Moderate       1.01
## 431           Short Shrubland              No           Moderate       1.41
## 432           Short  Woodland              No              Short       1.08
## 433           Short  Woodland              No              Short       0.99
## 434           Short      <NA>             Yes           Resident       1.33
## 435          Middle      <NA>             Yes           Resident       1.42
## 436           Short  Woodland              No         Withdrawal       1.04
## 437           Short  Woodland              No              Short       0.78
## 438           Short Shrubland              No           Resident       0.72
## 439           Short  Woodland              No         Withdrawal       1.34
## 440           Short  Woodland              No              Short       0.78
## 441           Short  Woodland              No              Short       0.81
## 442           Short  Woodland              No           Resident       1.01
## 443           Short  Woodland             Yes           Resident       1.03
## 444           Short  Woodland             Yes         Withdrawal       0.99
## 445           Short  Woodland             Yes           Resident       1.32
## 446           Short      <NA>             Yes           Resident       2.07
## 447          Middle      <NA>             Yes           Resident       2.04
## 448          Middle   Various             Yes         Withdrawal       1.89
## 449          Middle      <NA>             Yes           Resident       2.29
## 450          Middle Shrubland              No           Resident       0.94
## 451          Middle Shrubland              No           Resident       1.16
## 452           Short Shrubland             Yes              Short       1.00
## 453           Short Shrubland             Yes           Resident       1.59
## 454           Short   Various              No              Short       1.08
## 455           Short  Woodland             Yes           Resident       1.28
## 456           Short  Woodland             Yes              Short       1.04
## 457           Short   Wetland              No              Short       1.03
## 458           Short   Various              No              Short       1.21
## 459           Short   Wetland              No           Moderate       0.96
## 460           Short  Woodland              No              Short       0.99
## 461          Middle  Woodland             Yes              Short       1.90
## 462           Short  Woodland             Yes           Resident       1.90
## 463          Middle  Woodland              No              Short       1.44
## 464           Short  Woodland              No              Short       1.48
## 465           Short Shrubland              No              Short       1.47
## 466           Short  Woodland              No              Short       1.51
## 467           Short  Woodland              No              Short       1.89
## 468           Short  Woodland              No              Short       1.42
## 469           Short  Woodland             Yes           Moderate       1.65
## 470          Middle Shrubland              No         Withdrawal       1.47
## 471           Short   Wetland             Yes         Withdrawal       1.27
## 472          Middle  Woodland              No         Withdrawal       1.65
## 473           Short  Woodland              No           Moderate       1.63
## 474           Short  Woodland              No           Moderate       1.04
## 475          Middle  Woodland              No         Withdrawal       1.31
## 476          Middle  Woodland             Yes              Short       1.29
## 477           Short  Woodland              No               Long       1.15
## 478           Short   Various              No           Moderate       1.08
## 479          Middle  Woodland             Yes           Moderate       1.53
## 480           Short  Woodland             Yes           Resident       1.80
## 481           Short  Woodland              No              Short       1.43
## 482           Short  Woodland              No           Moderate       1.00
## 483           Short  Woodland              No           Moderate       1.00
## 484           Short  Woodland              No         Withdrawal       0.85
## 485           Short  Woodland              No           Moderate       1.04
## 486           Short   Various             Yes              Short       1.32
## 487           Short Shrubland              No           Moderate       1.61
## 488           Short  Woodland              No         Withdrawal       1.74
## 489           Short  Woodland             Yes         Withdrawal       1.58
## 490           Short Shrubland              No              Short       1.16
## 491           Short  Woodland             Yes           Moderate       1.62
## 492           Short Shrubland              No           Moderate       0.93
## 493           Short  Woodland              No           Resident       1.04
## 494           Short  Woodland              No           Moderate       1.19
## 495           Short Shrubland              No              Short       1.06
## 496           Short  Woodland              No           Moderate       1.24
## 497          Middle  Woodland             Yes           Resident       1.90
## 498           Short  Woodland              No           Resident       1.82
## 499          Middle  Woodland              No           Resident       1.67
## 500           Short  Woodland              No           Resident       1.86
## 501          Middle  Woodland             Yes           Resident       1.40
## 502          Middle  Woodland             Yes           Resident       1.81
## 503           Short  Woodland              No           Resident       2.16
## 504           Short  Woodland             Yes           Resident       1.91
## 505          Middle  Woodland             Yes           Resident       1.78
## 506           Short  Woodland              No           Resident       1.48
## 507          Middle  Woodland              No              Short       2.05
## 508           Short  Woodland             Yes              Short       2.16
## 509          Middle  Woodland              No           Resident       1.58
## 510          Middle  Woodland             Yes           Resident       2.47
## 511          Middle  Woodland             Yes           Resident       1.84
## 512           Short  Woodland              No              Short       1.76
## 513          Middle  Woodland              No           Resident       1.67
## 514          Middle  Woodland             Yes         Withdrawal       1.86
## 515           Short  Woodland              No              Short       1.70
## 516          Middle  Woodland              No           Resident       1.79
## 517           Short  Woodland              No              Short       1.68
## 518           Short  Woodland             Yes              Short       1.69
## 519          Middle   Wetland              No              Short       3.06
## 520          Middle   Wetland              No              Short       2.62
## 521           Short   Wetland              No           Moderate       2.66
## 522           Short   Wetland              No           Resident       2.12
## 523           Short   Wetland              No              Short       2.62
## 524           Short   Wetland              No           Moderate       3.01
## 525            Long     Ocean              No               Long       3.45
## 526            Long     Ocean              No              Short       2.79
## 527            Long     Ocean              No               Long       2.75
## 528            Long     Ocean              No               Long       2.90
## 529          Middle      <NA>             Yes           Resident       1.48
## 530          Middle      <NA>             Yes           Resident       2.37
## 531          Middle      <NA>             Yes           Resident       2.08
## 532          Middle      <NA>             Yes           Resident       2.50
## 533          Middle      <NA>             Yes           Resident       2.11
## 534          Middle  Woodland             Yes           Resident       1.86
## 535          Middle  Woodland             Yes           Resident       2.85
## 536          Middle  Woodland              No          Irruptive       2.14
## 537          Middle Grassland             Yes              Short       2.18
## 538          Middle  Woodland             Yes           Resident       2.24
## 539           Short  Woodland              No           Resident       1.85
## 540          Middle  Woodland              No           Resident       3.03
## 541            Long  Woodland             Yes           Resident       3.18
## 542            Long  Woodland              No              Short       2.47
## 543           Short  Woodland              No          Irruptive       2.51
## 544           Short  Woodland              No           Resident       1.79
## 545          Middle  Woodland              No              Short       2.00
## 546          Middle Grassland              No              Short       2.51
## 547            Long Grassland              No          Irruptive       3.36
## 548          Middle  Woodland              No           Resident       2.77
## 549           Short  Woodland              No           Resident       1.96
## 550            Long   Various             Yes         Withdrawal       2.61
## 551           Short  Woodland              No              Short       1.85
##     mean_eggs_per_clutch mean_age_at_sexual_maturity population_size
## 1                    9.0                         1.0              NA
## 2                    7.5                         1.0              NA
## 3                   10.5                         3.0              NA
## 4                    3.5                         2.5              NA
## 5                    9.5                         2.0              NA
## 6                   13.5                         1.0              NA
## 7                   10.0                         0.6              NA
## 8                    8.5                         2.0              NA
## 9                    5.0                         1.0              NA
## 10                   8.0                         1.0              NA
## 11                  10.0                         1.0              NA
## 12                   5.0                         2.5              NA
## 13                   8.0                         2.0              NA
## 14                  10.5                         2.0              NA
## 15                   5.5                         3.5              NA
## 16                   8.5                         1.0              NA
## 17                  13.0                         1.0              NA
## 18                   9.5                         1.5              NA
## 19                   9.5                         1.0              NA
## 20                   5.0                         3.0              NA
## 21                   8.0                         0.4              NA
## 22                   6.5                         1.0              NA
## 23                  11.0                         2.0              NA
## 24                   5.0                         2.0              NA
## 25                  10.0                         1.0              NA
## 26                   8.0                         2.0              NA
## 27                   9.5                         1.0              NA
## 28                   9.0                         1.0              NA
## 29                   5.0                         2.5              NA
## 30                   8.0                         1.0              NA
## 31                  10.0                         0.6              NA
## 32                  14.5                         2.5              NA
## 33                  10.0                         1.0              NA
## 34                  10.0                         1.0              NA
## 35                   4.0                         2.5              NA
## 36                   7.5                         1.0              NA
## 37                   4.0                         2.0              NA
## 38                   7.5                         2.5              NA
## 39                   6.5                         2.5              NA
## 40                   4.5                         5.5              NA
## 41                  10.0                         1.0              NA
## 42                   4.5                         3.0              NA
## 43                  11.0                         2.5              NA
## 44                  10.5                         1.0              NA
## 45                   5.0                         1.0        3.40e+05
## 46                   4.5                         1.0        8.00e+05
## 47                   2.0                         2.0        7.00e+05
## 48                   2.0                         1.0        4.00e+06
## 49                   2.5                         1.0        4.00e+06
## 50                   2.0                         1.0              NA
## 51                   1.5                         1.0              NA
## 52                   2.0                         1.0        6.00e+06
## 53                   2.0                         1.0              NA
## 54                   2.0                         1.0        2.00e+06
## 55                   2.0                         1.0        1.40e+06
## 56                   2.0                         1.0              NA
## 57                   2.0                         1.0        2.00e+07
## 58                   2.0                         1.0        1.10e+07
## 59                   2.0                         1.0              NA
## 60                   2.0                         1.0        6.00e+06
## 61                   2.0                         1.0              NA
## 62                   2.0                         1.0        1.80e+06
## 63                   2.0                         1.0        3.00e+06
## 64                   2.0                         1.0        2.00e+06
## 65                   2.0                         3.5              NA
## 66                   1.0                         4.5              NA
## 67                   2.0                         3.5              NA
## 68                   1.0                         3.0              NA
## 69                   1.0                         5.0              NA
## 70                   1.0                         5.5              NA
## 71                   1.0                         1.5              NA
## 72                   1.5                         3.5              NA
## 73                   1.0                         4.5              NA
## 74                   1.0                         2.5              NA
## 75                   1.0                         4.5              NA
## 76                   4.0                         1.0              NA
## 77                   3.0                         3.5              NA
## 78                   2.5                         4.5              NA
## 79                   4.0                         1.5              NA
## 80                   4.0                         1.0              NA
## 81                   4.0                         1.0              NA
## 82                   3.0                         1.0              NA
## 83                   4.0                         1.0              NA
## 84                   4.0                         1.0              NA
## 85                   3.5                         2.5              NA
## 86                   3.0                         0.9              NA
## 87                   3.0                         1.0              NA
## 88                   3.0                         1.5              NA
## 89                   2.0                         4.0              NA
## 90                   3.0                         2.0              NA
## 91                   3.5                         4.0              NA
## 92                   1.5                         3.0              NA
## 93                   2.0                         2.0              NA
## 94                   2.5                         3.5              NA
## 95                   3.0                         5.5              NA
## 96                   2.5                         4.5              NA
## 97                   2.5                         4.0              NA
## 98                   3.0                         4.0              NA
## 99                   3.0                         2.5              NA
## 100                  3.0                         1.0              NA
## 101                  3.0                         4.5              NA
## 102                  3.0                         1.5              NA
## 103                  3.0                         2.5              NA
## 104                  3.0                         2.0              NA
## 105                  2.5                         4.0              NA
## 106                  3.0                         4.5              NA
## 107                  3.0                         4.5              NA
## 108                  4.5                         1.5              NA
## 109                  4.0                         0.9              NA
## 110                  4.0                         1.0              NA
## 111                  4.0                         1.5              NA
## 112                  3.5                         1.5              NA
## 113                  4.0                         1.0              NA
## 114                  6.0                         1.5              NA
## 115                  4.0                         2.5              NA
## 116                  4.0                         1.0              NA
## 117                  4.0                         2.5              NA
## 118                  4.0                         1.0              NA
## 119                  4.0                         2.5              NA
## 120                  4.0                         2.5              NA
## 121                  4.0                         1.0              NA
## 122                  4.0                         1.0              NA
## 123                  4.0                         2.0              NA
## 124                  4.0                         2.0              NA
## 125                  3.5                         1.0              NA
## 126                  4.0                         1.0              NA
## 127                  4.0                         2.0              NA
## 128                  4.0                         1.0              NA
## 129                  4.0                         1.5              NA
## 130                  4.0                         1.0              NA
## 131                  4.0                         2.5              NA
## 132                  4.0                         1.5              NA
## 133                  2.0                         1.5              NA
## 134                  4.0                         1.0              NA
## 135                  3.5                         2.0              NA
## 136                  4.0                         1.0              NA
## 137                  4.0                         1.0              NA
## 138                  1.5                         4.5              NA
## 139                  2.5                         1.0              NA
## 140                  2.0                         2.5              NA
## 141                  3.0                         3.0              NA
## 142                  2.5                         2.0              NA
## 143                  2.0                         4.5              NA
## 144                  1.0                         3.5              NA
## 145                  2.0                         3.5              NA
## 146                  4.0                         2.0              NA
## 147                  4.5                         1.0              NA
## 148                  4.0                         1.0              NA
## 149                  3.5                         2.5              NA
## 150                  4.5                         1.5              NA
## 151                  3.5                         2.0              NA
## 152                  3.0                         1.0              NA
## 153                  4.5                         1.0              NA
## 154                  4.0                         1.0              NA
## 155                  3.5                         4.0              NA
## 156                  4.5                         1.5              NA
## 157                  3.5                         1.5              NA
## 158                  3.5                         2.0              NA
## 159                  3.0                         3.5              NA
## 160                  1.0                         5.0              NA
## 161                  1.5                         3.0              NA
## 162                  3.0                         1.5              NA
## 163                  4.5                         3.0              NA
## 164                  4.0                         2.0              NA
## 165                  4.0                         3.0              NA
## 166                  3.5                         1.0              NA
## 167                  3.5                         2.5              NA
## 168                  3.0                         3.0              NA
## 169                  2.0                         3.0              NA
## 170                  1.0                         3.0              NA
## 171                  3.5                         2.5              NA
## 172                  2.5                         2.5              NA
## 173                  2.5                         1.5              NA
## 174                  3.5                         2.0              NA
## 175                  1.5                         1.0        9.40e+05
## 176                  2.0                         0.2        2.00e+06
## 177                  2.0                         1.0              NA
## 178                  6.0                         1.0        8.70e+05
## 179                  2.0                         0.3        1.00e+08
## 180                  1.0                         1.0              NA
## 181                  2.0                         0.3              NA
## 182                  2.0                         1.0              NA
## 183                  2.0                         1.0              NA
## 184                  2.0                         1.0              NA
## 185                  2.0                         1.0        4.00e+06
## 186                  7.0                         0.8        1.70e+06
## 187                  4.5                         1.0              NA
## 188                  4.5                         1.0              NA
## 189                  4.0                         2.0              NA
## 190                  5.5                         2.0              NA
## 191                  5.0                         2.5        6.50e+05
## 192                  2.0                         4.5              NA
## 193                  2.5                         1.5        1.70e+06
## 194                  4.5                         2.0        7.00e+05
## 195                  3.0                         1.5        8.00e+04
## 196                  2.5                         5.5        1.30e+05
## 197                  2.0                         2.0              NA
## 198                  9.0                         1.0        5.00e+04
## 199                  2.5                         1.0              NA
## 200                  3.0                         2.0        2.00e+05
## 201                  4.0                         2.5        7.00e+05
## 202                  3.5                         1.0        1.10e+06
## 203                  3.0                         3.0        2.00e+06
## 204                  4.5                         2.5        3.00e+05
## 205                  4.5                         2.0        5.00e+05
## 206                  2.0                         1.5              NA
## 207                  3.0                         1.0              NA
## 208                  3.0                         1.5        5.40e+05
## 209                  2.5                         2.0              NA
## 210                  4.5                         1.0              NA
## 211                  1.5                         2.0              NA
## 212                  2.0                         7.5              NA
## 213                  1.0                         6.0              NA
## 214                  2.0                        10.5        5.10e+06
## 215                  5.0                         1.0        2.20e+06
## 216                  2.5                         1.0              NA
## 217                  2.5                         3.0              NA
## 218                  4.5                         2.0        4.00e+04
## 219                  5.0                         1.5        1.30e+06
## 220                  4.0                         2.0              NA
## 221                  4.0                         1.5        7.00e+04
## 222                  3.0                         3.0        2.00e+05
## 223                  3.0                         1.0              NA
## 224                 12.5                         0.8              NA
## 225                 13.0                         0.8        2.80e+06
## 226                 11.0                         1.0        3.90e+06
## 227                 10.0                         1.0              NA
## 228                  9.0                         1.0        3.00e+05
## 229                 17.0                         1.0              NA
## 230                 15.5                         1.0        3.00e+06
## 231                 15.5                         1.0        5.00e+05
## 232                 15.0                         1.0        1.30e+06
## 233                  9.5                         1.0        1.40e+07
## 234                  6.5                         1.0              NA
## 235                 11.0                         0.8        4.00e+05
## 236                  7.5                         1.0              NA
## 237                 11.0                         1.0              NA
## 238                  9.0                         1.0        4.00e+06
## 239                 10.5                         0.3              NA
## 240                 11.0                         0.6        6.00e+05
## 241                  6.0                         0.9              NA
## 242                  6.0                         1.0              NA
## 243                  9.0                         1.0        1.40e+07
## 244                  2.0                         2.5              NA
## 245                  2.0                         1.5              NA
## 246                  2.0                         2.5              NA
## 247                  2.0                         3.0              NA
## 248                  6.0                         1.0              NA
## 249                  2.0                         4.5              NA
## 250                  2.0                         4.5              NA
## 251                  9.0                         1.0              NA
## 252                  6.0                         1.0              NA
## 253                 10.5                         1.0              NA
## 254                  7.0                         1.0              NA
## 255                 10.0                         1.0              NA
## 256                  7.5                         1.0              NA
## 257                  9.5                         1.0              NA
## 258                  9.5                         1.0              NA
## 259                  7.5                         1.0              NA
## 260                  4.5                         1.0        2.30e+06
## 261                  3.5                         1.0        8.00e+07
## 262                  3.5                         1.0              NA
## 263                  5.0                         1.0        2.00e+06
## 264                  3.5                         1.0        5.20e+07
## 265                  5.0                         1.0        6.00e+05
## 266                  3.5                         1.0        1.20e+07
## 267                  4.0                         2.0        1.80e+07
## 268                  4.0                         1.0        2.00e+07
## 269                  4.0                         1.0              NA
## 270                  3.0                         1.0        7.80e+07
## 271                  4.0                         1.0        5.60e+06
## 272                  3.0                         1.0        9.10e+07
## 273                  3.5                         1.0        1.00e+07
## 274                  2.5                         1.0        1.20e+06
## 275                  3.0                         1.0        4.10e+06
## 276                  3.0                         1.0        1.00e+07
## 277                  4.0                         1.0        1.10e+07
## 278                  5.0                         1.0        8.50e+06
## 279                  4.5                         1.0        1.70e+05
## 280                  4.5                         2.0        2.70e+07
## 281                  6.0                         2.0        5.40e+06
## 282                  4.5                         1.0        1.30e+07
## 283                  4.0                         5.0              NA
## 284                  5.5                         1.0        4.00e+05
## 285                  3.5                         2.0        2.30e+05
## 286                  5.0                         1.0        6.00e+06
## 287                  4.5                         2.0        4.50e+05
## 288                  3.0                         2.5              NA
## 289                  3.5                         1.0        2.00e+07
## 290                  4.0                         1.0              NA
## 291                  5.0                         3.0        1.40e+05
## 292                  4.5                         2.0        8.00e+05
## 293                  4.5                         2.0        7.60e+05
## 294                  4.0                         1.0        2.20e+06
## 295                  4.5                         1.0        1.50e+06
## 296                  6.5                         2.0              NA
## 297                  3.0                         2.0        8.00e+05
## 298                  5.0                         1.0        2.00e+07
## 299                  3.5                         1.0        2.00e+05
## 300                  4.5                         1.0        2.00e+06
## 301                  3.5                         1.0        1.70e+05
## 302                  3.5                         1.0        2.90e+07
## 303                  3.5                         1.0        1.30e+07
## 304                  3.5                         1.0        5.20e+06
## 305                  4.0                         1.0        7.60e+06
## 306                  4.0                         1.0        3.00e+06
## 307                  4.5                         1.0        2.10e+08
## 308                  4.0                         1.0        5.60e+07
## 309                  4.5                         1.0        2.00e+08
## 310                  4.0                         2.0        5.80e+07
## 311                  3.5                         1.0        7.60e+06
## 312                  3.5                         1.0              NA
## 313                  4.0                         1.0        2.00e+07
## 314                  4.0                         1.0        4.00e+06
## 315                  4.0                         1.0        3.00e+07
## 316                  3.0                         1.0        4.10e+06
## 317                  4.0                         2.0        2.00e+06
## 318                  4.0                         1.0        4.00e+05
## 319                  5.0                         1.0        6.00e+07
## 320                  5.0                         2.0        9.10e+06
## 321                  4.5                         1.0        6.90e+06
## 322                  4.0                         1.0        8.00e+06
## 323                  4.5                         1.0        7.00e+07
## 324                  4.0                         1.0              NA
## 325                  4.0                         1.0              NA
## 326                  3.5                         1.0              NA
## 327                  3.5                         1.0              NA
## 328                  3.5                         1.0        4.00e+06
## 329                  4.0                         1.0        1.70e+08
## 330                  4.5                         1.0        1.60e+05
## 331                  5.0                         1.0              NA
## 332                  4.5                         1.0              NA
## 333                  5.0                         1.0        1.40e+07
## 334                  4.0                         1.0        1.30e+08
## 335                  4.5                         1.0        3.00e+07
## 336                  4.0                         2.0        2.80e+07
## 337                  3.5                         1.0        6.00e+07
## 338                  4.5                         1.0        1.40e+08
## 339                  3.5                         1.0              NA
## 340                  5.0                         0.9        4.20e+07
## 341                  4.5                         1.0              NA
## 342                  4.5                         1.0        2.90e+06
## 343                  4.5                         1.0        5.00e+07
## 344                  3.5                         1.0        3.90e+06
## 345                  4.5                         1.0        1.30e+07
## 346                  4.5                         1.0        3.50e+07
## 347                  4.5                         2.0        3.00e+05
## 348                  4.5                         1.0        3.90e+06
## 349                  4.0                         1.0        3.00e+06
## 350                  3.5                         1.0        4.00e+07
## 351                  4.5                         1.0        6.30e+06
## 352                  4.0                         1.0        8.00e+06
## 353                  3.5                         0.4        2.00e+07
## 354                  5.0                         1.0        3.30e+07
## 355                  4.0                         1.0        1.00e+06
## 356                  6.0                         1.0        1.40e+07
## 357                  5.0                         1.0        1.70e+07
## 358                  4.5                         1.0        5.80e+06
## 359                  4.0                         1.0              NA
## 360                  4.0                         1.0              NA
## 361                  4.0                         1.0        1.20e+07
## 362                  3.0                         1.3        7.00e+06
## 363                  5.5                         1.0        2.00e+07
## 364                  1.0                         2.0        1.00e+06
## 365                  4.0                         1.0        1.10e+08
## 366                  4.5                         2.0        6.20e+06
## 367                  5.0                         1.0        6.10e+07
## 368                  5.0                         1.0        2.20e+07
## 369                  4.0                         1.0        3.00e+05
## 370                  4.0                         1.0        9.20e+06
## 371                  5.0                         2.0        1.20e+08
## 372                  4.5                         1.0        5.00e+06
## 373                  3.5                         1.0        1.80e+06
## 374                  3.5                         1.0              NA
## 375                  3.0                         1.0              NA
## 376                  3.0                         1.0              NA
## 377                  5.0                         1.0        7.90e+07
## 378                  4.0                         1.0        1.10e+07
## 379                  5.5                         1.0        4.90e+06
## 380                  5.5                         1.0              NA
## 381                  3.5                         0.8        6.00e+04
## 382                  4.0                         0.8        4.90e+06
## 383                  3.0                         0.6        2.00e+05
## 384                  3.0                         0.8        9.00e+04
## 385                  3.0                         1.0        1.10e+06
## 386                  3.5                         1.0        2.70e+07
## 387                  3.5                         0.8        6.00e+04
## 388                  3.5                         0.8        1.20e+05
## 389                  4.0                         1.0        2.70e+07
## 390                  5.5                         2.0        5.90e+06
## 391                  4.5                         1.0        1.80e+07
## 392                  5.5                         1.0        9.00e+05
## 393                  7.0                         0.5        4.10e+07
## 394                  6.5                         1.0        1.20e+07
## 395                  6.0                         1.0        9.00e+04
## 396                  6.5                         1.0        1.20e+07
## 397                  6.0                         1.0        9.70e+06
## 398                  6.5                         1.0              NA
## 399                  9.0                         1.0        7.50e+06
## 400                  7.0                         1.0        6.80e+05
## 401                  6.0                         1.0        8.50e+06
## 402                  4.5                         1.0        3.50e+06
## 403                  3.5                         1.0        3.90e+07
## 404                  5.0                         1.0        2.00e+07
## 405                  4.0                         1.0        2.10e+06
## 406                  4.0                         1.0        2.40e+06
## 407                  4.5                         1.0        1.00e+07
## 408                  6.5                         1.0        7.00e+06
## 409                  4.0                         1.0        1.90e+07
## 410                  3.5                         1.0        8.30e+07
## 411                  4.0                         1.0        2.50e+06
## 412                  5.0                         1.0        3.60e+05
## 413                  4.5                         1.0        1.20e+07
## 414                  4.0                         1.0        4.00e+07
## 415                  4.5                         1.0        3.20e+07
## 416                  4.0                         1.0        1.30e+07
## 417                  4.0                         1.0        1.90e+07
## 418                  5.0                         2.0        8.00e+07
## 419                  4.0                         1.0        2.20e+07
## 420                  4.0                         1.0              NA
## 421                  4.5                         1.0        1.30e+07
## 422                  4.0                         1.0        1.20e+07
## 423                  4.0                         1.0        3.50e+06
## 424                  4.5                         1.0        1.60e+06
## 425                  5.5                         1.0        7.00e+07
## 426                  4.0                         2.0        1.70e+07
## 427                  3.5                         1.0              NA
## 428                  3.5                         1.0        6.00e+07
## 429                  5.5                         1.0        8.30e+05
## 430                  4.5                         1.0        9.00e+07
## 431                  4.0                         1.0        1.10e+07
## 432                  4.5                         1.0        1.30e+08
## 433                  4.5                         1.0        1.80e+06
## 434                  5.0                         1.0              NA
## 435                  4.5                         0.4        8.20e+07
## 436                  3.5                         1.0              NA
## 437                  4.0                         1.0        1.10e+08
## 438                  3.0                         1.0              NA
## 439                  2.5                         0.8        1.10e+06
## 440                  9.5                         1.0        9.60e+07
## 441                  7.5                         2.0        9.00e+07
## 442                  5.0                         1.0        1.10e+06
## 443                  7.0                         1.0        2.00e+06
## 444                  6.5                         1.0        2.00e+07
## 445                  7.0                         1.0        8.30e+06
## 446                  3.5                         1.0              NA
## 447                  4.0                         1.0              NA
## 448                  5.0                         1.0        5.70e+07
## 449                  2.0                         1.0              NA
## 450                  3.5                         1.0              NA
## 451                  4.0                         1.0        1.30e+06
## 452                  6.0                         1.0        4.00e+06
## 453                  4.5                         0.7        2.90e+06
## 454                  5.0                         2.0        3.00e+05
## 455                  5.0                         1.0        1.30e+07
## 456                  6.0                         1.0        4.20e+07
## 457                  5.5                         1.0        9.00e+06
## 458                  5.5                         1.0        2.80e+06
## 459                  5.0                         1.0        6.20e+06
## 460                  5.5                         2.0        1.69e+07
## 461                  4.0                         1.0        3.00e+08
## 462                  3.0                         1.0              NA
## 463                  5.0                         1.0        1.90e+07
## 464                  4.5                         1.0        4.00e+07
## 465                  6.0                         1.0        4.60e+06
## 466                  4.0                         1.0        9.70e+05
## 467                  3.0                         1.0        2.00e+07
## 468                  5.0                         1.0        4.50e+06
## 469                  3.0                         1.0        1.10e+07
## 470                  5.0                         1.0        5.20e+06
## 471                  4.5                         1.0        1.00e+06
## 472                  4.5                         1.0              NA
## 473                  3.5                         2.0        2.10e+06
## 474                  3.0                         1.0        7.80e+06
## 475                  4.5                         1.0              NA
## 476                  5.0                         1.0        3.20e+07
## 477                  3.0                         1.0        5.50e+06
## 478                  3.5                         2.0        3.00e+06
## 479                  6.0                         1.0        6.70e+06
## 480                  3.5                         2.0              NA
## 481                  3.5                         1.5              NA
## 482                  3.5                         2.0        1.90e+07
## 483                  4.0                         1.0        3.60e+07
## 484                  2.5                         2.0              NA
## 485                  4.0                         1.0        7.30e+06
## 486                  5.0                         1.0        4.00e+06
## 487                  4.5                         1.0        8.70e+06
## 488                  3.5                         1.0              NA
## 489                  3.0                         1.0              NA
## 490                  2.5                         2.0              NA
## 491                  4.0                         1.0        2.10e+07
## 492                  4.0                         1.0        3.60e+06
## 493                  4.0                         1.0        9.60e+05
## 494                  4.0                         1.0        1.50e+07
## 495                  4.0                         1.0        1.80e+07
## 496                  4.0                         1.0        3.50e+06
## 497                  5.0                         1.5        1.50e+06
## 498                  4.0                         1.0        1.10e+06
## 499                  3.0                         1.0              NA
## 500                  4.0                         1.0        8.00e+05
## 501                  5.5                         1.0        1.40e+07
## 502                  4.0                         1.0        4.00e+05
## 503                  6.0                         1.0        2.00e+05
## 504                  5.5                         1.0        5.80e+05
## 505                  3.5                         1.0        8.60e+06
## 506                  5.0                         1.0        2.10e+06
## 507                  7.0                         1.0        7.00e+04
## 508                  7.5                         1.0        8.10e+06
## 509                  4.5                         1.0        6.00e+05
## 510                  4.0                         1.0        1.90e+06
## 511                  4.0                         1.0        1.00e+07
## 512                  5.5                         1.0        2.00e+06
## 513                  3.5                         1.0              NA
## 514                  6.5                         1.0        1.20e+06
## 515                  4.5                         1.0        2.00e+06
## 516                  4.5                         1.0        1.50e+05
## 517                  5.0                         2.0        3.00e+05
## 518                  5.5                         1.0        1.00e+07
## 519                  3.5                         1.0              NA
## 520                  3.5                         1.5              NA
## 521                  5.5                         1.0              NA
## 522                  4.5                         1.0              NA
## 523                  6.5                         1.5              NA
## 524                  4.5                         1.0              NA
## 525                  1.0                         9.0              NA
## 526                  1.0                        12.5              NA
## 527                  1.0                         6.0              NA
## 528                  1.0                         6.0              NA
## 529                  5.0                         0.5              NA
## 530                  3.5                         2.0              NA
## 531                  6.5                         2.5              NA
## 532                  3.5                         4.0              NA
## 533                  4.0                         3.0              NA
## 534                  5.0                         2.5              NA
## 535                  2.5                         2.0        3.00e+06
## 536                  4.5                         1.0              NA
## 537                  8.0                         1.0        7.00e+05
## 538                  4.5                         1.0        1.10e+06
## 539                  4.0                         1.0              NA
## 540                  3.5                         3.5        9.00e+04
## 541                  2.5                         2.0        4.00e+06
## 542                  5.5                         1.0        1.50e+04
## 543                  7.0                         1.0        6.00e+04
## 544                  5.0                         1.0        6.00e+04
## 545                  5.5                         1.0              NA
## 546                  5.5                         1.0        6.00e+05
## 547                  7.0                         2.0        1.00e+05
## 548                  2.5                         3.0              NA
## 549                  3.0                         1.0              NA
## 550                  5.5                         1.0        1.60e+05
## 551                  3.5                         2.0              NA
##     winter_range_area range_in_cbc strata circles   feeder_bird median_trend
## 1             3212473         99.1     82    1453            No        1.014
## 2             7145842         61.7    124    1951            No        0.996
## 3             1812841         69.8     37     502            No        1.039
## 4              360134         53.7     19     247            No        0.998
## 5              854350          5.3     36     470            No        1.004
## 6             9204678          0.5      5      97            No        1.196
## 7             5466645         17.9     26     479            No        1.030
## 8             8055421         72.4    134    2189            No        1.004
## 9             8124299         93.8    145    2581 Occassionally        1.035
## 10            5781869         73.4    103    1613            No        0.983
## 11            6279681          8.5     16     315            No        1.008
## 12             803235          1.6     16     177            No        1.005
## 13            9346116         92.4    142    2366            No        0.987
## 14            6845969         91.5    123    2055            No        1.023
## 15             249044          0.0      2      11            No        1.040
## 16           12883658          6.1     13     260            No        1.043
## 17            4220203          1.2      2      12            No        1.020
## 18            6452371         72.1    124    1980            No        1.049
## 19            2079974         58.2     75    1077            No        0.912
## 20            1595716         46.3     34     557            No        1.128
## 21            7368712         70.0    125    1995            No        1.014
## 22             517664          4.5     18     238            No        1.026
## 23            5592391         97.6    131    2191            No        1.051
## 24            1375519          9.0      8      63            No        0.963
## 25            6047381         51.5    126    2078            No        1.014
## 26             752255         13.4     42     650            No        0.945
## 27            8625762         80.0    156    3006 Occassionally        1.021
## 28             214673         82.6     10     192            No        1.032
## 29             212068         95.2     34     549            No        1.055
## 30            7795723         62.1    122    1951            No        0.975
## 31            5171693         47.5     95    1508            No        1.032
## 32            1028971          7.8     82    1360            No        0.993
## 33            4997611         65.0     99    1490            No        1.020
## 34            6622932         69.9    119    2031            No        1.035
## 35             696175         75.7     27     313            No        1.130
## 36            4718281         56.1    103    1638            No        1.018
## 37            1767331         84.7     83    1302            No        0.986
## 38             376143          2.6      2      12            No        0.988
## 39             816102          3.4     29     476            No        1.006
## 40             360967         90.6     16     256            No        1.077
## 41           14379684          0.0      6      46            No        1.004
## 42             674315         78.1     40     691            No        0.988
## 43             902422          3.8     38     587            No        1.025
## 44            6715722         81.2    104    1805            No        1.031
## 45             998800          0.0      1      26            No        1.031
## 46            1570664         30.4      6     186            No        1.002
## 47              42584          4.1      1      35           Yes        1.167
## 48            1271937         83.7     14     344           Yes        1.049
## 49             238000          0.0      3      35           Yes        1.087
## 50             372714          0.0      1       5           Yes        1.043
## 51             466659          0.0      2      15           Yes        1.105
## 52             260306          0.2      1       4           Yes        1.092
## 53             584491         30.7      4      51           Yes        1.094
## 54             196229          0.0      1       5           Yes        1.157
## 55             391181         29.0      3      94           Yes        1.061
## 56             715030          0.0      1       6           Yes        1.086
## 57             899178          1.2      6     116           Yes        1.057
## 58             233245          0.0      7      72           Yes        1.092
## 59             417628          0.0      1       4           Yes        1.122
## 60             415454         23.5      2      55            No        0.957
## 61           13832520          0.2      2      33            No        0.984
## 62            1257144         25.3      2      42            No        1.022
## 63            9232686          0.0      3      22            No        1.032
## 64            1344198         18.8      2      57            No        0.985
## 65             727930          4.0      5      96            No        1.048
## 66            2238664          0.4      2      13            No        1.046
## 67            1665552          0.7      8     105            No        1.022
## 68             424739         48.0      3      41            No        1.018
## 69            1218342          1.9      9     175            No        0.923
## 70            2189639          0.2      7      75            No        1.017
## 71             691331          2.7      7     145            No        0.995
## 72             408500         40.4      8     128            No        1.022
## 73             646654          1.0      7      68            No        1.185
## 74             732327          1.2      5     102            No        1.057
## 75            2267113          1.2      5      39            No        0.968
## 76            1401869         18.2     14     223            No        0.976
## 77            1203075         24.4     12     122            No        1.017
## 78             271789         54.4      6     106            No        1.048
## 79            1691279         29.8     28     454            No        1.002
## 80            8535689          3.2     10     224            No        1.080
## 81            8940735         60.4    114    2046            No        1.001
## 82             870130         31.9      2      38            No        0.984
## 83            5113012          1.1      3      60            No        0.994
## 84             236163         77.5     11     118            No        0.976
## 85            1672043         26.0     18     277            No        1.024
## 86             637405         34.5      9     125            No        1.006
## 87             847760          2.3      5      74            No        0.978
## 88             130396         23.6      3      50            No        0.831
## 89            2323220          0.3     16     191            No        0.981
## 90            3826413         62.1     79    1118            No        1.021
## 91             894484          6.4     19     362            No        1.023
## 92             864017          0.0      1      17            No        0.963
## 93             727423          0.0      2      24            No        0.975
## 94            1792607         14.8     32     460            No        0.967
## 95             954904         19.5     13     299            No        0.973
## 96            1112138         79.8     46     748            No        1.000
## 97             786775          6.6      2      49            No        1.010
## 98            5442058         58.4    119    2033            No        0.972
## 99            3393524          7.4     18     304            No        1.011
## 100           2175338          0.0      4      33            No        0.970
## 101           4671221          3.6     20     237            No        1.126
## 102            569305         39.5      2      12            No        0.962
## 103            113054         97.6      9     256            No        1.003
## 104           4308667         57.6    130    2212            No        1.024
## 105           1728204         31.4     33     622            No        1.023
## 106            157748         24.3      5     172            No        1.012
## 107            199954          4.5      3       4            No        1.052
## 108          14335169          1.0     12     172            No        0.971
## 109           1596300         98.9     40     586            No        0.994
## 110            798005         16.4      6     144            No        0.998
## 111           1105302         23.4     36     608            No        1.004
## 112          21489140          5.1     48     772            No        1.026
## 113          10407083         21.1     47     722            No        1.000
## 114          20496957          3.1     27     436            No        0.998
## 115           1636664         15.0      9     197            No        0.997
## 116           2954234         38.5     27     453            No        1.030
## 117            718543         53.6     15     187            No        1.026
## 118          13348747          0.1     11      60            No        0.924
## 119            312598         78.7     17     155            No        1.003
## 120           1681271         12.2     15     177            No        1.004
## 121           4174725          0.0      3      27            No        0.861
## 122           1215141         31.1      9      92            No        0.988
## 123           2076624          4.7     23     288            No        1.030
## 124           3121796         13.9     28     417            No        0.998
## 125           1224857          0.0      6      68            No        0.739
## 126           1459679          8.1     13     223            No        1.037
## 127          16712332          0.6      1      13            No        1.019
## 128          19553676          6.3     31     613            No        1.009
## 129          12977352          0.5      4      83            No        1.061
## 130           1633017         19.0      6     114            No        0.994
## 131            583618         35.9      3      63            No        0.976
## 132           3194025         56.2     28     484            No        1.005
## 133           2308618         16.0     11     152            No        1.004
## 134            419242          0.0      4      21            No        0.993
## 135           1498799         34.5     16     263            No        1.000
## 136           5981504          0.0      1       4            No        1.118
## 137           9604425         57.8    116    1991            No        0.986
## 138           6327061          0.0      3      41            No        1.018
## 139          27061720          0.0      1      33            No        1.022
## 140           1536201         12.0     15     254            No        0.993
## 141           4759009          0.1      9     113            No        0.889
## 142           3178877         29.3     28     451            No        1.025
## 143           2275017          4.9      3      59            No        0.986
## 144           3418376          5.7     14     218            No        1.012
## 145           3091608          2.9      4      84            No        1.021
## 146          14272680          0.4     16     281            No        1.022
## 147           3490819         42.4     28     497            No        0.982
## 148          17035591          9.9     53     721            No        1.038
## 149          25380422         28.3     18     396            No        0.981
## 150           9519274         69.2    133    2480            No        1.019
## 151          19081394          4.3     45     756            No        1.031
## 152           4770884         18.6     22     471            No        0.984
## 153           4237613          3.7      4      87            No        1.030
## 154           6207606          5.5     16     279            No        0.994
## 155            630128          8.6      7     114            No        1.002
## 156          17835338          2.4     26     470            No        1.028
## 157           2477342         16.1     17     262            No        1.002
## 158           2562842          2.6      9     162            No        0.996
## 159          14909423          2.6      4     101            No        1.056
## 160           8285259          0.1      1      42            No        1.015
## 161           2168484         43.0     35     484            No        1.087
## 162           2377715          0.3     17     261            No        1.045
## 163            703143          2.4      5     138            No        1.042
## 164           3435520         57.4     91    1400            No        1.074
## 165            226000         11.1     22     240            No        1.034
## 166          18055579          0.2      7     107            No        1.086
## 167           2739839         53.6      8     184            No        1.023
## 168            644235          0.3      3      12            No        0.960
## 169          10284417          0.0      1      12            No        0.977
## 170            492970          1.8     22     228            No        1.045
## 171            662259         34.6      8     124            No        1.078
## 172           8489285          1.0      5     130            No        1.054
## 173           1265518         22.3     16     257            No        1.036
## 174           5958026          6.0      8     155            No        1.113
## 175           2340120         23.8      7     237           Yes        1.009
## 176           6798642         17.5     15     295 Occassionally        0.997
## 177            973690         96.2     68     835           Yes        1.262
## 178           2939860         32.9     18     248           Yes        0.999
## 179          10076271         73.1    143    2775           Yes        1.024
## 180            923503          0.0      1       4            No        1.007
## 181          14617274          0.0      3      17            No        1.143
## 182          11296231        100.0      3      53           Yes        0.996
## 183            240194          0.7      1      12            No        0.996
## 184          13486868          0.3      2      26 Occassionally        1.045
## 185            577101         25.0     17     268           Yes        1.176
## 186          10344861         70.6    139    2644            No        1.007
## 187          17030975          1.0      6      52            No        1.013
## 188          16727698          0.1      2      23            No        1.056
## 189           2629777          4.4      3      61            No        0.921
## 190          14200618          0.4      1      30            No        0.866
## 191           2939045         62.2     24     400 Occassionally        0.999
## 192           6505972         89.7    145    2575            No        1.046
## 193           6556365          0.3      2      61            No        0.989
## 194           8177752         76.1    133    2423 Occassionally        1.038
## 195           2866056         71.0     31     491            No        1.017
## 196          10086859         78.4     47     767            No        1.007
## 197          11201397          0.2      1       8            No        1.065
## 198           6001693          5.7      8     122            No        1.015
## 199           9848122          0.0      1       3            No        1.035
## 200          13235779         54.4    129    1661 Occassionally        0.995
## 201           9138915         71.6    129    2439            No        1.009
## 202           3241122         82.9     81    1570            No        1.034
## 203           8831886         75.2    143    2849            No        1.028
## 204           6991058         98.2    103    1976            No        1.005
## 205          14477905         47.2    142    2577 Occassionally        1.025
## 206          14433437          0.2      1      44            No        1.065
## 207          11541636          0.5      1      25            No        1.068
## 208            666124          0.0      5      94            No        0.965
## 209           9468449          0.9      3      52            No        1.067
## 210           9441677          4.1     10     256            No        1.025
## 211           5749950          0.0      1      10            No        1.060
## 212          21048613          8.8     64     852            No        1.071
## 213            103930        100.0      1       5            No        1.029
## 214          21850843          7.8     68    1234            No        1.037
## 215          19048901         32.3    135    2634            No        1.005
## 216          12752998          1.7      1      12            No        1.102
## 217           4303949          5.4      5     111            No        1.112
## 218           8319239         43.3      3      25            No        1.011
## 219           7235784         46.4     84    1444 Occassionally        1.032
## 220          23458389         13.3     40     680            No        1.044
## 221           4411375         78.6     41     660            No        1.009
## 222          16735373          2.8     25     482            No        1.053
## 223            606419          2.3      2      16            No        0.989
## 224           4008729         90.0    114    1725 Occassionally        1.070
## 225           1273325         73.8     20     372 Occassionally        1.024
## 226            545492         74.2     11     108 Occassionally        1.006
## 227            690632         10.1      3      24 Occassionally        0.995
## 228            346469         96.2      5      98 Occassionally        0.990
## 229           4565937         84.1     76    1212 Occassionally        0.934
## 230           1419094         48.4     11     123 Occassionally        0.950
## 231            972623        100.0     10     101 Occassionally        0.956
## 232           2766114         91.6     25     355 Occassionally        0.968
## 233           4496716         96.3     95    1613 Occassionally        0.988
## 234           2100788         74.4      2      22            No        0.973
## 235            415886        100.0      4      36            No        0.998
## 236           1420287        100.0      2      20            No        0.946
## 237            517363        100.0      1       3            No        0.858
## 238           7211074         80.0      2       8            No        1.082
## 239           8017073         49.4     70    1356 Occassionally        0.998
## 240           6068000         36.8     25     245            No        0.998
## 241           7569597         22.0      5      45            No        1.026
## 242           1631990         47.4      1       5            No        0.953
## 243           7626416          7.7      5      43            No        0.991
## 244           1283083          2.6     88    1355            No        1.027
## 245            629647          3.0      8     220            No        1.020
## 246            827210          2.5     28     440            No        1.021
## 247            395628          4.7      2      24            No        0.986
## 248          12296399          0.9      2      67            No        0.994
## 249           1203806         68.5     23     361            No        1.056
## 250             61809         99.9      1       5            No        1.029
## 251           7372263         63.6    125    2113            No        1.007
## 252            486349         45.0      4      47            No        1.016
## 253            561747         22.7     19     238            No        1.002
## 254          11946490          9.8     23     443            No        1.010
## 255           1192685         68.7     16     230            No        0.988
## 256          13375011          0.7      7      85            No        1.010
## 257           4646721         23.4     31     536            No        1.007
## 258           2096169         45.8     54     785            No        1.017
## 259            454494         97.8      1       9            No        1.029
## 260           2166444         71.3     27     517           Yes        0.998
## 261           8813103         87.4    129    2249 Occassionally        0.991
## 262                11        100.0      2      14            No        0.972
## 263            979536         66.7     49     784 Occassionally        0.965
## 264          11021363         74.8    144    2601 Occassionally        1.019
## 265            822612         58.3      5      33            No        0.986
## 266            809142          0.0     18     108 Occassionally        0.971
## 267           1560666          0.1     11      80            No        0.977
## 268           1541533          0.0      1      22 Occassionally        0.964
## 269           7203177          0.1      1       6            No        1.144
## 270           1417350          0.0      4     114 Occassionally        1.010
## 271            339905          3.3      1      13            No        1.027
## 272           5848254         77.6    113    2208           Yes        1.007
## 273           1212418          3.0      2      75 Occassionally        0.998
## 274           1524764         37.1     11     173           Yes        0.992
## 275           2138141          0.0      2      32 Occassionally        1.011
## 276           4984802          0.0      2      38            No        1.037
## 277            982223          0.5      1      60            No        1.027
## 278          12166776         78.2    145    2741           Yes        1.006
## 279           4816434         54.9     33     539            No        1.004
## 280           6966508         97.4    150    2900           Yes        1.025
## 281           5674675         72.8     51     744           Yes        0.998
## 282           6814741         85.1    129    2484           Yes        1.008
## 283            678899          1.4      1       3 Occassionally        0.970
## 284           1513859         38.4     12     114            No        1.039
## 285           2255002         99.1     24     256 Occassionally        1.001
## 286          15172222         31.5     99    1868 Occassionally        1.022
## 287            973495         98.9     40     555            No        0.994
## 288              5713         89.2      1      45 Occassionally        0.975
## 289           7285592         26.4     42     640           Yes        1.003
## 290           1098430          3.7      2      34           Yes        1.057
## 291            573973         17.2      5      35 Occassionally        1.000
## 292           6966508         97.4      4     113            No        0.982
## 293           1338832         99.3     21     186           Yes        0.958
## 294           3108587         78.6     35     662           Yes        1.022
## 295           2288764         74.8     31     509           Yes        1.018
## 296             83615         99.6      1      66 Occassionally        0.987
## 297            170665         97.8      5      51 Occassionally        1.012
## 298           6344073         99.6    106    1931           Yes        0.995
## 299           1743398         99.6      5      68            No        0.988
## 300            378199         19.4      1      11            No        1.002
## 301            983801         13.3      5      60            No        1.000
## 302           1608640         34.6     12     150 Occassionally        1.008
## 303           1473870         39.8     10     120            No        1.022
## 304           1978442         44.3     19     332 Occassionally        1.001
## 305           1623658         45.2      6      81            No        0.980
## 306           1441050         65.2     10      79            No        0.964
## 307           1600039         33.9     71    1252 Occassionally        1.044
## 308           1207378          8.3      4      66 Occassionally        0.980
## 309           9770523         91.3    157    3098           Yes        1.005
## 310           1826327         99.4    110    2107 Occassionally        1.002
## 311           3193199         94.6     81    1479 Occassionally        0.979
## 312            152221          4.5      1       2            No        0.977
## 313           3440488         94.1     96    1644           Yes        1.010
## 314            661727         93.3     17     387           Yes        1.015
## 315           3411741         37.0     13     221            No        1.016
## 316           2361146         39.4      9     124 Occassionally        1.007
## 317           1173722         99.7     38     518           Yes        0.985
## 318            659976         99.3      4      26            No        1.093
## 319           7952267         97.8     58     856            No        1.046
## 320           1738168         43.9      9     138            No        0.953
## 321           2357653         35.4     17     373 Occassionally        0.986
## 322           1460606         99.6     18     245            No        1.011
## 323           3546247         48.5     45     789 Occassionally        1.009
## 324             72143          0.0      4      18            No        0.979
## 325            485945          6.3      2      34 Occassionally        1.011
## 326           1188741         54.1     17     268 Occassionally        0.996
## 327            176617         13.8      2      17            No        1.045
## 328           1025406         74.9     13     196            No        0.995
## 329           4729295         58.7     85    1512 Occassionally        1.016
## 330            114047         95.1     16     164            No        0.993
## 331            257377         96.1     17     182            No        0.967
## 332            901812        100.0      6      32            No        1.031
## 333           7453512         71.4     67    1359 Occassionally        0.988
## 334           7791648         91.7    146    2834           Yes        1.010
## 335           4079215         77.1     99    1790 Occassionally        0.998
## 336           3644867         57.9     47     745            No        1.005
## 337           7072892         81.7    116    2130           Yes        1.011
## 338           4052858         95.3    117    2259           Yes        1.001
## 339            479735          5.4      1      11            No        0.978
## 340           7645879         93.7    151    2973           Yes        1.015
## 341           2194408         95.7     15     161           Yes        0.972
## 342           2646009         77.9     27     318           Yes        0.983
## 343          10848592         51.3     31     393           Yes        1.031
## 344           9655747         88.3     58     924           Yes        0.980
## 345          10710974         43.8     11     179           Yes        1.027
## 346           7569978         78.8    144    2485           Yes        1.024
## 347            336615         58.2      1      80 Occassionally        0.956
## 348           2910632         30.5     28     448           Yes        1.034
## 349          10433369         45.3     50     980           Yes        1.028
## 350          11977351         78.7    107    1679           Yes        1.007
## 351           4598602         98.8    105    2102           Yes        0.964
## 352          12021838         74.9     44     658 Occassionally        0.993
## 353          12058892         50.7     28     532 Occassionally        1.030
## 354          18835675          0.0      3      91            No        1.072
## 355            464039          0.0      3      40            No        1.396
## 356           1494513         14.3      4      62            No        1.064
## 357           2104870         14.6     24     454            No        0.998
## 358           1141973          0.1      1      85            No        0.985
## 359            562622          3.2      1       8 Occassionally        1.018
## 360            266829          2.5      1      13 Occassionally        1.003
## 361           2603940          5.3     17     299 Occassionally        0.985
## 362           5208194         54.3     43     608           Yes        1.094
## 363           6144684         75.0     63    1063           Yes        0.988
## 364           1845398          1.7      4      54            No        1.022
## 365           5331486         68.4    116    2171           Yes        0.980
## 366            771106          1.0      2      59 Occassionally        1.024
## 367           3699260         99.5    115    2058           Yes        0.976
## 368           6118248         58.0     80    1427 Occassionally        0.973
## 369            514746          0.0      1       6 Occassionally        1.050
## 370           1429703          0.0      1      18 Occassionally        1.026
## 371           8721974         82.4    137    2636           Yes        0.996
## 372           2650532         99.6     83    1345            No        0.969
## 373            662341          0.0      2      15 Occassionally        1.023
## 374          13484421          0.1      2      14            No        1.080
## 375            214377          8.1      1       6 Occassionally        0.939
## 376            108584         99.2      5     138            No        1.014
## 377           5543921         75.3     54     919 Occassionally        0.987
## 378           1697037         25.0     14     169            No        1.005
## 379           5697055         70.2     75    1334 Occassionally        0.979
## 380           7482153         83.4     80    1536 Occassionally        1.003
## 381            284783         49.4      2      35 Occassionally        0.967
## 382           1534024         99.0     63    1074 Occassionally        0.981
## 383            167680         85.1      4     134 Occassionally        1.001
## 384           1014077         39.9     10     108 Occassionally        1.004
## 385           2019882         36.2     13     182 Occassionally        1.014
## 386           1662422         38.1     38     694 Occassionally        1.011
## 387            251917         79.7      4      30 Occassionally        0.995
## 388            299693         32.4      3      45 Occassionally        1.008
## 389          10332117         81.0    100    1964 Occassionally        1.003
## 390           1632036         52.2      6      86 Occassionally        1.022
## 391           4976099         57.2     79    1214            No        0.990
## 392           1693913         43.3      7     110            No        0.995
## 393           8811370         66.2    106    2145           Yes        1.016
## 394           7084888         18.3     28     401           Yes        1.000
## 395            534444         13.4      4      43 Occassionally        1.001
## 396           2317990         99.7     63     997           Yes        1.010
## 397            957284         93.3     13     320           Yes        1.026
## 398            202673          0.6      1       3 Occassionally        0.974
## 399           2696632         95.8     38     595           Yes        1.002
## 400           1237055         97.8     20     295           Yes        1.012
## 401           2975357         99.6     95    1760           Yes        1.016
## 402           1760288         42.3     16     195 Occassionally        1.006
## 403           3720370          0.9      2      61            No        1.001
## 404           3254073          9.3     14     308 Occassionally        1.008
## 405            363800          1.8      1      43            No        1.003
## 406            639974          0.0      4     127            No        1.023
## 407           1485514          0.9      2      60            No        1.033
## 408            614301          0.0      1      28            No        0.982
## 409           1270578          0.0      1       2            No        0.997
## 410           3549972         24.9     44     805            No        0.996
## 411            564932          0.0      1      48            No        1.047
## 412           1736752          0.0     11      27            No        1.003
## 413            920379          0.0      5      38            No        0.989
## 414           1117672          0.0      2      48            No        0.978
## 415            924446          8.3      4     104            No        1.013
## 416            607470          0.9      1      70            No        1.010
## 417           3423967          0.2      3      74            No        0.999
## 418           2973905         51.1     46     813 Occassionally        1.024
## 419           1674789          5.3      3      81            No        0.992
## 420            585084          0.0      1      11            No        1.038
## 421            812959         47.9     32     515            No        1.021
## 422           1142777         92.1     42     693 Occassionally        1.031
## 423            338649         25.5      4     109            No        0.987
## 424           1405081          7.0      9      37            No        0.968
## 425           1566944          0.0      2      31            No        1.004
## 426            924764         16.2      5     187 Occassionally        1.067
## 427           8283217          0.0      1       5            No        0.986
## 428           1372897          5.2      5     141            No        1.019
## 429            906385          0.0      9      40            No        1.002
## 430           3464873          0.8      3      74            No        1.040
## 431            951871          0.0      5      57            No        1.025
## 432           6285052         58.7    113    2049 Occassionally        1.000
## 433            807544         27.5      9     168 Occassionally        0.998
## 434             43577        100.0      3      50 Occassionally        1.113
## 435          23373085         40.9    159    3173           Yes        0.971
## 436            696010         12.7      2      18            No        1.019
## 437           3006701         23.6     27     566            No        1.008
## 438           1240186         27.7      9     127            No        1.032
## 439            173490          0.0      8     202            No        1.003
## 440           8332693         92.7    140    2658 Occassionally        1.011
## 441           5699957         73.5    104    1999 Occassionally        1.009
## 442            943968         99.1     27     339           Yes        1.014
## 443           1957817         80.6     23     256           Yes        1.000
## 444          12353990         78.1    149    2760           Yes        1.037
## 445           8627596         88.1    141    2595           Yes        1.021
## 446             31977         95.6      2      29            No        0.986
## 447               193         99.7      2       8            No        0.790
## 448          14768466         65.7    159    3202           Yes        1.005
## 449           3986938        100.0      1      10            No        0.999
## 450           1188701          0.1      1       4            No        1.019
## 451            246472         92.6      5     170            No        1.001
## 452           3157687         70.0     42     738           Yes        1.008
## 453           1853985         42.8     15     207           Yes        0.979
## 454           4308423         67.8     28     364            No        0.990
## 455           3222789         88.6     84    1554           Yes        1.022
## 456          21039688          6.6     53     914 Occassionally        1.013
## 457           4307745         61.5     69    1100            No        1.007
## 458           3107454         54.6     27     422            No        1.002
## 459           6151813         17.2     24     365            No        1.009
## 460           3789041         96.8    106    1854 Occassionally        1.000
## 461           9481125         85.0    154    3008 Occassionally        1.015
## 462           1065932          0.0      1       8            No        1.163
## 463           3336421         73.6     97    1643           Yes        1.037
## 464           4727129         66.0     97    1790 Occassionally        1.021
## 465           2766244         68.7     25     389 Occassionally        0.993
## 466           3857414         86.4     44     650 Occassionally        1.017
## 467            796201         92.4     15     362           Yes        0.993
## 468           1914233         47.9     21     377 Occassionally        1.016
## 469            648985          0.0      2      30            No        0.930
## 470            929501         10.6      5      83            No        1.037
## 471           3167428         14.7     17     335            No        1.043
## 472           9798191          0.0     10      49            No        1.000
## 473            691722          1.3      2      47            No        1.103
## 474            956681          1.0      1      17            No        1.079
## 475          10345844          0.0      6      24            No        1.063
## 476           2662399         66.7     58     907            No        1.034
## 477           5175732          0.0      6      34            No        0.960
## 478            963609          2.7      3      70            No        1.031
## 479           1280084          1.5      1      66            No        1.002
## 480          16257566          0.3      2      41            No        1.077
## 481            633122          0.0      2      10            No        0.984
## 482            785543          0.0      2      21            No        1.117
## 483           1030657          1.0      2      27            No        1.059
## 484           1337098          0.1      2      15            No        1.028
## 485            300029          0.0     10      89            No        0.948
## 486           2569426         37.9     21     413            No        1.017
## 487            306586          0.0      3      71            No        1.020
## 488            447357          0.0      4      13            No        1.005
## 489          16454457          0.0      3      47            No        1.089
## 490          13366214          3.5     12     204            No        1.017
## 491            405146          5.1     23     221            No        1.023
## 492            292808          0.0      1       3            No        1.019
## 493           1218668         37.5     11     283            No        1.025
## 494           2411678         38.5      3      61            No        1.076
## 495           1253417         46.0     14     300            No        1.004
## 496           1482032          0.0     10      98            No        0.972
## 497           1471346         29.8     10     223 Occassionally        1.022
## 498           7879880         26.3     11     141            No        1.001
## 499            331556          3.9      1      12            No        1.041
## 500           6758554         36.3     11     120            No        1.000
## 501          12796462         69.6    157    3138           Yes        1.014
## 502            566884         28.1      3      55 Occassionally        1.014
## 503            378586         40.4      2      43 Occassionally        0.988
## 504           1400115         22.9      7      97 Occassionally        1.019
## 505          13293441         64.1    155    3019           Yes        1.012
## 506           2826965         36.1     19     273 Occassionally        1.015
## 507           2557826         92.9     12     217            No        0.978
## 508           9861155         88.5    146    2833           Yes        1.001
## 509            184169         98.5      4     148 Occassionally        1.035
## 510           5926897         76.1    114    2128           Yes        1.022
## 511           2995201         99.4     96    1832           Yes        1.024
## 512            724386         89.3     11     275 Occassionally        1.072
## 513            830311         98.6     11      98            No        0.965
## 514           3357983         99.5     70    1279 Occassionally        0.997
## 515           1851996         40.7     12     200 Occassionally        1.076
## 516            500006        100.0      9      73            No        1.003
## 517            748186         47.2      3      41            No        1.019
## 518           4371254         52.6     89    1628 Occassionally        1.007
## 519           1353850         30.3     24     433            No        0.986
## 520           3597487         34.4     38     592            No        0.978
## 521           2162317         67.2     88    1291            No        1.000
## 522           9209963          0.5      3      52            No        1.026
## 523          15019778         31.7    107    1912            No        1.017
## 524            430855          6.1     25     413            No        1.026
## 525          50824569          0.5      3      12            No        0.972
## 526          14100412          0.2      2      37            No        0.987
## 527         112387917          0.0      3      33            No        1.035
## 528         185968946          0.1      2      35            No        0.979
## 529           5889912        100.0      2      27 Occassionally        0.949
## 530            274576          0.0      1       6 Occassionally        1.308
## 531           2830019          0.0      7      83 Occassionally        1.214
## 532             83675          0.0      3      28 Occassionally        1.167
## 533           8282522        100.0      4      27            No        1.131
## 534            679522        100.0      2      22 Occassionally        1.148
## 535           6449044         77.8     91    1695            No        1.015
## 536           6959879         28.1      1       6            No        1.090
## 537          11793597          9.8     14     285            No        1.008
## 538           4947116         95.5    107    1962            No        1.020
## 539          12665156          0.5      1       4            No        1.071
## 540           5339094         27.2      2      25            No        1.029
## 541          26213789         37.2    141    2663            No        1.008
## 542          12421314         71.7     29     506            No        0.992
## 543           7126440         18.1      1      12            No        1.008
## 544           2767562         68.4     21     355            No        1.006
## 545          10827862         87.2     23     396            No        1.034
## 546          15419124         48.7     55     875            No        0.988
## 547           8828728         62.0     15     334            No        0.989
## 548           1223485         60.1      3      54            No        1.041
## 549            665345          2.2      1       8            No        1.060
## 550          26419123         23.1     52     798            No        1.015
## 551            654543          0.1      1       8            No        1.079
##     lower_95_percent_ci upper_95_percent_ci
## 1                 0.971               1.055
## 2                 0.964               1.009
## 3                 1.016               1.104
## 4                 0.956               1.041
## 5                 0.975               1.036
## 6                 1.152               1.243
## 7                 1.022               1.038
## 8                 0.982               1.017
## 9                 1.008               1.060
## 10                0.965               0.998
## 11                0.990               1.020
## 12                0.956               1.034
## 13                0.964               1.006
## 14                1.006               1.052
## 15                1.000               1.079
## 16                1.027               1.051
## 17                0.968               1.078
## 18                1.038               1.063
## 19                0.850               0.969
## 20                1.089               1.169
## 21                1.006               1.023
## 22                1.001               1.054
## 23                1.040               1.058
## 24                0.913               1.020
## 25                0.997               1.024
## 26                0.821               1.005
## 27                1.008               1.034
## 28                1.023               1.043
## 29                1.028               1.072
## 30                0.966               0.985
## 31                1.016               1.046
## 32                0.964               1.015
## 33                0.991               1.043
## 34                1.021               1.047
## 35                1.081               1.229
## 36                1.000               1.036
## 37                0.880               1.082
## 38                0.955               1.022
## 39                0.994               1.016
## 40                1.042               1.104
## 41                0.944               1.046
## 42                0.958               1.012
## 43                0.988               1.063
## 44                1.023               1.039
## 45                0.971               1.101
## 46                0.950               1.030
## 47                1.151               1.185
## 48                1.043               1.055
## 49                1.052               1.120
## 50                1.020               1.064
## 51                1.079               1.133
## 52                1.032               1.176
## 53                1.079               1.111
## 54                1.104               1.224
## 55                1.046               1.076
## 56                1.053               1.118
## 57                1.039               1.080
## 58                1.069               1.116
## 59                1.054               1.237
## 60                0.945               0.970
## 61                0.973               0.995
## 62                0.993               1.047
## 63                0.989               1.080
## 64                0.973               0.996
## 65                1.013               1.091
## 66                0.995               1.104
## 67                0.987               1.054
## 68                0.975               1.066
## 69                0.876               0.967
## 70                0.967               1.077
## 71                0.965               1.026
## 72                0.999               1.045
## 73                1.137               1.224
## 74                1.025               1.084
## 75                0.886               1.025
## 76                0.876               1.042
## 77                1.005               1.028
## 78                1.015               1.085
## 79                0.973               1.021
## 80                1.052               1.106
## 81                0.997               1.005
## 82                0.917               1.056
## 83                0.981               1.007
## 84                0.960               0.991
## 85                1.004               1.052
## 86                0.995               1.015
## 87                0.952               1.016
## 88                0.755               0.902
## 89                0.914               1.048
## 90                0.982               1.067
## 91                0.984               1.047
## 92                0.925               0.996
## 93                0.943               1.027
## 94                0.915               1.024
## 95                0.951               0.986
## 96                0.983               1.010
## 97                0.998               1.021
## 98                0.953               0.985
## 99                0.988               1.026
## 100               0.909               1.026
## 101               1.101               1.144
## 102               0.932               1.000
## 103               0.980               1.025
## 104               1.009               1.040
## 105               1.000               1.048
## 106               0.987               1.037
## 107               0.747               1.220
## 108               0.948               0.989
## 109               0.988               1.000
## 110               0.984               1.008
## 111               0.988               1.020
## 112               1.020               1.032
## 113               0.987               1.012
## 114               0.980               1.009
## 115               0.980               1.020
## 116               1.009               1.047
## 117               0.995               1.071
## 118               0.804               0.983
## 119               0.982               1.025
## 120               0.964               1.039
## 121               0.728               0.991
## 122               0.929               1.039
## 123               1.016               1.044
## 124               0.985               1.009
## 125               0.676               0.798
## 126               1.010               1.072
## 127               0.996               1.038
## 128               1.005               1.013
## 129               1.035               1.088
## 130               0.966               1.018
## 131               0.967               0.987
## 132               0.985               1.028
## 133               0.981               1.015
## 134               0.871               1.118
## 135               0.975               1.023
## 136               1.002               1.300
## 137               0.977               0.996
## 138               0.966               1.055
## 139               1.004               1.041
## 140               0.979               1.006
## 141               0.861               0.912
## 142               1.016               1.036
## 143               0.973               1.000
## 144               1.000               1.024
## 145               1.004               1.035
## 146               1.017               1.028
## 147               0.975               0.990
## 148               1.026               1.052
## 149               0.966               1.001
## 150               1.017               1.022
## 151               1.023               1.038
## 152               0.979               0.990
## 153               1.013               1.052
## 154               0.987               1.000
## 155               0.991               1.014
## 156               1.019               1.037
## 157               0.996               1.010
## 158               0.989               1.004
## 159               1.045               1.067
## 160               1.001               1.033
## 161               1.060               1.121
## 162               1.027               1.069
## 163               1.030               1.055
## 164               1.056               1.104
## 165               1.003               1.065
## 166               1.060               1.109
## 167               1.005               1.041
## 168               0.915               1.020
## 169               0.934               1.015
## 170               1.020               1.071
## 171               1.061               1.093
## 172               1.035               1.079
## 173               1.027               1.047
## 174               1.065               1.170
## 175               0.990               1.028
## 176               0.985               1.014
## 177               1.097               1.304
## 178               0.982               1.013
## 179               1.021               1.026
## 180               0.922               1.089
## 181               1.066               1.223
## 182               0.977               1.014
## 183               0.972               1.024
## 184               1.030               1.061
## 185               1.136               1.222
## 186               1.006               1.009
## 187               0.992               1.029
## 188               1.039               1.074
## 189               0.887               0.958
## 190               0.820               0.909
## 191               0.993               1.004
## 192               1.034               1.057
## 193               0.975               1.001
## 194               1.036               1.039
## 195               0.980               1.026
## 196               0.997               1.016
## 197               1.042               1.084
## 198               1.002               1.031
## 199               0.983               1.084
## 200               0.963               1.004
## 201               1.006               1.011
## 202               1.031               1.037
## 203               1.026               1.029
## 204               1.000               1.011
## 205               1.016               1.033
## 206               1.054               1.077
## 207               1.048               1.091
## 208               0.946               0.985
## 209               1.045               1.085
## 210               1.016               1.035
## 211               1.033               1.082
## 212               1.064               1.079
## 213               0.947               1.127
## 214               1.029               1.043
## 215               1.004               1.007
## 216               1.063               1.152
## 217               1.103               1.123
## 218               0.983               1.036
## 219               1.029               1.035
## 220               1.029               1.054
## 221               1.005               1.012
## 222               1.048               1.058
## 223               0.972               1.008
## 224               1.030               1.102
## 225               1.012               1.034
## 226               0.995               1.018
## 227               0.955               1.041
## 228               0.963               1.017
## 229               0.926               0.943
## 230               0.890               0.979
## 231               0.916               0.991
## 232               0.939               0.991
## 233               0.974               1.003
## 234               0.947               1.001
## 235               0.935               1.055
## 236               0.859               1.002
## 237               0.790               0.937
## 238               1.022               1.144
## 239               0.988               1.006
## 240               0.968               1.019
## 241               0.987               1.049
## 242               0.878               1.031
## 243               0.936               1.048
## 244               1.009               1.048
## 245               1.007               1.034
## 246               1.007               1.034
## 247               0.961               1.010
## 248               0.959               1.024
## 249               1.020               1.094
## 250               1.018               1.042
## 251               0.986               1.019
## 252               0.991               1.038
## 253               0.993               1.012
## 254               1.004               1.018
## 255               0.975               1.000
## 256               0.985           18000.000
## 257               0.999               1.014
## 258               0.966               1.031
## 259               1.002               1.054
## 260               0.982               1.006
## 261               0.970               1.005
## 262               0.927               1.019
## 263               0.905               0.998
## 264               1.001               1.029
## 265               0.933               1.046
## 266               0.929               0.995
## 267               0.940               1.013
## 268               0.936               0.994
## 269               1.092               1.217
## 270               0.992               1.029
## 271               0.973               1.092
## 272               1.006               1.009
## 273               0.985               1.011
## 274               0.982               1.001
## 275               0.986               1.035
## 276               1.020               1.051
## 277               1.001               1.042
## 278               1.003               1.008
## 279               0.998               1.011
## 280               1.022               1.028
## 281               0.991               1.004
## 282               1.006               1.009
## 283               0.885               1.053
## 284               1.010               1.062
## 285               0.978               1.015
## 286               1.005               1.037
## 287               0.979               1.012
## 288               0.966               0.982
## 289               0.984               1.017
## 290               1.039               1.078
## 291               0.979               1.031
## 292               0.967               0.993
## 293               0.915               0.992
## 294               1.014               1.029
## 295               1.011               1.026
## 296               0.977               0.997
## 297               0.997               1.025
## 298               0.984               1.004
## 299               0.966               1.018
## 300               0.965               1.041
## 301               0.980               1.020
## 302               0.995               1.021
## 303               0.991               1.058
## 304               0.993               1.008
## 305               0.960               1.003
## 306               0.914               1.018
## 307               1.035               1.054
## 308               0.944               1.012
## 309               0.996               1.013
## 310               0.999               1.004
## 311               0.975               0.982
## 312               0.883               1.109
## 313               1.005               1.014
## 314               1.006               1.022
## 315               1.003               1.029
## 316               0.990               1.027
## 317               0.976               0.993
## 318               1.041               1.153
## 319               0.991               1.082
## 320               0.887               1.023
## 321               0.969               0.999
## 322               0.998               1.024
## 323               1.003               1.015
## 324               0.761               1.149
## 325               1.001               1.022
## 326               0.982               1.006
## 327               1.013               1.076
## 328               0.968               1.017
## 329               1.007               1.021
## 330               0.972               1.019
## 331               0.946               0.985
## 332               0.936               1.099
## 333               0.939               1.018
## 334               1.008               1.012
## 335               0.994               1.003
## 336               0.988               1.030
## 337               1.004               1.019
## 338               0.997               1.004
## 339               0.953               1.008
## 340               1.006               1.023
## 341               0.839               1.038
## 342               0.937               1.003
## 343               0.976               1.056
## 344               0.953               1.003
## 345               0.977               1.069
## 346               1.018               1.028
## 347               0.921               0.993
## 348               1.022               1.056
## 349               0.999               1.048
## 350               0.968               1.027
## 351               0.955               0.971
## 352               0.946               1.028
## 353               0.946               1.080
## 354               1.027               1.116
## 355               1.308               1.499
## 356               1.026               1.106
## 357               0.972               1.018
## 358               0.956               1.013
## 359               1.004               1.033
## 360               0.977               1.026
## 361               0.970               0.998
## 362               1.062               1.131
## 363               0.981               0.996
## 364               0.991               1.052
## 365               0.970               0.989
## 366               1.009               1.039
## 367               0.964               0.987
## 368               0.969               0.977
## 369               1.018               1.083
## 370               0.996               1.056
## 371               0.986               1.004
## 372               0.944               0.989
## 373               0.999               1.050
## 374               0.927               1.278
## 375               0.923               0.954
## 376               0.982               1.040
## 377               0.981               0.993
## 378               0.945               1.047
## 379               0.977               0.982
## 380               1.000               1.006
## 381               0.946               0.986
## 382               0.978               0.985
## 383               0.987               1.013
## 384               0.994               1.015
## 385               1.005               1.025
## 386               1.005               1.016
## 387               0.976               1.018
## 388               0.996               1.020
## 389               1.001               1.005
## 390               0.994               1.055
## 391               0.976               1.016
## 392               0.978               1.009
## 393               0.979               1.020
## 394               0.988               1.011
## 395               0.985               1.016
## 396               1.008               1.012
## 397               1.019               1.034
## 398               0.944               1.018
## 399               0.989               1.009
## 400               1.003               1.018
## 401               1.014               1.018
## 402               0.996               1.016
## 403               0.978               1.016
## 404               1.003               1.014
## 405               0.988               1.018
## 406               1.008               1.041
## 407               1.014               1.052
## 408               0.958               1.006
## 409               0.915               1.069
## 410               0.990               1.000
## 411               1.028               1.069
## 412               0.862               1.035
## 413               0.963               1.019
## 414               0.917               1.006
## 415               0.996               1.028
## 416               1.000               1.019
## 417               0.985               1.013
## 418               1.018               1.030
## 419               0.983               1.001
## 420               1.023               1.054
## 421               1.012               1.028
## 422               1.025               1.036
## 423               0.981               0.993
## 424               0.735               1.027
## 425               0.975               1.036
## 426               1.057               1.077
## 427               0.954               1.026
## 428               1.001               1.033
## 429               0.932               1.480
## 430               1.022               1.058
## 431               1.010               1.039
## 432               0.995               1.004
## 433               0.993               1.004
## 434               1.084               1.160
## 435               0.882               0.988
## 436               0.996               1.052
## 437               1.002               1.013
## 438               1.015               1.050
## 439               0.990               1.016
## 440               1.007               1.015
## 441               1.006               1.012
## 442               1.008               1.020
## 443               0.983               1.017
## 444               1.032               1.042
## 445               1.019               1.022
## 446               0.976               0.996
## 447               0.763               0.827
## 448               0.996               1.014
## 449               0.948               1.051
## 450               0.965               1.081
## 451               0.970               1.020
## 452               1.004               1.011
## 453               0.967               0.988
## 454               0.977               0.999
## 455               1.019               1.024
## 456               1.008               1.017
## 457               0.997               1.015
## 458               0.995               1.008
## 459               1.000               1.016
## 460               0.984               1.007
## 461               0.999               1.026
## 462               1.114               1.229
## 463               1.033               1.040
## 464               1.018               1.024
## 465               0.943               1.025
## 466               1.011               1.023
## 467               0.962               1.011
## 468               0.934               1.097
## 469               0.901               0.954
## 470               1.018               1.058
## 471               1.038               1.047
## 472               0.578               1.116
## 473               1.090               1.123
## 474               1.043               1.123
## 475               1.002               1.126
## 476               1.031               1.037
## 477               0.713               1.007
## 478               1.018               1.048
## 479               0.993               1.011
## 480               1.062               1.094
## 481               0.954               1.009
## 482               1.080               1.161
## 483               1.031               1.088
## 484               1.001               1.058
## 485               0.652               1.027
## 486               1.013               1.021
## 487               0.995               1.047
## 488               0.936               1.060
## 489               1.057               1.124
## 490               1.007               1.026
## 491               0.991               1.051
## 492               0.851               1.207
## 493               1.017               1.032
## 494               1.056               1.097
## 495               1.000               1.010
## 496               0.907               1.012
## 497               1.012               1.030
## 498               0.987               1.014
## 499               1.020               1.066
## 500               0.980               1.017
## 501               1.013               1.015
## 502               1.004               1.024
## 503               0.974               1.003
## 504               1.010               1.026
## 505               1.008               1.015
## 506               1.011               1.018
## 507               0.962               0.994
## 508               0.999               1.003
## 509               1.027               1.042
## 510               1.014               1.031
## 511               1.015               1.032
## 512               1.049               1.087
## 513               0.948               0.982
## 514               0.988               1.004
## 515               1.053               1.096
## 516               0.985               1.028
## 517               0.994               1.043
## 518               0.991               1.010
## 519               0.971               1.000
## 520               0.918               0.996
## 521               0.986               1.019
## 522               1.011               1.043
## 523               1.008               1.027
## 524               1.014               1.040
## 525               0.845               1.237
## 526               0.943               1.041
## 527               0.977               1.090
## 528               0.943               1.015
## 529               0.878               1.046
## 530               1.226               1.418
## 531               1.166               1.263
## 532               1.065               1.265
## 533               1.002               1.228
## 534               1.053               1.223
## 535               1.012               1.018
## 536               1.015               1.146
## 537               1.000               1.017
## 538               1.016               1.023
## 539               1.023               1.131
## 540               0.991               1.067
## 541               1.006               1.010
## 542               0.972               1.006
## 543               0.939               1.030
## 544               0.994               1.015
## 545               1.015               1.047
## 546               0.977               0.998
## 547               0.972               1.002
## 548               1.023               1.060
## 549               1.034               1.090
## 550               1.010               1.022
## 551               1.052               1.110
```



```r
ducks <- filter(ecosphere, family=="Anatidae")
ducks
```

```
## # A tibble: 44 × 21
##    order    family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##    <chr>    <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
##  1 Anserif… Anati… "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09
##  2 Anserif… Anati… "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88
##  3 Anserif… Anati… "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96
##  4 Anserif… Anati… "Black… Branta… Vege… Long    Wetland No      Modera…    3.11
##  5 Anserif… Anati… "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02
##  6 Anserif… Anati… "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88
##  7 Anserif… Anati… "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56
##  8 Anserif… Anati… "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6 
##  9 Anserif… Anati… "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45
## 10 Anserif… Anati… "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08
## # … with 34 more rows, 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```



```r
ducks %>% 
  select(!order) %>% 
  select(!family)
```

```
## # A tibble: 44 × 19
##    commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶ mean_…⁷ mean_…⁸
##    <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>
##  1 "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09     9       1  
##  2 "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88     7.5     1  
##  3 "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96    10.5     3  
##  4 "Black… Branta… Vege… Long    Wetland No      Modera…    3.11     3.5     2.5
##  5 "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02     9.5     2  
##  6 "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88    13.5     1  
##  7 "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56    10       0.6
##  8 "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6      8.5     2  
##  9 "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45     5       1  
## 10 "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08     8       1  
## # … with 34 more rows, 9 more variables: population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass, ⁷​mean_eggs_per_clutch, ⁸​mean_age_at_sexual_maturity
```


Problem 7. (2 points) We might assume that all ducks live in wetland habitat. Is this true for the ducks in these data? If there are exceptions, list the species below.

```r
ducks %>%
  filter(habitat!="Wetland")
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Anserifo… Anati… Common… Somate… Inve… Middle  Ocean   No      Short      3.31
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

Problem 8. (4 points) In ducks, how is mean body mass associated with migratory strategy? Do the ducks that migrate long distances have high or low average body mass?

```r
ducks %>% 
  group_by(migratory_strategy) %>%
  summarise(body_mass=mean(log10_mass))
```

```
## # A tibble: 5 × 2
##   migratory_strategy body_mass
##   <chr>                  <dbl>
## 1 Long                    2.87
## 2 Moderate                3.11
## 3 Resident                4.03
## 4 Short                   2.98
## 5 Withdrawal              2.92
```

Problem 9. (2 points) Accipitridae is the family that includes eagles, hawks, kites, and osprey. First, make a new object `eagles` that only includes species in the family Accipitridae. Next, restrict these data to only include the variables common_name, scientific_name, and population_size.

```r
eagles <-
  ecosphere %>%
  filter(family=="Accipitridae") %>%
  select(common_name, scientific_name, population_size)
  eagles
```

```
## # A tibble: 20 × 3
##    common_name         scientific_name          population_size
##    <chr>               <chr>                              <dbl>
##  1 Bald Eagle          Haliaeetus leucocephalus              NA
##  2 Broad-winged Hawk   Buteo platypterus                1700000
##  3 Cooper's Hawk       Accipiter cooperii                700000
##  4 Ferruginous Hawk    Buteo regalis                      80000
##  5 Golden Eagle        Aquila chrysaetos                 130000
##  6 Gray Hawk           Buteo nitidus                         NA
##  7 Harris's Hawk       Parabuteo unicinctus               50000
##  8 Hook-billed Kite    Chondrohierax uncinatus               NA
##  9 Northern Goshawk    Accipiter gentilis                200000
## 10 Northern Harrier    Circus cyaneus                    700000
## 11 Red-shouldered Hawk Buteo lineatus                   1100000
## 12 Red-tailed Hawk     Buteo jamaicensis                2000000
## 13 Rough-legged Hawk   Buteo lagopus                     300000
## 14 Sharp-shinned Hawk  Accipiter striatus                500000
## 15 Short-tailed Hawk   Buteo brachyurus                      NA
## 16 Snail Kite          Rostrhamus sociabilis                 NA
## 17 Swainson's Hawk     Buteo swainsoni                   540000
## 18 White-tailed Hawk   Buteo albicaudatus                    NA
## 19 White-tailed Kite   Elanus leucurus                       NA
## 20 Zone-tailed Hawk    Buteo albonotatus                     NA
```



Problem 10. (4 points) In the eagles data, any species with a population size less than 250,000 individuals is threatened. Make a new column `conservation_status` that shows whether or not a species is threatened.

```r
?ifelse
```



```r
eagles<- eagles %>% 
  mutate(conservation_status = ifelse(population_size < 250000, "Threatened", "Not Threatened"))
eagles
```

```
## # A tibble: 20 × 4
##    common_name         scientific_name          population_size conservation_s…¹
##    <chr>               <chr>                              <dbl> <chr>           
##  1 Bald Eagle          Haliaeetus leucocephalus              NA <NA>            
##  2 Broad-winged Hawk   Buteo platypterus                1700000 Not Threatened  
##  3 Cooper's Hawk       Accipiter cooperii                700000 Not Threatened  
##  4 Ferruginous Hawk    Buteo regalis                      80000 Threatened      
##  5 Golden Eagle        Aquila chrysaetos                 130000 Threatened      
##  6 Gray Hawk           Buteo nitidus                         NA <NA>            
##  7 Harris's Hawk       Parabuteo unicinctus               50000 Threatened      
##  8 Hook-billed Kite    Chondrohierax uncinatus               NA <NA>            
##  9 Northern Goshawk    Accipiter gentilis                200000 Threatened      
## 10 Northern Harrier    Circus cyaneus                    700000 Not Threatened  
## 11 Red-shouldered Hawk Buteo lineatus                   1100000 Not Threatened  
## 12 Red-tailed Hawk     Buteo jamaicensis                2000000 Not Threatened  
## 13 Rough-legged Hawk   Buteo lagopus                     300000 Not Threatened  
## 14 Sharp-shinned Hawk  Accipiter striatus                500000 Not Threatened  
## 15 Short-tailed Hawk   Buteo brachyurus                      NA <NA>            
## 16 Snail Kite          Rostrhamus sociabilis                 NA <NA>            
## 17 Swainson's Hawk     Buteo swainsoni                   540000 Not Threatened  
## 18 White-tailed Hawk   Buteo albicaudatus                    NA <NA>            
## 19 White-tailed Kite   Elanus leucurus                       NA <NA>            
## 20 Zone-tailed Hawk    Buteo albonotatus                     NA <NA>            
## # … with abbreviated variable name ¹​conservation_status
```

Problem 11. (2 points) Consider the results from questions 9 and 10. Are there any species for which their threatened status needs further study? How do you know?

```r
eagles %>% 
  filter(is.na(conservation_status))
```

```
## # A tibble: 8 × 4
##   common_name       scientific_name          population_size conservation_status
##   <chr>             <chr>                              <dbl> <chr>              
## 1 Bald Eagle        Haliaeetus leucocephalus              NA <NA>               
## 2 Gray Hawk         Buteo nitidus                         NA <NA>               
## 3 Hook-billed Kite  Chondrohierax uncinatus               NA <NA>               
## 4 Short-tailed Hawk Buteo brachyurus                      NA <NA>               
## 5 Snail Kite        Rostrhamus sociabilis                 NA <NA>               
## 6 White-tailed Hawk Buteo albicaudatus                    NA <NA>               
## 7 White-tailed Kite Elanus leucurus                       NA <NA>               
## 8 Zone-tailed Hawk  Buteo albonotatus                     NA <NA>
```

Problem 12. (4 points) Use the `ecosphere` data to perform one exploratory analysis of your choice. The analysis must have a minimum of three lines and two functions. You must also clearly state the question you are attempting to answer.

```r
structure(ecosphere)
```

```
## # A tibble: 551 × 21
##    order    family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##    <chr>    <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
##  1 Anserif… Anati… "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09
##  2 Anserif… Anati… "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88
##  3 Anserif… Anati… "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96
##  4 Anserif… Anati… "Black… Branta… Vege… Long    Wetland No      Modera…    3.11
##  5 Anserif… Anati… "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02
##  6 Anserif… Anati… "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88
##  7 Anserif… Anati… "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56
##  8 Anserif… Anati… "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6 
##  9 Anserif… Anati… "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45
## 10 Anserif… Anati… "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08
## # … with 541 more rows, 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

What species that eats only vegetation has the highest 'mean_eggs_per_clutch'?

```r
ecosphere %>% 
  group_by(diet) %>% 
  filter(diet=="Vegetation") %>% 
  group_by(scientific_name) %>% 
  summarise(eggs= sum(mean_eggs_per_clutch)) %>% 
  arrange(desc(eggs))
```

```
## # A tibble: 31 × 2
##    scientific_name           eggs
##    <chr>                    <dbl>
##  1 Dendrocygna autumnalis    13.5
##  2 Dendrocygna bicolor       13  
##  3 Tympanuchus phasianellus  11  
##  4 Bonasa umbellus           10.5
##  5 Anas cyanoptera           10  
##  6 Anas discors              10  
##  7 Aythya affinis            10  
##  8 Aythya americana          10  
##  9 Aythya collaris           10  
## 10 Aythya fuligula           10  
## # … with 21 more rows
```

Please provide the names of the students you have worked with with during the exam:

```r
#Rafael Vasquez-Chan and Shefali Suresh
```

Please be 100% sure your exam is saved, knitted, and pushed to your github repository. No need to submit a link on canvas, we will find your exam in your repository.
