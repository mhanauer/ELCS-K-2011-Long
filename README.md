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
data1 = cbind(X1PRNCON = data$X1PRNCON, X2PRNCON = data$X2PRNCON, X4PRNCON = data$X4PRNCON, X1PRNSOC = data$X1PRNSOC, X2PRNSOC = data$X2PRNSOC, X4PRNSOC = data$X4PRNSOC, X1PRNSAD = data$X1PRNSAD, X2PRNSAD = data$X2PRNSAD, X4PRNSAD = data$X4PRNSAD, X1PRNIMP = data$X1PRNIMP,X2PRNIMP = data$X2PRNIMP,X4PRNIMP = data$X4PRNIMP, X1PRNAPP = data$X1PRNAPP, X2PRNAPP = data$X2PRNAPP, X4PRNAPP = data$X4PRNAPP, X_RACETHP_R = data$X_RACETHP_R, X_CHSEX_R = data$X_CHSEX_R, X1BMI = data$X1BMI, X2BMI = data$X2BMI, X4BMI = data$X4BMI, X1RESREL = data$X1RESREL, X2RESREL = data$X2RESREL, X4RESREL2 = data$X4RESREL2, X1HPARNT = data$X1HPARNT, X2HPARNT = data$X2HPARNT, X4HPARNT = data$X4HPARNT, X1PAR1AGE = data$X1PAR1AGE, X2PAR1AGE = data$X2PAR1AGE, X4PAR1AGE_R = data$X4PAR1AGE_R, X1PAR1RAC = data$X1PAR1RAC, X2PAR1RAC = data$X2PAR1RAC, X4PAR1RAC = data$X4PAR1RAC, X12PAR1ED_I = data$X12PAR1ED_I, X4PAR1ED_I = data$X4PAR1ED_I, X1PAR1EMP = data$X1PAR1EMP, X4PAR1EMP_I = data$X4PAR1EMP_I, X12LANGST = data$X12LANGST, X4LANGST = data$X4LANGST, X1HTOTAL = data$X1HTOTAL, X2HTOTAL = data$X2HTOTAL, X4HTOTAL = data$X4HTOTAL, X1NUMSIB = data$X1NUMSIB, X2NUMSIB = data$X2NUMSIB, X4NUMSIB = data$X4NUMSIB, X12SESL = data$X12SESL, X4SESL_I = data$X4SESL_I) 
              
data1 = apply(data1, 2, function(x){ifelse(x == -9, NA, x)})
data1 = na.omit(data1)
dim(data1)      
```
Now we need to figure out how to create duplicates 3 duplicates of Race and Sex, and two duplicates of the first Lang and SES variable.  Well just include them the appropriate number of times with different names
