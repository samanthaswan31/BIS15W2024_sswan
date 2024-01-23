---
title: "Lab 3 Homework"
author: "Samantha Swan"
date: "2024-01-22"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

### Mammals Sleep  
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R. The name of the data is `msleep`.  

The data is built in.

```r
msleep <- msleep
```

2. Store these data into a new data frame `sleep`.  

```r
sleep <- (msleep)
sleep
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # ℹ 73 more rows
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

The dimensions are 83 by 11

```r
dim(sleep)
```

```
## [1] 83 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code. 

Yes there are- anyNA is TRUE.

```r
anyNA(sleep)
```

```
## [1] TRUE
```

5. Show a list of the column names is this data frame.

```r
colnames(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

There are 32 herbivores.

```r
herb_number <- c(sleep$vore)
table(herb_number)
```

```
## herb_number
##   carni   herbi insecti    omni 
##      19      32       5      20
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 19kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small <- filter(sleep, bodywt <= 19)
large <- filter(sleep, bodywt >= 200)
```

8. What is the mean weight for both the small and large mammals?

Small mean = 1.797847 kg, large mean = 1747.071 kg

```r
mean(small$bodywt)
```

```
## [1] 1.797847
```


```r
mean(large$bodywt)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

Mean for small is 3.3, mean for large is 11.78644 so large sleep longer on average

```r
mean(large$sleep_total)
```

```
## [1] 3.3
```

```r
mean(small$sleep_total)
```

```
## [1] 11.78644
```

10. Which animal is the sleepiest among the entire dataframe?

Find the highest sleep total value

```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```
Produces a maximum sleep of 19.9- then filter to get that little brown bat is the "sleepiest."

```r
sleepiest <- filter(sleep, sleep_total == 19.9)
sleepiest
```

```
## # A tibble: 1 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Little… Myot… inse… Chir… <NA>                19.9         2         0.2   4.1
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   