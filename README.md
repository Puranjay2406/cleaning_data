#read data
data.test.x <- read.table('X_test.txt')
data.test.activity <- read.csv('y_test.txt', header = FALSE, sep = ' ')
data.test.subject <- read.csv('subject_test.txt',header = FALSE, sep = ' ')

data.test <-  data.frame(data.test.subject, data.test.activity, data.test.x)

data.train.x <- read.table('X_train.txt')
data.train.activity <- read.csv('y_train.txt', header = FALSE, sep = ' ')
data.train.subject <- read.csv('subject_train.txt',header = FALSE, sep = ' ')

data.train <-  data.frame(data.train.subject, data.train.activity, data.train.x)
#combine Data
train_test<-rbind(data.train,data.test)
#add headings and select mean and std
name<-read.csv("features.txt",header = FALSE,sep = " ")
names(train_test)<-c(c("subject","activity"),name)
x<-grep('std|mean',names(train_test))
poi<-train_test[,c(1,2,x)]
#meaning of activity
poi$activity<-gsub("5","STANDING",poi$activity)
poi$activity<-gsub("6","LAYING",poi$activity)
poi$activity<-gsub("4","SITTING",poi$activity)
poi$activity<-gsub("3","WALKING_DOWNSTAIRS",poi$activity)
poi$activity<-gsub("2","WALKING_UPSTAIRS",poi$activity)
poi$activity<-gsub("1","WALKING",poi$activity)
#clean names(poi) to readable format
x<-names(poi)
x<-gsub("[(][)]", "",x)
x<-gsub("Acc", "Accleration",x)
x<-gsub("Gyro", "Gyroscope",x)
x<-gsub("Mag", "Magnitude",x)
x<-gsub("Freq", "Frequency",x)
x<-gsub("std", "standard_deviation",x)
x<-gsub("^t", "Time_Domain",x)
x<-gsub("^f", "Frequency_Domain",x)
names(poi)<-x
z<-aggregate(poi[,4:81], by = list(activity = poi$activity, subject = poi$subject),FUN = mean)
write.table(z,"data_cleaned.txt", row.names = FALSE)



