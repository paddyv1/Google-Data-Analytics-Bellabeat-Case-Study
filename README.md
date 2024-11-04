# Google-Data-Analytics-Bellabeat-Case-Study
Google Data Analytics Certificate Project

#

_The case study follows the six steps of data analysis process:_

###  [Ask](#1-ask)
###  [Prepare](#2-prepare)
###  [Process](#3-process)
###  [Analyze](#4-analyze)
###  [Share](#5-share)
###  [Act](#6-act)

## Scenario
Bellabeat is a high-tech manufacturer of health-focused products for women. Although it is a small company, it is poised to become a significant competitor in the global smart device market. Currently, it has five focus products: the Bellabeat app, Leaf, Time, Spring, and Bellabeat membership. I have been tasked to analyze the available smart device data to understand how consumers use their smart devices. The insights we discover will then help guide the company's marketing strategy.

## 1. Ask
- Business Task: Analyze Fitbit data to gain insight and help guide marketing strategy for Bellabeat to grow as a global player.
- Key Stakeholders:
  - Urška Sršen and Sando Mur key executives
  - Bellabeat Marketing Team

## 2. Prepare
The data set that I will be using is from 30 current users who have agreed to have their data gathered for 30 days from the FitBit tracker: https://www.kaggle.com/arashnic/fitbit

A good data source is ROCCC which stands for Reliable, Original, Comprehensive, Current and Cited:
- Reliable - Only 30 people agreed to have their data recorded.
- Original - Data recorded from 30 FitBit devices whose users consented to have their data recorded.
- Comprehensive - Data minute-level output for physical activity, heart rate, and sleep monitoring. However, the sample size is small and most data is recorded during certain days of the week which could affect the findings from the dataset.
- Current - The dataset is outdated as it is many years old.
- Cited - The data has been collected from a third party therefore is is unknown.

## 3. Process 
 The first step is to remove any duplicates in the data for the three main tables: daily_activity, sleep_day and weight.
 ```
dim(sleep_day)
sum(is.na(sleep_day))
sum(duplicated(sleep_day))
sleep_day <- sleep_day[!duplicated(sleep_day), ]
```
After that the next step is to check for any duplicates in the data, but before doing that I needed to create a merged table which contains data from the three main tables:
```
#Add a new column for the weekdays
daily_activity <- daily_activity %>% mutate( Weekday = weekdays(as.Date(ActivityDate, "%m/%d/%Y")))

merged1 <- merge(daily_activity,sleep_day,by = c("Id"), all=TRUE)
merged_data <- merge(merged1, weight, by = c("Id"), all=TRUE)

#Order from Monday to Sunday for plot later
merged_data$Weekday <- factor(merged_data$Weekday, levels= c("Monday", 
    "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"))

merged_data[order(merged_data$Weekday), ]

#Save CSV for Tableau presentation
write_csv(merged_data, "merged_data.csv")

#Check for NA and duplicates in merged data. 
sum(is.na(merged_data))
sum(duplicated(merged_data))
n_distinct(merged_data$Id)
```
By running this code I found that the dataset had 33 users worth of activity, 24 users worth of sleep and only 8 from weight. Looking further into the dataset it was clear there was a trend of when data was recorded. I noticed users track their data more from Tuesday to Thursday and we have more of those days' data than other days.

![image](https://github.com/user-attachments/assets/fae00a2c-3bee-48ee-bd8a-3470ae7a5509)

## 4. Analyze
I checked the data for mean, median and any outliers in the merged data set, and the data found that the avg weight was around 135 lbs and burned nearly 2050 calories. A Fitbit user on average also performed around 10200 steps with the max step achieved by a user being around 36000 steps.

![image](https://github.com/user-attachments/assets/379b80e9-42c6-446b-ba26-fbfb91ca1e71)

The next part of the data I looked into was the total active minutes which were divided into four different categories, from the data we can that the majority of users spent just above 80% of their daily activity being sedentary and a very small amount of active minutes.

![newplot](https://github.com/user-attachments/assets/54c3729d-d6da-49db-ba37-77f40b9f6764)

It is recommended that at least 150 minutes of moderate or 75 minutes of intense activity be undertaken each week, which corresponds to around 20 minutes of fairly active minutes or 10 minutes of very active minutes.


## 5. Share
## 6. Act
