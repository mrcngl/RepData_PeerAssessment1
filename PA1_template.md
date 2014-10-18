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









