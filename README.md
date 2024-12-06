# Google Data Analytics Capstone: Bellabeat data analysis case study.
## INTRODUCTION
This case study presents a data analysis project conducted as part of the Google Data Analytics Professional Certificate course, Capstone Project, focusing on the usage of Bellabeat Smart Devices. Bellabeat is a high-tech manufacturer of health-focused smart products designed specifically for women.

The company was founded in 2013 by Urška Sršen and Sando Mur and has expanded quickly since, now with the possibility to become a greater player in the global smart device market. Our team have been asked to analyze smart device data to gain insight into how consumers are using their smart devices. The insights we discover will then help guide marketing strategy for the company.

**Currently, company has 6 key product/services offerings:**

1. **Bellabeat app:** Health and wellness app giving health related data and informing lifestyle decisions. Serves as the hub for information collected from the other company/non-company wellness products
2. **Leaf:** Wellness tracker that can be worn as a bracelet, necklace, or clip, tracks activity, sleep, and stress
3. **Time:** Smart watch to track activity, sleep, and stress
4. **Spring:** Smart water bottle to track hydration during the day
5. **Ivy:** Ivy is a smart-wearable product made specifically for women, just like its predecessor. It monitors women’s biometric data: respiratory rate, resting heart rate, heart rate variability (HRV), and cardiac coherence, as well as lifestyle data.
6. **Membership:** A subscription-based program to offer 24/7 personalized guidance on nutrition, health, activity, sleep, and beauty.

## ASK
**BUSINESS TASK:** Identify trends in how consumers use non-Bellabeat smart devices to gain insight and help guide marketing strategy for Bellabeat to grow as a global player.

**Key Stakeholders:**

· **Urška Sršen** — Bellabeat’s cofounder and Chief Creative Officer

· **Sando Mur** — Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team

· **Bellabeat marketing analytics team** — A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.

## PREPARE
Data Source: <a href="https://www.kaggle.com/datasets/arashnic/fitbit">FitBit Fitness Tracker Data</a>

**Data Organisation:** This data set contains personal fitness tracker from thirty fitbit users provided with 18 .CSV files. Data is organized in a long and wide format. The file contains detailed information about daily activity, sleep, weight, calories, and intensities.

The FitBit Fitness Tracker Data was collected in 2016 making the datasets outdated for current trend analysis. Additionally, while the data initially states a time range of 03-12-2016 to 05-12-2016, I am considering only the dataset with most recent available data (4/12/2016 to 5/12/2016).

**Licensing**

**This dataset is under CC0:** Public Domain license meaning the creator has waive his right to the work under the copyright law. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring.

**The dataset has limitations:**

Only 30 user data is available. The central limit theorem general rule of n≥30 applies. Limitations for this data exist due to the sample size and absence of key characteristics of the participants, such as gender, age, location, lifestyle.

**The data follows an ROCCC approch:**

• **Reliability:** The data is from 30 FitBit users who consented to the submission of personal tracker data and generated by from a distributed survey via Amazon Mechanical Turk.

• **Originality:** The data is from 30 FitBit users who agreed to a personal tracker, third party data collected using Amazon Mechanical Turk.

• **Comprehensive:** Data minute-level output for physical activity, heart rate, sleep monitoring, calories used, daily steps taken. The dataset is limited and most data is recorded during certain days of the week.

• **Current:** Information was gathered between March and May of 2016. Since the data is outdated in 2024, consumers’ current FitBit usage may have altered.

• **Cited:** CC0: Public Domain, dataset made available through Mobius.

## Process Phase
I will be choosing R for this project because, while I have some experience with tools like SQL and Excel, R is a new and exciting tool for me. My only exposure to R so far has been during this Certification course. R allows me to perform data cleaning, processing, and visualization efficiently within a single platform, making it an excellent choice for managing and presenting data insights effectively.

**Setting up my working directory**
```R
setwd("/cloud/project/BellaBeat/")
```
**Setting up my environment by loading the packages**
```R
install.packages("tidyverse")
install.packages("skimr")
install.packages("janitor")
install.packages("dplyr")
install.packages("ggplot2")
install.packages("tidyr")
install.packages("lubridate")
install.packages("plotly")
```
```R
library("tidyverse")
library ("skimr")
library("janitor")
library("dplyr")
library("ggplot2")
library("tidyr")
library("lubridate")
library("plotly")
```
**Reading .csv files**
```R
daily_activity <- read_csv("dailyActivity_merged.csv")
View(daily_activity)

hourly_calories <- read_csv("hourlyCalories_merged.csv")
View(hourly_calories)

hourly_intensities <- read_csv("hourlyIntensities_merged.csv")
View(hourly_intensities)

hourly_steps <- read_csv("hourlySteps_merged.csv")
View(hourly_steps)

daily_sleep <- read_csv("sleepDay_merged.csv")
View(daily_sleep)
```
**Display the first few rows of each dataframe**
```R
head(daily_activity)
```
```
Id ActivityDate TotalSteps TotalDistance TrackerDistance
       <dbl> <chr>             <dbl>         <dbl>           <dbl>
1 1503960366 12-04-2016        13162          8.5             8.5 
2 1503960366 13-04-2016        10735          6.97            6.97
3 1503960366 14-04-2016        10460          6.74            6.74
4 1503960366 15-04-2016         9762          6.28            6.28
5 1503960366 16-04-2016        12669          8.16            8.16
6 1503960366 17-04-2016         9705          6.48            6.48
  LoggedActivitiesDistance VeryActiveDistance ModeratelyActiveDistance
                     <dbl>              <dbl>                    <dbl>
1                        0               1.88                     0.55
2                        0               1.57                     0.69
3                        0               2.44                     0.4 
4                        0               2.14                     1.26
5                        0               2.71                     0.41
6                        0               3.19                     0.78
  LightActiveDistance SedentaryActiveDistance VeryActiveMinutes FairlyActiveMinutes
                <dbl>                   <dbl>             <dbl>               <dbl>
1                6.06                       0                25                  13
2                4.71                       0                21                  19
3                3.91                       0                30                  11
4                2.83                       0                29                  34
5                5.04                       0                36                  10
6                2.51                       0                38                  20
  LightlyActiveMinutes SedentaryMinutes Calories
                 <dbl>            <dbl>    <dbl>
1                  328              728     1985
2                  217              776     1797
3                  181             1218     1776
4                  209              726     1745
5                  221              773     1863
6                  164              539     1728
```
```R
head(hourly_calories)
```
```
## # A tibble: 6 × 3
##           id activityhour          calories
##        <dbl> <chr>                    <dbl>
## 1 1503960366 4/12/2016 12:00:00 AM       81
## 2 1503960366 4/12/2016 1:00:00 AM        61
## 3 1503960366 4/12/2016 2:00:00 AM        59
## 4 1503960366 4/12/2016 3:00:00 AM        47
## 5 1503960366 4/12/2016 4:00:00 AM        48
## 6 1503960366 4/12/2016 5:00:00 AM        48
```
```R
head(hourly_intensities)
```
```
## # A tibble: 6 × 4
##           id activityhour          totalintensity averageintensity
##        <dbl> <chr>                          <dbl>            <dbl>
## 1 1503960366 4/12/2016 12:00:00 AM             20            0.333
## 2 1503960366 4/12/2016 1:00:00 AM               8            0.133
## 3 1503960366 4/12/2016 2:00:00 AM               7            0.117
## 4 1503960366 4/12/2016 3:00:00 AM               0            0    
## 5 1503960366 4/12/2016 4:00:00 AM               0            0    
## 6 1503960366 4/12/2016 5:00:00 AM               0            0
```
```R
head(hourly_steps)
```
```
## # A tibble: 6 × 3
##           id activityhour          steptotal
##        <dbl> <chr>                     <dbl>
## 1 1503960366 4/12/2016 12:00:00 AM       373
## 2 1503960366 4/12/2016 1:00:00 AM        160
## 3 1503960366 4/12/2016 2:00:00 AM        151
## 4 1503960366 4/12/2016 3:00:00 AM          0
## 5 1503960366 4/12/2016 4:00:00 AM          0
## 6 1503960366 4/12/2016 5:00:00 AM          0
```
```R
head(daily_sleep)
```
```
## # A tibble: 6 × 5
##           id sleepday        totalsleeprecords totalminutesasleep totaltimeinbed
##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
## 1 1503960366 04-12-2016 00:…                 1                327            346
## 2 1503960366 4/13/2016 12:0…                 2                384            407
## 3 1503960366 4/15/2016 12:0…                 1                412            442
## 4 1503960366 4/16/2016 12:0…                 2                340            367
## 5 1503960366 4/17/2016 12:0…                 1                700            712
## 6 1503960366 4/19/2016 12:0…                 1                304            320
```

**Check the number of unique participants in each data set by counting distinct IDs**
```R
> n_unique(daily_activity$Id)
[1] 33
> n_unique(hourly_calories$Id)
[1] 33
> n_unique(hourly_intensities$Id)
[1] 33
> n_unique(hourly_steps$Id)
[1] 33
> n_unique(daily_sleep$Id)
[1] 24
```
**Clean and rename columns of our datasets**
```R
clean_names(daily_activity)
daily_activity <- rename_with(daily_activity, tolower)
```
```
## # A tibble: 940 × 15
##            id activity_date total_steps total_distance tracker_distance
##         <dbl> <chr>               <dbl>          <dbl>            <dbl>
##  1 1503960366 12-04-2016          13162           8.5              8.5 
##  2 1503960366 13-04-2016          10735           6.97             6.97
##  3 1503960366 14-04-2016          10460           6.74             6.74
##  4 1503960366 15-04-2016           9762           6.28             6.28
##  5 1503960366 16-04-2016          12669           8.16             8.16
##  6 1503960366 17-04-2016           9705           6.48             6.48
##  7 1503960366 18-04-2016          13019           8.59             8.59
##  8 1503960366 19-04-2016          15506           9.88             9.88
##  9 1503960366 20-04-2016          10544           6.68             6.68
## 10 1503960366 21-04-2016           9819           6.34             6.34
## # ℹ 930 more rows
## # ℹ 10 more variables: logged_activities_distance <dbl>,
## #   very_active_distance <dbl>, moderately_active_distance <dbl>,
## #   light_active_distance <dbl>, sedentary_active_distance <dbl>,
## #   very_active_minutes <dbl>, fairly_active_minutes <dbl>,
## #   lightly_active_minutes <dbl>, sedentary_minutes <dbl>, calories <dbl>
```
```
clean_names(hourly_calories)
hourly_calories <- rename_with(hourly_calories, tolower)
```
```
## # A tibble: 22,099 × 3
##            id activity_hour         calories
##         <dbl> <chr>                    <dbl>
##  1 1503960366 4/12/2016 12:00:00 AM       81
##  2 1503960366 4/12/2016 1:00:00 AM        61
##  3 1503960366 4/12/2016 2:00:00 AM        59
##  4 1503960366 4/12/2016 3:00:00 AM        47
##  5 1503960366 4/12/2016 4:00:00 AM        48
##  6 1503960366 4/12/2016 5:00:00 AM        48
##  7 1503960366 4/12/2016 6:00:00 AM        48
##  8 1503960366 4/12/2016 7:00:00 AM        47
##  9 1503960366 4/12/2016 8:00:00 AM        68
## 10 1503960366 4/12/2016 9:00:00 AM       141
## # ℹ 22,089 more rows
```
```
clean_names(hourly_intensities)
hourly_intensities <- rename_with(hourly_intensities, tolower)
```
```
## # A tibble: 22,099 × 4
##            id activity_hour         total_intensity average_intensity
##         <dbl> <chr>                           <dbl>             <dbl>
##  1 1503960366 4/12/2016 12:00:00 AM              20             0.333
##  2 1503960366 4/12/2016 1:00:00 AM                8             0.133
##  3 1503960366 4/12/2016 2:00:00 AM                7             0.117
##  4 1503960366 4/12/2016 3:00:00 AM                0             0    
##  5 1503960366 4/12/2016 4:00:00 AM                0             0    
##  6 1503960366 4/12/2016 5:00:00 AM                0             0    
##  7 1503960366 4/12/2016 6:00:00 AM                0             0    
##  8 1503960366 4/12/2016 7:00:00 AM                0             0    
##  9 1503960366 4/12/2016 8:00:00 AM               13             0.217
## 10 1503960366 4/12/2016 9:00:00 AM               30             0.5  
## # ℹ 22,089 more rows
```

```
clean_names(hourly_steps)
hourly_steps <- rename_with(hourly_steps, tolower)
```
```
## # A tibble: 22,099 × 3
##            id activity_hour         step_total
##         <dbl> <chr>                      <dbl>
##  1 1503960366 4/12/2016 12:00:00 AM        373
##  2 1503960366 4/12/2016 1:00:00 AM         160
##  3 1503960366 4/12/2016 2:00:00 AM         151
##  4 1503960366 4/12/2016 3:00:00 AM           0
##  5 1503960366 4/12/2016 4:00:00 AM           0
##  6 1503960366 4/12/2016 5:00:00 AM           0
##  7 1503960366 4/12/2016 6:00:00 AM           0
##  8 1503960366 4/12/2016 7:00:00 AM           0
##  9 1503960366 4/12/2016 8:00:00 AM         250
## 10 1503960366 4/12/2016 9:00:00 AM        1864
## # ℹ 22,089 more rows
```
```
clean_names(daily_sleep)
daily_sleep <- rename_with(daily_sleep, tolower)
```
```
## # A tibble: 410 × 5
##          id sleep_day total_sleep_records total_minutes_asleep total_time_in_bed
##       <dbl> <chr>                   <dbl>                <dbl>             <dbl>
##  1   1.50e9 04-12-20…                   1                  327               346
##  2   1.50e9 4/13/201…                   2                  384               407
##  3   1.50e9 4/15/201…                   1                  412               442
##  4   1.50e9 4/16/201…                   2                  340               367
##  5   1.50e9 4/17/201…                   1                  700               712
##  6   1.50e9 4/19/201…                   1                  304               320
##  7   1.50e9 4/20/201…                   1                  360               377
##  8   1.50e9 4/21/201…                   1                  325               364
##  9   1.50e9 4/23/201…                   1                  361               384
## 10   1.50e9 4/24/201…                   1                  430               449
## # ℹ 400 more rows
```

**Check the data again to verify that changes has been effected**

```
head(daily_activity)
```
```
## # A tibble: 6 × 15
##           id activitydate totalsteps totaldistance trackerdistance
##        <dbl> <chr>             <dbl>         <dbl>           <dbl>
## 1 1503960366 12-04-2016        13162          8.5             8.5 
## 2 1503960366 13-04-2016        10735          6.97            6.97
## 3 1503960366 14-04-2016        10460          6.74            6.74
## 4 1503960366 15-04-2016         9762          6.28            6.28
## 5 1503960366 16-04-2016        12669          8.16            8.16
## 6 1503960366 17-04-2016         9705          6.48            6.48
## # ℹ 10 more variables: loggedactivitiesdistance <dbl>,
## #   veryactivedistance <dbl>, moderatelyactivedistance <dbl>,
## #   lightactivedistance <dbl>, sedentaryactivedistance <dbl>,
## #   veryactiveminutes <dbl>, fairlyactiveminutes <dbl>,
## #   lightlyactiveminutes <dbl>, sedentaryminutes <dbl>, calories <dbl>
```
```
head(hourly_calories)
```
```
## # A tibble: 6 × 3
##           id activityhour          calories
##        <dbl> <chr>                    <dbl>
## 1 1503960366 4/12/2016 12:00:00 AM       81
## 2 1503960366 4/12/2016 1:00:00 AM        61
## 3 1503960366 4/12/2016 2:00:00 AM        59
## 4 1503960366 4/12/2016 3:00:00 AM        47
## 5 1503960366 4/12/2016 4:00:00 AM        48
## 6 1503960366 4/12/2016 5:00:00 AM        48
```
```
head(hourly_intensities)
```
```
## # A tibble: 6 × 4
##           id activityhour          totalintensity averageintensity
##        <dbl> <chr>                          <dbl>            <dbl>
## 1 1503960366 4/12/2016 12:00:00 AM             20            0.333
## 2 1503960366 4/12/2016 1:00:00 AM               8            0.133
## 3 1503960366 4/12/2016 2:00:00 AM               7            0.117
## 4 1503960366 4/12/2016 3:00:00 AM               0            0    
## 5 1503960366 4/12/2016 4:00:00 AM               0            0    
## 6 1503960366 4/12/2016 5:00:00 AM               0            0
```
```
head(hourly_steps)
```
```
## # A tibble: 6 × 3
##           id activityhour          steptotal
##        <dbl> <chr>                     <dbl>
## 1 1503960366 4/12/2016 12:00:00 AM       373
## 2 1503960366 4/12/2016 1:00:00 AM        160
## 3 1503960366 4/12/2016 2:00:00 AM        151
## 4 1503960366 4/12/2016 3:00:00 AM          0
## 5 1503960366 4/12/2016 4:00:00 AM          0
## 6 1503960366 4/12/2016 5:00:00 AM          0
```
```
head(daily_sleep)
```
```
## # A tibble: 6 × 5
##           id sleepday        totalsleeprecords totalminutesasleep totaltimeinbed
##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
## 1 1503960366 04-12-2016 00:…                 1                327            346
## 2 1503960366 4/13/2016 12:0…                 2                384            407
## 3 1503960366 4/15/2016 12:0…                 1                412            442
## 4 1503960366 4/16/2016 12:0…                 2                340            367
## 5 1503960366 4/17/2016 12:0…                 1                700            712
## 6 1503960366 4/19/2016 12:0…                 1                304            320
```
**Convert datetime format for all files to make it consistent**

In daily_sleep the date formats are not consistent, so we are making all in a single format. 

For our hourly_calories, hourly_intensities and hourly_steps dataset, I will convert date string to date-time. For daily_activity and daily_sleep we are converting using as.Date format.

```
#daily_sleep
daily_sleep <- daily_sleep %>%
  mutate(
    sleepday = case_when(
      grepl("\\d+/\\d+/\\d+ \\d+:\\d+:\\d+ [APap][Mm]", sleepday) ~ 
        as.character(as.POSIXct(sleepday, format = "%m/%d/%Y %I:%M:%S %p", tz = Sys.timezone())),
      grepl("\\d{2}-\\d{2}-\\d{4} \\d{2}:\\d{2}", sleepday) ~ 
        as.character(as.POSIXct(sleepday, format = "%m-%d-%Y %H:%M", tz = Sys.timezone())),
      TRUE ~ NA_character_  # Mark rows with unrecognized formats as NA
    )
  )
daily_sleep$sleepday <- as.Date(daily_sleep$sleepday, format = "%Y-%m-%d")
daily_sleep <- daily_sleep %>%
  rename(date = sleepday) 
class(daily_sleep$date)
```
```
## [1] "Date"
```

```
#daily_activity
daily_activity$activitydate <- as.Date(daily_activity$activitydate, format = "%d-%m-%Y")
daily_activity <- daily_activity %>%
  rename(date = activitydate)
class(daily_activity$date)
```
```
## [1] "Date"
```

```
##hourly_calories
hourly_calories <- hourly_calories %>% 
  rename(date_time = activityhour) %>% 
  mutate(date_time = as.POSIXct(date_time, format ="%m/%d/%Y %I:%M:%S %p" , tz=Sys.timezone()))
class(hourly_calories$date_time)
```
```
## [1] "POSIXct" "POSIXt"
```

```
##hourly_intensities
hourly_intensities <- hourly_intensities %>% 
  rename(date_time = activityhour) %>% 
  mutate(date_time = as.POSIXct(date_time, format ="%m/%d/%Y %I:%M:%S %p" , tz=Sys.timezone()))
class(hourly_intensities$date_time)
```
```
## [1] "POSIXct" "POSIXt"
```

```
##hourly_steps
hourly_steps<- hourly_steps %>% 
  rename(date_time = activityhour) %>% 
  mutate(date_time = as.POSIXct(date_time, format ="%m/%d/%Y %I:%M:%S %p" , tz=Sys.timezone()))
class(hourly_steps$date_time)
```
```
## [1] "POSIXct" "POSIXt"
```

## Analyse 
**Percentage Distribution of Active Minutes**
```
activity_summary <- daily_activity %>%
  group_by(id) %>%
  summarise(
    veryactiveminutes = sum(veryactiveminutes, na.rm = TRUE),
    fairlyactiveminutes = sum(fairlyactiveminutes, na.rm = TRUE),
    lightlyactiveminutes = sum(lightlyactiveminutes, na.rm = TRUE),
    sedentaryminutes = sum(sedentaryminutes, na.rm =TRUE)
  )

activity_summary <- activity_summary %>%
  select(veryactiveminutes, fairlyactiveminutes, lightlyactiveminutes, sedentaryminutes) %>% 
  pivot_longer(cols = everything(), names_to = "ActivityType", values_to = "Minutes") %>%
  mutate(Percentage = (Minutes / sum(Minutes)) * 100)

head(activity_summary)
```
```
## # A tibble: 6 × 3
##   ActivityType         Minutes Percentage
##   <chr>                  <dbl>      <dbl>
## 1 veryactiveminutes       1200     0.105 
## 2 fairlyactiveminutes      594     0.0518
## 3 lightlyactiveminutes    6818     0.595 
## 4 sedentaryminutes       26293     2.30  
## 5 veryactiveminutes        269     0.0235
## 6 fairlyactiveminutes      180     0.0157
```
**Vizualization for Percentage Distribution of Active Minutes**
```
plot_ly(activity_summary, labels = ~ActivityType, values = ~Minutes,
        type = 'pie',textposition = 'outside', textinfo = 'percent',
  text = ~paste(ActivityType, ": ", round(Percentage, 2), "%"),
  marker = list(colors = c('#AADEA7', '#64C2A6', '#F6AB49', '#F66D44')),
  width = 500,  # Set the width of the chart
  height = 500  # Set the height of the chart
) %>%
  layout(
    title = 'Percentage Distribution of Active Minutes'
  )
```
![piechart2](https://github.com/user-attachments/assets/6e4dcba3-cefd-4059-be42-9fd73d61e457)


**Key Observations from the Chart:**
- Percentage of active minutes in the four categories: very active, fairly active, lightly active and sedentary.
- The major issue here is the large portion of time spent sedentary and the minimal time spent in high-intensity activities (very active and fairly active).
- This pattern is common in modern lifestyles but can have serious consequences for long-term health. Gradually increase the percentage of time spent in moderate-to-vigorous activities.

**Calculation of Calories Per Distance by Day**
```
calories_per_distance <- daily_activity %>%
  mutate(WeekDay = weekdays(date)) %>%
  mutate(WeekDay = ordered(WeekDay, levels = c("Monday", "Tuesday", "Wednesday", "Thursday", 
                                               "Friday", "Saturday", "Sunday"))) %>%
  group_by(WeekDay) %>%
  summarise(
    calories_per_day = sum(calories, na.rm = TRUE),
    distance_per_day = sum(totaldistance, na.rm = TRUE),
    calories_per_distance = round(sum(calories, na.rm = TRUE) / sum(totaldistance, na.rm = TRUE), 2)
  )
print(calories_per_distance)
```
```
## # A tibble: 6 × 4
##   WeekDay   calories_per_day distance_per_day calories_per_distance
##   <ord>                <dbl>            <dbl>                 <dbl>
## 1 Monday              278905             666.35                  419.56
## 2 Tuesday             358114             886.50                  404.96
## 3 Wednesday           345393             823.25                  420.55
## 4 Thursday            323337             781.90                  414.06
## 5 Friday              293805             669.05                  439.14
## 6 Saturday            292016             726.98                  402.24
## 7 Sunday              273823             608. 29                 450.15
```
**Vizualization for Calculation of Calories Per Distance by Day**
```
ggplot(calories_per_distance, aes(x = WeekDay, y = calories_per_distance, group = 1)) +
  geom_line(color = "darkblue", linewidth = 1.2) + 
  geom_point(color = "lightblue", size = 3) +  
  scale_y_continuous(labels = scales::comma) + 
  labs(
    title = "Total Calories Burned Per Distance by Weekday",
    x = "Weekday",
    y = "Calories Per Distance"
  )
```
![calories burned per distance](https://github.com/user-attachments/assets/252d942e-bc60-47ce-b44e-d2ecd5cc537d)


**Key Observations from the Chart:**
- It is observed that Sundays exhibit a relatively high proportion of calories burned per distance compared to other days of the week.
- Leisure time on Sundays may lead users to engage in deliberate, higher-intensity physical activities.
- Fridays Show the Second-Highest Calories Burned per Distance, this may indicate, End-of-week activities that differ from the routine, such as evening walks, gym sessions, or sports.
#Weekdays (Monday to Friday) show a consistent range of 404–439 calories per distance, this could be attributed to a structured routine during the workweek, balancing moderate activity levels.
- Saturday has the lowest value means less intense physical activity.

**Sleep Patterns by Day of the Week**
```
sleep_data_summary <- daily_sleep %>%
    mutate(Weekdays = wday(`date`, label = TRUE, week_start = 1)) %>%
    group_by(Weekdays) %>%
    summarize(
      Avg_Minutes_Asleep = mean(totalminutesasleep), 
      Avg_Time_In_Bed = mean(totaltimeinbed)
    ) %>%
    arrange(Weekdays) %>%
    pivot_longer(
      cols = c(Avg_Minutes_Asleep, Avg_Time_In_Bed), 
      names_to = "Category", 
      values_to = "Value"
    ) %>%
    mutate(Value = round(Value, 2))  # Round the Value column
  head(sleep_data_summary)
```
```
## # A tibble: 6 × 3
##   Weekdays Category           Value
##   <ord>    <chr>              <dbl>
## 1 Mon      Avg_Minutes_Asleep  420.50
## 2 Mon      Avg_Time_In_Bed     457.35
## 3 Tue      Avg_Minutes_Asleep  405.54
## 4 Tue      Avg_Time_In_Bed     443.29
## 5 Wed      Avg_Minutes_Asleep  435.68
## 6 Wed      Avg_Time_In_Bed     470.03
```
**Visualization for Sleep Patterns by Day of the Week**
```
ggplot(sleep_data_summary, aes(x = Weekdays, y = Value, fill = Category)) +
   geom_bar(stat = "identity", position = position_dodge(width = 0.8)) +
   scale_fill_manual(values = c("Avg_Minutes_Asleep" = "blue", "Avg_Time_In_Bed" = "orange")) + 
   scale_y_continuous(labels = scales::label_number(accuracy = 1)) +  # Fully qualified function call
   labs(
    title = "Sleep Behavior by Day of the Week",
    x = "Day of Week",
    y = "Minutes",
    fill = "Legend"
    )
```
![sleep pattern by day of week](https://github.com/user-attachments/assets/d5ae7f72-54fe-4c5f-bd87-3d11296dd606)

**Key Observations from the Chart:**
- Sunday being the highest average sleep time (452.75 minutes, ~7.5 hours) and longest time in bed (503.51 minutes, ~8.4 hours).
- This indicates users might catch up on sleep on Sundays, reflecting a common recovery trend after a busy week. This is likely an effort to recover from sleep deficits accumulated during the week.
#In the graphs above we can deduce that users don’t take the recommended amount of sleep of 8 hours(480minutes).
  
**Steps per weekdays**
```
weekday_steps <- daily_activity %>%
  mutate(weekday = weekdays(date))
weekday_steps$weekday <-ordered(weekday_steps$weekday, levels=c("Monday", "Tuesday", "Wednesday", "Thursday",  "Friday", "Saturday", "Sunday"))

weekday_steps <-weekday_steps %>%
  group_by(weekday) %>%
  summarize (daily_steps = sprintf("%.2f", mean(totalsteps)))
print(weekday_steps)
```
```
 weekday   daily_steps
  <ord>     <chr>      
1 Monday    7780.87    
2 Tuesday   8125.01    
3 Wednesday 7559.37    
4 Thursday  7405.84    
5 Friday    7448.23    
6 Saturday  8152.98    
7 Sunday    6933.23
```
**Vizualization for Steps per weekdays**
```
ggplot(data = weekday_steps, aes(x = weekday, y = as.numeric(daily_steps))) + 
  geom_bar(stat = "identity",fill = 'lightblue',width = 0.8) +
  geom_hline(yintercept = 7500)+
  labs(
    title = "Weekly Average Steps Distribution",
    x = "Weekday",
    y = "Daily Steps"
  ) +
  scale_y_continuous(breaks = seq(0, 10000, by = 2000))
```
![weekly avg step distribution in blue colour grapgh](https://github.com/user-attachments/assets/3c09278b-63c9-4da9-a7f7-4af0fce6b84f)

**Key Observations from the Chart:**

- Most users are consistent with their daily steps routine, except Sunday which more like a relaxation day.
- Weekdays show consistent step counts averaging around 7,700 steps. This consistency suggests a routine influenced by daily schedules like work or commuting.
- Saturdays are the most active, likely due to non-workday opportunities for movement. As per 2011 CDC study 7,500–9,999 Steps per day is considered as Somewhat active.
- Most of the people who are using the BellaBeat product is achieving there weekly steps goals.

**Hourly steps throughout the day**
```
hourly_steps <- hourly_steps %>%
  mutate(
    date = format(date_time, "%Y-%m-%d"),  
    time = format(date_time, "%H:%M:%S")   
  ) %>%
  mutate(date = ymd(date)) %>%  
  select(id, date, time, steptotal) %>%  
  group_by(time) %>%  
  summarize(average_steps = mean(steptotal))  # Calculate average steps for each time

head(hourly_steps)
```
```
## # A tibble: 6 × 2
##   time     average_steps
##   <chr>            <dbl>
## 1 00:00:00         42.2 
## 2 01:00:00         23.1 
## 3 02:00:00         17.1 
## 4 03:00:00          6.43
## 5 04:00:00         12.7 
## 6 05:00:00         43.9
```
**Vizualization for Hourly steps throughout the day**
```
ggplot(hourly_steps, aes(x = time, y = average_steps, fill = average_steps)) +  # Map fill to average_steps
  geom_bar(stat = "identity") +  
  scale_fill_gradient(low = "lightblue", high = "darkblue") +  
  labs(
    title = "Hourly Step Activity", 
    x = "Time of Day", 
    y = "Average Steps"
  ) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
```
![hourly steps throughout the day](https://github.com/user-attachments/assets/ded0e9e9-2b70-4934-aa23-2276c9eab1f5)

**Key Observations from the Chart:**

- The hour of 6 p.m. is when the greatest average number of steps is recorded.
- It appears from this data that people are more likely to be physically active in the evening. After finishing their work for the day, individuals might head to the gym or go for a walk , as the majority of the activity takes place between 5pm to 7 p.m.

**Correlation of Calories with Active Minutes**
```
active_minutes_calories <- daily_activity %>%
  group_by(id, date) %>%
  summarise(
    Total_Very_Active_Minutes = sum(veryactiveminutes, na.rm = TRUE),  # Sum of Very Active Minutes
    Total_Calories = sum(calories, na.rm = TRUE)  # Sum of Calories
  ) %>%
  ungroup() %>%
  mutate(
      Calories_Per_VeryActiveMinute = if_else(Total_Very_Active_Minutes == 0, NA_real_, Total_Calories / Total_Very_Active_Minutes),
      Calories_Per_VeryActiveMinute = Total_Calories / Total_Very_Active_Minutes  # Calculate calories per very active minute
  )
head(active_minutes_calories)
```
```
id date       Total_Very_Active_Minutes Total_Calories
        <dbl> <date>                         <dbl>          <dbl>
 1 1503960366 2016-04-12                        25           1985
 2 1503960366 2016-04-13                        21           1797
 3 1503960366 2016-04-14                        30           1776
 4 1503960366 2016-04-15                        29           1745
 5 1503960366 2016-04-16                        36           1863
 6 1503960366 2016-04-17                        38           1728
 7 1503960366 2016-04-18                        42           1921
 8 1503960366 2016-04-19                        50           2035
 9 1503960366 2016-04-20                        28           1786
10 1503960366 2016-04-21                        19           1775
   Calories_Per_VeryActiveMinute
                           <dbl>
 1                          79.4
 2                          85.6
 3                          59.2
 4                          60.2
 5                          51.8
 6                          45.5
 7                          45.7
 8                          40.7
 9                          63.8
10                          93.4
```
**Vizualization for Correlation of Calories with Active Minutes**
```
active_minutes_calories %>%
  ggplot(aes(x = Total_Very_Active_Minutes, y = Total_Calories, color = Calories_Per_VeryActiveMinute)) +
  geom_point() +
  scale_color_gradient(low = "lightblue", high = "darkblue") +  
  labs(title = "Correlation Between Active Minutes and Calorie Burn", 
       x = "Total Very Active Minutes", 
       y = "Total Calories Burned") +
  theme_minimal()
```
![correlation of calories n active min](https://github.com/user-attachments/assets/9252a4ec-b079-4260-975b-abc0e7ebde1e)

**Key Observations from the Chart:**

- There is a more pronounced positive relationship between VeryActiveMinutes and calories burned.
- Even a small increase in VeryActiveMinutes leads to a meaningful increase in calories burned, demonstrating the disproportionate impact of intensity compared to activity duration alone.
- I assume we can encourage individuals to include even short bursts of high-intensity workouts (e.g., interval training) in their routines for maximum calorie burn.

## Act
**Insights from the Analysis**
- The analysis highlights key patterns in activity and sleep behaviour. Prolonged sedentary time (81.3%) and insufficient high-intensity activities increase health risks, emphasizing the need to reduce inactivity. 
- Sleep duration averages 6.7–7.5 hours, slightly below the recommended 7–9 hours, suggesting a focus on better sleep hygiene to improve rest quality.
- Step counts generally meet or exceed the 7,500-step baseline, with peak activity in the evening (5–7 PM). While Sundays are restful with improved sleep, they are marked by reduced activity, underscoring the importance of maintaining consistent physical activity throughout the week.
-  This summary encapsulates the overall findings based on the insights derived from the data analysis.
## Recommendations
- **Personalized Activity and Lifestyle Goals:** Set activity and step targets based on users’ past performance, lifestyle, or preferences, customizable during sign-up. Offer calorie-tracking features or integrate with third-party apps to align physical activity data with dietary goals. Provide meal suggestions or personalized diet plans and include hydration reminders to encourage a balanced lifestyle.
- **Sedentary and Heart Rate Alerts:** Notify users to engage in light activities after prolonged inactivity with prompts like, “You’ve been sedentary for 2 hours; take a 5-minute walk!”. Additionally, monitor heart rate and send alerts for abnormal fluctuations, ensuring timely action if thresholds are exceeded.
- **Sleep and Recovery Insights:** Provide tailored advice for improving sleep routines, such as setting a regular bedtime, reducing screen time, or relaxing before sleep. Include tips for creating a restful environment and maintaining consistent recovery practices.
- **Weekly Dashboard:** Summarize weekly activity, calories burned, and sleep performance with improvement tips in a user-friendly dashboard. Share a detailed summary every Sunday to help users track progress and adopt healthier habits.
- **Motivational Features and Social Engagement:** Incorporate gamified elements like streaks, badges, and friendly competitions to keep users motivated. Include period tracking features to help users monitor their menstrual cycles, including reminders for expected periods, ovulation, and tips for managing symptoms.
