---
title: "Midterm 1 Cheat Sheet"
author: "Tiffany Chan"
date: "2023-01-30"
output: 
  html_document: 
    keep_md: yes
---



How to search for a command '?'

```r
#?help or #?mean or #?arrange
```
Arithmetic Functions
mean(x, na.rm=T)
median(x)

'?' can also find built-in data
For Example,

```r
?msleep
```

```
## No documentation for 'msleep' in specified packages and libraries:
## you could try '??msleep'
```

###How to make a Vector
(Random Name)<-c(string of numbers)

##Example of making a Matrix
#1. Assign vectors

spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)

#2. Place all data into a single verctor
springs <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
springs #this is so that you can view the data in the vector you just created

#3. Make the matrix 
springs_matrix <- matrix(springs, nrow=8, byrow = T)
springs_matrix

#4. Name the Matrixes rows and columns
scientists <- c("Jill", "Steve", "Susan")

springs <- c("Bluebell Spring", "Opal Spring", "Riverside Spring", "Too Hot Spring", "Mystery Spring", "Emerald Spring", "Black Spring", "Pearl Spring")

colnames(springs_matrix) <- scientists
rownames(springs_matrix) <- springs
springs_matrix

#5. Calculating the mean of each row(each spring)
mean_vector <- rowMeans(springs_matrix)
mean_vector

#6. Adding column to matrix
springs_matrix2 <- cbind(springs_matrix, mean_vector)
springs_matrix2

#When selecting only n column 
[,n]

#When selecting only n row
[n,]

General format
[rows,columns]

###First Things to do when opening a new RMarkdown File

#Load tidyverse, janitor,  

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.1 
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

```r
library()
```

#Dimensions of data
glimpse()
dim()
str()
summary()

#Checking of NA in data
anyNA()
Results in TRUE or FALSE

#See Column Names
names()

names$vore
Looks into the data names and only looks at column vore.

#See the counts of each observation
Table()

filter() - sort out by row
select() - sort our by column
max() 
min() 
head() - first 5 rows
tail() - last 5 rows
levels() - displays there levels
###Assigning class to a variable/column
data$column <- as.factor(data$column)
class(data$column)

###Loading data from outside file
homerange <- readr::read_csv("~/Desktop/BIS15LW2021_jledford/lab4/data/Tamburelloetal_HomeRangeDatabase.csv")


##Arrange Data in descending order
arrange(desc())
big to small

arrange()
small to big

Ex. 
arrange(snakes, mean.hra.m2)
arrange(data, specific column)

###dplyr Superhero
#Changing certain data into NAs in data before it is imported.
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))

##Cleaning names with janitor
superhero_powers <- janitor::clean_names(superhero_powers)

tably vs table
instead of just counts they give percentages
tabyl(superhero_info,alignment)
