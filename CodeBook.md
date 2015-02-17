Code Book for Getting and Cleaing Data course project
------
This CodeBook explains the steps and variables in run_analysis.R file

1. There are three packages need to be installed and loaded are **tidyr**, **dplyr** and **data.table**

2. Load the feature data in 'features.txt' file. Its data are measure names and used as column names

3. Load training and test sets in 'X_test.txt' and 'X_train.txt' (training set first, and test set after), then combine them into 'x_data' variable . At the same time, appropriately labels the data set with descriptive variable names by set the column names to features$Feature
x_test<-read.table("./test/X_test.txt",col.names=features$Feature)

x_train<-read.table("./train/X_train.txt",col.names=features$Feature)

x_data<-bind_rows(x_test,x_train)

#Read training and test labels
#Read test labels first, then train labels,and combine them in the same order
y_test<-read.table("./test/y_test.txt",col.names="Activity")

y_train<-read.table("./train/y_train.txt",col.names="Activity")

y_data<-bind_rows(y_test,y_train)

#Read test and train subjects
subject_test<-read.table("./test/subject_test.txt",col.names="Subject")

subject_train<-read.table("./train/subject_train.txt",col.names="Subject")

subject_data<- bind_rows(subject_test,subject_train)

#Read all the class labels and their linked activity names
activity_labels<-read.table("activity_labels.txt",col.names=c("Labels","Activity"),colClasses = c("integer","character"))

#Below for() loop is used to decode activity labels in y_data with activity names
#In other words, we use activity labels and their names in activity_labels data to rename the activity labels in y_data 
for (i in 1:6) {
  y_data$Activity[y_data$Activity==i]<-activity_labels$Activity[i]
}


#Extracts only the measurements on the mean and standard deviation for each measurement 

stddata<-select(x_data,contains("std",ignore.case=FALSE))
meandata<-select(x_data,contains("mean",ignore.case=FALSE))

#Merges the subject, y data, std and mean training and the test setsto create one data set
#Covert the data.frame to data.table
dataset<-cbind(subject_data,y_data,stddata,meandata) %>%
      as.data.table


#Creat Tidy data with the average of each variable for each activity and each subject
#Reorder them based on Subject
#Gather them based on Subject and Activity
tidydataset<-dataset[,lapply(.SD,mean),by=c("Subject","Activity")] %>%

          setorder(Subject) %>%

          gather(Measure,Mean_value,-Subject,-Activity) 


