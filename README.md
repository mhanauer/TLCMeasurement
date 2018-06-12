---
title: "TLC Measurement Example"
output: ''
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Library the packages that you need
```{r}
library(foreign)
library(nnet)
library(ggplot2)
library(prettyR)
library(nlme)
library(prettyR)
library(descr)
library(Amelia)
library(mitools)
library(BaylorEdPsych)
library(lavaan)
library(MissMech)
```
Get the items for the confidence measure (put remember want this to be a general template).

Sample size likely to small to generate differences in smaller subsets of groups therefore for demographics create binary versions of them
```{r}
#setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/TLC")
#datTLC = read.csv("Target 1_SPSS Analysis File_4.20.18.csv", header = TRUE)
attach(datTLC)
summary(datTLC)
datTLC$Age = ifelse(datTLC$Age == 451, 45, datTLC$Age)
head(datTLC)

inQCats = data.frame(datTLC[c(9:10, 13, 15, 17:18)])
head(inQCats)

#Need to make sure nothing funny is going first
summary(inQCats)

# If you are changing something to binary, then you can say just go back to the original data point and that will allow a value to be na. 
inQCats = data.frame(apply(inQCats, 2, function(x){ifelse(x > 1, 0, x)}))
head(inQCats)

head(datTLC)
# Now combine the items with the demographics
inQItems = data.frame(datTLC[c(45:57)])
head(inQItems)

# Now combine the two data sets
inQAll = data.frame(inQCats, inQItems)
```
Now get the descriptives for both sets
```{r}
describe(inQItems)

apply(inQCats, 2, function(x){describe.factor(x)})
```
Now run missing at random test and get cronbach alpha
Remember to reverse items later on.
```{r}
LittleMCAR(inQAll)
TestMCARNormality(inQAll)
alpha(inQItems) 
```



