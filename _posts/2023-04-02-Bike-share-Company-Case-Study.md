---
layout: post
title: Bike-share Company Case Study Using R and Tableau
image: "/posts/photo-bikes.jpg"
tags: [R, RStudio,Tableau]
---

This is a case study I have done in order to obtain my Google Data Analyst Professional Certificate. Scenario: It's about Cyclistic, a bike-share company in Chicago. There are three plans available: Single Ride - $3.30/trip (one trip up to 30 minutes), Day Pass - $15/day (unlimited 3-hour rides in 24-hour period), and Annual Membership - $9/month ($108 billed upfront annually for unlimited 45-min rides). There are three types of bikes: classical, electric, and docked. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. My task was to answer the question "How do annual members and casual riders use Cyclistic bikes differently?" and come up with three recommendations based on my analysis. From these insights, the team will design a new marketing strategy to convert casual riders into annual members. Cyclistic is a fictional company. The data has been made available by Motivate International Inc. I have chosen data from January 2022 to December 2022. I used RStudio for data manupulation and Tableau to conduct the analysis and create visualisations and a dashboard.

---

##### Step 1: Importing required packages in RStudio

###### Load the packages
``` r
library(tidyverse)
library(ggplot2)
```

##### Step 2: Importing Data
Data was downloaded from the following 
[link](https://divvy-tripdata.s3.amazonaws.com/index.html)

###### Load 12 datasets correspoding to 12 months
``` r
data1=read.csv("trips_jan_22.csv")
data2=read.csv("trips_feb_22.csv")
data3=read.csv("trips_mar_22.csv")
data4=read.csv("trips_apr_22.csv")
data5=read.csv("trips_may_22.csv")
data6=read.csv("trips_jun_22.csv")
data7=read.csv("trips_jul_22.csv")
data8=read.csv("trips_aug_22.csv")
data9=read.csv("trips_sep_22.csv")
data10=read.csv("trips_oct_22.csv")
data11=read.csv("trips_nov_22.csv")
data12=read.csv("trips_dec_22.csv")
```
##### Step 3: Data preparation
###### Load packages for data manipulation and handling dates and times
``` r
library("dplyr")
library(lubridate)
```

###### Union them in one dataset
``` r
data=rbind(data1,data2,data3,data4,data5,data6,data7,data8,data9,data10,data11,data12)
```
###### View the new data
``` r
head(data)
colnames(data)
View(data)
str(data)
class(data$started_at)
class(data$ended_at)
```
###### Changind variables started_at_as_date and ended_at_as_date to date type
``` r
data$started_at_as_date=dmy_hm(data$started_at)
data$ended_at_as_date=dmy_hm(data$ended_at)

class(data$started_at_as_date)
class(data$ended_at_as_date)
```
###### Create a new variable ride_length_in_min for the ride duration in minutes
``` r
data$ride_length_in_min=as.numeric(data$ended_at_as_date-data$started_at_as_date,
                                   units="mins")
```
###### Create a new variable week_day_as_date to see the week day of the start of the trips
``` r
data$week_day_as_date=wday(data$started_at_as_date,label=TRUE, abbr=TRUE)
```
###### Change categories of the rideable_type variable to classic, electric, and docked
``` r
data=data %>% 
  mutate(rideable_type=replace(rideable_type, rideable_type=="classic_bike", "classic"))
data=data %>% 
  mutate(rideable_type=replace(rideable_type, rideable_type=="electric_bike", "electric"))
data=data %>% 
  mutate(rideable_type=replace(rideable_type, rideable_type=="docked_bike", "docked"))
```
###### View new data
``` r
View(data)
```
###### Load packages for summary statistics, cleaning, and preprocessing data
``` r
library(skimr)
library(janitor)
```
###### Generating some summary statistics
``` r
skim_without_charts(data)
data %>% 
  group_by(member_casual) %>% 
  summarise(mean(ride_length_in_min), min(ride_length_in_min), max(ride_length_in_min))
```

###### Filter data, keep only ride_length_in_min >= 3 minutes
``` r
data=filter(data, ride_length_in_min>=3)
data=filter(data, ride_length_in_min<=1440)
```
##### Step 3: Some descriptive statistics

###### Some stats
``` r
data %>% 
  group_by(member_casual) %>% 
  summarise(mean(ride_length_in_min), min(ride_length_in_min), max(ride_length_in_min))
```
##### Step 4: Creating some data visualisations

###### Create visualizations
``` r
ggplot(data=data)+geom_bar(mapping=aes(x=week_day_as_date, fill=rideable_type))+
  facet_grid(~member_casual)
```

###### Plot bar charts for different riders (casual and member) and different rideable_type (classic, docked, electric)
``` r
ggplot(data=data)+geom_bar(mapping=aes(x=rideable_type))+
  facet_grid(~member_casual)
```
##### Step 5: Saving and importing data to Tableau for furthure analysis and creating visualisations and a dashboard

```r
write.csv(data, "C:\\Users\\Owner\\My_bikes_2022.csv", row.names=FALSE)
```


##### Results
Casual riders took 1M less trips than annual members in 2022, but spent 23% more time traveling with higher median and average trip duration. Annual members travel more often during weekdays (to work), while casual riders travel more often during the weekends (leisure). During weekdays, more annual members than casual riders start their trips at 8am, 12pm, and 5pm. Casual riders choose electric type more often, but ride classical type for longer, average trip time is bigger for docked type. My three recommendations are to create a weekend membership plan and market it to casual riders. Once a casual member subscribes and benefits from the weekend membership, market them an annual membership by showing them that it's actually beneficial for them. This could be done by sending them emails with their stats together with the offer and placing ads at dock stations and electric bikes because casual riders use them more


##### My reccomendations
1) Recognizing the business potential in the trend of casual riders taking more and longer trips during weekends, it is recommended to create a dedicated weekend membership plan tailored to their preferences. This strategic initiative has the potential to not only cater to their needs but also contribute to increased revenue and customer loyalty <br> 
2) Capitalizing on Long-term Engagement: Subsequently, there is an opportunity to leverage the engagement of weekend plan members by introducing an annual membership option to them. This marketing strategy can be reinforced by elucidating the wide-ranging benefits of the annual membership, thereby fostering sustained customer commitment and driving revenue growth <br> 
3) Targeted Marketing Strategy: Utilize the preference for electric and docked bikes among casual riders to place compelling ads promoting the benefits of an annual membership. This focused approach can attract more members, driving increased revenue

##### Link to the Tableau dashboard

[Tableau dashboard is available there](https://public.tableau.com/app/profile/khatunakurdovanidze/viz/Bike-ShareCompanyCaseStudy/Dashboard1)

---

Photo source: Ankush Minda/Unsplash










