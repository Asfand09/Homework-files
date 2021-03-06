# Homework-files
---
title: "HomeWork.Asfand.Yar"
author: "Asfand Yar"
date: "9/22/2021"
output: pdf_document
---
The aim of this homework was to arrange the world country to height data obtained from (https://www.disabled-world.com/calculators-charts/height-chart.php). First bar plot involves bar plotting of countries vs males height (arranged with respect to height in cm). Similarly, the second plot is based on the females height from each country. Finally, the last plot consists of Average height (both males and females) vs countries.  


```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
```{r}
library(tidyverse)
library(ggplot2)
library(here)
library(lubridate)
```

## Importing the dataset.

```{r}
library(readr)
Height_Data_Edited <- read_csv("Height_Data_Edited.csv")
View(Height_Data_Edited)
```


## Bar plot for males height with respect to countries.

1- First, the dataset was filtered for Countries with 'NA' listing for male height.
```{r}
new_data <- rename(Height_Data_Edited, Countries = Country_Region, Height = Average_male_height_cm) 

new_data <- filter(new_data, !(is.na(Height)))
new_data
```

2- Next, bar plotting was done for male height and countries with necessary aesthetics and requirments. 
```{r}
ggplot(data = new_data, aes(y = Countries, x = Height, fill = Height)) + geom_bar(stat = 'identity') + labs(x = 'Males height(cm)', y = 'Countries') + theme(axis.text.y = element_text(size = 4.0))
```

3- Finally, countries were reordered with respect to decreasing height for better visualization.
```{r}
ggplot(data = new_data, aes(y = reorder(Countries, Height), x = Height, fill = Height)) + geom_bar(stat = 'identity') + labs(x = 'Males height(cm)', y = 'Countries') + theme(axis.text.y = element_text(size = 4.0))
```


## Bar plot for females height with respect to countries.

1- As done previously, the dataset was filtered for Countries with 'NA' listings for female height. Unnecessary columns were also removed.
```{r}
new_data <- rename(new_data, Height2 = 'Average_female_height_(cm)')

new_data <- filter(new_data, !(is.na(Height2)))

vars_out <- c("...7", "...8", "...9")
new_data <- select(new_data, -vars_out)
new_data
```

2- Bar plotting was done for female height and countries with necessary aesthetics and requirments. 
```{r}
ggplot(data = new_data, aes(y = Countries, x = Height2, fill = Height2)) + geom_bar(stat = 'identity') + labs(x = 'Females height(cm)', y = 'Countries') + theme(axis.text.y = element_text(size = 4.0))
```

3- Side bar chat was plotted and countries were reordered with respect to decreasing height for better visualization.
```{r}
ggplot(data = new_data, aes(y = reorder(Countries, Height2), x = Height2, fill = Height2)) + geom_bar(stat = 'identity') + labs(x = 'Females height(cm)', y = 'Countries') + theme(axis.text.y = element_text(size = 4.0))
```


## Bar plot for average height from each country (males and females in cm).
For this purpose, the males and females height columns were first renamed followed by average of each country's height data. 

```{r}
new_data <- mutate(new_data, Avg_Height = (Height + Height2)/2)
new_data
```


```{r}
ggplot(new_data, aes(y = reorder(Countries, Avg_Height), x = Avg_Height)) +
  geom_bar(stat = "identity", fill = "Magenta") +
  labs(title="Height(cm) Bar Chart",
       subtitle = "Average Height Vs Countries", 
       x = 'Average Height(cm)', 
       y = 'Countries') + 
  theme(axis.text.y = element_text(size = 5.0)) 
  
```
