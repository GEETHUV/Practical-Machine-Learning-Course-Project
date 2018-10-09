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

> names(training)

  [1] "X"                       
  [2] "user_name"               
  [3] "raw_timestamp_part_1"    
  [4] "raw_timestamp_part_2"    
  [5] "cvtd_timestamp"          
  [6] "new_window"              
  [7] "num_window"              
  [8] "roll_belt"               
  [9] "pitch_belt"              
 [10] "yaw_belt"                
 [11] "total_accel_belt"        
 [12] "kurtosis_roll_belt"      
 [13] "kurtosis_picth_belt"     
 [14] "kurtosis_yaw_belt"       
 [15] "skewness_roll_belt"      
 [16] "skewness_roll_belt.1"    
 [17] "skewness_yaw_belt"       
 [18] "max_roll_belt"           
 [19] "max_picth_belt"          
 [20] "max_yaw_belt"            
 [21] "min_roll_belt"           
 [22] "min_pitch_belt"          
 [23] "min_yaw_belt"            
 [24] "amplitude_roll_belt"     
 [25] "amplitude_pitch_belt"    
 [26] "amplitude_yaw_belt"      
 [27] "var_total_accel_belt"    
 [28] "avg_roll_belt"           
 [29] "stddev_roll_belt"        
 [30] "var_roll_belt"           
 [31] "avg_pitch_belt"          
 [32] "stddev_pitch_belt"       
 [33] "var_pitch_belt"          
 [34] "avg_yaw_belt"            
 [35] "stddev_yaw_belt"         
 [36] "var_yaw_belt"            
 [37] "gyros_belt_x"            
 [38] "gyros_belt_y"            
 [39] "gyros_belt_z"            
 [40] "accel_belt_x"            
 [41] "accel_belt_y"            
 [42] "accel_belt_z"            
 [43] "magnet_belt_x"           
 [44] "magnet_belt_y"           
 [45] "magnet_belt_z"           
 [46] "roll_arm"                
 [47] "pitch_arm"               
 [48] "yaw_arm"                 
 [49] "total_accel_arm"         
 [50] "var_accel_arm"           
 [51] "avg_roll_arm"            
 [52] "stddev_roll_arm"         
 [53] "var_roll_arm"            
 [54] "avg_pitch_arm"           
 [55] "stddev_pitch_arm"        
 [56] "var_pitch_arm"           
 [57] "avg_yaw_arm"             
 [58] "stddev_yaw_arm"          
 [59] "var_yaw_arm"             
 [60] "gyros_arm_x"             
 [61] "gyros_arm_y"             
 [62] "gyros_arm_z"             
 [63] "accel_arm_x"             
 [64] "accel_arm_y"             
 [65] "accel_arm_z"             
 [66] "magnet_arm_x"            
 [67] "magnet_arm_y"            
 [68] "magnet_arm_z"            
 [69] "kurtosis_roll_arm"       
 [70] "kurtosis_picth_arm"      
 [71] "kurtosis_yaw_arm"        
 [72] "skewness_roll_arm"       
 [73] "skewness_pitch_arm"      
 [74] "skewness_yaw_arm"        
 [75] "max_roll_arm"            
 [76] "max_picth_arm"           
 [77] "max_yaw_arm"             
 [78] "min_roll_arm"            
 [79] "min_pitch_arm"           
 [80] "min_yaw_arm"             
 [81] "amplitude_roll_arm"      
 [82] "amplitude_pitch_arm"     
 [83] "amplitude_yaw_arm"       
 [84] "roll_dumbbell"           
 [85] "pitch_dumbbell"          
 [86] "yaw_dumbbell"            
 [87] "kurtosis_roll_dumbbell"  
 [88] "kurtosis_picth_dumbbell" 
 [89] "kurtosis_yaw_dumbbell"   
 [90] "skewness_roll_dumbbell"  
 [91] "skewness_pitch_dumbbell" 
 [92] "skewness_yaw_dumbbell"   
 [93] "max_roll_dumbbell"       
 [94] "max_picth_dumbbell"      
 [95] "max_yaw_dumbbell"        
 [96] "min_roll_dumbbell"       
 [97] "min_pitch_dumbbell"      
 [98] "min_yaw_dumbbell"        
 [99] "amplitude_roll_dumbbell" 
[100] "amplitude_pitch_dumbbell"
[101] "amplitude_yaw_dumbbell"  
[102] "total_accel_dumbbell"    
[103] "var_accel_dumbbell"      
[104] "avg_roll_dumbbell"       
[105] "stddev_roll_dumbbell"    
[106] "var_roll_dumbbell"       
[107] "avg_pitch_dumbbell"      
[108] "stddev_pitch_dumbbell"   
[109] "var_pitch_dumbbell"      
[110] "avg_yaw_dumbbell"        
[111] "stddev_yaw_dumbbell"     
[112] "var_yaw_dumbbell"        
[113] "gyros_dumbbell_x"        
[114] "gyros_dumbbell_y"        
[115] "gyros_dumbbell_z"        
[116] "accel_dumbbell_x"        
[117] "accel_dumbbell_y"        
[118] "accel_dumbbell_z"        
[119] "magnet_dumbbell_x"       
[120] "magnet_dumbbell_y"       
[121] "magnet_dumbbell_z"       
[122] "roll_forearm"            
[123] "pitch_forearm"           
[124] "yaw_forearm"             
[125] "kurtosis_roll_forearm"   
[126] "kurtosis_picth_forearm"  
[127] "kurtosis_yaw_forearm"    
[128] "skewness_roll_forearm"   
[129] "skewness_pitch_forearm"  
[130] "skewness_yaw_forearm"    
[131] "max_roll_forearm"        
[132] "max_picth_forearm"       
[133] "max_yaw_forearm"         
[134] "min_roll_forearm"        
[135] "min_pitch_forearm"       
[136] "min_yaw_forearm"         
[137] "amplitude_roll_forearm"  
[138] "amplitude_pitch_forearm" 
[139] "amplitude_yaw_forearm"   
[140] "total_accel_forearm"     
[141] "var_accel_forearm"       
[142] "avg_roll_forearm"        
[143] "stddev_roll_forearm"     
[144] "var_roll_forearm"        
[145] "avg_pitch_forearm"       
[146] "stddev_pitch_forearm"    
[147] "var_pitch_forearm"       
[148] "avg_yaw_forearm"         
[149] "stddev_yaw_forearm"      
[150] "var_yaw_forearm"         
[151] "gyros_forearm_x"         
[152] "gyros_forearm_y"         
[153] "gyros_forearm_z"         
[154] "accel_forearm_x"         
[155] "accel_forearm_y"         
[156] "accel_forearm_z"         
[157] "magnet_forearm_x"        
[158] "magnet_forearm_y"        
[159] "magnet_forearm_z"        
[160] "classe"             

