---
title       : Getting Started With Data and dplyr
description : This chapter will introduce you to R and dplyr and how to work with and process data.

--- type:NormalExercise lang:r xp:100 skills:1 key:7dc7c83d61
## Let's get started

In this lesson we will look at dplyr which is an R library that will allow us to work more efficiently with data. R and R studio can be downloaded and allow local development of your projects.
In this example we will use a Web Based environment to simplify getting started.
All examples can be run locally in R with R studio.

In R `library()` is used to load packages into the environment. For example `library(plot_ly)' would load a package that helps use visualize data.

If you highlight a line of code you can run a single line and see the results.

*** =instructions
- Load the dplyr package.
- Load the sample student data.
- Click *Submit Answer* to run the code.

*** =hint
- use `library()` to load the dplyr package
 
*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# load the dplyr package

# Load sample student data
StudentData<-read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_1959/datasets/SampleClassData.csv")
# Dump the student data
StudentData
```
*** =solution
```{r}
# load the dplyr package
library(dplyr)
# Load sample student data
StudentData<-read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_1959/datasets/SampleClassData.csv")
# Dump the student data
StudentData
```
*** =sct
```{r}
test_library_function("dplyr")

test_error()
success_msg("That was not that hard. Now it is time to work with that data.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:a8e82e24f1
## Time to make the data tidy.
In tidy data:
- Each variable forms a column.
- Each observation forms a row.
- Each type of observational unit forms a table.

We want to clean up our student data.

You can pass data into a new variable using `<-` for example `x <- 2`

*** =instructions
## Let's Go!

The student data was loaded into a table `StudentData` from the pervious exercise. For this exercise:
- Load the tidyr libary
- Gather the data into a new variable `GatheredStudentData` 
- Remove NA's `na.omit()`


*** =hint

*** =pre_exercise_code
```{r}
# load the dplyr package
library(dplyr)
StudentData<-read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_1959/datasets/SampleClassData.csv")

```

*** =sample_code
```{r}
#Load the tidyr library

#Gather the data
_______________ <-StudentData %>% gather(Indicator,Value, -SID,-First,-Last)

# Remove NA's
GatheredStudentData <- GatheredStudentData %>% _____

# Dump the student data
glimpse(GatheredStudentData)

```

*** =solution
```{r}
#Load the tidyr library
library(tidyr)
#Gather the data
GatheredStudentData <-StudentData %>% gather(Indicator,Value, -SID,-First,-Last)

# Remove NA's
GatheredStudentData <- GatheredStudentData %>% na.omit()

# Dump the student data
glimpse(GatheredStudentData)

```

*** =sct
```{r}
test_library_function("tidyr")

ex() %>%
  check_function("na.omit", not_called_msg = "Make sure to use `na.omit` to clearn the data")

success_msg("Great Job!! Notice the data table has been organized and the glimpse command shows you the column names and the first couple of values. Look how the table has been condensed into Indicator and Value columns.")
```



--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:172ffa9438
## Dive into the data
Use the console window to examine the data to answer the question.

The data has been read in and loaded into the variable `GatheredStudentData`.

Run the following command and examine the output.

`GatheredStudentData %>% filter(Indicator=="ELA06") %>% arrange(desc(Value))`

Take note of the Indicator and Value columns.
Modify your command to answer the question below. 

Which student had the lowest MAT07 score? 
*** =instructions
- 990079      Erma     Nunez
- 364560     Bryan    Chavez 
- 516223     Peter      King
- 554694      Eula    Ingram 


*** =hint
Use a combination of the following commands to examine the data and answer the question.

GatheredStudentData %>% filter(Indicator=="MAT06) %>% arrange(desc(Value))
*** =pre_exercise_code
```{r}
#Load the tidyr and dplyr library
library(tidyr)
library(dplyr)

#READ AND CLEAN
#Read in the Student Data
StudentData<-read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_1959/datasets/SampleClassData.csv")
#Gather the data
GatheredStudentData <-StudentData %>% gather(Indicator,Value, -SID,-First,-Last)
# Remove NA's
GatheredStudentData <- GatheredStudentData %>% na.omit()

```

*** =sct
```{r}
test_mc(2)
```


--- type:NormalExercise lang:r xp:100 skills:1 key:305990cdbc
## Create table of data for the sheet or plot
You are going to create a table of data that will be used in the next exercise to create visuals.
But first we need to find all available data for those students and filter to what we interested in.
Data is loaded from previous exercise into `GatheredStudentData`

- Filter the data to include only the students in section 1
- Find all available data for those students
- Change the code to Filter only math PARCC assessments (MAT??, ALG01, ALG02, GEO01)
- Spread the data into a table



*** =instructions
Complete the following tasks:
- Modify the code to Filter the data to so that we only include only the students in section 1
- Change the code to Filter only math PARCC assessments (MAT??, ALG01, ALG02, GEO01)
- Spread the data into a table

*** =hint

*** =pre_exercise_code
```{r}
#Load the tidyr and dplyr library
library(tidyr)
library(dplyr)

#READ AND CLEAN
#Read in the Student Data
StudentData<-read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_1959/datasets/SampleClassData.csv")
#Gather the data
GatheredStudentData <-StudentData %>% gather(Indicator,Value, -SID,-First,-Last)
# Remove NA's
GatheredStudentData <- GatheredStudentData %>% na.omit()
```

*** =sample_code
```{r}
# Generate a List of the SID's we are looking for
FilteredStudents <- GatheredStudentData %>% 
    filter(Indicator=="X...SectionID",Value=="_____") %>% #Filter Students in Section 1
    select(SID) #Select only the SID field

# Find all Available data on Students
AllStudentDataInSection <- FilteredStudents %>%
    left_join(GatheredStudentData,by="SID") #Links two tables all records from Filtered Students and those from GatheredStudentData where the SID matches
    
# Filter Data for only PARCC assessments
FilteredStudentDatainSection <- AllStudentDataInSection %>%
    filter(Indicator %in% c("ELA02","ELA03","ELA04","ELA05","ELA06","ELA07","ELA08","ELA09","ELA10","ELA11",
                            "MAT02","MAT03","MAT04","MAT05","MAT06","MAT07","MAT08","ALG01","GEO01","ALG02"))         

#Spread Data Back out
SpreadFilteredData <- FilteredStudentDatainSection %>%
    ______(Indicator,Value)

SpreadFilteredData
```

*** =solution
```{r}
# Generate a List of the SID's we are looking for
FilteredStudents <- GatheredStudentData %>% 
    filter(Indicator=="X...SectionID",Value=="1") %>% #Filter Students in Section 1
    select(SID) #Select only the SID field

# Find all Available data on Students
AllStudentDataInSection <- FilteredStudents %>% 
    left_join(GatheredStudentData,by="SID") #Links two tables all records from Filtered Students and those from GatheredStudentData where the SID matches
    
# Filter Data for only PARCC assessments
FilteredStudentDatainSection <- AllStudentDataInSection %>%
    filter(Indicator %in% c("MAT02","MAT03","MAT04","MAT05","MAT06","MAT07","MAT08","ALG01","GEO01","ALG02"))         

#Spread Data Back out
SpreadFilteredData <- FilteredStudentDatainSection %>%
    spread(Indicator,Value)

SpreadFilteredData
```

*** =sct
```{r}
test_object("AllStudentDataInSection", 
            undefined_msg = "You need to create AllStudentDataInSection",
            incorrect_msg = "AllStudentDataInSection is not correct. Check your filter for the section.") 
 
test_object("SpreadFilteredData", 
            undefined_msg = "You need to create SpreadFilteredData",
            incorrect_msg = "SpreadFilteredData is not correct. Check your filter.") 
           
```



--- type:NormalExercise lang:r xp:100 skills:1 key:153a069111
## Ploting Student Data
 Now that we organized our data we can use that data to generate a visualization. 
 
 `SpreadFilteredData` is loaded from the previous exercise.

*** =instructions
Examine the existing code.
- Add a trace for the missing assement (ALG02).
- Run the code to create the graph. 

After creating the graph explore the interactivity on the graph.
 - Click on a trace.
 - Hover over a bar.
 - Click and drag a window.
*** =hint
  add_trace(y=~ALG02,name="ALG02")

*** =pre_exercise_code
```{r}
#Load the tidyr and dplyr library
library(tidyr)
library(dplyr)

#READ AND CLEAN
#Read in the Student Data
StudentData<-read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_1959/datasets/SampleClassData.csv")
#Gather the data
GatheredStudentData <-StudentData %>% gather(Indicator,Value, -SID,-First,-Last)
# Remove NA's
GatheredStudentData <- GatheredStudentData %>% na.omit()
# Generate a List of the SID's we are looking for
FilteredStudents <- GatheredStudentData %>% 
    filter(Indicator=="X...SectionID",Value=="1") %>% #Filter Students in Section 1
    select(SID) #Select only the SID field

# Find all Available data on Students
AllStudentDataInSection <- FilteredStudents %>% 
    left_join(GatheredStudentData,by="SID") #Links two tables all records from Filtered Students and those from GatheredStudentData where the SID matches
    
# Filter Data for only PARCC assessments
FilteredStudentDatainSection <- AllStudentDataInSection %>%
    filter(Indicator %in% c("ELA02","ELA03","ELA04","ELA05","ELA06","ELA07","ELA08","ELA09","ELA10","ELA11",
                            "MAT02","MAT03","MAT04","MAT05","MAT06","MAT07","MAT08","ALG01","GEO01","ALG02"))         

#Spread Data Back out
SpreadFilteredData <- FilteredStudentDatainSection %>%
    spread(Indicator,Value)

```

*** =sample_code
```{r}
library(plotly)

plot_ly(SpreadFilteredData,y=~ALG01,name="ALG01",type="bar",x=~paste(Last,First,sep=", ")) %>%
 add_trace(y=~GEO01,name="Geo") %>%
 add_trace(___________________) %>%
 layout(yaxis = list("title"="Score",range=c(650,850)))
```

*** =solution
```{r}
library(plotly)

plot_ly(SpreadFilteredData,y=~ALG01,name="ALG01",type="bar",x=~paste(Last,First,sep=", ")) %>%
 add_trace(y=~GEO01,name="Geo") %>%
 add_trace(y=~ALG02,name="ALG02")%>%
 layout(yaxis = list("title"="Score",range=c(650,850)))
```

*** =sct
```{r}

```



--- type:NormalExercise lang:r xp:100 skills:1 key:deeec9af89
## Summerize Student Data
Here is a more complex example based on the data.
- Click Run Code.
- Hover over the results and explore by zooming and turning on and off traces.
- Experiment with the code by changing different components.

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
#Load the tidyr and dplyr library
library(tidyr)
library(dplyr)

#READ AND CLEAN
#Read in the Student Data
StudentData<-read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_1959/datasets/SampleClassData.csv")
#Gather the data
GatheredStudentData <-StudentData %>% gather(Indicator,Value, -SID,-First,-Last)
# Remove NA's
GatheredStudentData <- GatheredStudentData %>% na.omit()
# Generate a List of the SID's we are looking for
FilteredStudents <- GatheredStudentData %>% 
    filter(Indicator=="X...SectionID",Value=="1") %>% #Filter Students in Section 1
    select(SID) #Select only the SID field

# Find all Available data on Students
AllStudentDataInSection <- FilteredStudents %>% 
    left_join(GatheredStudentData,by="SID") #Links two tables all records from Filtered Students and those from GatheredStudentData where the SID matches
    
# Filter Data for only PARCC assessments
FilteredStudentDatainSection <- AllStudentDataInSection %>%
    filter(Indicator %in% c("ELA02","ELA03","ELA04","ELA05","ELA06","ELA07","ELA08","ELA09","ELA10","ELA11",
                            "MAT02","MAT03","MAT04","MAT05","MAT06","MAT07","MAT08","ALG01","GEO01","ALG02"))         

#Spread Data Back out
SpreadFilteredData <- FilteredStudentDatainSection %>%
    spread(Indicator,Value)
    


```

*** =sample_code
```{r}
library(plotly)

plot_ly(SpreadFilteredData,x=~ALG01,name="ALG01",type="box",jitter = 0.3, boxmean='sd',Text=~paste(Last,First,sep=", "),y="ALG01") %>%
 add_markers(x=~ALG01,y="ALG01",name="ALG01") %>%
 add_trace(x=~GEO01,name="GEO01",y="GEO01") %>%
 add_markers(x=~GEO01,y="GEO01",name="GEO01") %>%
 add_trace(x=~ALG02,name="ALG02",y="ALG02")%>%
 add_markers(x=~ALG02,y="ALG02",name="ALG02") %>%
 layout(xaxis = list("title"="Score",range=c(650,850))) 
```

*** =solution
```{r}

```

*** =sct
```{r}

```
