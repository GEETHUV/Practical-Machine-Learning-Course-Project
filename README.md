# Practical-Machine-Learning-Course-Project
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: http://web.archive.org/web/20161224072740/http:/groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset).
Data

The training data for this project are available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The test data are available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

The data for this project come from this source: http://web.archive.org/web/20161224072740/http:/groupware.les.inf.puc-rio.br/har.

LOADING THE DATA

> training<-read.csv("training.csv",sep="'",header=TRUE,na.strings=c("NA","",'#DIV/0!'))

> testing <- read.csv("testing.csv", sep=",", header=TRUE, na.strings = c("NA","",'#DIV/0!'))

To find the dimension of the data,

> dim(training)
[1] 19622     160
> dim(testing)
[1]  20 160


  
 The training data set is made of 19622 observations on 160 columns. We can see that many columns have NA values on almost every observation. So we have to remove them, because they will not produce any information. The first seven columns give information about the people who did the test, and also timestamps. We will not take them in our model.
 # DATA CLEANING
 
> indColToRemove <- which(colSums(is.na(training) |training=="")>0.9*dim(training)[1])

> TrainDataClean <- training[,-indColToRemove]

> TrainDataClean <- TrainDataClean[,-c(1:7)]

> dim(TrainDataClean)

 [1] 19622    53

Now we have to do the same on the test data set

 >> indColToRemove <- which(colSums(is.na(testing) |testing=="")>0.9*dim(testing)[1])

 > TestDataClean <- testing[,-indColToRemove]

 > TestDataClean <- TestDataClean[,-1]

 > dim(TestDataClean)

 [1] 20 59

After the cleaning process, the new training data set has only 53 columns.

# Summary of the data
            
> str(TestDataClean)

> str(TrainDataClean)

> names(TrainDataClean)

> names(TestDataClean)
