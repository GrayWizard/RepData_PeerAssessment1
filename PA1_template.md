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
Convert the **date** feature from *factor* into a *Date*

```r
data <- transform(data,date=as.Date(date))
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
avg_steps_per_interval <- aggregate(list(avg_steps=data$steps),by=list(interval=data$interval),FUN=mean,na.rm=TRUE)
```
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
library(lattice)
xyplot(avg_steps~interval,data=avg_steps_per_interval,type="l",main="Average number of steps per interval",)
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


Find the interval with the maximum number of steps

```r
avg_steps_per_interval$interval[which.max(avg_steps_per_interval$avg_steps)]
```

```
## [1] 835
```
## Imputing missing values
Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
sum(is.na(data))
```

```
## [1] 2304
```
Fill in the NAs

```r
data_nona <- merge(data,avg_steps_per_interval,by.x="interval",by.y="interval")
data_nona$steps[which(is.na(data_nona$steps))] <- data_nona$avg_steps[which(is.na(data_nona$steps))]
data_nona <- data_nona[,1:3]
```
Calculate the total number of steps per day

```r
steps_per_day <- aggregate(list(steps=data_nona$steps),by=list(date=data_nona$date),FUN=sum,na.rm=TRUE)
```
Plot the histogram 

```r
hist(steps_per_day$steps,main="Total number of steps per day",xlab="Steps")
```

![plot of chunk unnamed-chunk-14](figure/unnamed-chunk-14.png) 


Calculate the mean number of steps per day

```r
mean(steps_per_day$steps,na.rm=TRUE)
```

```
## [1] 10766
```
Calculate the median number of steps per day

```r
median(steps_per_day$steps,na.rm=TRUE)
```

```
## [1] 10766
```
## Are there differences in activity patterns between weekdays and weekends?
Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

```r
data_nona$we <- as.factor(ifelse(weekdays(data_nona$date) %in% c("Saturday","Sunday"), "Weekend", "Weekday")) 
```
Calculate the average number of steps for each interval accross all days

```r
avg_steps_per_interval <- aggregate(list(avg_steps=data_nona$steps),by=list(interval=data_nona$interval,we=data_nona$we),FUN=mean,na.rm=TRUE)
```
Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). The plot should look something like the following, which was creating using simulated data:

```r
library(lattice)
xyplot(avg_steps~interval|we,data=avg_steps_per_interval,type="l",main="Average number of steps per interval",layout = c(1, 2))
```

![plot of chunk unnamed-chunk-19](figure/unnamed-chunk-19.png) 

