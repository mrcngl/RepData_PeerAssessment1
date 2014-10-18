---
title: "Reproducible Research: Peer Assessment 1"
author: "Mrcngl"
output: 
  html_document:
    keep_md: true
---

# Reproducible Research: Assignment 1
Loading Lattice data visualization system

```r
library(lattice)
```

## Loading and preprocessing the data
Loading Activity csv data **activity.csv** and date conversion to **R Date class**  

```r
activitydata <- read.csv("activity.csv")
```

```
## Warning in file(file, "rt"): cannot open file 'activity.csv': No such file
## or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

```r
activitydata$date <- as.Date(activitydata$date,"%Y-%m-%d")
```

```
## Error in as.Date(activitydata$date, "%Y-%m-%d"): object 'activitydata' not found
```

## What is mean total number of steps taken per day?
1. Make a histogram of the total number of steps taken each day
2. Calculate and report the mean and median total number of steps taken per day


Total number of steps per day  

```r
totalsteps <- tapply(activitydata$steps, activitydata$date,sum)
```

```
## Error in tapply(activitydata$steps, activitydata$date, sum): object 'activitydata' not found
```
Plot histogram of **total number of steps/day**

```r
hist(totalsteps,col="blue",xlab="Total Steps per Day", 
      ylab="Frequency", main="Histogram of Total Steps/Day")
```

```
## Error in hist(totalsteps, col = "blue", xlab = "Total Steps per Day", : object 'totalsteps' not found
```
Calculate Mean total **steps/day**

```r
mean(totalsteps,na.rm=TRUE)
```

```
## Error in mean(totalsteps, na.rm = TRUE): object 'totalsteps' not found
```

Calculate Median total **steps/day**

```r
median(totalsteps,na.rm=TRUE)
```

```
## Error in median(totalsteps, na.rm = TRUE): object 'totalsteps' not found
```

## What is the average daily activity pattern?
1. Make a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
2. Calculate which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps


Calculate Mean of steps across all days by time interval

```r
meansteps <- tapply(activitydata$steps,activitydata$interval,
                                 mean,na.rm=TRUE)
```

```
## Error in tapply(activitydata$steps, activitydata$interval, mean, na.rm = TRUE): object 'activitydata' not found
```
Time series plot

```r
plot(row.names(meansteps),meansteps,type="l",
     xlab="5-minute interval", 
     ylab="Steps taken average", 
     main="Average number of steps Taken at 5 minute Intervals",
     col="red")
```

```
## Error in row.names(meansteps): object 'meansteps' not found
```
Maximum average number of steps across all days by time interval

```r
interval_number <- which.max(meansteps)
```

```
## Error in which.max(meansteps): object 'meansteps' not found
```

```r
interval_max <- names(interval_number)
```

```
## Error in eval(expr, envir, enclos): object 'interval_number' not found
```

```r
interval_max
```

```
## Error in eval(expr, envir, enclos): object 'interval_max' not found
```



## Imputing missing values

1. Calculate and report the total number of missing values in the dataset.
2. Devise a strategy for filling in all of the missing values in the dataset.
3. Create a new dataset that is equal to the original dataset but with the missing data filled in.
4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. 


Calculate the total of NA values in the original dataset

```r
total_NA <- sum(is.na(activitydata))
```

```
## Error in eval(expr, envir, enclos): object 'activitydata' not found
```

```r
total_NA 
```

```
## Error in eval(expr, envir, enclos): object 'total_NA' not found
```

Filling in all of the missing values with the **average interval value across all days**

```r
na_index <-  which(is.na(activitydata))
```

```
## Error in which(is.na(activitydata)): object 'activitydata' not found
```

```r
activity_imputed <- meansteps[as.character(activitydata[na_index,3])]
```

```
## Error in eval(expr, envir, enclos): object 'meansteps' not found
```

```r
names(activity_imputed) <- na_index
```

```
## Error in eval(expr, envir, enclos): object 'na_index' not found
```

```r
for (i in na_index) {
    activitydata$steps[i] = activity_imputed[as.character(i)]
}
```

```
## Error in eval(expr, envir, enclos): object 'na_index' not found
```

```r
sum(is.na(activitydata)) 
```

```
## Error in eval(expr, envir, enclos): object 'activitydata' not found
```

```r
totalsteps <- tapply(activitydata$steps, activitydata$date,sum)
```

```
## Error in tapply(activitydata$steps, activitydata$date, sum): object 'activitydata' not found
```

```r
hist(totalsteps,col="red",xlab="Total Steps per Day", 
      ylab="Frequency", main="Histogram of Total Steps taken per day")
```

```
## Error in hist(totalsteps, col = "red", xlab = "Total Steps per Day", ylab = "Frequency", : object 'totalsteps' not found
```


## Are there differences in activity patterns between weekdays and weekends?
1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.
2. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).


```r
days <- weekdays(activitydata$date)
```

```
## Error in weekdays(activitydata$date): object 'activitydata' not found
```

```r
activitydata$day_type <- ifelse(days == "Saturday" | days == "Sunday", 
                                "Weekend", "Weekday")
```

```
## Error in ifelse(days == "Saturday" | days == "Sunday", "Weekend", "Weekday"): object 'days' not found
```

```r
meansteps <- aggregate(activitydata$steps,
                                    by=list(activitydata$interval,
                                            activitydata$day_type),mean)
```

```
## Error in aggregate(activitydata$steps, by = list(activitydata$interval, : object 'activitydata' not found
```

```r
names(meansteps) <- c("interval","day_type","steps")
```

```
## Error in names(meansteps) <- c("interval", "day_type", "steps"): object 'meansteps' not found
```

```r
xyplot(steps~interval | day_type, meansteps,type="l",
       layout=c(1,2),xlab="Interval",ylab = "Number of steps")
```

```
## Error in eval(substitute(groups), data, environment(x)): object 'meansteps' not found
```
Computing the mean, median, max and min of the steps across all intervals and Weekdays/Weekends

```r
tapply(meansteps$steps,meansteps$day_type,
       function (x) { c(MINIMUM=min(x),MEAN=mean(x),
                        MEDIAN=median(x),MAXIMUM=max(x))})
```

```
## Error in tapply(meansteps$steps, meansteps$day_type, function(x) {: object 'meansteps' not found
```
The end!
