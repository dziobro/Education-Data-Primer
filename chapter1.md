---
title       : Getting Started With Data and dplyr
description : This chapter will introduce you to R and dplyr and how to work with and process data.

--- type:NormalExercise lang:r xp:100 skills:1 key:7dc7c83d61
## Let's get started

In this lesson we will look at dplyr which is an R library that will allow us to work more efficiently with data. R and R studio can be downloaded and allow local development of your projects.
In this example we will use a Web Based environment to simplify getting started.
All examples can be run locally in R with R studio.


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

We want to 


*** =instructions
## Let's Go!

The student data is loaded into StudentData from the pervious exercise.
1) load the tidyr libary.
2) Gather the data into a new variable GatheredStudentData 
3) Remove NA's


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
GatheredStudentData <-StudentData %>% gather(Indicator,Value, -SID,-First,-Last)

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

```



--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:172ffa9438
## Dive into the data
Use the code window to examine and rehape the data to answer the question

The data has been read in and loaded into the variable GatheredStudentData.

Use a combination of the following commands to examine the data and answer the question.

`GatheredStudentData %>% filter(Indicator=="ELA06") %>% arrange(desc(Value))`

Which student had the lowest MAT07 score? 
*** =instructions
- 516223     Peter      King
- 990079      Erma     Nunez
- 364560     Bryan    Chavez 
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
test_mc(1)
```


--- type:NormalExercise lang:r xp:100 skills:1 key:305990cdbc
## Create table of data for the sheet or plot
You are going to create a table of data that will be used in the next exersise to create visuals.
- Filter the data to include only the students in section 1
- Find all available data for those students
- Filter the data to include only PARCC assessments (ELA??,MAT??, ALG01, ALG02, GEO01)
- Spead the data into a table

Data is loaded from previous exercise into GatheredStudentData
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
    spread(Indicator,Value)


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
    filter(Indicator %in% c("ELA02","ELA03","ELA04","ELA05","ELA06","ELA07","ELA08","ELA09","ELA10","ELA11",
                            "MAT02","MAT03","MAT04","MAT05","MAT06","MAT07","MAT08","ALG01","GEO01","ALG02"))         

#Spread Data Back out
SpreadFilteredData <- FilteredStudentDatainSection %>%
    spread(Indicator,Value)


```

*** =sct
```{r}

```



--- type:NormalExercise lang:r xp:100 skills:1 key:153a069111
## Ploting Student Data
 Create a bar plot of the student data showing all highschool math PARCC assessments.
 
 SpreadFilteredData is loaded from the previous exercise.

*** =instructions
 Use the existing code and add a trace for the missing assement. (ALG02)
 After creating the graph explore the interactivity on the graph.
 - Click on a trace.
 - Hover over a bar.
 - Click and drag a window
 - 
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
- Run the code
- Hover over the results and explore by zooming and turning on and off traces

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
--- type:NormalExercise lang:r xp:100 skills:1 key:75463b9466
## Plotly diamonds are forever

You'll use several datasets throughout the tutorial to showcase the power of plotly. In the next exercises you will make use of the [`diamond`](https://www.rdocumentation.org/packages/ggplot2/versions/2.1.0/topics/diamonds) dataset. A dataset containing the prices and other attributes of 1000 diamonds. 

<center><img src="http://www.picgifs.com/glitter-gifs/d/diamonds/animaatjes-diamonds-61528.gif" alt="Diamonds" height="100px"></center>

Don't forget: 

You're encouraged to think about how the examples can be applied to your own data-sets! Also, Plotly graphs are interactive. So make sure to experiment a bit with your plot: click-drag to zoom, shift-click to pan, double-click to autoscale.

*** =instructions
- `plotly` has already been loaded for you. 
- Take a look at the first `plot_ly()` graph. It plots the `carat` (FYI: the carat is a unit of mass. Hence it gives info on the weight of a diamond,) against the `price` (in US dollars). You don't have to change anything to this command. Tip: note the `~` syntax. 
- In the second call of `plot_ly()`, change the `color` argument. The color should be dependent on the weight of the diamond.
- In the third call of `plot_ly()`, change the `size` argument as well. The size should be dependent on the weight of the diamond.

*** =hint
- The second argument of the second `plot_ly()` should contain argument `color` set to `carat`. 

*** =pre_exercise_code
```{r}
library(plotly)
library(ggplot2)
diamonds <- diamonds[sample(nrow(diamonds), 1000), ]
```

*** =sample_code
```{r}
# The diamonds dataset
str(diamonds)

# A firs scatterplot has been made for you
plot_ly(diamonds, x = ~carat, y = ~price)

# Replace ___ with the correct vector
plot_ly(diamonds, x = ~carat, y = ~price, color = ~___)
        
# Replace ___ with the correct vector
plot_ly(diamonds, x = ~carat, y = ~price, color = ~___, size = ~___)
```

*** =solution
```{r}
# The diamonds dataset
str(diamonds)

# A firs scatterplot has been made for you
plot_ly(diamonds, x = ~carat, y = ~price)

# Replace ___ with the correct vector
plot_ly(diamonds, x = ~carat, y = ~price, color = ~carat)
        
# Replace ___ with the correct vector
plot_ly(diamonds, x = ~carat, y = ~price, color = ~carat, size = ~carat)
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

# Test str function
msg <-  "Call [`str()`](http://www.rdocumentation.org/packages/utils/functions/str) with the `diamonds` dataset as an argument."
test_function("str", "object", not_called_msg = msg, incorrect_msg = msg)

# Test first plotly function
test_function("plot_ly", args = c("data","x","y"),
              not_called_msg = "Have you used `plot_ly()` 3 times for 3 different graphs?",
              index = 1,
              args_not_specified = c("Have you correctly specified that `data` should be `diamonds`?",
                                "Have you correctly specified that `x` should be `carat`?",
                                "Have you correctly specified that `y` should be `price`?"))

# Test second plotly function
test_function("plot_ly", args = c("data","x","y","color"),
              not_called_msg = "Have you used `plot_ly()` 3 times for 3 different graphs?",
              index = 2,
              args_not_specified = c("Have you correctly specified that `data` should be `diamonds`?",
                                "Have you correctly specified that `x` should be `carat`?",
                                "Have you correctly specified that `y` should be `price`?",
                                "Have you correctly specified that `color` should depend on `carat`?"))

# Test third plotly function
test_function("plot_ly", args = c("data","x","y","color","size"),
              not_called_msg = "Have you used `plot_ly()` 3 times for 3 different graphs?",
              index = 3,
              args_not_specified = c("Have you correctly specified that `data` should be `diamonds`?",
                                "Have you correctly specified that `x` should be `carat`?",
                                "Have you correctly specified that `y` should be `price`?",
                                "Have you correctly specified that `color` should depend on `carat`?",
                                "Have you correctly specified that `size` should depend on `carat`?"))
                                
success_msg("Wow. Those are some nice looking plots! You are a natural.")

```

--- type:NormalExercise lang:r xp:100 skills:1 key:97ba0a444c
## The interactive bar chart

You've likely encountered a bar chart before. With plotly you can now turn those dull, basic bar charts into interactive masterpieces!

You will work again with the `diamonds` dataset. The goal is to create a bar chart that buckets our diamonds based on quality of the `cut`. Next, for each cut, you want to see how many diamonds there are for each `clarity` variable.  

Exciting!

*** =instructions
- The `plotly` and `dplyr` package are already loaded in.
- Calculate the number of diamonds for each cut/clarity combination using the `count()` function from the [`dplyr`]((https://www.rdocumentation.org/packages/dplyr/versions/0.5.0)) package. Assign the result to `diamonds_bucket`. 
- Create a chart of type `"bar"`. The `color` of the bar depends on the `clarity` of the diamond. Bucket your diamonds by the `cut` over the x-axis. 


*** =hint
- Calculate the numbers of diamonds for each cut/clarity using `count(cut, clarity)`. (Not familiar with dplyr? Check [our course](https://www.datacamp.com/courses/dplyr-data-manipulation-r-tutorial)).
- Indicate you want a bar chart in plotly using `type= "bar"`

*** =pre_exercise_code
```{r}
library(plotly)
library(ggplot2)
library(dplyr)
diamonds <- diamonds[sample(nrow(diamonds), 1000), ]
```

*** =sample_code
```{r}

# Calculate the numbers of diamonds for each cut<->clarity combination
diamonds_bucket <- diamonds %>% count(___, ___)

# Replace ___ with the correct vector
plot_ly(diamonds_bucket, x = ___, y = ~n, type = ___, color = ___) 

```

*** =solution
```{r}

# Calculate the numbers of diamonds for each cut<->clarity combination
diamonds_bucket <- diamonds %>% count(cut, clarity)

# Replace ___ with the correct vector
plot_ly(diamonds_bucket, x = ~cut, y = ~n, type = "bar", color = ~clarity) 

```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

# Test dplyr function
test_error()       

test_function_result("count", 
                     incorrect_msg = paste("Have you correctly performed the count operation?",
                                           "Make sure this is the `dplyr` verb you call on `diamonds`."))


# Test first plotly function
test_function("plot_ly", args = c("data","x","y","type","color"),
              not_called_msg = "Have you used `plot_ly()` to create a bar chart?",
              index = 1,
              args_not_specified = c("Have you correctly specified that `data` should be `diamonds_bucket`?",
                                "Have you correctly specified that `x` should be `cut`?",
                                "Have you correctly specified that `y` should be `n`?",
                                "Have you correctly specified that `type` should be `bar`?",
                                "Have you correctly specified that `color` should be `clarity`?"
                                ))
                                
success_msg("Well done. Time to move from the bar to the box...")

```
--- type:NormalExercise lang:r xp:100 skills:1 key:126082cf3d
## From the bar to the box: the box plot

In the final exercise of this chapter, you will make an interactive box plot in R. 

Using plotly, you can create box plots that are grouped, colored, and that display the underlying data distribution. The code to create a simple box plot using plotly is provided on your right. 

Note how you use `type= "box"` in the function `plot_ly()` to create a box plot. Make sure to run the code (`plotly` is already loaded in). 
 
*** =instructions
- Create a second, more fancy, box plot using `diamonds`. The y-axis should represent the `price`. The color should depend on the `cut`.
- Create a third box plot where you bucket the diamonds not only by `cut` but also by `clarity`. The color should depend on the `clarity` of the diamond.  


*** =hint
- For the third box plot the `x` argument should depend on the `cut`.

*** =pre_exercise_code
```{r}
library(plotly)
library(ggplot2)
library(dplyr)
diamonds <- diamonds[sample(nrow(diamonds), 1000), ]
```

*** =sample_code
```{r}

# The Non Fancy Box Plot
plot_ly(y = ~rnorm(50), type = "box")

# The Fancy Box Plot
plot_ly(diamonds, y = ___, color = ___, type = ___)

# The Super Fancy Box Plot
plot_ly(diamonds, x = ___, y = ___, color = ___, type = ___) %>%
  layout(boxmode = "group")
  
```

*** =solution
```{r}

# The Non Fancy Box Plot
plot_ly(y = ~rnorm(50), type = "box")

# The Fancy Box Plot
plot_ly(diamonds, y = ~price, color = ~cut, type = "box")

# The Super Fancy Box Plot
plot_ly(diamonds, x = ~cut, y = ~price, color = ~clarity, type = "box") %>%
  layout(boxmode = "group")
  
```

*** =sct
```{r}
# Test first plotly function
test_function("plot_ly", index = 1, args = c("y","type"), incorrect_msg = c("You don't have to change the [`plot_ly`](https://www.rdocumentation.org/packages/plotly/versions/4.5.2/topics/plotly) command, it was predefined for you.","You don't have to change the [`plot_ly`](https://www.rdocumentation.org/packages/plotly/versions/4.5.2/topics/plotly) command, it was predefined for you."))

# Test second plotly function
test_function("plot_ly", args = c("data","y","color","type"),
              not_called_msg = "Have you used `plot_ly()` 3 times for 3 different graphs?",
              index = 2,
              args_not_specified = c("Have you correctly specified that `data` should be `diamonds`?",
                                "Have you correctly specified that `y` should be `price`?",
                                "Have you correctly specified that `color` should depend on `cut`?",
                                "Make sure to let plotly know you need a plot of type box?"))
                                
# Test third plotly function
test_function("plot_ly", args = c("data","x","y","color","type"),
              not_called_msg = "Have you used `plot_ly()` 3 times for 3 different graphs?",
              index = 3,
              args_not_specified = c("Have you correctly specified that `data` should be `diamonds`?",
                                "Have you correctly specified that `x` should be `cut`?",
                                "Have you correctly specified that `y` should be `price`?",
                                "Have you correctly specified that `color` should depend on `clarity`?",
                                "Make sure to let plotly know you need a plot of type box?"))

test_function("layout", args = c("boxmode"),
                     incorrect_msg = paste("No need to change `layout()`!"))
                                           
success_msg("You really aced this chapter. Time to level up.")

```
