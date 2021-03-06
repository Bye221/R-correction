Introduction to the tidyverse
================

Getting started
---------------

``` r
if (!require("tidyverse")) install.packages(c("tidyverse")) # Only installs if missing
library(tidyverse)
```

A glimpse of the data

``` r
head(diamonds) # diamonds is a built-in dataset in the tidyverse (ggplot2, actually)
```

    ## # A tibble: 6 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ## 2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ## 3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ## 4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ## 5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ## 6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48

``` r
# Below the name of the variable, you will find the variable type:
# dbl  = double = real number,
# int  = integer = integer number,
# ord  = ordered factor,
# fact = unordered factor,
# char = character.
tail(diamonds)
```

    ## # A tibble: 6 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  0.72 Premium   D     SI1      62.7    59  2757  5.69  5.73  3.58
    ## 2  0.72 Ideal     D     SI1      60.8    57  2757  5.75  5.76  3.5 
    ## 3  0.72 Good      D     SI1      63.1    55  2757  5.69  5.75  3.61
    ## 4  0.7  Very Good D     SI1      62.8    60  2757  5.66  5.68  3.56
    ## 5  0.86 Premium   H     SI2      61      58  2757  6.15  6.12  3.74
    ## 6  0.75 Ideal     D     SI2      62.2    55  2757  5.83  5.87  3.64

First plots
-----------

A first example:

``` r
ggplot(diamonds) + geom_point(aes(x = carat, y = price), size = 0.5, color = "#004C99") + xlim(0,3) + 
    stat_smooth(aes(x =carat, y = price), method = "lm", color = "red")
```

    ## Warning: Removed 32 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 32 rows containing missing values (geom_point).

![](Figures/Plotting%20diamonds-1.png) Note: 32 instances are absent because the corresponing diamonds are too big. The green line shows the linearized relationship between size and price. The color is provided using the hexadecimal code for RGB colors.

We can add features.

``` r
ggplot(diamonds) + geom_point(aes(x = carat, y = price, color = clarity, size = cut), alpha = 3/10) + 
  scale_y_sqrt() + ggtitle("Plot of diamond prices") + xlim(0.1,4) 
```

    ## Warning: Removed 5 rows containing missing values (geom_point).

![](Figures/Adding%20graphical%20features-1.png)

Another popular graph type is the bar plot.

``` r
ggplot(diamonds) + geom_bar(aes(x = cut, fill = clarity)) + ggtitle("Number of diamonds") 
```

![](Figures/Alternative%20representation-1.png)

Plotting is closely related to layers (like in Photoshop). Several layers can be superposed. Alternative colour palettes can be used.

``` r
ggplot(diamonds,aes(x = price, fill = color)) + geom_hline(yintercept = 1000) + 
  geom_histogram(bins = 15, position= "dodge") + ggtitle("Number of diamonds") +
    scale_fill_brewer(palette = "RdYlBu")
```

![](Figures/Going%20even%20further%20&%20adding%20a%20layer%20+%20changing%20colour%20palette-1.png)

Further references for graph types:
<https://developers.google.com/chart/interactive/docs/gallery>
<https://plot.ly/r/>
<https://www.r-graph-gallery.com/all-graphs/>
<https://plot.ly/r/shiny-gallery/> (dynamic & interactive)

Data manipulation
-----------------

First, it is imperative to get to know the data.

``` r
summary(diamonds)
```

    ##      carat               cut        color        clarity     
    ##  Min.   :0.2000   Fair     : 1610   D: 6775   SI1    :13065  
    ##  1st Qu.:0.4000   Good     : 4906   E: 9797   VS2    :12258  
    ##  Median :0.7000   Very Good:12082   F: 9542   SI2    : 9194  
    ##  Mean   :0.7979   Premium  :13791   G:11292   VS1    : 8171  
    ##  3rd Qu.:1.0400   Ideal    :21551   H: 8304   VVS2   : 5066  
    ##  Max.   :5.0100                     I: 5422   VVS1   : 3655  
    ##                                     J: 2808   (Other): 2531  
    ##      depth           table           price             x         
    ##  Min.   :43.00   Min.   :43.00   Min.   :  326   Min.   : 0.000  
    ##  1st Qu.:61.00   1st Qu.:56.00   1st Qu.:  950   1st Qu.: 4.710  
    ##  Median :61.80   Median :57.00   Median : 2401   Median : 5.700  
    ##  Mean   :61.75   Mean   :57.46   Mean   : 3933   Mean   : 5.731  
    ##  3rd Qu.:62.50   3rd Qu.:59.00   3rd Qu.: 5324   3rd Qu.: 6.540  
    ##  Max.   :79.00   Max.   :95.00   Max.   :18823   Max.   :10.740  
    ##                                                                  
    ##        y                z         
    ##  Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.: 4.720   1st Qu.: 2.910  
    ##  Median : 5.710   Median : 3.530  
    ##  Mean   : 5.735   Mean   : 3.539  
    ##  3rd Qu.: 6.540   3rd Qu.: 4.040  
    ##  Max.   :58.900   Max.   :31.800  
    ## 

Often, people are interested by subsets of databases.

``` r
filter(diamonds, carat > 4)
```

    ## # A tibble: 5 x 10
    ##   carat cut     color clarity depth table price     x     y     z
    ##   <dbl> <ord>   <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  4.01 Premium I     I1       61      61 15223  10.1 10.1   6.17
    ## 2  4.01 Premium J     I1       62.5    62 15223  10.0  9.94  6.24
    ## 3  4.13 Fair    H     I1       64.8    61 17329  10    9.85  6.43
    ## 4  5.01 Fair    J     I1       65.5    59 18018  10.7 10.5   6.98
    ## 5  4.5  Fair    J     I1       65.8    58 18531  10.2 10.2   6.72

``` r
filter(diamonds, carat > 3 & cut == "Ideal")
```

    ## # A tibble: 4 x 10
    ##   carat cut   color clarity depth table price     x     y     z
    ##   <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1  3.22 Ideal I     I1       62.6    55 12545  9.49  9.42  5.92
    ## 2  3.5  Ideal H     I1       62.8    57 12587  9.65  9.59  6.03
    ## 3  3.01 Ideal J     SI2      61.7    58 16037  9.25  9.2   5.69
    ## 4  3.01 Ideal J     I1       65.4    60 16538  8.99  8.93  5.86

Sometimes, only a few columns matter and it can be useful to only keep those (possibly in a separate variable).

``` r
select(diamonds, carat, cut, color, price)
```

    ## # A tibble: 53,940 x 4
    ##    carat cut       color price
    ##    <dbl> <ord>     <ord> <int>
    ##  1 0.23  Ideal     E       326
    ##  2 0.21  Premium   E       326
    ##  3 0.23  Good      E       327
    ##  4 0.290 Premium   I       334
    ##  5 0.31  Good      J       335
    ##  6 0.24  Very Good J       336
    ##  7 0.24  Very Good I       336
    ##  8 0.26  Very Good H       337
    ##  9 0.22  Fair      E       337
    ## 10 0.23  Very Good H       338
    ## # ... with 53,930 more rows

``` r
select(diamonds, - clarity, - x, - y, - z) # Using the minus sign performs the opposite manipulation and removes the corresponding variable
```

    ## # A tibble: 53,940 x 6
    ##    carat cut       color depth table price
    ##    <dbl> <ord>     <ord> <dbl> <dbl> <int>
    ##  1 0.23  Ideal     E      61.5    55   326
    ##  2 0.21  Premium   E      59.8    61   326
    ##  3 0.23  Good      E      56.9    65   327
    ##  4 0.290 Premium   I      62.4    58   334
    ##  5 0.31  Good      J      63.3    58   335
    ##  6 0.24  Very Good J      62.8    57   336
    ##  7 0.24  Very Good I      62.3    57   336
    ##  8 0.26  Very Good H      61.9    55   337
    ##  9 0.22  Fair      E      65.1    61   337
    ## 10 0.23  Very Good H      59.4    61   338
    ## # ... with 53,930 more rows

Data manipulation is all about sequences of tasks: filtering rows, selecting columns, re-arranging, plotting, etc. A very convenient way to write these sequences is to resort to the **PIPE** operator **%&gt;%**. It works like this:
Data variable %&gt;% Instruction 1 %&gt;% Instruction 2 %&gt;% Instruction 3 %&gt;% ...
Below, we apply successively a filter and a selection. On top of that, you can add a plot.

``` r
filter(diamonds, carat > 4) %>% select(carat, cut, color, price) # Or, equivalently,
```

    ## # A tibble: 5 x 4
    ##   carat cut     color price
    ##   <dbl> <ord>   <ord> <int>
    ## 1  4.01 Premium I     15223
    ## 2  4.01 Premium J     15223
    ## 3  4.13 Fair    H     17329
    ## 4  5.01 Fair    J     18018
    ## 5  4.5  Fair    J     18531

``` r
diamonds %>% 
    filter(carat > 4) %>% 
    select(carat, cut, color, price)
```

    ## # A tibble: 5 x 4
    ##   carat cut     color price
    ##   <dbl> <ord>   <ord> <int>
    ## 1  4.01 Premium I     15223
    ## 2  4.01 Premium J     15223
    ## 3  4.13 Fair    H     17329
    ## 4  5.01 Fair    J     18018
    ## 5  4.5  Fair    J     18531

``` r
diamonds %>% 
    filter(cut == "Fair") %>% 
    ggplot(aes(x = carat, y = price)) + geom_point() + geom_smooth() 
```

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

![](Figures/piping:%20combine%20two%20or%20more%20functions!-1.png)

``` r
# The last part aims to ease visual pattern detection
```

The tidyverse is very efficient at building pivot tables. Effortlessly (almost), they can then be plotted.

``` r
diamonds %>% 
    group_by(cut) %>% 
    summarize(number = n(), avg_price = mean(price)) # n() simply counts the number of occurrences
```

    ## # A tibble: 5 x 3
    ##   cut       number avg_price
    ##   <ord>      <int>     <dbl>
    ## 1 Fair        1610     4359.
    ## 2 Good        4906     3929.
    ## 3 Very Good  12082     3982.
    ## 4 Premium    13791     4584.
    ## 5 Ideal      21551     3458.

``` r
diamonds %>% 
    group_by(cut, clarity) %>% 
    summarize(avg_carat = mean(carat), avg_price = mean(price)) %>%
    #arrange(avg_price)
    ggplot() + geom_point(aes(x = avg_carat, y = avg_price, color = clarity, shape = cut, size = 2)) # Yes, you can control the point size!
```

    ## Warning: Using shapes for an ordinal variable is not advised

![](Figures/Analytics%20101:%20combine%20group_by()%20and%20summarize()%20&%20get%20pivot%20tables!-1.png) Rectangular data can be organized is several ways.

``` r
diamonds %>% group_by(cut, color) %>% summarize(avg_price = mean(price)) # one way to see things.. how do I get a matrix out of this? => spread()!
```

    ## # A tibble: 35 x 3
    ## # Groups:   cut [?]
    ##    cut   color avg_price
    ##    <ord> <ord>     <dbl>
    ##  1 Fair  D         4291.
    ##  2 Fair  E         3682.
    ##  3 Fair  F         3827.
    ##  4 Fair  G         4239.
    ##  5 Fair  H         5136.
    ##  6 Fair  I         4685.
    ##  7 Fair  J         4976.
    ##  8 Good  D         3405.
    ##  9 Good  E         3424.
    ## 10 Good  F         3496.
    ## # ... with 25 more rows

``` r
diamonds %>% group_by(cut, color) %>% summarize(avg_price = mean(price)) %>% spread(key = cut, value = avg_price)
```

    ## # A tibble: 7 x 6
    ##   color  Fair  Good `Very Good` Premium Ideal
    ##   <ord> <dbl> <dbl>       <dbl>   <dbl> <dbl>
    ## 1 D     4291. 3405.       3470.   3631. 2629.
    ## 2 E     3682. 3424.       3215.   3539. 2598.
    ## 3 F     3827. 3496.       3779.   4325. 3375.
    ## 4 G     4239. 4123.       3873.   4501. 3721.
    ## 5 H     5136. 4276.       4535.   5217. 3889.
    ## 6 I     4685. 5079.       5256.   5946. 4452.
    ## 7 J     4976. 4574.       5104.   6295. 4918.
