This README.md have contents below:

1. CodeBook.md: All working process, explaination variables and data
2. run_analysis.R: Used R program code


##1. CodeBook.md###################################################
##the variables, the data, and any transformations or work

##Merges the training and the test sets to create one data set.##########

1. subj_test: reading subject data in test group from ./test/subject_test.txt 

2. X_test_tmp: reading X_test data in test group from ./test/X_test.txt. and converting X_test data from " " to "#" for well splicing to measurement variables

3. measurement: it is a variable list for 561 measurements. 

4. X_test: splicing to appropriate 561 measurement variables using colsplit function, the variables's names were given from measurement.

5. Y_test: reading Y_test data in test group from ./test/Y_test.txt 

6. giving variable name to Y_test as "ACT"

7. Test_set : making dataset for subjects in test group using cbind included subj_test, X_test, and Y_Test

8. subj_train: reading subject data in training group from ./train/subject_train.txt 

9. X_train_tmp: reading X_train data in training group from ./train/X_train.txt. and, converting X_train data from " " to "#" for well splicing to measurement variables

10. X_train: splicing to appropriate 561 measurement variables using colsplit function, the variables's names were given from measurement.

11. Y_train: reading Y_train data in training group from ./train/Y_train.txt 

12. Train_set: making dataset for subjects in training group using cbind included subj_train, X_train, and Y_train

13. data_set: merging Test_set and Train_set by both subject and activity number

##Extracts only the measurements on the mean and standard deviation for each measurement. 

1. KEY variable: conbining subject and activity number for calculating mean and s.d. grouped by both subject and activity

2. key_list: extracting unique keys for "KEY" and sorting by both subject and activity

3 key_n: number of stat_key, 180

4. tidy_set: making tidy format. columns: "SUBJ", "Act_no", "Measurement", "MEAN", and "SD".

5. Calculating mean and sd for all subjects using "for roop" function. And, tidy_set got mean and sd.

##Uses descriptive activity names to name the activities in the data set
1. label: reading labels of activity groups from ./activity_labels.txt

2. tidy_tmp: merging tidy_set and label

3. tidy_tmp sorted by subjects, activities, and measurements

##tidy
1. tidy: columns that "SUBJ", "ACTIVITY", "MEASUREMENTS", "MEAN", and "SD".

2. tidy.txt: using write.table function


####2. run_analysis.R ###################################################

setwd("D:/Course_Project")

library(plyr)
library(reshape)
library(reshape2)

###Step1.################
subj_test <- read.table("./test/subject_test.txt",sep=' ')
names(subj_test) <- c('SUBJ')

X_test_tmp <- read.table("./test/X_test.txt",sep='\n',header=FALSE,stringsAsFactors=F)
X_test_tmp <- gsub(" ","#",X_test_tmp$V1)
X_test_tmp <- gsub("##","#",X_test_tmp)

no_measure<- seq(0,561,1)
measurement <- paste0("measurement_",no_measure)

X_test <-colsplit(X_test_tmp,"#",names=c(measurement))

X_test[,1] <- NULL

Y_test <- read.table("./test/Y_test.txt",sep='\t')
names(Y_test) <- c("ACT")

Test_set <- cbind(subj_test,Y_test,X_test)

subj_train <- read.table("./train/subject_train.txt",sep=' ')
names(subj_train) <- c('SUBJ')

X_train_tmp <- read.table("./train/X_train.txt",sep='\n',header=FALSE,stringsAsFactors=F)

X_train_tmp <- gsub(" ","_",X_train_tmp$V1)
X_train_tmp <- gsub("__","_",X_train_tmp)

no_measure<- seq(0,561,1)
measurement <- paste0("measurement_",no_measure)
X_train<-colsplit(X_train_tmp,"_",names=c(measurement))

X_train[,1] <- NULL

Y_train<- read.table("./train/Y_train.txt",sep='\t')
names(Y_train) <- c("ACT")

Train_set <- cbind(subj_train,Y_train,X_train)

data_set <- rbind(Test_set,Train_set)

###Step 2, 4#############################
data_set$KEY <- paste(data_set$SUBJ,data_set$ACT,sep="-")

key_list <- unique(data_set[,c("SUBJ","ACT","KEY")])
key_list <- key_list[order(key_list$SUBJ,key_list$ACT),]
key_n<- nrow(key_list)

no_measure<- seq(1,561,1)
measurement <- paste0("measurement_",no_measure)

tidy_set <- expand.grid("SUBJ"=seq(1,30,1), "ACT"=seq(1,6,1),"MEASURE"=measurement)
tidy_set <- tidy_set[order(tidy_set$MEASURE,tidy_set$SUBJ,tidy_set$ACT),]
tidy_set$MEAN <- NA
tidy_set$SD <- NA

for(i in 1:length(measurement))
{
	for(j in 1:key_n)
	{
		obs <- (i-1)*180+j
		tidy_set_sub <- subset(data_set[,c(1,2,2+i)],data_set$KEY==key_list[j,3])
		tidy_set$MEAN[obs] <- mean(tidy_set_sub[,3],na.rm=T)
		tidy_set$SD[obs] <- sd(tidy_set_sub[,3],na.rm=T)
	}
}

###Step 3################################################
label <- read.table("activity_labels.txt",sep=' ')
names(label) <- c('ACT', 'ACTIVITY')

tidy_tmp <- merge(tidy_set,label)
tidy_tmp <- tidy_tmp[order(tidy_tmp$SUBJ,tidy_tmp$ACT,tidy_tmp$MEASURE),]


###Step 5###############################
tidy <- data.frame(
SUBJ=tidy_tmp$SUBJ,
ACTIVITY=tidy_tmp$ACTIVITY,
MEASUREMENTS=tidy_tmp$MEASURE,
MEAN=tidy_tmp$MEAN,
SD=tidy_tmp$SD)

##Please upload your data set as a txt file created with write.table() using row.name=FALSE
write.table(tidy,"tidy.txt",row.name=FALSE)




