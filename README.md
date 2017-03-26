### Getting-and-Cleaning-Data-Course-Project
This is the final project for the Getting and Cleaning Data course
Author: Joe Logan
Version 1.0
Date: 25-March-2017

#### Introduction
The purpose of this project is to demonstrate the ability to aggregate normalized data into a single dataset to allow for easier analysis and statistical processing.  The data example used was acceleration information gathered from several Samsung Galaxy S II smartphones attached to the waste of 30 people with embedded acceleraometers and gyroscopes.

#### Activities
The assignment had 5 basic tasks.
1. Merge the training and the test sets to create one data set.
2. Extract only the measurements on the mean and standard deviation for each measurement.
3. Use descriptive activity names to name the activities in the data set
4. Appropriately label the data set with descriptive variable names.
5. From the data set in step 4, create a second, independent tidy data set with the average of each variable for each activity and each subject.

#### Data
The data was gathered from an archive at the Donald Bren School of Information and Computer Sciences and can be downloaded [here](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip "Zipped Data").
The file contained  the following
* activity_labels.txt
* reatures.txt
* features_info.txt
* README.txt
* test
        + subject_test.txt
        + X_test.txt
        + Y_test.txt
* train
        + subject_train.txt
        + X_train.txt
        + Y_train.txt
        
The result of the aggregation exercise was a dataset that contained observations of 30 individuals.  3-axis (x,y,z) data was collected across 561 variables in an effort to identify if the person was 
1. WALKING
2. WALKING_UPSTAIRS
3. WALKING_DOWNSTAIRS
4. SITTING
5. STANDING
6. LAYING
For this exercise we only collected things having to do with the mean or standard deviation.  Here were the resulting 68 varables that were used.

The following lines describe each variable, column number, type of data and range of data for the tidy dataset file.
------------------------
 [1] "subject"                                                       
 [2] "activity"                                                      
 [3] "Time Body Accelerometer Mean X"                                
 [4] "Time Body Accelerometer Mean Y"                                
 [5] "Time Body Accelerometer Mean Z"                                
 [6] "Time Body Accelerometer Standard Deviation X"                  
 [7] "Time Body Accelerometer Standard Deviation Y"                  
 [8] "Time Body Accelerometer Standard Deviation Z"                  
 [9] "Time Gravity Accelerometer Mean X"                             
[10] "Time Gravity Accelerometer Mean Y"                             
[11] "Time Gravity Accelerometer Mean Z"                             
[12] "Time Gravity Accelerometer Standard Deviation X"               
[13] "Time Gravity Accelerometer Standard Deviation Y"               
[14] "Time Gravity Accelerometer Standard Deviation Z"               
[15] "Time Body Accelerometer Jerk Mean X"                           
[16] "Time Body Accelerometer Jerk Mean Y"                           
[17] "Time Body Accelerometer Jerk Mean Z"                           
[18] "Time Body Accelerometer Jerk Standard Deviation X"             
[19] "Time Body Accelerometer Jerk Standard Deviation Y"             
[20] "Time Body Accelerometer Jerk Standard Deviation Z"             
[21] "Time Body Gyroscope Mean X"                                    
[22] "Time Body Gyroscope Mean Y"                                    
[23] "Time Body Gyroscope Mean Z"                                    
[24] "Time Body Gyroscope Standard Deviation X"                      
[25] "Time Body Gyroscope Standard Deviation Y"                      
[26] "Time Body Gyroscope Standard Deviation Z"                      
[27] "Time Body Gyroscope Jerk Mean X"                               
[28] "Time Body Gyroscope Jerk Mean Y"                               
[29] "Time Body Gyroscope Jerk Mean Z"                               
[30] "Time Body Gyroscope Jerk Standard Deviation X"                 
[31] "Time Body Gyroscope Jerk Standard Deviation Y"                 
[32] "Time Body Gyroscope Jerk Standard Deviation Z"                 
[33] "Time Body Accelerometer Magnitude Mean"                        
[34] "Time Body Accelerometer Magnitude Standard Deviation"          
[35] "Time Gravity Accelerometer Magnitude Mean"                     
[36] "Time Gravity Accelerometer Magnitude Standard Deviation"       
[37] "Time Body Accelerometer Jerk Magnitude Mean"                   
[38] "Time Body Accelerometer Jerk Magnitude Standard Deviation"     
[39] "Time Body Gyroscope Magnitude Mean"                            
[40] "Time Body Gyroscope Magnitude Standard Deviation"              
[41] "Time Body Gyroscope Jerk Magnitude Mean"                       
[42] "Time Body Gyroscope Jerk Magnitude Standard Deviation"         
[43] "Frequency Body Accelerometer Mean X"                           
[44] "Frequency Body Accelerometer Mean Y"                           
[45] "Frequency Body Accelerometer Mean Z"                           
[46] "Frequency Body Accelerometer Standard Deviation X"             
[47] "Frequency Body Accelerometer Standard Deviation Y"             
[48] "Frequency Body Accelerometer Standard Deviation Z"             
[49] "Frequency Body Accelerometer Jerk Mean X"                      
[50] "Frequency Body Accelerometer Jerk Mean Y"                      
[51] "Frequency Body Accelerometer Jerk Mean Z"                      
[52] "Frequency Body Accelerometer Jerk Standard Deviation X"        
[53] "Frequency Body Accelerometer Jerk Standard Deviation Y"        
[54] "Frequency Body Accelerometer Jerk Standard Deviation Z"        
[55] "Frequency Body Gyroscope Mean X"                               
[56] "Frequency Body Gyroscope Mean Y"                               
[57] "Frequency Body Gyroscope Mean Z"                               
[58] "Frequency Body Gyroscope Standard Deviation X"                 
[59] "Frequency Body Gyroscope Standard Deviation Y"                 
[60] "Frequency Body Gyroscope Standard Deviation Z"                 
[61] "Frequency Body Accelerometer Magnitude Mean"                   
[62] "Frequency Body Accelerometer Magnitude Standard Deviation"     
[63] "Frequency Body Accelerometer Jerk Magnitude Mean"              
[64] "Frequency Body Accelerometer Jerk Magnitude Standard Dev"
[65] "Frequency Body Gyroscope Magnitude Mean"                       
[66] "Frequency Body Gyroscope Magnitude Standard Deviation"         
[67] "Frequency Body Gyroscope Jerk Magnitude Mean"                  
[68] "Frequency Body Gyroscope Jerk Magnitude Standard Deviation"   