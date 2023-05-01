# Marathon_Training_Prediction
Explore Strava data to gain insights on Marathon training.

## Problem Statement

There are about 1 million runners per year attempting to complete a full marathon. Running a full marathon requires an individual to run a 26.2 mile / 42.2 km course. To prepare for a full marathon a typical training plan encompasses 3-7 days of running per week for 16 weeks. Due to the physical demands of running a full marathon physical recovery is needed between finishing a marathon and the next training cycle. With the amount of training that is required and the time for recovery that is required it is difficult to fully train for more than 1 or 2 marathons a year. Many runners will participate in numerous marathons per year but to fully prepare to run your best marathon you need to focus on a proper training cycle and fully recover before training for the next race. Given this timeframe there is not many opportunities for feedback on what worked well and what did not work well in each training cycle. This is a reason why you can see individuals closer to 40 years old than 20 years old winning major marathons. It takes years of trial and error to run your best race. Shorter distance races are typically dominated by individuals closer to 20 years old. For shorter distance races there is less build up in training and recovery needed so variations in training can be attempted and evaluated frequently. I am going to evaluate which factors in marathon training impact the finish time the most.

The idea is to determine if high mileage, faster pace, more days running, or some other feature contribute more positively or negatively to the final race time. It would appear obvious that running more and running at a faster pace will improve the race time the most but increasing mileage and pace increases the risk of running related injuries and overtraining. Many amateur marathoners approach race day over trained, which means due to excessive training their body has not fully recovered and they will not be able to perform their best. Another issue with overtraining is the possibility of an injury. Injuries could pop up at any time in the training cycle. Injuries early on could result in lost workouts which could lead to less training time or an attempt to overcome the lost workouts which could lead to more injuries. Injuries that occur late in a training cycle could carry into the race which will reduce the ability to run at full potential.

## Data

With the high usage of GPS enabled running watches and the sharing of this data to various websites such as Garmin Connect, Strava, Runkeeper, and MapMyRun there is an increase in available training and racing data. To obtain the marathon training data, publicly reported data on Strava will be used. Existing data has already been collected (Alfonseca, Watanabe, & Duarte, 2022) and will be used to identify marathon training cycles. To identify the training cycle an individual that has completed a run between 41.8 km and 42.5 km will be used as the ending point of the race and then looking back over the previous 16 weeks to obtain the data for the training cycle. Since a marathon is measured as the shortest distance from the starting line to the finish line and it is very difficult or impossible to run that exact line due to other runners being on the course a buffer is used to determine the exact distance of the race.

Typical marathon training plans are 16 weeks with each week slightly increasing in duration and/or effort. This buildup lasts for 2-3 weeks then a recovery week occurs which reduces the mileage and the process repeats itself. The last 3 weeks from the marathon date the taper begins where the mileage reduces to allow the body to recover for the race. 
The original data provides:
- **Datetime**: the date of the running activity
-	**Athlete**: an integer to identify the runner and provide anonymity
-	**Distance**: the total distance of the run, in kilometers
-	**Duration**: the total time of the run, in minutes
-	**Gender**: “M” or “F”
-	**Age Group**: age interval: 18-34, 35-44, 55+
-	**Country**: country of origin of the athlete
-	**Major**: list of major marathons the runner has run

Since the original data was used for all types of runners the first step was to remove runners that did not complete a run between 41.8 km and 42.5 km. Exploration of the data showed that there were finish times far longer than a typical marathon cutoff time. This could be marathoners that had physical difficulties or an extreme marathon that was off-road with significant elevation gain or other environmental factors. Any marathons finish times greater than 8 hours were removed from the data set. Each data point represents a single running event, since the goal is to evaluate a training plan the runner was grouped by athlete and the preceding 16 weeks from a marathon distance run. This resulted in some runners having multiple training cycles. With the data grouped by athlete and training week the following features were added:
-	**Minimum Distance**: Shortest run of the week in kilometers
-	**Maximum Distance**: Longest run of the week in kilometers
-	**Mean Distance**: Average run of the week in kilometers
-	**Total Distance**: Cumulative kilometers of the week
-	**Minimum Duration**: Shortest run of the week in minutes
-	**Maximum Duration**: Longest run of the week in minutes
-	**Mean Duration**: Average run of the week in minutes
-	**Slowest Pace**: Slowest run of the week in % of race pace
-	**Fastest Pace**: Fastest run of the week in % of race pace
-	**Average Pace**: Average run of the week in % of race pace
-	**Number of runs**: Total number of runs of the week

With the above features added for each week the data set used for this report resulted in a datapoint for each runner/training cycle with each feature above and the week number. This results in 16 weeks * 11 features = 176 features. Additional categorical features of Gender, Age Group, and, Country were also added for a total of 180 features.

## Methodology

There are many popular marathon training styles that exist which have their main focus on high mileage, faster pace, or an 80/20 mix, which encompasses running most miles at an easier pace with some faster workouts. As a first step a clustering algorithm of the training data will be performed to identify different training styles.
