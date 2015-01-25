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


