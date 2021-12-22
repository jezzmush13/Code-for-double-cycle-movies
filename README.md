# Code-for-double-cycle-movies
Just for dividing halfwidht and duration


#RCaMP Calcium Analysis JMR

#load the library 
library(tidyverse)
library(plyr)
library(dplyr)
library(stringr)

#DO NOT FORGET TO SET WORK DIRECTORY CORRECTLY

#load the data 
filenames <- list.files(pattern="*.csv") #read all the .csv files 


#create a dataframe; Remove missing values
df<-purrr::map_df(filenames, read_csv, .id = 'filename') 
df<- na.omit(df) #remove missing values 

#change the names to lowercase
df$roiName<- tolower(df$roiName)
df$Spot<- tolower(df$Spot)
df$Condition<- tolower(df$Condition)
df$drug<- tolower(df$drug)

#Change nominal and date format
df$filename = factor(df$filename)
df$roiName = factor(df$roiName)
df$Animal = factor(df$Animal)
df$drug = factor(df$drug)
df$Spot = factor(df$Spot)
df$filename = factor(df$filename)
df$date = factor(df$date)
view(df)

#Modifying variables and cleaning data frame
#Short df and create a unique ROIname
df_short2c <- df[ ,c('date', 'drug', 'Condition', 'Spot', 'Animal', 'roiName', 'amplitude', 'halfWidth','peakAUC')]
#Replace after drugs for baseline
#df_short2c$drug <- str_replace_all(df_short2c$drug, 'after drugs', 'baseline')
#adding unique Spot
df_short2c$Unique_Spot <-paste(df_short2c$Animal, df_short2c$Spot, sep= "_")
#adding unique ROI name
df_short2c$Unique_ROIname <-paste(df_short2c$Animal, df_short2c$Spot, df_short2c$roiName, sep= "_")
#create a new variable for Process, Soma and Sphincter
unique(df_short2c$roiName)
#replacing "sp" for "t" labeling because it overlaps with "s" and "p" for using grepl function
df_short2c$roiName <- str_replace_all(df_short2c$roiName, 'sp1', 't1')
df_short2c$roiName <- str_replace_all(df_short2c$roiName, 'sp2', 't2')
df_short2c$roiName <- str_replace_all(df_short2c$roiName, 'sp3', 't3')
df_short2c$roiName <- str_replace_all(df_short2c$roiName, 'sp2-1', 't2-1')
unique(df_short2c$roiName)
view(df_short2c)
#It is better now, create a new variable for Process, Soma and Sphincter
df_short2c$ROI=0
df_short2c$ROI[grepl("p",df_short2c$roiName)]="Process"
df_short2c$ROI[grepl("s",df_short2c$roiName)]="Soma"
df_short2c$ROI[grepl("t",df_short2c$roiName)]="Sphincter"
view(df_short2c)
unique(df_short2c$drug)
unique(df_short2c$ROI)
unique(df_short2c$Animal)

#Dividing parameters peakAUC and of Baseline of R72L and R72R because it is 2 cycles instead of one
#Dividing parameters peakAUC and halfWidth  of Baseline of R72L and R72R because it is 2 cycles instead of one
df_short2c$peakAUC<- (df_short2c$peakAUC)/2
df_short2c$halfWidth<- (df_short2c$halfWidth)/2
View(df_short2c)

#RETURN TO RCaMP code analysis





