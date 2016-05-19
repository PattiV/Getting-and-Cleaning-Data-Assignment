##Getting-and-Cleaning-Data-Assignment
###Title: Getting and Cleaning Data Course Project
###Data: Human Activity Recognition Using Smartphones Dataset 
###Data Source: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
###Project Goal: Prepare a tidy dataset that can be used for later analyses
###Date Completed: May 19, 2016 by PV

## R Script Steps:
### 1. Merges the training and the test sets to create one data set
###	2. Extracts only the measurement on the mean and SD for each measurement
###	3. Uses descriptive activity names to name the activities in the data set
###	4. Appropriately labels the data set with descriptive variable names
###	5. From the data set in step 4, creates a second, independent tidy data set with the average of each variables for each activity and each subject

#### Step 1: Merge (stack) the Training and the Test sets to create one data set
##### 30% of subjects are in test data and 70% in training set (same 561 columns, so can rbind)

###### train <- read.table("./UCI Har Dataset/train/X_train.txt", header=F)
###### test <- read.table("./UCI Har Dataset/test/X_test.txt", header=F)
###### test_train <- rbind(test,train)
###### dim(test_train)
###### head(test_train)

#### Step 2: Add column names to merged data so can keep only Mean and SD for each measurement
#### Use data "'features.txt': List of all features" for the 561 column names

###### feat <- read.table("./UCI Har Dataset/features.txt", header=F)
###### dim(feat)
###### head(feat)
###### names(test_train)=feat$V2

#### Keep variables that are means or std (note: was getting a duplicate col name error so deleted dups)
###### U <- test_train[ , !duplicated(colnames(test_train))]
###### mean <- select(U, contains("mean"))
###### std <- select(U, contains("std"))
###### mean_std <- cbind(mean,std)

#### Step 3: Use Descriptive Activity Names for the vars in the Activities Data Set

###### act_labels <- read.table("./UCI Har Dataset/activity_labels.txt",col.names=c("activity_id","activity_name"))
###### train_labels <- read.table("./UCI Har Dataset/train/y_train.txt",col.names="activity_id")
###### test_labels <- read.table("./UCI Har Dataset/test/y_test.txt",col.names="activity_id")

#### Stack activity test and train data, then merge in activity names
###### act_all <- rbind(test_labels,train_labels)
###### act_desc <- full_join(act_all,act_labels) #Joining by: "activity_id"

#### Stack subject train and subject test data sets then add activities
###### sub_train <- read.table("./UCI Har Dataset/train/subject_train.txt",col.names="subject_id")
###### sub_test <- read.table("./UCI Har Dataset/test/subject_test.txt", col.names="subject_id")
###### all_sub <- rbind(sub_test,sub_train)
###### all_sub_act <-cbind(all_sub,act_desc)

#### Step 4: Appropriately labels the data set with descriptive variable names
#### Combine subject activity ids with measurement mean and SDs
###### all_data <- cbind(all_sub_act,mean_std)

#### Step5 Create a Second Independent Data Set With the Average of Each Variable for each activity and subject
###### group <- group_by(all_data,subject_id, activity_name)
###### avg_subject_activity<-summarise_each(group, funs(mean))

#### Outputting Tidy DataSets
###### write.table(all_data, row.name=FALSE, "tidy_data_all.txt")
###### write.table(avg_subject_activity, row.name=FALSE, "avg_subject_activity.txt")

Data Source Citation:

Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. A Public Domain Dataset for Human Activity Recognition Using Smartphones. 21th European Symposium on Artificial Neural Networks, Computational Intelligence and Machine Learning, ESANN 2013. Bruges, Belgium 24-26 April 2013.
