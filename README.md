# R2Python

This repo documents how to translate common coding tasks from R to Python.


## Exploring a dataframe

Print the top columns

```
# R
head(mtcars)
head(mtcars, 10)

# Python
mtcars.head()
mtcars.head(10)
```

Getting information about the dataframe: number of rows and column, data type for each column...

```
# R
dim(mtcars)
str(mtcars)

# Python
mtcars.info()
mtcars.shape # shape is an attribute, not a method
```

Obtain summary statistics for quantitative variables:

```
# R
summary(mtcars)

# Python
mtcars.describe()
```

Set/Get row and column names from a dataframe:

```
# R
rownames(mtcars)
colnames(mtcars

# Python
mtcars.index # attribute, not method, returns an "Index" object
mtcars.columns
```
