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
colnames(mtcars)

# Python
mtcars.index # attribute, not method, returns an "Index" object
mtcars.columns
```

## Data wrangling

Arrange rows based on values of a specific variable (column):

```
# R
mtcars[order(mtcars$mpg), ] # base R
mtcars[order(mtcars$mpg, decreasing = TRUE), ]
arrange(mtcars, mpg) # dplyr (tidyverse) way
arrange(mtcars, desc(mpg))
arrange(mtcars, desc(cyl), mpg) # multiple variables

# Python
mtcars.sort_values("mpg", ascending = True)
mtcars.sort_values("mpg", ascending = False)
mtcars.sort_values(["cyl", "mpg"], ascending = [False, True])
```

## Plotting with seaborn

Importing seaborn and matplotlib conventions:

```
# Python
import matplotlib.pyplot as plt
import seaborn as sns
```

Plot a barplot of counts for a categorical variable:

```
# R
mtcars %>%
  group_by(cyl) %>%
  summarize(n = n()) %>%
  ggplot(aes(x = cyl, y = n)) +
    geom_col()

# Python
sns.countplot(data = mtcars, x = "cyl")
plt.show()
```



