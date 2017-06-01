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

class = cbind(X1TCHPER = data$X1TCHPER, X2TCHPER = data$X2TCHPER, X4TCHPER = data$X4TCHPER, X1TCHEXT = data$X1TCHEXT,X2TCHEXT = data$X2TCHEXT, X4TCHEXT = data$X4TCHEXT, X1TCHINT = data$X1TCHINT, X2TCHINT = data$X2TCHINT, X4TCHINT = data$X4TCHINT, X1ATTNFS = data$X1ATTNFS, X2ATTNFS = data$X2ATTNFS, X4ATTNFS = data$X4ATTNFS, X1INBCNT = data$X1INBCNT, X2INBCNT = data$X2INBCNT, X4INBCNT = data$X4INBCNT)

school = cbind(X1LOCALE = data$X1LOCALE, X2LOCALE = data$X2LOCALE, X4LOCALE = data$X4LOCALE, X1PUBPRI = data$X1PUBPRI, X2PUBPRI = data$X2PUBPRI, X4PUBPRI = data$X4PUBPRI) 
school = as.data.frame(school)

home = cbind(X1RESREL = data$X1RESREL, X2RESREL = data$X2RESREL, X4RESREL2 = data$X4RESREL2,X1HPARNT = data$X1HPARNT, X2HPARNT = data$X2HPARNT, X4HPARNT = data$X4HPARNT, X1PAR1AGE = data$X1PAR1AGE, X2PAR1AGE = data$X2PAR1AGE, X4PAR1AGE_R = data$X4PAR1AGE_R, X1PAR1RAC = data$X1PAR1RAC, X2PAR1RAC = data$X2PAR1RAC, X4PAR1RAC = data$X4PAR1RAC,X1PAR1ED_I = data$X12PAR1ED_I, X2PAR1ED_I = data$X12PAR1ED_I, X4PAR1ED_I = data$X4PAR1ED_I, X1HTOTAL = data$X1HTOTAL, X2HTOTAL = data$X2HTOTAL, X4HTOTAL = data$X4HTOTAL, X1LANGST = data$X12LANGST, X2LANGST = data$X12LANGST, X4LANGST = data$X4LANGST)
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

data1 = reshape(data1, varying = list(c("X1PRNCON", "X2PRNCON", "X4PRNCON"), c("X1PRNSOC","X2PRNSOC","X4PRNSOC"),c("X1PRNSAD","X2PRNSAD","X4PRNSAD"),c("X1PRNIMP","X2PRNIMP","X4PRNIMP"),c("X1PRNAPP","X2PRNAPP","X4PRNAPP"), c("X1TCHPER", "X2TCHPER", "X4TCHPER"), c("X1TCHEXT", "X2TCHEXT", "X4TCHEXT"),c("X1TCHINT", "X2TCHINT", "X4TCHINT"), c("X1ATTNFS", "X2ATTNFS", "X4ATTNFS"), c("X1INBCNT", "X2INBCNT", "X4INBCNT"), c("X1LOCALE", "X2LOCALE", "X4LOCALE"), c("X1PUBPRI", "X2PUBPRI", "X4PUBPRI"), c("X1RESREL", "X2RESREL", "X4RESREL2"), c("X1HPARNT", "X2HPARNT", "X4HPARNT"), c("X1PAR1AGE", "X2PAR1AGE", "X4PAR1AGE_R"), c("X1PAR1RAC", "X2PAR1RAC", "X4PAR1RAC"), c("X1HTOTAL", "X2HTOTAL", "X4HTOTAL"), c("X1KAGE_R", "X2KAGE_R", "X4AGE"), c("X1PAR1ED_I", "X2PAR1ED_I", "X4PAR1ED_I"), c("X1LANGST", "X2LANGST", "X4LANGST")), times = c(1,2,4), direction = "long")
data1 = as.data.frame(data1)
dim(data1)
```
Here we need to make all of the transformations.

For locale, we need to create three dummy variables one for city (1 = city), one for suburb (2 = suburb), and one for rural (4 = rural)

For Public private change from 2 private to be zero.

```{r}

# Change the Locale variable
city = ifelse(data1$X1LOCALE == 1, 1, 0)
city = as.data.frame(city)
names(city) = c("city")
head(city)

suburb = ifelse(data1$X1LOCALE == 2, 1,0)
suburb = as.data.frame(suburb)
names(suburb) = c("suburb")
head(suburb)

rural = ifelse(data1$X1LOCALE == 4, 1,0)
rural = as.data.frame(rural)
names(rural) = c("rural")
head(rural)

# Need to change public private
public = ifelse(data1$X1PUBPRI == 2, 1,0)
public = as.data.frame(public)
names(public) = c("public")
head(public)

# Change X1RESREL, X1HPARNT, X1LANGST all to one's and zero's for else

data2 = cbind(X1RESREL = data1$X1RESREL, X1HPARNT = data1$X1HPARNT, X1LANGST =data1$X1LANGST)
data2 = ifelse(data2 == 1, 1, 0)
data2 = as.data.frame(data2)
head(data2)

# This is seperate, X1PAR1RAC

parrace = ifelse(data1$X1PAR1RAC == 1,0,1)
names(parrace) = c("X1PAR1RAC")
parrace = as.data.frame(parrace)


data1 = cbind( X1PRNCON= data1$X1PRNCON, X1PRNSOC=  data1$X1PRNSOC, X1PRNSAD = data1$X1PRNSAD, X1PRNIMP= data1$X1PRNIMP,  X1PRNAPP= data1$X1PRNAPP, X1TCHPER = data1$X1TCHPER, X1TCHEXT= data1$X1TCHEXT, X1TCHINT = data1$X1TCHINT, X1ATTNFS = data1$X1ATTNFS, X1INBCNT= data1$X1INBCNT, city = city, suburb = suburb, rural = rural, public = public, X1RESREL = data2$X1RESREL, X1HPARNT = data2$X1HPARNT, X1LANGST= data2$X1LANGST, X1HPARNT=  data1$X1HPARNT, X1PAR1AGE =  data1$X1PAR1AGE, X1PAR1RAC = parrace, X1HTOTAL = data1$X1HTOTAL, X1KAGE_R = data1$X1KAGE_R, X1PAR1ED_I = data1$X1PAR1ED_I, time = data1$time)
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
head(myData)
xName = "X1TCHPER" ; x2Name = "X1TCHEXT"; x3Name = "X1TCHINT"; x4Name = "X1ATTNFS"; x5Name = "X1INBCNT"; x6Name = "city"; x7Name = "suburb"; x8Name = "rural"; x9Name = "public"; x10Name = "X1RESREL"; x11Name = "X1HPARNT"; x12Name = "X1LANGST"; x13Name = "X1HPARNT"; x14Name = "X1PAR1AGE"; x15Name = "parrace"; x16Name = "X1HTOTAL"; x17Name = "X1KAGE_R"; x18Name = "X1PAR1ED_I" ; yName = "X1PRNCON" ; sName="time"
fileNameRoot = "HierLinRegressData-Jags-" 

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
setwd("~/Google Drive/PARCS/Projects/ECLSK2011/ELCSK2011Long/Data")
source("ECLS-K-2011-LongLow.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName ,x2Name = x2Name,x3Name = x3Name, x4Name=x4Name ,x5Name = x5Name, x6Name = x6Name, x7Name=x7Name ,x8Name = x8Name,x9Name = x9Name, x10Name=x10Name ,x11Name = x11Name,x12Name = x12Name, x13Name = x13Name, x14Name = x14Name, x15Name = x15Name, x16Name = x16Name, x17Name = x17Name, x18Name = x18Name, yName=yName , sName=sName ,
                    numSavedSteps=20000 , thinSteps=15 , saveName=fileNameRoot )

#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , saveName=fileNameRoot )
show(summaryInfo)
gelman.diag(mcmcCoda)
```
