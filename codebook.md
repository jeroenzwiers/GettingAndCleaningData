---
title: "CodeBook"
author: "Jeroen Zwiers"
date: "19 juli 2019"
output:
  md_document:
    variant: markdown_github
---

## Codebook Getting and Cleaning Data Course Project

#Introduction
This codebook describes the variables, the data, and the transformations and work that I performed to clean up the data.

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.

The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

tBodyAcc-XYZ
tGravityAcc-XYZ
tBodyAccJerk-XYZ
tBodyGyro-XYZ
tBodyGyroJerk-XYZ
tBodyAccMag
tGravityAccMag
tBodyAccJerkMag
tBodyGyroMag
tBodyGyroJerkMag
fBodyAcc-XYZ
fBodyAccJerk-XYZ
fBodyGyro-XYZ
fBodyAccMag
fBodyAccJerkMag
fBodyGyroMag
fBodyGyroJerkMag

The set of variables that were estimated from these signals are: 

mean(): Mean value
std(): Standard deviation
mad(): Median absolute deviation 
max(): Largest value in array
min(): Smallest value in array
sma(): Signal magnitude area
energy(): Energy measure. Sum of the squares divided by the number of values. 
iqr(): Interquartile range 
entropy(): Signal entropy
arCoeff(): Autorregresion coefficients with Burg order equal to 4
correlation(): correlation coefficient between two signals
maxInds(): index of the frequency component with largest magnitude
meanFreq(): Weighted average of the frequency components to obtain a mean frequency
skewness(): skewness of the frequency domain signal 
kurtosis(): kurtosis of the frequency domain signal 
bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
angle(): Angle between to vectors.

Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:

gravityMean
tBodyAccMean
tBodyAccJerkMean
tBodyGyroMean
tBodyGyroJerkMean

# 1. Merging the training and the test sets to create one data set.

To get the raw data, I downloaded the zipfile

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

I used the following files for getting the data:

* subject_train.txt (I made the variable training_subjects which is a list of 7352 observations of 1 variable). This is a list of subject ids of the volunteers

* X_train.txt (I made the variable training_normalized_observations which is a list of 7352 observations 0f 81 variables which are measured by the smartphone)

* Y_train.txt (I made the variable training_labels which is a list of 7352 observations of 1 variable). This is a list of activity_ids. These id's match with the data in activity_labels.txt.

* subject_test.txt (I made the variable test_subjects which is a list of 2947 observations of 1 variable). This is a list of subject ids of the volunteers

* X_test.txt (I made the variable test_normalized_observations which is a list of 2947 observations 0f 81 variables which are measured by the smartphone)

* Y_test.txt (I made the variable test_labels which is a list of 2947 observations of 1 variable). This is a list of activity_ids. These id's match with the data in activity_labels.txt.

X_train and X_test contain of 81 variables, which were measured per subject and per activity.
 
I merged the data of the testgroup and the traininggroup to get the following datatables:

* total_labels, which is the result of the merge of the lists training_labels and test_labels
* total_subjects,which is the result of the merge of the lists training_subjects and test_subjects
* total_normalized_observations, which is the result of the merg of the lists training_normalized_observations and test_normalized observations

The columnname of total_subjects was changed from "V1" to "subject_id".
The columnname of total_labels was changed from "V1" to "activity".
The columnnames of total_normalized_observations were replaced by the vector of feature_names.txt, column "V2".

The lists total_subjects, total_labels and total_normalized_observations were combined to get the new dataframe "total data".

# 2. Extracting only the measurements on the mean and standard deviation for each measurement.
With Regex statement grepl("subject_id|activity|mean|std", names(total_data)) I made a logical vector, with which I filtered the columns. So the dataframe "total data" only contains the colums subject_id, activity and all the columns which give information about the mean and standard deviation.

# 3. Use descriptive activity names to name the activities in the data set
As it says in activity_labels.txt, the activity ids correspond to the following:

1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING

In the column "activity", the numbers were replaced with the strings. 

# 4. Appropriately labeling the data set with descriptive variable names.

The following actions were taken on the colomn names of the dataframe total_data:

* I replaced "-" by " "
* I replaced "mean()" by "Mean"
* I replaced "std()" by "Standard Deviation"
* If the column name started with a "t" I replaced this by "Time"
* If the column name started with a "f" I replaced this by "Frequency domain signals"
* I replaced "Acc" by "Acceleration"
* I replaced "Jerk" by "Jerk Signals"
* I replaced "Mag" by "Magnitude"
* I replaced "Gyro" by "Gyroscope"
* I replaced "Freq" by "Frequency"
* I replaced "()" by ""
* I trimmed the first empty spaces of each columnname.



# 5. Creating a second, independent tidy data set with the average of each variable for each activity and each subject.
A new tidy dataset, tidydata.txt, was created by getting the mean of each variable, grouping by subject_id and activity. After that I ordered by subject_id and activity. 
