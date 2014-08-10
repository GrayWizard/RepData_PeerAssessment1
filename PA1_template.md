# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
Unzip the provided data file

```r
unzip("activity.zip")
```
Load the data

```r
data <- read.csv("activity.csv")
```
Convert the **date** feature from *factor* into a *Date* and the **interval** feature from *int* to *factor*

```r
data <- transform(data,date=as.Date(date),interval=as.factor(interval))
```
## What is mean total number of steps taken per day?
Calculate the total number of steps per day

```r
steps_per_day <- aggregate(list(steps=data$steps),by=list(date=data$date),FUN=sum,na.rm=TRUE)
```
Plot the histogram 

```r
hist(steps_per_day$steps,main="Total number of steps per day",xlab="Steps")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 


Calculate the mean number of steps per day

```r
mean(steps_per_day$steps,na.rm=TRUE)
```

```
## [1] 9354
```
Calculate the median number of steps per day

```r
median(steps_per_day$steps,na.rm=TRUE)
```

```
## [1] 10395
```
## What is the average daily activity pattern?
Calculate the average number of steps for each interval accross all days

```r
avg_steps_per_interval <- aggregate(list(steps=data$steps),by=list(interval=data$interval),FUN=mean,na.rm=TRUE)
```
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
library(lattice)
xyplot(steps~interval,data=avg_steps_per_interval,type="l",main="Average number of steps per interval")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


Find the interval with the maximum number of steps

```r
avg_steps_per_interval$interval[which.max(avg_steps_per_interval$steps)]
```

```
## [1] 835
## 288 Levels: 0 5 10 15 20 25 30 35 40 45 50 55 100 105 110 115 120 ... 2355
```
## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
