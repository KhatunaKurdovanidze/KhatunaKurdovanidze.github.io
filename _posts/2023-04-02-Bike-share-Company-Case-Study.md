---
layout: post
title: Bike-share Company Case Study
image: "/posts/photo-bikes.jpg"
tags: [Python, Random Forests, Classification Model]
---

This is a case study I have done for my Google Data Analyst Professional Certificate. Scenario: It's about Cyclistic, a bike-share company in Chicago. There are three plans available: Single Ride - $3.30/trip (one trip up to 30 minutes), Day Pass - $15/day (unlimited 3-hour rides in 24-hour period), and Annual Membership - $9/month ($108 billed upfront annually for unlimited 45-min rides). There are three types of bikes: classical, electric, and docked. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. My task is to answer the question "How do annual members and casual riders use Cyclistic bikes differently?" and come up with three recommendations based on my analysis. From these insights, the team will design a new marketing strategy to convert casual riders into annual members. Data: Cyclistic is a fictional company. The data has been made available by Motivate International Inc. I have chosen data from January 2022 to December 2022. 
---

##### Step 1: Import required packages in RStudio
``` r
# Load the packages
library(tidyverse)
library(ggplot2)
```

##### Step 2: Import Data
Data was downloaded from the following link <iframe title="Data Source" width="1140" height="541.25" src="[https://app.powerbi.com/reportEmbed?reportId=67ccd2f0-b1b4-4995-afdb-9f1a65317abd&autoAuth=true&ctid=15830474-cef0-4326-88db-96e5ab019d8a](https://divvy-tripdata.s3.amazonaws.com/index.html)" frameborder="0" allowFullScreen="true"></iframe>

```r
# Load 12 datasets for 12 months
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

```r
# Union them in one dataset
data=rbind(data1,data2,data3,data4,data5,data6,data7,data8,data9,
           data10,data11,data12)

# View the new data
head(data)
colnames(data)
View(data)
str(data)
class(data$started_at)
class(data$ended_at)

# Load packages for data transformation
library("dplyr")
library(lubridate)

# Change date variable to date type
data$started_at_as_date=dmy_hm(data$started_at)
data$ended_at_as_date=dmy_hm(data$ended_at)

class(data$started_at_as_date)
class(data$ended_at_as_date)

# Create a new variable in minutes
data$ride_length_in_min=as.numeric(data$ended_at_as_date-data$started_at_as_date,
                                   units="mins")
# Create a new variable for week days
data$week_day_as_date=wday(data$started_at_as_date,label=TRUE, abbr=TRUE)

# Change some variable names
data=data %>% 
  mutate(rideable_type=replace(rideable_type, rideable_type=="classic_bike", "classic"))
data=data %>% 
  mutate(rideable_type=replace(rideable_type, rideable_type=="electric_bike", "electric"))
data=data %>% 
  mutate(rideable_type=replace(rideable_type, rideable_type=="docked_bike", "docked"))

# View new data
View(data)
# Load other packages
library(skimr)
library(janitor)
library(dplyr)
skim_without_charts(data)
# Some stats
data %>% 
  group_by(member_casual) %>% 
  summarise(mean(ride_length_in_min), min(ride_length_in_min), max(ride_length_in_min))


# Filter data, keep only ride_length_in_min >= 3 minutes
data=filter(data, ride_length_in_min>=3)
data=filter(data, ride_length_in_min<=1440)
```
##### Step 3: Descriptive statistics

```r
# Some stats
data %>% 
  group_by(member_casual) %>% 
  summarise(mean(ride_length_in_min), min(ride_length_in_min), max(ride_length_in_min))

```
##### Step 4: Creating some data visualisations


```r
# Create visualizations
ggplot(data=data)+geom_bar(mapping=aes(x=week_day_as_date, fill=rideable_type))+
  facet_grid(~member_casual)


# Plot bar charts for different riders (casual and member) and 
#different rideable_type (classic, docked, electric)
ggplot(data=data)+geom_bar(mapping=aes(x=rideable_type))+
  facet_grid(~member_casual)
```
##### Step 5: Saving and importing data to Tableau for furthure analysis and creating visualisations and dashboard

```r
write.csv(data, "C:\\Users\\Owner\\My_bikes_2022.csv", row.names=FALSE)
```


##### Our Results
Casual riders took 1M less trips than annual members in 2022, but spent 23% more time traveling with higher median and average trip duration. Annual members travel more often during weekdays (to work), while casual riders travel more often during the weekends (leisure). During weekdays, more annual members than casual riders start their trips at 8am, 12pm, and 5pm. Casual riders choose electric type more often, but ride classical type for longer, average trip time is bigger for docked type. My three recommendations are to create a weekend membership plan and market it to casual riders. Once a casual member subscribes and benefits from the weekend membership, market them an annual membership by showing them that it's actually beneficial for them. This could be done by sending them emails with their stats together with the offer and placing ads at dock stations and electric bikes because casual riders use them more


##### Reccomendations
1) Recognizing the business potential in the trend of casual riders taking more and longer trips during weekends, it is recommended to create a dedicated weekend membership plan tailored to their preferences. This strategic initiative has the potential to not only cater to their needs but also contribute to increased revenue and customer loyalty <br> 
2) Capitalizing on Long-term Engagement: Subsequently, there is an opportunity to leverage the engagement of weekend plan members by introducing an annual membership option to them. This marketing strategy can be reinforced by elucidating the wide-ranging benefits of the annual membership, thereby fostering sustained customer commitment and driving revenue growth <br> 
3) Targeted Marketing Strategy: Utilize the preference for electric and docked bikes among casual riders to place compelling ads promoting the benefits of an annual membership. This focused approach can attract more members, driving increased revenue

##### Link to the Tableau dashboard
<div class='tableauPlaceholder' id='viz1707758617079' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Bi&#47;Bike-ShareCompanyCaseStudy&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Bike-ShareCompanyCaseStudy&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Bi&#47;Bike-ShareCompanyCaseStudy&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-GB' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1707758617079');                    var vizElement = divElement.getElementsByTagName('object')[0];                    if ( divElement.offsetWidth > 800 ) { vizElement.style.width='1020px';vizElement.style.height='3427px';} else if ( divElement.offsetWidth > 500 ) { vizElement.style.width='1020px';vizElement.style.height='3427px';} else { vizElement.style.width='100%';vizElement.style.height='3677px';}                     var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
---

Photo source: Ankush Minda/Unsplash










