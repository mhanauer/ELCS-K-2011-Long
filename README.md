---
title: "ECLS-K-2011-Long"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Here
```{r}
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/Data")
data = read.csv("ELCS-K-2011.csv", header = TRUE)

data1 = cbind(X1PRNCON = data$X1PRNCON, X2PRNCON = data$X2PRNCON, X4PRNCON = data$X4PRNCON, X1PRNSOC = data$X1PRNSOC, X2PRNSOC = data$X2PRNSOC, X4PRNSOC = data$X4PRNSOC, X1PRNSAD = data$X1PRNSAD, X2PRNSAD = data$X2PRNSAD, X4PRNSAD = data$X4PRNSAD, X1PRNIMP = data$X1PRNIMP,X2PRNIMP = data$X2PRNIMP,X4PRNIMP = data$X4PRNIMP, X1PRNAPP = data$X1PRNAPP, X2PRNAPP = data$X2PRNAPP, X4PRNAPP = data$X4PRNAPP, X1_RACETHP_R = data$X_RACETHP_R, X2_RACETHP_R = data$X_RACETHP_R, X4_RACETHP_R = data$X_RACETHP_R, X1_CHSEX_R = data$X_CHSEX_R, X2_CHSEX_R = data$X_CHSEX_R, X4_CHSEX_R = data$X_CHSEX_R, X1BMI = data$X1BMI, X2BMI = data$X2BMI, X4BMI = data$X4BMI, X1RESREL = data$X1RESREL, X2RESREL = data$X2RESREL, X4RESREL2 = data$X4RESREL2, X1HPARNT = data$X1HPARNT, X2HPARNT = data$X2HPARNT, X4HPARNT = data$X4HPARNT, X1PAR1AGE = data$X1PAR1AGE, X2PAR1AGE = data$X2PAR1AGE, X4PAR1AGE_R = data$X4PAR1AGE_R, X1PAR1RAC = data$X1PAR1RAC, X2PAR1RAC = data$X2PAR1RAC, X4PAR1RAC = data$X4PAR1RAC, X1PAR1ED_I = data$X12PAR1ED_I, X2PAR1ED_I = data$X12PAR1ED_I, X4PAR1ED_I = data$X4PAR1ED_I, X1LANGST = data$X12LANGST, X2LANGST = data$X12LANGST, X4LANGST = data$X4LANGST, X1HTOTAL = data$X1HTOTAL, X2HTOTAL = data$X2HTOTAL, X4HTOTAL = data$X4HTOTAL, X1NUMSIB = data$X1NUMSIB, X2NUMSIB = data$X2NUMSIB, X4NUMSIB = data$X4NUMSIB, X1SESL = data$X12SESL, X2SESL = data$X12SESL, X4SESL_I = data$X4SESL_I) 
              
data1 = apply(data1, 2, function(x){ifelse(x == -9, NA, x)})
data1 = na.omit(data1)
data1 = as.data.frame(data1)
dim(data1)
```
Need to create longitudinal data sets.
```{r}
data1 = reshape(data1, varying = list(c("X1PRNCON", "X2PRNCON", "X4PRNCON"), c("X1PRNSOC", "X2PRNSOC", "X4PRNSOC"), c("X1PRNSAD", "X2PRNSAD", "X4PRNSAD"), c("X1PRNIMP", "X2PRNIMP", "X4PRNIMP"), c("X1PRNAPP", "X2PRNAPP", "X4PRNAPP"), c("X1_RACETHP_R", "X2_RACETHP_R", "X4_RACETHP_R"), c("X1_CHSEX_R", "X2_CHSEX_R", "X4_CHSEX_R"), c("X1BMI", "X2BMI", "X4BMI"), c("X1RESREL", "X2RESREL", "X4RESREL2"), c("X1HPARNT", "X2HPARNT", "X4HPARNT"), c("X1PAR1AGE", "X2PAR1AGE", "X4PAR1AGE_R"), c("X1PAR1RAC", "X2PAR1RAC", "X4PAR1RAC"), c("X1PAR1ED_I", "X2PAR1ED_I", "X4PAR1ED_I"), c("X1LANGST", "X2LANGST", "X4LANGST"), c("X1HTOTAL", "X2HTOTAL", "X4HTOTAL"), c("X1NUMSIB", "X2NUMSIB", "X4NUMSIB"), c("X1SESL", "X2SESL", "X4SESL_I")), v.names = c("X1PRNCON", "X1PRNSOC", "X1PRNSAD", "X1PRNIMP", "X1PRNAPP", "X1_RACETHP_R", "X1_CHSEX_R", "X1BMI", "X1RESREL", "X1HPARNT", "X1PAR1AGE", "X1PAR1RAC", "X1PAR1ED_I", "X1LANGST", "X1HTOTAL", "X1NUMSIB", "X1SESL"), times = c(1,2,4), direction = "long")
dim(data1)
summary(data1$time)
sum(is.na(data1))
```
Now we can make the same transformation like in the previous study.  We have the NA ifelse in there, because I just copied the code from the other study when I was thinking about the missing data component. 

Create a data set that has the variables not included below to combine at the end.
```{r}
data2 = cbind(X1LANGST = data1$X1LANGST,  X1_CHSEX_R = data1$X_CHSEX_R, X1RESREL = data1$X1RESREL, X1HPARNT = data1$X1HPARNT, X1PRIMNW = data1$X1PRIMNW)

data2 = ifelse(is.na(data2), NA, ifelse(data2 == 1, 1,0))
data2 = as.data.frame(data2)
head(data2)

# Here is the ethnicity variable that needs to be transformed into the original variables that you used.  Remember that original variable were incorrect and just keeping the names the same here for consistency.
data3 = cbind(data1$X1_RACETHP_R)
X_HISP_R = ifelse(is.na(data3), NA, ifelse(data3 == 3, 1, 0))
X_HISP_R = as.data.frame(X_HISP_R)
names(X_HISP_R) = c("X_HISP_R")

X_WHITE_R = ifelse(is.na(data3), NA, ifelse(data3 == 1, 1, 0))
X_WHITE_R = as.data.frame(X_WHITE_R)
names(X_WHITE_R) = c("X_WHITE_R")

X_BLACK_R = ifelse(is.na(data3), NA, ifelse(data3 == 2, 1, 0))
X_BLACK_R = as.data.frame(X_BLACK_R)
names(X_BLACK_R) = c("X_BLACK_R")

X_ASIAN_R = ifelse(is.na(data3), NA, ifelse(data3 == 5, 1, 0))
X_ASIAN_R = as.data.frame(X_ASIAN_R)
names(X_ASIAN_R) = c("X_ASIAN_R")

X_AMINAN_R = ifelse(is.na(data3), NA, ifelse(data3 == 7, 1, 0))
X_AMINAN_R = as.data.frame(X_AMINAN_R)
names(X_AMINAN_R) = c("X_AMINAN_R")

X_HAWPI_R = ifelse(is.na(data3), NA, ifelse(data3 == 6, 1, 0))
X_HAWPI_R = as.data.frame(X_HAWPI_R)
names(X_HAWPI_R) = c("X_HAWPI_R")

X_MULTR_R = ifelse(is.na(data3), NA, ifelse(data3 == 8, 1, 0))
X_MULTR_R = as.data.frame(X_MULTR_R)
names(X_MULTR_R) = c("X_MULTR_R")


data4 = cbind(X_HISP_R, X_WHITE_R, X_BLACK_R, X_ASIAN_R, X_AMINAN_R, X_AMINAN_R, X_HAWPI_R, X_MULTR_R)
data4 = as.data.frame(data4)
head(data4)



# Need to change 2 through 8 to be 1 and 1 to be zero
data5 = cbind(X1PAR1RAC = data1$X1PAR1RAC)
data5 = ifelse(is.na(data4), NA, ifelse(data4 == 1, 0,1))
data5 = as.data.frame(data5)

# Grab the other variables not included in the transformation to include to combine with the transformed data so you don't double up variables, combining with the original data set.
data6 = cbind(time = data1$time, X1PRNCON= data1$X1PRNCON, X1PRNSOC = data1$X1PRNSOC, X1PRNSAD= data1$X1PRNSAD,X1PRNIMP=  data1$X1PRNIMP,X1PRNAPP = data1$X1PRNAPP, X1BMI = data1$X1BMI, X1PAR1AGE = data1$X1PAR1AGE,X1PAR1ED_I = data1$X1PAR1ED_I, X1HTOTAL = data1$X1HTOTAL, X1NUMSIB = data1$X1NUMSIB, X1SESL = data1$X1SESL)

# Not including data3, because that data frame is the old race variable no longer needed.
data1 = cbind(data2, data4, data5, data6)
head(data1)
write.csv(data1, "ECLSK-2011-Long.csv")
```
Now we are going to test the current Bayesian program with three variables and time as the hierlevel
```{r}
# Example for Jags-Ymet-XmetSsubj-MrobustHier.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
#rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load data file and specity column names of x (predictor) and y (predicted):
setwd("~/Desktop/Extending")
myData = read.csv(file="ECLSK-2011-Long.csv")
xName = "X1BMI" ; x2Name = "X1RESREL"; x3Name = "X1SESL"; yName = "X1PRNCON" ; sName="time"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
source("Jags-Ymet-XmetSsubj-Dicot-MrobustHierExtend.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)
```



