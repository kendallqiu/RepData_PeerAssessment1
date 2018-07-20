Load and process data
=============================================
#coclass = c("integer", "character", "integer")
#data <- read.csv("activity.csv", head=TRUE, colClasses=coclass, na.strings="NA")
#head(data)
##  steps       date interval
##1    NA 2012-10-01        0
##2    NA 2012-10-01        5
##3    NA 2012-10-01       10
##4    NA 2012-10-01       15
##5    NA 2012-10-01       20
##6    NA 2012-10-01       25
#data$date <- as.Date(data$date)
#data_ign <- subset(data, !is.na(data$steps))
#dasum <- tapply(data_ign$steps, data_ign$date, sum, na.rm=TRUE, simplify=T)
#dasum <- dasum[!is.na(dasum)]
#hist(x=dasum, col="red", breaks=20, xlab="Daily total steps", ylab="Frequency",main="The distribution of daily total (missing data ignored)")
mean total of steps
======================================
#mean(dasum)
##[1] 10766.19
#median(dasum)
##[1] 10765
#int_avg <- tapply(data_ign$steps, data_ign$interval, mean, na.rm=TRUE, simplify=T)
#data_ia <- data.frame(interval=as.integer(names(int_avg)), avg=int_avg)
#with(data_ia, plot(interval, avg, type="l", xlab="5-minute intervals", ylab="average steps in the interval across all days"))
#max_steps <- max(data_ia$avg)
#data_ia[data_ia$avg == max_steps, ]
##    interval      avg
##835      835 206.1698
Imput missing values
==============================================
#sum(is.na(data$steps))
##[1] 2304
#data_impute <- data
#ndx <- is.na(data_impute$steps)
#int_avg <- tapply(data_ign$steps, data_ign$interval, mean, na.rm=TRUE, simplify=T)
#data_impute$steps[ndx] <- int_avg[as.character(data_impute$interval[ndx])]
#new_dasum <- tapply(data_impute$steps, data_impute$date, sum, na.rm=TRUE, simplify=T)
#hist(x=new_dasum, col="red", breaks=20, xlab="daily steps", ylab="frequency", main="The distribution of daily total (with missing data imputed)")
#mean(new_dasum)
##[1] 10766.19
#median(new_dasum)
##[1] 10766.19
differences between weekdays and weekends
=============================================================
#is_weekday <- function(d) {
#     wd <- weekdays(d)
#     ifelse (wd == "Saturday" | wd == "Sunday", "weekend", "weekday")
#} 
#wx <- sapply(data_impute$date, is_weekday)
#data_impute$wk <- as.factor(wx)
#head(data_impute)
##      steps       date interval      wk
##1 1.7169811 2012-10-01        0 weekday
##2 0.3396226 2012-10-01        5 weekday
##3 0.1320755 2012-10-01       10 weekday
##4 0.1509434 2012-10-01       15 weekday
##5 0.0754717 2012-10-01       20 weekday
##6 2.0943396 2012-10-01       25 weekday
#wk_data <- aggregate(steps ~ wk+interval, data=data_impute, FUN=mean)
#library(lattice)
#xyplot(steps ~ interval | factor(wk), layout = c(1, 2), xlab="Interval", ylab="Number of steps", type="l", lty=1, data=wk_data)
