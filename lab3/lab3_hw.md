---
title: "Lab 3 Homework"
author: "Please Add Your Name Here"
date: "2023-01-18"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

##Install tidyverse

```r
#install.packages("tidyverse")
```

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.
The data was taken from V.M. Savage and G.B. West. and the proceedings came from National Academy of Sciences. 

```r
?msleep
```


```r
mammal_sleep_pattern<-msleep
```

2. Store these data into a new data frame `sleep`

```r
sleep<-data.frame(mammal_sleep_pattern)
sleep
```

```
##                              name         genus    vore           order
## 1                         Cheetah      Acinonyx   carni       Carnivora
## 2                      Owl monkey         Aotus    omni        Primates
## 3                 Mountain beaver    Aplodontia   herbi        Rodentia
## 4      Greater short-tailed shrew       Blarina    omni    Soricomorpha
## 5                             Cow           Bos   herbi    Artiodactyla
## 6                Three-toed sloth      Bradypus   herbi          Pilosa
## 7               Northern fur seal   Callorhinus   carni       Carnivora
## 8                    Vesper mouse       Calomys    <NA>        Rodentia
## 9                             Dog         Canis   carni       Carnivora
## 10                       Roe deer     Capreolus   herbi    Artiodactyla
## 11                           Goat         Capri   herbi    Artiodactyla
## 12                     Guinea pig         Cavis   herbi        Rodentia
## 13                         Grivet Cercopithecus    omni        Primates
## 14                     Chinchilla    Chinchilla   herbi        Rodentia
## 15                Star-nosed mole     Condylura    omni    Soricomorpha
## 16      African giant pouched rat    Cricetomys    omni        Rodentia
## 17      Lesser short-tailed shrew     Cryptotis    omni    Soricomorpha
## 18           Long-nosed armadillo       Dasypus   carni       Cingulata
## 19                     Tree hyrax   Dendrohyrax   herbi      Hyracoidea
## 20         North American Opossum     Didelphis    omni Didelphimorphia
## 21                 Asian elephant       Elephas   herbi     Proboscidea
## 22                  Big brown bat     Eptesicus insecti      Chiroptera
## 23                          Horse         Equus   herbi  Perissodactyla
## 24                         Donkey         Equus   herbi  Perissodactyla
## 25              European hedgehog     Erinaceus    omni  Erinaceomorpha
## 26                   Patas monkey  Erythrocebus    omni        Primates
## 27      Western american chipmunk      Eutamias   herbi        Rodentia
## 28                   Domestic cat         Felis   carni       Carnivora
## 29                         Galago        Galago    omni        Primates
## 30                        Giraffe       Giraffa   herbi    Artiodactyla
## 31                    Pilot whale Globicephalus   carni         Cetacea
## 32                      Gray seal  Haliochoerus   carni       Carnivora
## 33                     Gray hyrax   Heterohyrax   herbi      Hyracoidea
## 34                          Human          Homo    omni        Primates
## 35                 Mongoose lemur         Lemur   herbi        Primates
## 36               African elephant     Loxodonta   herbi     Proboscidea
## 37           Thick-tailed opposum    Lutreolina   carni Didelphimorphia
## 38                        Macaque        Macaca    omni        Primates
## 39               Mongolian gerbil      Meriones   herbi        Rodentia
## 40                 Golden hamster  Mesocricetus   herbi        Rodentia
## 41                          Vole       Microtus   herbi        Rodentia
## 42                    House mouse           Mus   herbi        Rodentia
## 43               Little brown bat        Myotis insecti      Chiroptera
## 44           Round-tailed muskrat      Neofiber   herbi        Rodentia
## 45                     Slow loris     Nyctibeus   carni        Primates
## 46                           Degu       Octodon   herbi        Rodentia
## 47     Northern grasshopper mouse     Onychomys   carni        Rodentia
## 48                         Rabbit   Oryctolagus   herbi      Lagomorpha
## 49                          Sheep          Ovis   herbi    Artiodactyla
## 50                     Chimpanzee           Pan    omni        Primates
## 51                          Tiger      Panthera   carni       Carnivora
## 52                         Jaguar      Panthera   carni       Carnivora
## 53                           Lion      Panthera   carni       Carnivora
## 54                         Baboon         Papio    omni        Primates
## 55                Desert hedgehog   Paraechinus    <NA>  Erinaceomorpha
## 56                          Potto  Perodicticus    omni        Primates
## 57                     Deer mouse    Peromyscus    <NA>        Rodentia
## 58                      Phalanger     Phalanger    <NA>   Diprotodontia
## 59                   Caspian seal         Phoca   carni       Carnivora
## 60                Common porpoise      Phocoena   carni         Cetacea
## 61                        Potoroo      Potorous   herbi   Diprotodontia
## 62                Giant armadillo    Priodontes insecti       Cingulata
## 63                     Rock hyrax      Procavia    <NA>      Hyracoidea
## 64                 Laboratory rat        Rattus   herbi        Rodentia
## 65          African striped mouse     Rhabdomys    omni        Rodentia
## 66                Squirrel monkey       Saimiri    omni        Primates
## 67          Eastern american mole      Scalopus insecti    Soricomorpha
## 68                     Cotton rat      Sigmodon   herbi        Rodentia
## 69                       Mole rat        Spalax    <NA>        Rodentia
## 70         Arctic ground squirrel  Spermophilus   herbi        Rodentia
## 71 Thirteen-lined ground squirrel  Spermophilus   herbi        Rodentia
## 72 Golden-mantled ground squirrel  Spermophilus   herbi        Rodentia
## 73                     Musk shrew        Suncus    <NA>    Soricomorpha
## 74                            Pig           Sus    omni    Artiodactyla
## 75            Short-nosed echidna  Tachyglossus insecti     Monotremata
## 76      Eastern american chipmunk        Tamias   herbi        Rodentia
## 77                Brazilian tapir       Tapirus   herbi  Perissodactyla
## 78                         Tenrec        Tenrec    omni    Afrosoricida
## 79                     Tree shrew        Tupaia    omni      Scandentia
## 80           Bottle-nosed dolphin      Tursiops   carni         Cetacea
## 81                          Genet       Genetta   carni       Carnivora
## 82                     Arctic fox        Vulpes   carni       Carnivora
## 83                        Red fox        Vulpes   carni       Carnivora
##    conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
## 1            lc        12.1        NA          NA 11.90      NA   50.000
## 2          <NA>        17.0       1.8          NA  7.00 0.01550    0.480
## 3            nt        14.4       2.4          NA  9.60      NA    1.350
## 4            lc        14.9       2.3   0.1333333  9.10 0.00029    0.019
## 5  domesticated         4.0       0.7   0.6666667 20.00 0.42300  600.000
## 6          <NA>        14.4       2.2   0.7666667  9.60      NA    3.850
## 7            vu         8.7       1.4   0.3833333 15.30      NA   20.490
## 8          <NA>         7.0        NA          NA 17.00      NA    0.045
## 9  domesticated        10.1       2.9   0.3333333 13.90 0.07000   14.000
## 10           lc         3.0        NA          NA 21.00 0.09820   14.800
## 11           lc         5.3       0.6          NA 18.70 0.11500   33.500
## 12 domesticated         9.4       0.8   0.2166667 14.60 0.00550    0.728
## 13           lc        10.0       0.7          NA 14.00      NA    4.750
## 14 domesticated        12.5       1.5   0.1166667 11.50 0.00640    0.420
## 15           lc        10.3       2.2          NA 13.70 0.00100    0.060
## 16         <NA>         8.3       2.0          NA 15.70 0.00660    1.000
## 17           lc         9.1       1.4   0.1500000 14.90 0.00014    0.005
## 18           lc        17.4       3.1   0.3833333  6.60 0.01080    3.500
## 19           lc         5.3       0.5          NA 18.70 0.01230    2.950
## 20           lc        18.0       4.9   0.3333333  6.00 0.00630    1.700
## 21           en         3.9        NA          NA 20.10 4.60300 2547.000
## 22           lc        19.7       3.9   0.1166667  4.30 0.00030    0.023
## 23 domesticated         2.9       0.6   1.0000000 21.10 0.65500  521.000
## 24 domesticated         3.1       0.4          NA 20.90 0.41900  187.000
## 25           lc        10.1       3.5   0.2833333 13.90 0.00350    0.770
## 26           lc        10.9       1.1          NA 13.10 0.11500   10.000
## 27         <NA>        14.9        NA          NA  9.10      NA    0.071
## 28 domesticated        12.5       3.2   0.4166667 11.50 0.02560    3.300
## 29         <NA>         9.8       1.1   0.5500000 14.20 0.00500    0.200
## 30           cd         1.9       0.4          NA 22.10      NA  899.995
## 31           cd         2.7       0.1          NA 21.35      NA  800.000
## 32           lc         6.2       1.5          NA 17.80 0.32500   85.000
## 33           lc         6.3       0.6          NA 17.70 0.01227    2.625
## 34         <NA>         8.0       1.9   1.5000000 16.00 1.32000   62.000
## 35           vu         9.5       0.9          NA 14.50      NA    1.670
## 36           vu         3.3        NA          NA 20.70 5.71200 6654.000
## 37           lc        19.4       6.6          NA  4.60      NA    0.370
## 38         <NA>        10.1       1.2   0.7500000 13.90 0.17900    6.800
## 39           lc        14.2       1.9          NA  9.80      NA    0.053
## 40           en        14.3       3.1   0.2000000  9.70 0.00100    0.120
## 41         <NA>        12.8        NA          NA 11.20      NA    0.035
## 42           nt        12.5       1.4   0.1833333 11.50 0.00040    0.022
## 43         <NA>        19.9       2.0   0.2000000  4.10 0.00025    0.010
## 44           nt        14.6        NA          NA  9.40      NA    0.266
## 45         <NA>        11.0        NA          NA 13.00 0.01250    1.400
## 46           lc         7.7       0.9          NA 16.30      NA    0.210
## 47           lc        14.5        NA          NA  9.50      NA    0.028
## 48 domesticated         8.4       0.9   0.4166667 15.60 0.01210    2.500
## 49 domesticated         3.8       0.6          NA 20.20 0.17500   55.500
## 50         <NA>         9.7       1.4   1.4166667 14.30 0.44000   52.200
## 51           en        15.8        NA          NA  8.20      NA  162.564
## 52           nt        10.4        NA          NA 13.60 0.15700  100.000
## 53           vu        13.5        NA          NA 10.50      NA  161.499
## 54         <NA>         9.4       1.0   0.6666667 14.60 0.18000   25.235
## 55           lc        10.3       2.7          NA 13.70 0.00240    0.550
## 56           lc        11.0        NA          NA 13.00      NA    1.100
## 57         <NA>        11.5        NA          NA 12.50      NA    0.021
## 58         <NA>        13.7       1.8          NA 10.30 0.01140    1.620
## 59           vu         3.5       0.4          NA 20.50      NA   86.000
## 60           vu         5.6        NA          NA 18.45      NA   53.180
## 61         <NA>        11.1       1.5          NA 12.90      NA    1.100
## 62           en        18.1       6.1          NA  5.90 0.08100   60.000
## 63           lc         5.4       0.5          NA 18.60 0.02100    3.600
## 64           lc        13.0       2.4   0.1833333 11.00 0.00190    0.320
## 65         <NA>         8.7        NA          NA 15.30      NA    0.044
## 66         <NA>         9.6       1.4          NA 14.40 0.02000    0.743
## 67           lc         8.4       2.1   0.1666667 15.60 0.00120    0.075
## 68         <NA>        11.3       1.1   0.1500000 12.70 0.00118    0.148
## 69         <NA>        10.6       2.4          NA 13.40 0.00300    0.122
## 70           lc        16.6        NA          NA  7.40 0.00570    0.920
## 71           lc        13.8       3.4   0.2166667 10.20 0.00400    0.101
## 72           lc        15.9       3.0          NA  8.10      NA    0.205
## 73         <NA>        12.8       2.0   0.1833333 11.20 0.00033    0.048
## 74 domesticated         9.1       2.4   0.5000000 14.90 0.18000   86.250
## 75         <NA>         8.6        NA          NA 15.40 0.02500    4.500
## 76         <NA>        15.8        NA          NA  8.20      NA    0.112
## 77           vu         4.4       1.0   0.9000000 19.60 0.16900  207.501
## 78         <NA>        15.6       2.3          NA  8.40 0.00260    0.900
## 79         <NA>         8.9       2.6   0.2333333 15.10 0.00250    0.104
## 80         <NA>         5.2        NA          NA 18.80      NA  173.330
## 81         <NA>         6.3       1.3          NA 17.70 0.01750    2.000
## 82         <NA>        12.5        NA          NA 11.50 0.04450    3.380
## 83         <NA>         9.8       2.4   0.3500000 14.20 0.05040    4.230
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  
83 Observationals and 11 Variables

```r
dim(mammal_sleep_pattern)
```

```
## [1] 83 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.
Yes

```r
anyNA(mammal_sleep_pattern)
```

```
## [1] TRUE
```

5. Show a list of the column names is this data frame.

```r
names(mammal_sleep_pattern)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  
#How to see what number herbivore is, so not sure how many herbivores there are
There are 32 herbivores in the data

```r
table(mammal_sleep_pattern$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```


7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
large_mammals<-filter(msleep,bodywt>=200)
large_mammals
```

```
## # A tibble: 7 × 11
##   name    genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake brainwt bodywt
##   <chr>   <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>   <dbl>  <dbl>
## 1 Cow     Bos   herbi Arti… domest…     4       0.7   0.667  20     0.423   600 
## 2 Asian … Elep… herbi Prob… en          3.9    NA    NA      20.1   4.60   2547 
## 3 Horse   Equus herbi Peri… domest…     2.9     0.6   1      21.1   0.655   521 
## 4 Giraffe Gira… herbi Arti… cd          1.9     0.4  NA      22.1  NA       900.
## 5 Pilot … Glob… carni Ceta… cd          2.7     0.1  NA      21.4  NA       800 
## 6 Africa… Loxo… herbi Prob… vu          3.3    NA    NA      20.7   5.71   6654 
## 7 Brazil… Tapi… herbi Peri… vu          4.4     1     0.9    19.6   0.169   208.
## # … with abbreviated variable names ¹​conservation, ²​sleep_total, ³​sleep_rem,
## #   ⁴​sleep_cycle
```


```r
small_mammals<-filter(msleep,bodywt<=1)
small_mammals
```

```
## # A tibble: 36 × 11
##    name  genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake  brainwt bodywt
##    <chr> <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>    <dbl>  <dbl>
##  1 Owl … Aotus omni  Prim… <NA>       17       1.8  NA       7    0.0155   0.48 
##  2 Grea… Blar… omni  Sori… lc         14.9     2.3   0.133   9.1  0.00029  0.019
##  3 Vesp… Calo… <NA>  Rode… <NA>        7      NA    NA      17   NA        0.045
##  4 Guin… Cavis herbi Rode… domest…     9.4     0.8   0.217  14.6  0.0055   0.728
##  5 Chin… Chin… herbi Rode… domest…    12.5     1.5   0.117  11.5  0.0064   0.42 
##  6 Star… Cond… omni  Sori… lc         10.3     2.2  NA      13.7  0.001    0.06 
##  7 Afri… Cric… omni  Rode… <NA>        8.3     2    NA      15.7  0.0066   1    
##  8 Less… Cryp… omni  Sori… lc          9.1     1.4   0.15   14.9  0.00014  0.005
##  9 Big … Epte… inse… Chir… lc         19.7     3.9   0.117   4.3  0.0003   0.023
## 10 Euro… Erin… omni  Erin… lc         10.1     3.5   0.283  13.9  0.0035   0.77 
## # … with 26 more rows, and abbreviated variable names ¹​conservation,
## #   ²​sleep_total, ³​sleep_rem, ⁴​sleep_cycle
```

8. What is the mean weight for both the small and large mammals?
Weight of Small Animals = 0.2596667
Weight of Large Animals = 1747.071

```r
colMeans(small_mammals[,11])
```

```
##    bodywt 
## 0.2596667
```


```r
colMeans(large_mammals[,11])
```

```
##   bodywt 
## 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  
small mammals sleep longer on average

```r
colMeans(small_mammals[,6])
```

```
## sleep_total 
##    12.65833
```


```r
colMeans(large_mammals[,6])
```

```
## sleep_total 
##         3.3
```

10. Which animal is the sleepiest among the entire dataframe?
The little brown bat is the sleepiest.

```r
max(msleep$sleep_total)
```

```
## [1] 19.9
```


```r
sleepiest<-filter(msleep,sleep_total==19.9)
sleepiest
```

```
## # A tibble: 1 × 11
##   name    genus vore  order conse…¹ sleep…² sleep…³ sleep…⁴ awake brainwt bodywt
##   <chr>   <chr> <chr> <chr> <chr>     <dbl>   <dbl>   <dbl> <dbl>   <dbl>  <dbl>
## 1 Little… Myot… inse… Chir… <NA>       19.9       2     0.2   4.1 0.00025   0.01
## # … with abbreviated variable names ¹​conservation, ²​sleep_total, ³​sleep_rem,
## #   ⁴​sleep_cycle
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
