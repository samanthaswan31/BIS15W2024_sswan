---
title: "Lab 2 Homework"
author: "Samantha Swan"
date: "2024-01-17"
output:
  html_document:
    theme: spacelab
    keep_md: yes
    toc: no
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  

A vector is a simple way of formatting data in a list, stored as an object in R.

2. What is a data matrix in R?  

A data matrix is a multi-dimensional format for storing data in R.

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  

First create object with all springs in it

```r
spring_compiled <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
spring_compiled
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

Then create matrix using the object, specifying nrow = #rows, byrow = T/F

```r
spring_matrix <- matrix(spring_compiled, nrow = 8, byrow = T)
spring_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.

Making two new vectors with names of springs/scientists


```r
spring_name <- c("Bluebell_Spring", "Opal_Spring", "Riverside_Spring", "Too_Hot_Spring", "Mystery_Spring", "Emerald_Spring", "Black_Spring", "Pearl_Spring")
scientist_name <- c("Jill", "Steve", "Susan")
spring_name
```

```
## [1] "Bluebell_Spring"  "Opal_Spring"      "Riverside_Spring" "Too_Hot_Spring"  
## [5] "Mystery_Spring"   "Emerald_Spring"   "Black_Spring"     "Pearl_Spring"
```

```r
scientist_name
```

```
## [1] "Jill"  "Steve" "Susan"
```

Name columns/rows

```r
colnames(spring_matrix) <- scientist_name
rownames(spring_matrix) <- spring_name
spring_matrix
```

```
##                   Jill Steve Susan
## Bluebell_Spring  36.25 35.40 35.30
## Opal_Spring      35.15 35.35 33.35
## Riverside_Spring 30.70 29.65 29.20
## Too_Hot_Spring   39.70 40.05 38.65
## Mystery_Spring   31.85 31.40 29.30
## Emerald_Spring   30.20 30.65 29.75
## Black_Spring     32.90 32.50 32.80
## Pearl_Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
mean_temperature <- rowMeans(spring_matrix)
```

7. Add this as a new column in the data matrix.  

```r
final_spring_matrix <- cbind(spring_matrix, mean_temperature)
final_spring_matrix
```

```
##                   Jill Steve Susan mean_temperature
## Bluebell_Spring  36.25 35.40 35.30         35.65000
## Opal_Spring      35.15 35.35 33.35         34.61667
## Riverside_Spring 30.70 29.65 29.20         29.85000
## Too_Hot_Spring   39.70 40.05 38.65         39.46667
## Mystery_Spring   31.85 31.40 29.30         30.85000
## Emerald_Spring   30.20 30.65 29.75         30.20000
## Black_Spring     32.90 32.50 32.80         32.73333
## Pearl_Spring     36.80 36.45 33.15         35.46667
```

8. Show Susan's value for Opal Spring only.

```r
final_spring_matrix[2,3]
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  

```r
Jill_is_mean <- final_spring_matrix[ ,1]
mean(Jill_is_mean)
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.

```r
mean1 <- final_spring_matrix[ ,1]
mean2 <- final_spring_matrix[ ,2]
mean3 <- final_spring_matrix[ ,3]
mean(mean1+mean2+mean3)
```

```
## [1] 100.8125
```

C
## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
