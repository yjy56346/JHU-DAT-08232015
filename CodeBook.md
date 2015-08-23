---
title: "Getting and Cleaning Data - Course Project"
author: "Y.J. Yoon"
date: "August 22, 2015"
output: html_document
---

 You should create one R script called run_analysis.R that does the following:
 
 a) Merges the training and the test sets to create one data set.
 
 b) Extracts only the measurements on the mean and standard deviation for each 
    measurement.
    
 c) Uses descriptive activity names to name the activities in the data set.
 
 d) Appropriately labels the data set with descriptive variable names.
 
 e) From the data set in step d), creates a second, independent tidy data set 
    with the average of each variable for each activity and each subject.
    

```{r}
setwd("~/JHU EMBA/R/#3 - DAT/UCI HAR Dataset")
library(dplyr)
```

 [a] read TRAIN & TEST data, bind them, and mutate them by adding activity label
 
   [a-1] y_train/test.txt: explained in "activity_labels.txt"
   
    1 = WALKING, 2 = WALKING_UPSTAIRS, 3 = WALKING_DOWNSTAIRS,
    4 = SITTING, 5 = STANDING,         6 = LAYING

```{r}
activityLabels <- read.table("activity_labels.txt", stringsAsFactors = FALSE)

actNum <- 
    read.table("train/y_train.txt", col.names=c("ACTIVITY_NUM"), 
               stringsAsFactors = FALSE) %>%
    rbind(read.table("test/y_test.txt", col.names=c("ACTIVITY_NUM"), 
                     stringsAsFactors = FALSE))

activities <- mutate(actNum, ACTIVITY=activityLabels[actNum[,1],2])
```

   [a-2] subject_train/test.txt: each row indicated the subject {1 ... 30} measured
   
```{r}
subNum <- 
    read.table("train/subject_train.txt", col.names=c("SUBJECT_NUM"), 
                     stringsAsFactors = FALSE) %>%
    rbind(read.table("test/subject_test.txt", col.names=c("SUBJECT_NUM"), 
                     stringsAsFactors = FALSE))
```

   [a-3] X_train/test.txt: explained in "features.txt" & "features_info.txt"
   
```{r}
featureLabels <- read.table("features.txt", stringsAsFactors = FALSE)
```

    use the 2nd column of the featureLabels to name columns, so that we can use
    dplyr's substring select method to filter out "mean()" and "std()" columns
  
```{r}
datDF <- 
    read.table("train/X_train.txt", col.names= featureLabels[,2], 
               stringsAsFactors = FALSE) %>%
    rbind(read.table("test/X_test.txt", col.names= featureLabels[,2], 
                     stringsAsFactors = FALSE))
```

 [b] extract out the rows that have "mean()" or "std()" in it's name using dplyr!
 
    colNums <- c(1, 2, 3, 4, 5, 6, 41, 42, 43, 44, 45, 46, 81, 82, 83, 84, 85, 86,
             ..., 503, 504, 516, 517, 529, 530, 542, 543) = length of 66!

```{r}
datDF2 <- select(datDF, contains(".mean.."), contains(".std.."))
print(length(datDF2))
```

 [b-1] extract only MEAN/STD columns as listed in features.txt using dplyr select!
 
    1 tBodyAcc-mean()-X, 2 tBodyAcc-mean()-Y, 3 tBodyAcc-mean()-Z,
    ..., 542 fBodyBodyGyroJerkMag-mean(), 543 fBodyBodyGyroJerkMag-std()
    
    Row 119 - 53 = 66 rows => column selection of "-mean()" or "-std()" entries
    
 [c] Uses descriptive activity names to name the activities in the data set.
   -> done above...
   
 [d] Appropriately labels the data set with descriptive variable names.
   -> done above...
   
   [d-1] Let's combine the "subNum", "activities", and "datDF2"
   
```{r}
cData <- subNum %>% cbind(activities) %>% cbind(datDF2)
```

 [e] From the data set in step [d], creates a second, independent tidy data set 
   with the average of each variable for each activity and each subject.
   
   [e-1] Using "cData" group_by their "SUBJECT_NUM" and "ACTIVITY_NUM" and
        summarize_each columns using mean function per SUBJECT/ACTIVITY! ;-)
        
    NOTE: unfortunately, I have to remove ACTIVITY column to apply "mean"
    functions to all "-mean()" and "-std()" columns!
    
```{r}
cDatSubAct <- group_by(select(cData, -ACTIVITY), SUBJECT_NUM, ACTIVITY_NUM) %>% 
    summarise_each(funs(mean))
```


