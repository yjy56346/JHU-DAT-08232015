---
title: "Getting and Cleaning Data - Course Project: JHU-DAT-08232015"
author: "Y.J. Yoon"
date: "August 22, 2015"
output: html_document
---

 One R script called run_analysis.R does the following:
 
 a) Merges the training and the test sets to create one data set.
 
 b) Extracts only the measurements on the mean and standard deviation for each 
    measurement.
    
 c) Uses descriptive activity names to name the activities in the data set.
 
 d) Appropriately labels the data set with descriptive variable names.
 
 e) From the data set in step d), creates a second, independent tidy data set 
    with the average of each variable for each activity and each subject.
    
Background on the data is as follows from the data provider's readme:
=====================================================================

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

For each record it is provided:
======================================

- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

The dataset used includes the following files:
==============================================

- 'README.txt'

- 'features_info.txt': Shows information about the variables used on the feature vector.

- 'features.txt': List of all features.

- 'activity_labels.txt': Links the class labels with their activity name.

- 'train/X_train.txt': Training set.

- 'train/y_train.txt': Training labels.

- 'test/X_test.txt': Test set.

- 'test/y_test.txt': Test labels.

The following files are available for the train and test data. Their descriptions are equivalent. 

- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 


Notes: 
======
- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.


```{r}
source("run_analysis.R")
```
    
    Entire course project can be run by sourcing the R script file 
    with "features.txt", "activity_labels.txt" in the working directory
    plus subdirectories "train" and "test" containing the following files:
        train\  y_train.txt,    X_train.txt, and    subject_train.txt
        test\   y_test.txt,     X_test.txt, and     subject_test.txt
        
    Above code will generate the following two tidy data files:
        tidyDataSet.txt
        tidyDataSetMeansGroupBySubActivities.txt
        
    

License:
========
Use of this dataset in publications must be acknowledged by referencing the following publication [1] 

[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

This dataset is distributed AS-IS and no responsibility implied or explicit can be addressed to the authors or their institutions for its use or misuse. Any commercial use is prohibited.

Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012.
