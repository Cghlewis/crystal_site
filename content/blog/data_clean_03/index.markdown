---
title: "Cleaning sample data using R"
subtitle: 
excerpt: "Part 3 in a series on creating a data cleaning workflow"
author: "Crystal Lewis"
date: "2023-02-16"
draft: true
# layout options: single, single-sidebar
layout: single
categories:
- tutorials
tags:
- data cleaning
- rstats
- data management
- data sharing
---

In this final post of the series, I attempt to answer the question, "What does this data workflow look like when implemented in the real world?". To tackle this question I created a very simple sample dataset based on the following fictional scenario:

I am managing data for a longitudinal RCT. For this RCT, schools are randomized to either a treatment or control group where students receive additional services to help improve math comprehension. For this fictitious study, data is collected in two waves (wave 1 is in the fall of the 2022-23 school year, and wave 2 is collected in the spring). At this point in time, we have collected wave 1 of our student survey on a paper form and we set up a data entry form for staff to enter the information into. Data has been [double-entered], checked for entry errors, and has been exported to a folder where it is waiting to be cleaned.

Let me be the first to say here, I am NOT a math person. I did not study math, nor do I have any understanding of specific math interventions. I simply picked a subject and created some completely fictional data for the purposes of demonstrating some data cleaning techniques. So please do not take this scenario, or the data, too seriously. :)

## Doing the prep work

As we learned in the previous [post]() there are several steps I need to take before I begin cleaning my data.

1. My team has created a style guide. This guide lays out how my team should structure our directories, name files and folders, as well as name and code variables.
2. My team has set up a folder structure following the directions in our style guide and we have named files based on the style guide as well.
3. My data dictionary is created and it also follows any rules laid out in our style guide.
4. My data cleaning plan has been created and my team has approved the transformations I plan to make.
5. Last, I have checked the data folder for any readme files and I have found that there is one readme to review.

So let's review all of our documents.

**Data dictionary**

![](img/dict.PNG)

**Data cleaning plan**

<img src="img/data_cleaning_plan.PNG" width="70%" style="display: block; margin: auto;" />

**Readme**

<img src="img/readme.PNG" width="50%" style="display: block; margin: auto;" />

It looks like I have all of the documents I need and I am ready to start the data cleaning process!

## Data cleaning process

The remainder of this post will provide an overview of how I would clean this data writing code using the R programming language, integrating the steps we spoke about in the previous blog post that help ensure that our data is reproducible and reliable. Following the steps laid out in my data cleaning plan, I can start the process.

A sample of our raw data, along with a sampling of our data cleaning syntax are seen below. The actual sample data and the full sample syntax can be accessed in this GitHub repository.

**Our raw data**


| stu_id|school              |dob        |svy_date   | grade_level|study | curricula1| curricula2| curricula3| math1|math2 | math3| math4|
|------:|:-------------------|:----------|:----------|-----------:|:-----|----------:|----------:|----------:|-----:|:-----|-----:|-----:|
|   1347|hickman             |2008-05-14 |2022-10-01 |           9|40%   |          1|         NA|          1|     2|1     |     3|     3|
|   1368|Hickman high school |2007-03-08 |2022-10-01 |          10|30%   |         NA|          1|         NA|     3|2     |     2|     2|
|   1377|Rockbridge          |2007-12-11 |2022-10-01 |           9|10%   |         NA|         NA|         NA|     4|4     |     4|     4|
|   1387|RBS                 |2006-06-19 |2022-10-02 |          11|11%   |         NA|          1|         NA|     3|3     |    NA|    NA|
|   1347|Hickman high        |2008-04-22 |2022-10-02 |           9|20%   |          1|         NA|         NA|     2|2     |     4|     2|
|   1399|Rockbridge          |2005-11-19 |2022-10-01 |          12|0%    |         NA|          1|          1|     4|1     |     3|     1|


**A sample of our data cleaning syntax**


```r
## Data cleaning date: 2023-02-14
## Cleaned by: Crystal Lewis
## Project: Math intervention project
## Wave: Wave 1
## Data: Student Survey Data

## libraries ##
library(tidyverse)
library(janitor)
library(lubridate)
library(stringr)

# Read in the data ----

svy <- read_csv("data/w1_mathproj_stu_svy_raw.csv")

# (01) Review the data ----

## I have 6 rows and 13 columns
## After checking our participant tracking, I see that only 5 students completed the student survey in wave 1
## I also see that the variable types for `study` and for `math2` do not match my data dictionary. My data cleaning plan accounts for the errors in `study` but the errors in `math2` are new. I will need to add this to my data cleaning plan.

glimpse(svy)

# (02) Adjust cases as needed ----

## Check for duplicates - 1347 is duplicated

svy %>%
  get_dupes(stu_id)

## Check - Are both surveys complete? - Yes

svy %>%
  filter(stu_id == 1347 & !is.na(math4))

## Remove duplicates - `distinct()` keeps the first instance of an identifier

svy <- svy %>%
  arrange(svy_date) %>%
  distinct(stu_id, .keep_all = TRUE)

## Check - Review data after dropping the duplicates

svy

# (07) Normalize variables ----

### Fix `study`

## Check - Review before

svy %>%
  tabyl(study)

## Remove %

svy <- svy %>%
  mutate(study = str_remove(study, "%"))

## Check - Review after

svy %>%
  tabyl(study)

### Fix `math2`

## Check - Review before - there is a return in one value

svy %>%
  tabyl(math2)

## Remove "\n"

svy <- svy %>%
  mutate(math2 = str_remove(math2, "\n"))

## Check - Review after

svy %>%
  tabyl(math2)

# (08) Standardize variables ----

## Recode `school` with consistent spellings

## Check - Review before

svy %>%
  tabyl(school)

# Recode `school`

svy <- svy %>%
  mutate(school = str_to_lower(school),
         school = case_when(
    str_detect(school, "hickman") ~ "Hickman High",
    str_detect(school, "rock|rbs") ~ "Rockbridge High"
  ))

## Check - Review after

# (09) Update variable types ----

## Update both `study` and `math2` to numeric

## Check - Review before

svy %>%
  select(study, math2) %>%
  map(class)

# Change variable types - pay close attention to warnings here - if warnings are thrown there may still be characters in the variable

svy <- svy %>%
  mutate(across(c(study, math2), as.numeric))

## Check - Review after

svy %>%
  select(study, math2) %>%
  map(class)
```


1. I start with a coding template that helps standardize the information that is provided in each data cleaning script. Then I will read in my data. My data is housed in a folder called "data" and it is named "w1_mathproj_stu_svy_raw.csv". I read in my data using a relative path.

2. I will review my data.

I have 6 rows and 13 columns. I already know that I potentially have duplicate rows because my `readme` told me to expect that. When I review our project `participant tracking database`, I can see that 5 students are tracked as having completed the wave 1 survey, so I know that I will need to fix this in my next step.

When I compare my number of columns to those in my data dictionary I see that my dictionary has 16 variables, but 3 of them are "calculated" so this information adds up for me. All variable names are already correct.

I start to compare my variable types and everything is making sense except my `study` variable and my `math2` variables are not my expected variable type. I was prepared for the `study` variable and already made plans to correct this later in my process. However, I was not prepared to correct `math2` so I will need to add this correction to my data cleaning plan.

3. Next I need to adjust cases

I see that ID 1347 occurs twice in our data, as the `readme` informed me. Next, I would go to my `study protocol` to see what our rule is for removing duplicates. In this case, our rule is to keep the first, complete case of a form. So I will drop my duplicates accordingly. **Remember**, never drop your duplicates randomly. Make sure your data is arranged first before dropping cases.


3. Next I plan to transform my `study` variable.



