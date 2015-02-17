Code Book for Getting and Cleaing Data course project
------
This CodeBook explains the steps and variables in run_analysis.R file

1. There are three packages need to be installed and loaded are **tidyr**, **dplyr** and **data.table**

2. Load the feature data in `features.txt`. Its data are measure names and used as column names

3. Load training and test sets (measure data) in `X_test.txt` and `X_train.txt` (training set first, and test set after), then combine them into **x_data** variable. At the same time, appropriately labels the data set with descriptive variable names by set the column names to features$Feature

4. Load training and test labels (activity label codes) in `y_test.txt` and `y_train.txt`, then combine them into **y_data**
5. Load test and train subjects in `subject_test.txt` and `subject_train.txt`, then combine them into **subject_data**
6. Load the class labels and their linked activity names in `activity_labels.txt`
7. Decode activity labels in **y_data** with activity names. In other words, we use activity labels and their names in **subject_data** data to rename the activity labels in **y_data** 

8. Extracts only the measurements on the mean `mean` and standard deviation `std` for each measurement. Check case sensitive by using argument `ignore.case=FALSE`

9. Merges the subject, y data, std and mean training and the test sets to create one data set. Then, covert the data frame to data table class. We can do it by using `cbind`, `%>%` and `as.data.table` 

10. Lastly, we creat tidy data **tidydataset** with the average of each variable for each activity and each subject using `lapply(.SD,mean)`. Reorder them based on Subject `setorder(Subject)` and Gather them based on Subject and Activity `gather(Measure,Mean_value,-Subject,-Activity)`. We use `%>%` to chain all the results
