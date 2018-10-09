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
 # Data cleaning
 
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

# Data processing

> partition <- createDataPartition(TrainDataClean$classe, p = 0.6, list = FALSE)

> sub_train <- TrainDataClean[partition,]

> sub_test <- TrainDataClean[-partition,]

> set.seed(18)

Now we have to do algorithms in machine learning.

> mod_rf<-randomForest(formula = classe ~ ., data = sub_train)

> mod_rf

Call:

 randomForest(formula = classe ~ ., data = sub_train) 
 
               Type of random forest: classification
                     Number of trees: 500
                     
No. of variables tried at each split: 7

        OOB estimate of  error rate: 0.63%
        
Confusion matrix:

     A    B    C    D    E class.error
     
A 3343    5    0    0    0 0.001493429

B   16 2259    4    0    0 0.008775779

C    0   19 2031    4    0 0.011197663

D    0    0   20 1910    0 0.010362694

E    0    0    2    4 2159 0.002771363

# Prediction

> prediction <- predict(mod_rf, testing, type = "class")

> prediction

 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 
 B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B 
19 20 
 B  B 
 
Levels: A B C D E
