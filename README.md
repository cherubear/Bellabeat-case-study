# Bellabeat Case Study
a capstone project for Google Data Analytics Professional Certificate program

## Background
In this case study, you will perform data analysis for Bellabeat, a high-tech manufacturer of health-focused products for women. You will analyze smart device data to gain insight into how consumers are using their smart devices. Your analysis will help guide future marketing strategies for your team. Along the way, you will perform numerous real-world tasks of a junior data analyst by following the steps of the data analysis process: Ask, Prepare, Process, Analyze, Share, and Act. By the time you are done, you will have a portfolio-ready case study to help you demonstrate your knowledge and skills to potential employers!

A detailed case study packet is saved in the Business Ask folder.

To facilitate the planning stage of this project, I put together [this mind map](https://miro.com/app/board/uXjVL_Uxt_4=/?share_link_id=308983498196) based on the guides in the packet and some action plans I jotted down as I read the requirements.

![image](https://github.com/user-attachments/assets/00b7bc18-3011-48ba-9369-18e8488c7810)

Now I am starting this case study by following the steps of the data analysis process: **ask**, **prepare**, **process**, **analyze**, **share**, and **act**. So this document will follow the same structure.

## Ask
**Business Ask**: Or the purpose statement of this study is, to identify key characteristics of users using wearable fitness tracker, and then use these insights to guide Bellabeat's marketing strategy.

**Stakeholders**: The findings and recommendations will be shared with Bellabeat's executive team, among whom are Urška Sršen, cofounder and Chief Creative Officer, and Sando Mur, Mathematician and cofounder. In addition, I will also collaborate and share results with the marketing analytics team, which includes a team of data experts and analysts just like myself.

## Prepare
Urška has pointed me to a dataset of FitBit tracker. Upon inspection, this dataset contains usage data of 30 individual users. The number of observations is on the low side, barely making the cut to qualify it as a sample good enough to be representative of the user space of smart device market. [This link](https://www.scribbr.com/statistics/central-limit-theorem/#:~:text=The%20central%20limit%20theorem%20states%20that%20the%20sampling%20distribution%20of,size%20is%20n%20%E2%89%A5%2030.) provides a brief explanation of why a sample size of greater than 30 is desirable.

I tried searching in [GitHub](https://github.com/), [Kaggle](https://www.kaggle.com/datasets), [Tableau](https://public.tableau.com/app/search/all), and [Google](https://www.google.com/). The only dataset other than the abovementioned FitBit data is one that records activity data of one single Redmi Fuel Band user over the past three years. While the data appear to be well maintained, and it is very applaudable that this individual is willing to share their data, I decide not to include it because it adds very little incremental value to our FitBit dataset. I was not surprised by how scarce data could be, when it comes to information related to health and activity, since people are very vigilent about their privacy. We can build on this thought when Bellabeat collects and uses user data in the future.

Moving on, let's take a closer look to the FitBit dataset. [This dataset](https://www.kaggle.com/datasets/arashnic/fitbit) is available on Kaggle, or alternatively [here](https://zenodo.org/records/53894#.YMoUpnVKiP9) on Zenodo. These datasets were generated by respondents to a distributed survey via Amazon Mechanical Turk between 03.12.2016-05.12.2016. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. Variation between output represents use of different types of Fitbit trackers and individual tracking behaviors and/or preferences. 

Here is my evaluation regarding how the data fare as good data, on a scale of 1-10 (10 being the best):
* **Reliable - 8**: Activity data are collected by the FitBit device, so I consider it quite reliable as opposed to be from a self-report survey. However, users may not wear the tracker 24/7, so activities such as steps and sleep time may be under-reprensented.
* **Original - 10**: The data is collected through FitBit trackers by user who consented to a distributed survey via Amazon Mechanical Turk.
* **Comprehensive - 8**: The data included is directly governed by what the tracker is able to track, so I think it is pretty good in terms of comprehensiveness, but not ideal. For example, if we want to raise awareness about [obesity](https://www.who.int/news-room/fact-sheets/detail/obesity-and-overweight), the best way to evaluate a person is by their BMI. However, neither weight nor height can be measured by a fitness tracker. In this dataset, some weight data are available if and only if user logs them in the app.
* **Current - 1**: Data include activity recorded from March to April of 2016, which is more than 8 years ago. Generational trends and user behavior probably have changed by now, so this dataset fares poorly on this scale, and may not provide useful business insights today.
* **Cited - 10**: Data is collected by Julia E Brinton, Mike D Keating, Alexa M Ortiz, Kelly R Evenson, Robert D Furberg in [this study](https://pubmed.ncbi.nlm.nih.gov/28450274/).

Data files are collected over 2 periods of time: one from Mar 12, 2016 to Apr 11, 2016, and the other from Apr 12, 2016 to May 12, 2016. But data files across periods do not necessarily align. If the same data tables exist for both time periods, I can simply stack them (when Col 3 and 4 are both "Yes" in the following table).

|      | File Name | In Mar-Apr Batch | In Apr-May Batch | Number of Fields (Col) | Description of Data | Issues & Actions |
|:---: | --------- | :--------------: | :--------------: | :--------------------: | ------------------- | ---------------- |
|**1**| dailyActivity_merged.csv | Yes | Yes | 15 | <ol><li>Id#️⃣</li><li>ActivityDate📅</li><li>TotalSteps#️⃣</li><li>TotalDistance#️⃣</li><li>TrackerDistance#️⃣</li><li>LoggedActivitiesDistance#️⃣</li><li>VeryActiveDistance#️⃣</li><li>ModeratelyActiveDistance#️⃣</li><li>LightActiveDistance#️⃣</li><li>SedentaryActiveDistance#️⃣</li><li>VeryActiveMinutes#️⃣</li><li>FairlyActiveMinutes#️⃣</li><li>LightlyActiveMinutes#️⃣</li><li>SedentaryMinutes#️⃣</li><li>Calories#️⃣</li></ol> | <ul><li>Some numerical data are stored as text strings, need to convert data type.</li><li>TotalDistance may not equate the sum of its subcategories, need to inspect closer. </li><li>This table aggregates steps, distances, calories from the tables below, but does not capture all information, e.g. some users or days entries are left out.</li></ul> |
|**2**| dailyCalories_merged.csv | No | Yes | 3 | Data already captured in file 1 | |
|**3**| dailyIntensities_merged.csv | No | Yes | 10 | Data already captured in file 1 | |
|**4**| dailySteps_merged.csv | No | Yes | 3 | Data already captured in file 1 | |
|**5**| hearrate_seconds_merged.csv | Yes | Yes | 3 | <ol><li>Id#️⃣</li><li>Time🕐</li><li>Value#️⃣</li></ol> | This is indeed 30 users' heart rate by the second over two months, so there are too many rows to be processed in a spreadsheet application. |
|**6**| hourlyCalories_merged.csv | Yes | Yes | 3 | <ol><li>Id#️⃣</li><li>ActivityHour🕐</li><li>Value#️⃣</li></ol> | |
|**7**| hourlyIntensities_merged.csv | Yes | Yes | 4 | <ol><li>Id#️⃣</li><li>ActivityHour🕐</li><li>TotalIntensity#️⃣</li><li>AverageIntensity#️⃣: hourly value / 60</li></ol>
|**8**| hourlySteps_merged.csv | Yes | Yes | 3 | <ol><li>Id#️⃣</li><li>ActivityHour🕐</li><li>StepTotal#️⃣</li></ol> ||
|**9**| minuteCaloriesNarrow_merged.csv | Yes | Yes | 3 | Similar to file 6, but broken down into minutes ||
|**10**| minuteCaloriesWide_merged.csv | No | Yes | 62 | Same data as file 9, but in wide format with each minute of hour as a column ||
|**11**| minuteIntensitiesNarrow_merged.csv | Yes | Yes | 3 | Similar to file 7, but broken down into minutes ||
|**12**| minuteIntensitiesWide_merged.csv | No | Yes | 62 | Same data as file 11, but in wide format with each minute of hour as a column ||
|**13**| minuteMETsNarrow_merged.csv | Yes | Yes | 3 | <ol><li>Id#️⃣</li><li>ActivityHour🕐</li><li>METs#️⃣: metabolic equivalents, used to estimate activity intensity</li></ol>
|**14**| minuteSleep_merged.csv | Yes | Yes | 3 | <ol><li>Id#️⃣</li><li>Date🕐</li><li>value#️⃣: in fact category labels, 1=light, 2=deep, 3=REM</li></ol> ||
|**15**| minuteStepsNarrow_merged.csv | Yes | Yes | 3 | Similar to file 8, but broken down into minutes ||
|**16**| minuteStepsWide_merged.csv | No | Yes | 62 | Same data as file 16, but in wide format with each minute of hour as a column ||
|**17**| weightLogInfo_merged.csv | Yes | Yes | 8 | <ol><li>Id#️⃣</li><li>Date🕐</li><li>WeightKg#️⃣</li><li>WeightPounds#️⃣</li><li>Fat#️⃣</li><li>BMI#️⃣</li><li>IsManualReport🔤</li><li>LogId🔤</li></ol> ||
|**18**| sleepDay_merged.csv | No | Yes | 5 | <ol><li>Id#️⃣</li><li>SleepDay🕐</li><li>TotalSleepRecords#️⃣</li><li>TotalMinutesAsleep#️⃣</li><li>TotalTimeInBed#️⃣</li></ol> | Not available for Mar-Apr, but can be calculated from file 14 |

Building on my purpose statement, and taking into account what data I have at hand, I decide to look into these research questions:
* Activity
    * Do fitness tracker users tend to be very active?
    * Are they more active on certain days of the week?
    * Do steps, calories, activity intensity tell the same story?
    * How to calories burnt correlate to number of steps?
* Sleep
    * Do users wear the tracker during sleep?
    * How are users' sleep patterns?
* Health
    * Do users use tracker to address a specific health concern?
    * Are people with a heart condition or obesity concern more likely to use a fitness tracker?
 
Next, I will use the following data files to address these questions:
1. **dailyActivity_merged.csv**: The format and structure of this table is pretty good, but there are missing entries and wrong data types in the provided file. So I will stick to this format, but calculate data with the other relevant files.
2. **sleepDay_merged.csv**: since Mar-Apr data does not include this file, I will use **minuteSleep_merged.csv** to calculate all the fields for Mar-Apr and append it to Apr-May data.
3. **hearrate_seconds_merged.csv** and **weightLogInfo_merged.csv** are both great for answering my question regarding health, but there are two issues:
    * Weight and BMI are self-reported, and there are a lot of missing data.
    * It is difficult to define a normal and a subnormal heart rate or BMI level, without knowing the person's age and other health conditions. 

## Process
The **dailyActivity_merged.csv** may be incomplete. For exmaple the first user 1503960366 only has data from 2016-03-25, but hourly and minute versions actually do have data starting from 2016-03-12. We will start from the hourly version and build my way to the same format as dailyActivity_merged.csv file.
![image](https://github.com/user-attachments/assets/24114f03-d842-4bb9-a4b3-564ca2b4b61c)

During discovery, there are a few files in long format that are impossible to process in a spreadsheet application. I will use R throughout the data processing and visualization phases of this project.

Documentation of any cleaning or manipulation of data:
* Files from two time periods are insepcted and combined if they have the same structure and formats.
* When daily data is not available for Mar-Apr, I aggregate them from hourly data before merging with Apr-May data.
* Complete duplicates were removed, although no duplicate was found - there are duplicates in the dailyActivity.csv file, but I created my own version by aggregating the other files.
* Dates were cast from string to datetime data type.
* There are occurences where column names may be too generic, or in conflict with an R keyword (such as date), so they are renamed.

Please see detailed steps in Process phase in [this RMD file](https://github.com/cherubear/Bellabeat-case-study/blob/main/bellabeat-case-study.Rmd).

## Analyze

Please see detailed steps in Analyze phase in [this RMD file](https://github.com/cherubear/Bellabeat-case-study/blob/main/bellabeat-case-study.Rmd).

## Share

Key observation 1: A typical user tends not to be very active. On average, they take under 6000 steps a day, while CDC recommends at least 10,000 a day. This goal was only hit 31% of the time.
![image](https://github.com/user-attachments/assets/eb5f7dc3-4423-42e3-b90a-0b5ff5617425)

Key observation 2: A typical user is likely someone who holds a regular five-day work week in an office. On any given day, number of steps peak during lunch time and after 5PM. Over a week, it is not surprising that most steps were taken on Satudays. Wednesdays are not bad either, but Sundays and Mondays are when users walked the least. It is a bit counterintuitive that Friday also appears to be an inactive day in terms of steps taken.
![image](https://github.com/user-attachments/assets/8ea874c1-ff1e-4fc5-ba24-104da93f3196)
![image](https://github.com/user-attachments/assets/83b00abc-57e9-40cc-ae83-71acf64f28a4)

Key observation 3: Users do not take the fitness tracker to bed, more often than not. Considering we have data of 30 users wearing FitBit over two months' time (that is 1800+ user-days), we only see 49% of the days when the tracker logged some sleep data. Sleep tracking may not be a top concern for these users, and not a key feature they emphasize when shopping for a fitness tracker.
![image](https://github.com/user-attachments/assets/7dd73c2a-f3d5-4b9c-9129-b2b234dae367)

Key observation 4: Getting weight data is hard given everyone's privacy concerns, but out of the 13 users who did log weight data, a majority (9 out of 13) was overweight or obese. This, combined with the fact that our sample users were not very active, showed that a typical user interested in getting a fitness tracker may be hoping to track their activity and motivate them to be fit.
![image](https://github.com/user-attachments/assets/5d1ce687-f143-444a-8291-55d485cac67e)

I understand that a lot more can be done with the dataset, producing fancy charts showing trends and relationships between different variables. But these are the four key observations I identified that can get us started with a profile of potential customer of our fitness products. There certainly are a lot more I want to recover, just to name a few:
* What type of activities most efficiently contribute to weight loss (high or medium in intensity)?
* Does users' behavior change over time, in other words, does wearing a fitness tracker help them build healthier lifestyle?
* I have my hypotheses why users may not wear fitness tracker to bed, but can we find out more, maybe through a survey?
* Does the type of activity, or when the activity is done, affect quality of sleep?
However, the data in this dataset as well as on the internet as of today is very scarce. Bellabeat can start collecting data with our own products, push out insights we obtain from this data, and share them with our users.

## Act

Based on my analysis, I would recommend taking these actions with Bellabeat Leaf and our smartphone application:

**Marketing**

* Since a typical user is probably a working class individual, we can deploy ads on subways, and elevators or shopping malls near office buildings.
* We can collaborate with YouTube or podcast KOLs who are known to have a young working population (office jobs more desirable) as their audience, for example, a Business and Finance podcast channel.
* We can sponsor a corporate fitness event.

**Data Collecting**

* Fully educate user on how we will use our data, e.g. how we use data to uncover how certain activity/sleep pattern can contribute to one's health.
* Openly acknowledge their privacy concerns, and explain what we do to protect their privacy.
* Upon setup in app, use a short questionnaire to understand what the user hope to achieve with a smart fitness tracker. If we find out, for example, activity tracking is a much higher demand than sleep tracking, we know what to prioritize in product development.

**Product Features**

* Share insights we learned from our own data, show fund and easy-to-read visualizations and articles that help people to stay fit. E.g. introduce creative ways to exercise, like a short exercise before bed, what to do when it's too cold outside, what can you do when working at a desk, etc.
* Motivate! Use badge system, gamification, or simply some kind words.
* On the Leaf tracker, implement a reminder for user to stand up and take a short walk.
* Since we are targetting a female market, maybe we can use something like a color light on the Leaf to remind user of menstral cycle status.
