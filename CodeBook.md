#read data
#combine Data
#meaning of activity
#clean names(poi) to readable format
#aggregate based on subject and activity
z<-aggregate(poi[,4:81], by = list(activity = poi$activity, subject = poi$subject),FUN = mean)
#write data
write.table(z,"data_cleaned.txt", row.names = FALSE)


