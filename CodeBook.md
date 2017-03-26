---
title: "CodeBook"
author: "Joe Logan"
date: "March 25, 2017"
output: html_document
---

#### Referring to R code: run_analysis.R
#### Author: Joe Logan
#### Date 23-March 2017
#### Assignment: Getting and Cleaning Data - Course Project
#### More information regarding the data is found in the README.md

###_____________________________________________________________________
### STEP 0 > PREWORK GETTING ALL DATA INTO THEIR OWN DATAFRAMES
#### Create the data directory if it doesn't already exist
if(!file.exists("./data")){dir.create("./data")}

#### Store the URL in a variable and download into the data directory file_Url
#### Only do it once...
if(!file.exists("./data/Dataset.zip")) {
        file_Url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(file_Url,destfile="./data/Dataset.zip")
#### In code, unzip the contents and place in the data directory
unzip(zipfile="./data/Dataset.zip",exdir="./data")
}


#### Load variables but only once
if(!exists("features_Training_Data") || !exists("features_Test_Data") || !exists("activity_Training_Data") || 
           !exists("activity_Test_Data") || !exists("sub_Training_Data") || !exists("sub_Test_Data")) {
        ## Read in tables
        # Reading Feature Files
        features_Training_Data  <- read.table("./data/UCI HAR Dataset/train/X_train.txt", header = FALSE)
        features_Test_Data <- read.table("./data/UCI HAR Dataset/test/X_test.txt", header = FALSE)
        
        # Reading Activity Files
        activity_Training_Data <- read.table("./data/UCI HAR Dataset/train/Y_train.txt", header = FALSE)
        activity_Test_Data  <- read.table("./data/UCI HAR Dataset/test/Y_test.txt", header = FALSE)
        
        # Reading subject files
        sub_Training_Data <- read.table("./data/UCI HAR Dataset/train/subject_train.txt", header = FALSE)
        sub_Test_Data <- read.table("./data/UCI HAR Dataset/test/subject_test.txt", header = FALSE)
}


###_____________________________________________________________________
### STEP 1 > MERGE THE TRAINING AND THE TEST SETS TO CREATE ONE DATA SET

#### -- Using a simple method of placing the files in order in rbind we are hoping that they line up
allFeaturesData <- rbind(features_Training_Data, features_Test_Data)
allActivityData <- rbind(activity_Training_Data, activity_Test_Data)
allSubjectData <- rbind(sub_Training_Data, sub_Test_Data)

#### Assigne column names now < simple enough only one column
names(allSubjectData)<-c("subject")
names(allActivityData)<- c("activity")
#### Features column assignment a bit more difficult, but not too bad.  Just need to read it and set it
featureColumnNames <- read.table("./data/UCI HAR Dataset/features.txt",header = FALSE)
names(allFeaturesData) <- featureColumnNames$V2 #V1 is just the identifier
featuresVariableNames <- names(allFeaturesData) # < will need this later

#### Now we will combine everything into one NOT YET tidy table via column extension
tmpCombinedSubAct <- cbind(allSubjectData,allActivityData)
allData <- cbind(tmpCombinedSubAct,allFeaturesData)

###_____________________________________________________________________
### STEP 2 > EXTRACT ONLY THE MEASUREMENTS ON THE MEAN AND STANDARD DEVIATION FOR EACH MEASUREMENT

#### Here we are searching via regular expression for any column names with mean or std in them
#### Will give a subset of columns from 561 to 66 (Note that we do not want gravitymean!)
onlyMeanAndStdColumnNames <- featuresVariableNames[grep("mean\\(\\)|std\\(\\)", featuresVariableNames)]
### adding the subject and activity cols)
SubActMeanStdColumns <- c("subject", "activity",as.character(onlyMeanAndStdColumnNames))

#### Finally we can now eliminate unwanted columns (information) from the dataset
allData <- subset(allData,select=SubActMeanStdColumns)


###_____________________________________________________________________
### STEP 3 > USES DESCRIPTIVE ACTIVITY NAMES TO NAME THE ACTIVITIES IN THE DATA SET

#### Get the formal names from the activity labels file
activityLabels <- read.table("./data/UCI HAR Dataset/activity_labels.txt" ,header = FALSE)
#### Factor/replace the numerical identifiers with the actual activity names
allData$activity <- factor(allData$activity, labels = as.character(activityLabels[,2]))


###_____________________________________________________________________
### STEP 4 > APPROPRIATELY LABELS THE DATA SET WITH DESCRIPTIVE VARIABLE NAMES

#### Remove the dashes and replace with more readable spaces
names(allData)<-gsub("-X", " X", names(allData))
names(allData)<-gsub("-Y", " Y", names(allData))
names(allData)<-gsub("-Z", " Z", names(allData))

#### Remove function params
names(allData)<-gsub("\\(\\)", "", names(allData))
#### This is a duplicate
names(allData)<-gsub("BodyBody", "Body", names(allData))

#### Remove Abbreviations
names(allData)<-gsub("^t", "Time-", names(allData)) 
names(allData)<-gsub("^f", "Frequency-", names(allData))
names(allData)<-gsub("Body", "Body ", names(allData))
names(allData)<-gsub("Gravity", "Gravity ", names(allData))
names(allData)<-gsub("Acc", "Accelerometer ", names(allData))
names(allData)<-gsub("Gyro", "Gyroscope ", names(allData))
names(allData)<-gsub("Mag", "Magnitude ", names(allData))
names(allData)<-gsub("Jerk", "Jerk ", names(allData))
names(allData)<-gsub("std", "Standard Deviation ", names(allData))

#### Remove dashes and double spaces
names(allData)<-gsub("-", " ", names(allData))
names(allData)<-gsub("  ", " ", names(allData))
names(allData)<-gsub("  ", " ", names(allData)) #May have been 4 spaces in a row - need to run twice

#### Misc Cleanup
names(allData)<-gsub("^\\s+|\\s+$", "", names(allData)) #Remove leading and trailing spaces
names(allData)<-gsub("mean", "Mean", names(allData)) #Capitalize mean for consistancy

###_____________________________________________________________________
### STEP 5 > FROM THE DATA SET IN STEP 4, CREATE A SECOND, INDEPENDENT TIDY DATA SET WITH THE AVERAGE
### OF EACH VARIABLE FOR EACH ACTIVITY AND EACH SUBJECT

#### Create function for use in ddply to get all means except subj and act
tmpColMeans <- function(theData) { colMeans(theData[,-c(1,2)]) }
tidyMeans <- ddply(allData, .(subject, activity), tmpColMeans)

#### Need to identify the values as mean values now via column identification
names(tidyMeans)[-c(1,2)] <- paste0("Mean ", names(tidyMeans)[-c(1,2)])

#### Create tidy dataset from step 5
write.table(tidyMeans, file="TidyData.txt", row.names=FALSE, col.names=TRUE, sep="\t", quote=TRUE)
