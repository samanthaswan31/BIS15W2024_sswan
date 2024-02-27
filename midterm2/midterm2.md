---
title: "Midterm 2 W24"
author: "Samantha Swan"
date: "2024-02-27"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code must be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. 

Your code must knit in order to be considered. If you are stuck and cannot answer a question, then comment out your code and knit the document. You may use your notes, labs, and homework to help you complete this exam. Do not use any other resources- including AI assistance.  

Don't forget to answer any questions that are asked in the prompt. Some questions will require a plot, but others do not- make sure to read each question carefully.  

For the questions that require a plot, make sure to have clearly labeled axes and a title. Keep your plots clean and professional-looking, but you are free to add color and other aesthetics.  

Be sure to follow the directions and upload your exam on Gradescope.    

## Background
In the `data` folder, you will find data about shark incidents in California between 1950-2022. The [data](https://catalog.data.gov/dataset/shark-incident-database-california-56167) are from: State of California- Shark Incident Database.   

## Load the libraries

```r
library("tidyverse")
library("janitor")
library("naniar")
```

## Load the data
Run the following code chunk to import the data.

```r
sharks <- read_csv("data/SharkIncidents_1950_2022_220302.csv") %>% clean_names()
```


```r
options(scipen=999)
```

## Questions
1. (1 point) Start by doing some data exploration using your preferred function(s). What is the structure of the data? Where are the missing values and how are they represented?  

```r
glimpse(sharks)
```

```
## Rows: 211
## Columns: 16
## $ incident_num     <chr> "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "1…
## $ month            <dbl> 10, 5, 12, 2, 8, 4, 10, 5, 6, 7, 10, 11, 4, 5, 5, 8, …
## $ day              <dbl> 8, 27, 7, 6, 14, 28, 12, 7, 14, 28, 4, 10, 24, 19, 21…
## $ year             <dbl> 1950, 1952, 1952, 1955, 1956, 1957, 1958, 1959, 1959,…
## $ time             <chr> "12:00", "14:00", "14:00", "12:00", "16:30", "13:30",…
## $ county           <chr> "San Diego", "San Diego", "Monterey", "Monterey", "Sa…
## $ location         <chr> "Imperial Beach", "Imperial Beach", "Lovers Point", "…
## $ mode             <chr> "Swimming", "Swimming", "Swimming", "Freediving", "Sw…
## $ injury           <chr> "major", "minor", "fatal", "minor", "major", "fatal",…
## $ depth            <chr> "surface", "surface", "surface", "surface", "surface"…
## $ species          <chr> "White", "White", "White", "White", "White", "White",…
## $ comment          <chr> "Body Surfing, bit multiple times on leg, thigh and b…
## $ longitude        <chr> "-117.1466667", "-117.2466667", "-122.05", "-122.15",…
## $ latitude         <dbl> 32.58833, 32.58833, 36.62667, 36.62667, 35.13833, 35.…
## $ confirmed_source <chr> "Miller/Collier, Coronado Paper, Oceanside Paper", "G…
## $ wfl_case_number  <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
```


```r
summary(sharks)
```

```
##  incident_num           month             day             year     
##  Length:211         Min.   : 1.000   Min.   : 1.00   Min.   :1950  
##  Class :character   1st Qu.: 6.000   1st Qu.: 7.50   1st Qu.:1985  
##  Mode  :character   Median : 8.000   Median :18.00   Median :2004  
##                     Mean   : 7.858   Mean   :16.54   Mean   :1998  
##                     3rd Qu.:10.000   3rd Qu.:25.00   3rd Qu.:2014  
##                     Max.   :12.000   Max.   :31.00   Max.   :2022  
##                                                                    
##      time              county            location             mode          
##  Length:211         Length:211         Length:211         Length:211        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     injury             depth             species            comment         
##  Length:211         Length:211         Length:211         Length:211        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##   longitude            latitude     confirmed_source   wfl_case_number   
##  Length:211         Min.   :32.59   Length:211         Length:211        
##  Class :character   1st Qu.:34.04   Class :character   Class :character  
##  Mode  :character   Median :36.70   Mode  :character   Mode  :character  
##                     Mean   :36.36                                        
##                     3rd Qu.:38.18                                        
##                     Max.   :41.56                                        
##                     NA's   :6
```


```r
miss_var_summary(sharks)
```

```
## # A tibble: 16 × 3
##    variable         n_miss pct_miss
##    <chr>             <int>    <dbl>
##  1 wfl_case_number     202   95.7  
##  2 time                  7    3.32 
##  3 latitude              6    2.84 
##  4 longitude             5    2.37 
##  5 confirmed_source      1    0.474
##  6 incident_num          0    0    
##  7 month                 0    0    
##  8 day                   0    0    
##  9 year                  0    0    
## 10 county                0    0    
## 11 location              0    0    
## 12 mode                  0    0    
## 13 injury                0    0    
## 14 depth                 0    0    
## 15 species               0    0    
## 16 comment               0    0
```

2. (1 point) Notice that there are some incidents identified as "NOT COUNTED". These should be removed from the data because they were either not sharks, unverified, or were provoked. It's OK to replace the `sharks` object.

```r
sharks <- filter(sharks, incident_num != "NOT COUNTED")
```

3. (3 points) Are there any "hotspots" for shark incidents in California? Make a plot that shows the total number of incidents per county. Which county has the highest number of incidents?

Highest is number of incidents is in San Diego.

```r
sharks %>%
  count(county) %>%
  ggplot(aes(x = county, y = n))+
  geom_col(fill = "turquoise4")+
  coord_flip()+
  labs(title = "Number of Shark Incidents per County",
       x = "Number of Incidents",
       y = "County")
```

![](midterm2_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

4. (3 points) Are there months of the year when incidents are more likely to occur? Make a plot that shows the total number of incidents by month. Which month has the highest number of incidents?  

October has the highest number of incidents

```r
sharks %>%
  count(month) %>%
  group_by(month) %>%
  mutate(month=as.factor(month)) %>%
  ggplot(aes(x = month, y = n))+
  geom_col(fill = "red")+
  labs(title = "Number of Shark Incidents by Month",
       x = "Month",
       y = "Number of Shark Incidents")
```

![](midterm2_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

5. (3 points) How do the number and types of injuries compare by county? Make a table (not a plot) that shows the number of injury types by county. Which county has the highest number of fatalities?  

Highest number of fatalities = San Luise Obispo

```r
sharks %>%
  group_by(county) %>%
  select(county, injury) %>%
  count(injury) %>%
  pivot_wider(names_from = "county",
              values_from = "n")
```

```
## # A tibble: 4 × 22
##   injury `Del Norte` Humboldt `Island - Catalina` `Island - Farallones`
##   <chr>        <int>    <int>               <int>                 <int>
## 1 minor            2        2                   1                    NA
## 2 none             1        9                   3                    NA
## 3 major           NA        7                  NA                     7
## 4 fatal           NA       NA                  NA                    NA
## # ℹ 17 more variables: `Island - San Miguel` <int>,
## #   `Island - San Nicolas` <int>, `Island - Santa Cruz` <int>,
## #   `Island - Santa Rosa` <int>, `Los Angeles` <int>, Marin <int>,
## #   Mendocino <int>, Monterey <int>, Orange <int>, `San Diego` <int>,
## #   `San Francisco` <int>, `San Luis Obispo` <int>, `San Mateo` <int>,
## #   `Santa Barbara` <int>, `Santa Cruz` <int>, Sonoma <int>, Ventura <int>
```

6. (2 points) In the data, `mode` refers to a type of activity. Which activity is associated with the highest number of incidents?

Surfing/boarding is associated with the highest number of incidents.

```r
sharks %>%
  group_by(mode) %>%
  count(mode) %>%
  arrange(desc(n))
```

```
## # A tibble: 7 × 2
## # Groups:   mode [7]
##   mode                    n
##   <chr>               <int>
## 1 Surfing / Boarding     80
## 2 Freediving             35
## 3 Kayaking / Canoeing    29
## 4 Swimming               22
## 5 Scuba Diving           19
## 6 Hookah Diving          10
## 7 Paddleboarding          7
```

7. (4 points) Use faceting to make a plot that compares the number and types of injuries by activity. (hint: the x axes should be the type of injury) 


```r
sharks %>%
  group_by(mode, injury) %>%
  count(mode) %>%
  mutate(injury = as.factor(injury)) %>%
  ggplot(aes(x = injury, y = n, fill = injury))+
  geom_col()+
  facet_wrap(~mode)+
  labs(title = "Types of Injury by Activity",
       y = "Number of Injuries",
       x = "Type of Injury")
```

![](midterm2_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

8. (1 point) Which shark species is involved in the highest number of incidents?  

Great White!

```r
sharks %>%
  group_by(species) %>%
  count(species) %>%
  arrange(desc(n))
```

```
## # A tibble: 8 × 2
## # Groups:   species [8]
##   species        n
##   <chr>      <int>
## 1 White        179
## 2 Unknown       13
## 3 Hammerhead     3
## 4 Blue           2
## 5 Leopard        2
## 6 Salmon         1
## 7 Sevengill      1
## 8 Thresher       1
```


9. (3 points) Are all incidents involving Great White's fatal? Make a plot that shows the number and types of injuries for Great White's only.  

```r
sharks %>%
  filter(species == "White") %>%
  count(injury)
```

```
## # A tibble: 4 × 2
##   injury     n
##   <chr>  <int>
## 1 fatal     15
## 2 major     59
## 3 minor     31
## 4 none      74
```


## Background
Let's learn a little bit more about Great White sharks by looking at a small dataset that tracked 20 Great White's in the Fallaron Islands. The [data](https://link.springer.com/article/10.1007/s00227-007-0739-4) are from: Weng et al. (2007) Migration and habitat of white sharks (_Carcharodon carcharias_) in the eastern Pacific Ocean.

## Load the data

```r
white_sharks <- read_csv("data/White sharks tracked from Southeast Farallon Island, CA, USA, 1999 2004.csv", na = c("?", "n/a")) %>% clean_names()
```

10. (1 point) Start by doing some data exploration using your preferred function(s). What is the structure of the data? Where are the missing values and how are they represented?

```r
glimpse(white_sharks)
```

```
## Rows: 20
## Columns: 10
## $ shark           <chr> "1-M", "2-M", "3-M", "4-M", "5-F", "6-M", "7-F", "8-M"…
## $ tagging_date    <chr> "19-Oct-99", "30-Oct-99", "16-Oct-00", "5-Nov-01", "5-…
## $ total_length_cm <dbl> 402, 366, 457, 457, 488, 427, 442, 380, 450, 530, 427,…
## $ sex             <chr> "M", "M", "M", "M", "F", "M", "F", "M", "M", "F", NA, …
## $ maturity        <chr> "Mature", "Adolescent", "Mature", "Mature", "Mature", …
## $ pop_up_date     <chr> "2-Nov-99", "25-Nov-99", "16-Apr-01", "6-May-02", "19-…
## $ track_days      <dbl> 14, 26, 182, 182, 256, 275, 35, 60, 209, 91, 182, 240,…
## $ longitude       <dbl> -124.49, -125.97, -156.80, -141.47, -133.25, -138.83, …
## $ latitude        <dbl> 38.95, 38.69, 20.67, 26.39, 21.13, 26.50, 37.07, 34.93…
## $ comment         <chr> "Nearshore", "Nearshore", "To Hawaii", "To Hawaii", "O…
```


```r
miss_var_summary(white_sharks)
```

```
## # A tibble: 10 × 3
##    variable        n_miss pct_miss
##    <chr>            <int>    <dbl>
##  1 sex                  3       15
##  2 maturity             1        5
##  3 longitude            1        5
##  4 latitude             1        5
##  5 shark                0        0
##  6 tagging_date         0        0
##  7 total_length_cm      0        0
##  8 pop_up_date          0        0
##  9 track_days           0        0
## 10 comment              0        0
```


```r
anyNA(white_sharks)
```

```
## [1] TRUE
```

11. (3 points) How do male and female sharks compare in terms of total length? Are males or females larger on average? Do a quick search online to verify your findings. (hint: this is a table, not a plot).  

```r
white_sharks %>%
  group_by(sex) %>%
  summarize(mean_total_length = mean(total_length_cm))
```

```
## # A tibble: 3 × 2
##   sex   mean_total_length
##   <chr>             <dbl>
## 1 F                  462 
## 2 M                  425.
## 3 <NA>               425
```

12. (3 points) Make a plot that compares the range of total length by sex.

```r
white_sharks %>%
  group_by(sex, total_length_cm) %>%
  ggplot(aes(x = sex, y = total_length_cm, fill = sex))+
  geom_boxplot()+
  labs(title = "Total Length of White Sharks by Sex",
       x = "Sex",
       y = "Total Length")
```

![](midterm2_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

13. (2 points) Using the `sharks` or the `white_sharks` data, what is one question that you are interested in exploring? Write the question and answer it using a plot or table. 

Have shark incidences increased or decreased over time?

```r
sharks %>%
  group_by(year) %>%
  count(year) %>%
  ggplot(aes(x = year, y = n))+
  geom_col(fill = "blue")+
  labs(title = "Shark Incidents Throughout Time",
       x = "Year",
       y = "Number of Incidents")
```

![](midterm2_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

