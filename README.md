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

Scatterplot:

```
# R
mtcars %>%
  ggplot(aes(drat, wt)) +
    geom_point()

# Python
sns.scatterplot(data = mtcars, x = "drat", y = "wt")
```

Scatterplot with a categorical variable mapped to color:

```
# R
mtcars %>%
  ggplot(aes(drat, wt, color = as.character(cyl))) +
  geom_point() +
  scale_color_manual(values = c("red", "yellow", "blue"))

# Python
hue_colors = {4:"red", 6:"yellow", 8: "blue"}
sns.scatterplot(data = mtcars, x = "drat", y = "wt", hue = "cyl", hue_order = [4, 6, 8], palette = hue_colors)
```

Scatterplot split into subplots by levels of a categorical variable:

```
# R
ggplot(iris, aes(Sepal.Length, Sepal.Width)) +
  geom_point() +
  facet_wrap(~Species)


# Python
sns.relplot(data = "iris", x = "Sepal.Length", y = "Sepal.Width", kind = "scatter", col = "Species", col_order = ["virginica", "versicolor", "setosa])
```

We can also map variables to "size", "hue", or "style" in sns.relplot. We can set the alpha to make it more transparent.

Plotting a scatterplot with seaborn is not the best option when using a continuous color scale because it discretizes the values. Thus, we need to resort to matplotlibs flexibility. Let's say that we have an AnnData object with precomputed QC metrics, and we want to plot number of detected genes VS library size colored by % of mitochondrial exprssion. With matplotlib:

```
# Python
scatterplot = plt.scatter(
    adata.obs["total_counts"],
    adata.obs["n_genes_by_counts"],
    c=adata.obs["pct_counts_mt"],
    s = 0.75,
    alpha = 0.85,
    cmap='inferno'
)
colorbar = plt.colorbar(scatterplot)
colorbar.set_label("Percentage of mitochondrial expression (%)")
plt.xlabel("Library size (total UMI)")
plt.ylabel("Number of detected genes")
plt.show()
```

