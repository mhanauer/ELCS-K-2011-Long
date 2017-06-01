---
title: "ECLS-K-2011-Long"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Here we are grabbing all the relevant data from the original data set.  I am creating four data sets and then combining them into data1 so that I can keep track of which variables I am including.  I am placing them into data1 instead of data, because I do not want to overwrite data.
```{r}
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/Data")
data = read.csv("ELCS-K-2011.csv", header = TRUE)

data = as.data.frame(data)

dep = cbind(X1PRNCON = data$X1PRNCON, X2PRNCON = data$X2PRNCON, X4PRNCON = data$X4PRNCON, X1PRNSOC = data$X1PRNSOC, X2PRNSOC = data$X2PRNSOC, X4PRNSOC = data$X4PRNSOC, X1PRNSAD = data$X1PRNSAD, X2PRNSAD = data$X2PRNSAD, X4PRNSAD = data$X4PRNSAD, X1PRNIMP = data$X1PRNIMP,X2PRNIMP = data$X2PRNIMP,X4PRNIMP = data$X4PRNIMP, X1PRNAPP = data$X1PRNAPP, X2PRNAPP = data$X2PRNAPP, X4PRNAPP = data$X4PRNAPP)

class = cbind(X1TCHPER = data$X1TCHPER, X2TCHPER = data$X2TCHPER, X4TCHPER = data$X4TCHPER, X1TCHEXT = data$X1TCHEXT,X2TCHEXT = data$X2TCHEXT, X4TCHEXT = data$X4TCHEXT, X1TCHINT = data$X1TCHINT, X2TCHINT = data$X2TCHINT, X4TCHINT = data$X4TCHINT, X1ATTNFS = data$X1ATTNFS, X2ATTNFS = data$X2ATTNFS, X4ATTNFS = data$X4ATTNFS, X1INBCNT = data$X1INBCNT, X2INBCNT = data$X2INBCNT, X4INBCNT = data$X4INBCNT, X2CNFLCT = data$X2CNFLCT, X4CNFLCT = data$X4CNFLCT, X12CHGTCH = data$X12CHGTCH, X34CHGTCH = data$X34CHGTCH)

school = cbind(X12CHGSCH = data$X12CHGSCH, X1LOCALE = data$X1LOCALE, X2LOCALE = data$X2LOCALE, X4LOCALE = data$X4LOCALE, X_DISTPOV = data$X_DISTPOV, X4DISTPOV = data$X4DISTPOV, X1PUBPRI = data$X1PUBPRI, X2PUBPRI = data$X2PUBPRI, X4PUBPRI = data$X4PUBPRI, X2LOWGRD = data$X2LOWGRD, X4LOWGRD = data$X4LOWGRD, X2KENRLS = data$X2KENRLS, X4ENRLS = data$X4ENRLS, X2KRCETH = data$X2KRCETH, X4RCETH = data$X4RCETH) 
school = as.data.frame(school)

home = cbind(X1RESREL = data$X1RESREL, X2RESREL = data$X2RESREL, X4RESREL2 = data$X4RESREL2,X1HPARNT = data$X1HPARNT, X2HPARNT = data$X2HPARNT, X4HPARNT = data$X4HPARNT, X1PAR1AGE = data$X1PAR1AGE, X2PAR1AGE = data$X2PAR1AGE, X4PAR1AGE_R = data$X4PAR1AGE_R, X1PAR1RAC = data$X1PAR1RAC, X2PAR1RAC = data$X2PAR1RAC, X4PAR1RAC = data$X4PAR1RAC,X12PAR1ED_I = data$X12PAR1ED_I, X4PAR1ED_I = data$X4PAR1ED_I, X1PAR1EMP = data$X1PAR1EMP, X4PAR1EMP_I = data$X4PAR1EMP_I, X1PAR1SCR_I = data$X1PAR1SCR_I, X4PAR1SCR_I = data$X4PAR1SCR_I, X1HTOTAL = data$X1HTOTAL, X2HTOTAL = data$X2HTOTAL, X4HTOTAL = data$X4HTOTAL, X1PRIMNW = data$X1PRIMNW, X2POVTY = data$X2POVTY, X4POVTY_I = data$X4POVTY_I, X2FSADRA2 = data$X2FSADRA2, X4FSADRA2 = data$X4FSADRA2, X12LANGST = data$X12LANGST, X4LANGST = data$X4LANGST)
home = as.data.frame(home)

ageControl = cbind(X1KAGE_R = data$X1KAGE_R, X2KAGE_R = data$X2KAGE_R, X4AGE = data$X4AGE)

data1 = cbind(dep, class, school, home, ageControl)
              
data1 = apply(data1, 2, function(x){ifelse(x == -9, NA, x)})
data1 = as.data.frame(data1)
data1 = na.omit(data1)
data1 = as.data.frame(data1)
dim(data1)
```
Need to take a sample of 100 before you create the long data set and see how long it will take
```{r}
set.seed(12345)
index = sample(1:nrow(data1), 400, replace = FALSE)
data1 = data1[index, ]
data1 = as.data.frame(data1)
dim(data1)
```

Here we create the data the long data. Instead of erasing everything, I just kept it.  Only the data set three will work, because we cannot have missing covariates.
```{r}

three = reshape(data1, varying = list(c("X1PRNCON", "X2PRNCON", "X4PRNCON"), c("X1PRNSOC","X2PRNSOC","X4PRNSOC"),c("X1PRNSAD","X2PRNSAD","X4PRNSAD"),c("X1PRNIMP","X2PRNIMP","X4PRNIMP"),c("X1PRNAPP","X2PRNAPP","X4PRNAPP"), c("X1TCHPER", "X2TCHPER", "X4TCHPER"), c("X1TCHEXT", "X2TCHEXT", "X4TCHEXT"),c("X1TCHINT", "X2TCHINT", "X4TCHINT"), c("X1ATTNFS"
,"X2ATTNFS", "X4ATTNFS"), c("X1INBCNT", "X2INBCNT", "X4INBCNT"), c("X1LOCALE", "X2LOCALE", "X4LOCALE"), c("X1PUBPRI", "X2PUBPRI", "X4PUBPRI"), c("X1RESREL", "X2RESREL", "X4RESREL2"), c("X1HPARNT", "X2HPARNT", "X4HPARNT"), c("X1PAR1AGE", "X2PAR1AGE", "X4PAR1AGE_R"), c("X1PAR1RAC", "X2PAR1RAC", "X4PAR1RAC"), c("X1HTOTAL", "X2HTOTAL", "X4HTOTAL"), c("X1KAGE_R", "X2KAGE_R", "X4AGE")), times = c(1,2,4), direction = "long")
three = as.data.frame(three)
head(three)
data1 = three[c(27:46)]


twoFour = reshape(data1, varying = list(
c("X2CNFLCT", "X4CNFLCT"), c("X12CHGTCH", "X34CHGTCH"), c("X2LOWGRD", "X4LOWGRD"), c("X2KENRLS", "X4ENRLS"), c("X2KRCETH", "X4RCETH"), c("X2POVTY", "X4POVTY_I"), c("X2FSADRA2", "X4FSADRA2"), c("X12LANGST", "X4LANGST"), c("X12PAR1ED_I", "X4PAR1ED_I")), times = c(2,4), direction = "long")


oneFour = reshape(data1, varying = list(c("X_DISTPOV", "X4DISTPOV"), c("X1PAR1EMP", "X4PAR1EMP_I"), c("X1PAR1SCR_I", "X4PAR1SCR_I")), times = c(1,4), direction = "long")

one = reshape(data1, varying = list(c("X12CHGSCH", "X1PRIMNW")), times = c(1,1), direction = "long")

data1  = as.data.frame(three)
dim(data1)
head(data1)
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
dim(data1)
write.csv(data1, "ECLSK-2011-Long.csv")
```
Now we have a model for the SC variable
```{r}
# Example for Jags-Ymet-XmetSsubj-MrobustHier.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load data file and specity column names of x (predictor) and y (predicted):
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
myData = read.csv(file="ECLSK-2011-Long.csv")
xName = "X1LANGST" ; x2Name = "X1RESREL"; x3Name = "X1HPARNT"; x4Name = "X_HISP_R"; x5Name = "X_WHITE_R"; x6Name = "X_BLACK_R"; x7Name = "X_ASIAN_R"; x8Name = "X_AMINAN_R"; x9Name = "X_HAWPI_R"; x10Name = "X_MULTR_R"; x11Name = "X1BMI"; x12Name = "X1PAR1AGE"; x13Name = "X1PAR1ED_I"; x14Name = "X1HTOTAL"; x15Name = "X1NUMSIB"; x16Name = "X1SESL"; yName = "X1PRNCON" ; sName="time"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
source("ECLS-K-2011-LongLow.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, x4Name=x4Name ,x5Name = x5Name, x6Name = x6Name, x7Name=x7Name ,x8Name = x8Name,x9Name = x9Name, x10Name=x10Name ,x11Name = x11Name,x12Name = x12Name, x13Name = x13Name, x14Name = x14Name, x15Name = x15Name, x16Name = x16Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)
```
Now we have a model for the SI variable
```{r}
# Example for Jags-Ymet-XmetSsubj-MrobustHier.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load data file and specity column names of x (predictor) and y (predicted):
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
myData = read.csv(file="ECLSK-2011-Long.csv")
xName = "X1LANGST" ; x2Name = "X1RESREL"; x3Name = "X1HPARNT"; x4Name = "X_HISP_R"; x5Name = "X_WHITE_R"; x6Name = "X_BLACK_R"; x7Name = "X_ASIAN_R"; x8Name = "X_AMINAN_R"; x9Name = "X_HAWPI_R"; x10Name = "X_MULTR_R"; x11Name = "X1BMI"; x12Name = "X1PAR1AGE"; x13Name = "X1PAR1ED_I"; x14Name = "X1HTOTAL"; x15Name = "X1NUMSIB"; x16Name = "X1SESL"; yName = "X1PRNSOC" ; sName="time"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
source("ECLS-K-2011-LongLow.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, x4Name=x4Name ,x5Name = x5Name, x6Name = x6Name, x7Name=x7Name ,x8Name = x8Name,x9Name = x9Name, x10Name=x10Name ,x11Name = x11Name,x12Name = x12Name, x13Name = x13Name, x14Name = x14Name, x15Name = x15Name, x16Name = x16Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)
```
Now we have a model for the Sad Lonley variable
```{r}
# Example for Jags-Ymet-XmetSsubj-MrobustHier.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load data file and specity column names of x (predictor) and y (predicted):
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
myData = read.csv(file="ECLSK-2011-Long.csv")
xName = "X1LANGST" ; x2Name = "X1RESREL"; x3Name = "X1HPARNT"; x4Name = "X_HISP_R"; x5Name = "X_WHITE_R"; x6Name = "X_BLACK_R"; x7Name = "X_ASIAN_R"; x8Name = "X_AMINAN_R"; x9Name = "X_HAWPI_R"; x10Name = "X_MULTR_R"; x11Name = "X1BMI"; x12Name = "X1PAR1AGE"; x13Name = "X1PAR1ED_I"; x14Name = "X1HTOTAL"; x15Name = "X1NUMSIB"; x16Name = "X1SESL"; yName = "X1PRNSAD" ; sName="time"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
source("ECLS-K-2011-LongLow.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, x4Name=x4Name ,x5Name = x5Name, x6Name = x6Name, x7Name=x7Name ,x8Name = x8Name,x9Name = x9Name, x10Name=x10Name ,x11Name = x11Name,x12Name = x12Name, x13Name = x13Name, x14Name = x14Name, x15Name = x15Name, x16Name = x16Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)
```
Now we have a model for the X1PRNIMP variable
```{r}
# Example for Jags-Ymet-XmetSsubj-MrobustHier.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load data file and specity column names of x (predictor) and y (predicted):
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
myData = read.csv(file="ECLSK-2011-Long.csv")
xName = "X1LANGST" ; x2Name = "X1RESREL"; x3Name = "X1HPARNT"; x4Name = "X_HISP_R"; x5Name = "X_WHITE_R"; x6Name = "X_BLACK_R"; x7Name = "X_ASIAN_R"; x8Name = "X_AMINAN_R"; x9Name = "X_HAWPI_R"; x10Name = "X_MULTR_R"; x11Name = "X1BMI"; x12Name = "X1PAR1AGE"; x13Name = "X1PAR1ED_I"; x14Name = "X1HTOTAL"; x15Name = "X1NUMSIB"; x16Name = "X1SESL"; yName = "X1PRNIMP" ; sName="time"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
source("ECLS-K-2011-LongLow.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, x4Name=x4Name ,x5Name = x5Name, x6Name = x6Name, x7Name=x7Name ,x8Name = x8Name,x9Name = x9Name, x10Name=x10Name ,x11Name = x11Name,x12Name = x12Name, x13Name = x13Name, x14Name = x14Name, x15Name = x15Name, x16Name = x16Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)
```
Now we have a model for the X1PRNAPP variable
```{r}
# Example for Jags-Ymet-XmetSsubj-MrobustHier.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load data file and specity column names of x (predictor) and y (predicted):
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
myData = read.csv(file="ECLSK-2011-LongTest.csv")
xName = "X1LANGST" ; x2Name = "X1RESREL"; x3Name = "X1HPARNT"; x4Name = "X_HISP_R"; x5Name = "X_WHITE_R"; x6Name = "X_BLACK_R"; x7Name = "X_ASIAN_R"; x8Name = "X_AMINAN_R"; x9Name = "X_HAWPI_R"; x10Name = "X_MULTR_R"; x11Name = "X1BMI"; x12Name = "X1PAR1AGE"; x13Name = "X1PAR1ED_I"; x14Name = "X1HTOTAL"; x15Name = "X1NUMSIB"; x16Name = "X1SESL"; yName = "X1PRNAPP" ; sName="time"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
setwd("~/Desktop")
source("ECLS-K-2011-LongLow.R")
library(runjags)
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, x4Name=x4Name ,x5Name = x5Name, x6Name = x6Name, x7Name=x7Name ,x8Name = x8Name,x9Name = x9Name, x10Name=x10Name ,x11Name = x11Name,x12Name = x12Name, x13Name = x13Name, x14Name = x14Name, x15Name = x15Name, x16Name = x16Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)
gelman.diag(mcmcCoda)
```

