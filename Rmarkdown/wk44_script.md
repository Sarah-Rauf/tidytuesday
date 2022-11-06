---
title: "wk44_tidytuesday"
author: "Sarah Rauf"
date: '2022-11-06'
output: 
  html_document:
    keep_md: true
---



LOading Packages

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.2 --
## v ggplot2 3.3.5      v purrr   0.3.4 
## v tibble  3.1.6      v dplyr   1.0.10
## v tidyr   1.1.4      v stringr 1.4.0 
## v readr   2.1.1      v forcats 0.5.1 
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(here)
```

```
## here() starts at C:/Users/sarah/OneDrive/Documents/TidyTuesday/tidytuesday_wk44
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
library(ggrepel)

# importing fonts
library(showtext)
```

```
## Loading required package: sysfonts
## Loading required package: showtextdb
```

```r
font_add_google("Griffy")
showtext_auto()
windows()
```

Loading the theme()

```r
source(here("functions", "sarah_theme.R"))
theme_set(sarah_theme())
```

Loading the Data

```r
data<-read_csv(here("data", "2022", "2022-11-01", "horror_movies.csv"))
```

```
## Rows: 32540 Columns: 20
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (10): original_title, title, original_language, overview, tagline, post...
## dbl   (8): id, popularity, vote_count, vote_average, budget, revenue, runtim...
## lgl   (1): adult
## date  (1): release_date
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Reorganizing the Data & Plotting the Graphs

```r
table<-data %>% 
  dplyr::mutate(year = lubridate::year(release_date),
                month = lubridate::month(release_date), 
                day = lubridate::day(release_date)) %>% 
  group_by(month) %>%
  summarise(revenue_mean=mean(revenue, na.rm = TRUE)) 

month_written = c("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December")
table$month_written<-month_written
table$month_written<-factor(table$month_written, levels=c("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"))
table
```

```
## # A tibble: 12 x 3
##    month revenue_mean month_written
##    <dbl>        <dbl> <fct>        
##  1     1      631209. January      
##  2     2     1582155. February     
##  3     3     1134126. March        
##  4     4     1090063. April        
##  5     5     1364678. May          
##  6     6     2083872. June         
##  7     7     1875757. July         
##  8     8     1973259. August       
##  9     9     1754071. September    
## 10    10     1407257. October      
## 11    11     1179617. November     
## 12    12     1052624. December
```

```r
graph <-
  table %>% 
  ggplot() + 
  geom_bar(aes(x=month_written, y=revenue_mean), fill="#FF2C2C", stat='identity') + 
  geom_text(aes(x=month_written, y=revenue_mean, label=round(revenue_mean)), vjust=-0.3, color="white", size=3) + 
  ggtitle("Average Revenue of Horror Movies By Month") +
  theme(plot.title=element_text(size=20, family="Griffy")) + 
  theme(text = element_text(size = 12, family = "Griffy", 
                        face = "plain", 
                        colour = "#FF2C2C", 
                        lineheight = 0.9, 
                        hjust = 0.5, 
                        vjust = 0.5, 
                        angle = 0, 
                        margin = margin(), 
                        debug = FALSE)) +
  theme(panel.background = element_rect(fill = "#000000")) +
  theme(plot.background = element_rect(fill = "#000000")) +
  xlab("Month") + 
  ylab("Average Revenue")
```

Saving the Plot

```r
ggsave(here("figures", "graph.pdf"), graph,
       width = 190, height = 120, units = "mm")
```

