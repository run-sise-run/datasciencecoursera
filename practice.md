---
output: pdf_document
---
Practice Assignment
========================================================

The goal of this assignment is to provide a "bridge" between the first two weeks of lectures and assignment 1 for those either new to R or struggling with how to approach the assignment.

This guided example, will **not** provide a solution for programming assignment 1.  However, it will guide you through some core concepts and give you some practical experience to hopefully make assignment 1 seems less daunting.

To begin, download this file and unzip it into your R working directory.  
http://s3.amazonaws.com/practice_assignment/diet_data.zip

You can do  this in R with the following code:
```{r}
dataset_url <- "http://s3.amazonaws.com/practice_assignment/diet_data.zip"
download.file(dataset_url, "diet_data.zip")
unzip("diet_data.zip", exdir = "diet_data")
```


If you're not sure where your working directory is, you can find out with the `getwd()` command.  Alternatively, you can view/change it through the Tools > Global Options menu in R Studio.

So assuming you've unzipped the file into your R directory, you should have a folder called diet_data.  In that folder there are five files.  Let's get a list of those files:

```{r}
list.files("diet_data")
```

Okay, so we have 5 files.  Let's take a look at one to see what's inside:

```{r}
andy <- read.csv("diet_data/Andy.csv")
head(andy)
```

It appears that the file has 4 columns, Patient.Name, Age, Weight, and Day.  Let's figure out how many rows there are by looking at the length of the 'Day' column:

```{r}
length(andy$Day)
```

30 rows.  OK.

Alternatively, you could look at the dimensions of the data.frame:
```{r}
dim(andy)
```

This tells us that we 30 rows of data in 4 columns.  There are some other commands we might want to run to get a feel for a new data file, `str()`, `summary()`, and `names()`.

```{r}
str(andy)
summary(andy)
names(andy)
```

So we have 30 days of data.  To save you time, all of the other files match this format and length.  I've made up 30 days worth of weight data for 5 subjects of an imaginary diet study.

Let's play around with a couple of concepts.  First, how would we see Andy's starting weight?  We want to subset the data.  Specifically, the first row of the 'Weight' column:
```{r}
andy[1, "Weight"]
```

We can do the same thing to find his final weight on Day 30:
```{r}
andy[30, "Weight"]
```

Alternatively, you could create a subset of the 'Weight' column where the data where 'Day' is equal to 30.
```{r}
andy[which(andy$Day == 30), "Weight"]
andy[which(andy[,"Day"] == 30), "Weight"]
```

Or, we could use the `subset()` function to do the same thing:
```{r}
subset(andy$Weight, andy$Day==30)
```



There are lots of ways to get from A to B when using R.  However it's important to understand some of the various approaches to subsetting data.

Let's assign Andy's starting and ending weight to vectors:
```{r}
andy_start <- andy[1, "Weight"]
andy_end <- andy[30, "Weight"]
```

We can find out how much weight he lost by subtracting the vectors:
```{r}
andy_loss <- andy_start - andy_end
andy_loss
```

Andy lost 5 pounds over the 30 days.  Not bad.  What if we want to look at other subjects or maybe even everybody at once?  

Let's look back to the `list.files()` command.  It returns the contents of a directory in alphabetical order.  You can type `?list.files` at the R prompt to learn more about the function.

Let's take the output of `list.files()` and store it:
```{r}
files <- list.files("diet_data")
files
```

Knowing that 'files' is now a list of the contents of 'diet_data' in alphabetical order, we can call a specific file by subsetting it:
```{r}
files[1]
files[2]
files[3:5]
```

Let's take a quick look at John.csv:
```{r error=TRUE}
head(read.csv(files[3]))
```

Woah, what happened?  Well, John.csv is sitting inside the diet_data folder.  We just tried to run the equivalent of `read.csv("John.csv")` and R correctly told us that there isn't a file called John.csv in our working directory.  To fix this, we need to append the directory to the beginning of the file name.

One approach would be to use `paste()` or `sprintf()`.  However, if you go back to the help file for `list.files()`, you'll see that there is
