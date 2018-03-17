This is a note for learning R4DS from my big shen Hadley Wickham.

***

# 1 Introduction
> When your data is tidy, each column is a variable, and each row is an observation.

> Together, tidying and transforming are called wrangling, because getting your data in a form that’s natural to work with often feels like a fight!

> You will get better faster if you dive deep, rather than spreading yourself thinly over many topics. 

```r
library(tidyverse)
#> Loading tidyverse: ggplot2
#> Loading tidyverse: tibble
#> Loading tidyverse: tidyr
#> Loading tidyverse: readr
#> Loading tidyverse: purrr
#> Loading tidyverse: dplyr
```

Once upon a time, I thought `reshape2` was in this R package, but……

Below are some packages from outside the tidyverse, you can install packages using `c()`, too.
```r
install.packages(c("nycflights13", "gapminder", "Lahman"))
```


> In the book, output is commented out with #>; 

Google and Stackoverflow is very useful when you get stuck.

> For packages in the tidyverse, the easiest way to check is to run `tidyverse_update()`.

You need to make sure your code concise, yet informative.

> Investing a little time in learning R each day will pay off handsomely in the long run.

# 2 Explore
# 2.1 Introducaion
> Data exploration is the art of looking at your data, rapidly generating hypotheses, quickly testing them, then repeating again and again and again. 

You can get clear payoff from visualization.

# 3 Data visualization
> I have been writing R code for years, and every day I still write code that doesn’t work!
> "+ " has to come at the end of the line, not the start.

## Facets
facet_wrap(~c)
facet_grid(r~c)
you can use . in facet_grid()

## Geometric objects
You can use different dataset for different layers.
```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(data = filter(mpg, class == "subcompact"), se = FALSE)
```

## Statistical tranformations
you can using `stat_count() `instead of `geom_bar()`.


> This works because every geom has a default stat; and every stat has a default geom. This means that you can typically use geoms without worrying about the underlying statistical transformation. 

If the height of the bar is already present in the data, you can use `stat=‘identity’`
```r
ggplot(data = demo) +
  geom_bar(mapping = aes(x = cut, y = freq), stat = "identity")
```

You can use the below to display a bar chart of proportion.
```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = ..prop.., group = 1))
```

## Position adjustments
> The stacking is performed automatically by the **position adjustment** specified by the position argument. If you don’t want a stacked bar chart, you can use one of three other options: `identity`, `dodge` or `fill`.

> position = `jitter` adds a small amount of random noise to each point. This spreads the points out because no two points are likely to receive the same amount of random noise.

## Coordinate systems
> `coord_flip()` switches the x and y axes. This is useful (for example), if you want horizontal boxplots. It’s also useful for long labels: it’s hard to get them to fit without overlapping on the x-axis.

> `coord_polar()` uses polar coordinates. Polar coordinates reveal an interesting connection between a bar chart and a Coxcomb chart.

## The layered grammer of graphics
```r
ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(
     mapping = aes(<MAPPINGS>),
     stat = <STAT>, 
     position = <POSITION>
  ) +
  <COORDINATE_FUNCTION> +
  <FACET_FUNCTION>
```

# 4 Workflow:basics
> Frustration is natural when you start programming in R, because it is such a stickler for punctuation, and even one character out of place will cause it to complain. But while you should expect to be a little frustrated, take comfort in that it’s both typical and temporary: it happens to everyone, and the only way to get over it is to keep trying.
> Code is miserable to read on a good day, so giveyoureyesabreak and use spaces.

## Some tips
In console:
>type “this”, press TAB, add characters until you have a unique prefix, then press return.
> Type “this” then press Cmd/Ctrl + ↑. That will list all the commands you’ve typed that start those letters. 
> If you want more help, press F1 to get all the details in the help tab in the lower right pane.
> Press TAB once more when you’ve selected the function you want. 

> This common action can be shortened by surrounding the assignment with parentheses, which causes assignment and “print to screen” to happen.
```r
(y <- seq(1, 10, length.out = 5))
```

Press `Alt + Shift + K`,and you will get all keyboard shortcuts!Amazing!