---
title: "Lab 10 Homework"
author: "Please Add Your Name Here"
date: "2023-02-16"
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


the data looks tidy since NAs and treated as NAs.

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

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?


There are 40 distinct species and 10 distinct genuses. There is a total of 34786 ovservations in the data.
The merrami species was the most frequently sampled in the study whereas the "clarki, scutalatus, tereticaudus, tigris, uniparens, viridis" were least frequently sampled.




```r
deserts %>% 
  count(species_id) %>% 
  arrange(-n)
```

```
## # A tibble: 48 × 2
##    species_id     n
##    <chr>      <int>
##  1 DM         10596
##  2 PP          3123
##  3 DO          3027
##  4 PB          2891
##  5 RM          2609
##  6 DS          2504
##  7 OT          2249
##  8 PF          1597
##  9 PE          1299
## 10 NL          1252
## # … with 38 more rows
```


```r
deserts %>% 
  count(species) %>% 
  arrange(-n)
```

```
## # A tibble: 40 × 2
##    species          n
##    <chr>        <int>
##  1 merriami     10596
##  2 penicillatus  3123
##  3 ordii         3027
##  4 baileyi       2891
##  5 megalotis     2609
##  6 spectabilis   2504
##  7 torridus      2249
##  8 flavus        1597
##  9 eremicus      1299
## 10 albigula      1252
## # … with 30 more rows
```


```r
deserts %>% 
  count(genus) %>% 
  arrange(-n)  
```

```
## # A tibble: 26 × 2
##    genus                n
##    <chr>            <int>
##  1 Dipodomys        16167
##  2 Chaetodipus       6029
##  3 Onychomys         3267
##  4 Reithrodontomys   2694
##  5 Peromyscus        2234
##  6 Perognathus       1629
##  7 Neotoma           1252
##  8 Ammospermophilus   437
##  9 Amphispiza         303
## 10 Spermophilus       249
## # … with 16 more rows
```
  


```r
deserts %>% 
  count(genus,species) %>% 
  arrange(-n)
```

```
## # A tibble: 48 × 3
##    genus           species          n
##    <chr>           <chr>        <int>
##  1 Dipodomys       merriami     10596
##  2 Chaetodipus     penicillatus  3123
##  3 Dipodomys       ordii         3027
##  4 Chaetodipus     baileyi       2891
##  5 Reithrodontomys megalotis     2609
##  6 Dipodomys       spectabilis   2504
##  7 Onychomys       torridus      2249
##  8 Perognathus     flavus        1597
##  9 Peromyscus      eremicus      1299
## 10 Neotoma         albigula      1252
## # … with 38 more rows
```

3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.



```r
deserts %>% 
  ggplot(aes(taxa))+
  geom_bar()+
  scale_y_log10()+
  coord_flip()+
  labs(title = "Proportion of Taxa",
       x="Taxa",
       y="log10 Count")
```

![](lab10_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`




```r
deserts %>% 
  ggplot(aes(x=taxa, fill=plot_type))+
  geom_bar()+
  scale_y_log10()+
  coord_flip()+
  labs(title="Proportion of Plot Type",
       x="Plot Type",
       y="Count Log10")
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.



```r
deserts %>% 
  filter(weight!="NA") %>% 
  ggplot(aes(x=species_id,y=weight))+
  geom_boxplot()+
  labs(title= "Weight Distribution of Species",
       x="Species ID",
       y="Weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.

```r
names(deserts)
```

```
##  [1] "record_id"       "month"           "day"             "year"           
##  [5] "plot_id"         "species_id"      "sex"             "hindfoot_length"
##  [9] "weight"          "genus"           "species"         "taxa"           
## [13] "plot_type"
```

```r
deserts %>% 
  filter(weight!="NA") %>% 
  ggplot(aes(x=species_id,y=weight))+
  geom_boxplot()+
  geom_point(alpha=0.1,color="tomato", position="jitter")+
  labs(title= "Weight Distribution of Species",
       x="Species ID",
       y="Weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?

```r
deserts %>% 
  filter(species=="merriami") %>% 
  group_by(year) %>% 
  count()
```

```
## # A tibble: 26 × 2
## # Groups:   year [26]
##     year     n
##    <dbl> <int>
##  1  1977   264
##  2  1978   389
##  3  1979   209
##  4  1980   493
##  5  1981   559
##  6  1982   609
##  7  1983   528
##  8  1984   396
##  9  1985   667
## 10  1986   406
## # … with 16 more rows
```

8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.

```r
deserts %>% 
  ggplot(aes(x=weight,y=hindfoot_length))+
  geom_point(size=0.3)
```

```
## Warning: Removed 4048 rows containing missing values (`geom_point()`).
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.


albigula and spectabillis have the highest weight on average.

```r
deserts %>% 
  group_by(species) %>% 
  summarize("average_weight"=mean(weight,na.rm=T)) %>% 
  arrange(-average_weight) %>% 
  head(n=2)
```

```
## # A tibble: 2 × 2
##   species     average_weight
##   <chr>                <dbl>
## 1 albigula              159.
## 2 spectabilis           120.
```




```r
deserts %>% 
  filter(species=="albigula"|species=="spectabilis")%>% 
  group_by(species) %>% 
  summarize("weight_hindfoot_ratio"=weight/hindfoot_length,
            "sex"=sex) %>% 
  ggplot(aes(x=species,fill=sex ,y=weight_hindfoot_ratio))+
  geom_boxplot()+
  coord_flip()
```

```
## `summarise()` has grouped output by 'species'. You can override using the
## `.groups` argument.
```

```
## Warning: Removed 684 rows containing non-finite values (`stat_boxplot()`).
```

![](lab10_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.



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



How does hindfoot length compare between sexes in rodents?



```r
deserts %>% 
  filter(sex!="NA"&taxa=="Rodent") %>% 
  ggplot(aes(x=sex,fill=sex,y=hindfoot_length))+
  geom_boxplot()+
  labs(title="Hindfoot Length Comparison between Gender in Rodents",
       x="Sex",
       y="Hindfoot Length")
```

```
## Warning: Removed 1668 rows containing non-finite values (`stat_boxplot()`).
```

![](lab10_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
