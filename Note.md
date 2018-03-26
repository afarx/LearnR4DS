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

# Data transformation
## Introduction
 These describe the type of each variable:
```
int stands for integers.
dbl stands for doubles, or real numbers.
chr stands for character vectors, or strings.
dttm stands for date-times (a date + a time).
lgl stands for logical, vectors that contain only TRUE or FALSE.
fctr stands for factors, which R uses to represent categorical variables with fixed possible values.
date stands for dates.
```

Five key dplyr functions:
- Pick observations by their values (filter()).
- Reorder the rows (arrange()).
- Pick variables by their names (select()).
- Create new variables with functions of existing variables (mutate()).
- Collapse many values down to a single summary (summarise()).

These can all be used in conjunction with group_by() which changes the scope of each function from operating on the entire dataset to operating on it group-by-group. These six functions provide the verbs for a language of data manipulation.

All verbs work similarly:

- The first argument is a data frame.
- The subsequent arguments describe what to do with the data frame, using the variable names (without quotes).
- The result is a new data frame.

## filter()

> R provides the standard suite: >, >=, <, <=, != (not equal), and == (equal).

> Boolean operators yourself: & is “and”, | is “or”, and ! is “not”.

```
nov_dec <- filter(flights, month %in% c(11, 12))
```

## arrange()
```
arrange(flights, desc(arr_delay))
```
Missing values are always sorted at the end.

## select()
```
select(flights, -(year:day))
```

There are a number of helper functions you can use within select():

- starts_with("abc"): matches names that begin with “abc”.
- ends_with("xyz"): matches names that end with “xyz”.
- contains("ijk"): matches names that contain “ijk”.
- matches("(.)\\1"): selects variables that match a regular expression. This one matches any variables that contain repeated characters. You’ll learn more about regular expressions in strings.
- num_range("x", 1:3) matches x1, x2 and x3.

```
rename(flights, tail_num = tailnum)
```

You can use everything() to move a handful of variables into the start of the data frame.
```
select(flights, time_hour, air_time, everything())
```

## mutate()
It’s often useful to add new columns that are functions of existing columns. That’s the job of mutate().
If you only want to keep the new variables, use transmute().

Modular arithmetic: %/% (integer division) and %% (remainder)

> All else being equal, I recommend using log2() because it’s easy to interpret: a difference of 1 on the log scale corresponds to doubling on the original scale and a difference of -1 corresponds to halving.

> Offsets: lead() and lag() allow you to refer to leading or lagging values. They are most useful in conjunction with group_by()

> Cumulative and rolling aggregates: R provides functions for running sums, products, mins and maxes: cumsum(), cumprod(), cummin(), cummax(); and dplyr provides cummean() for cumulative means.

> Ranking: there are a number of ranking functions, but you should start with min_rank().If min_rank() doesn’t do what you need, look at the variants row_number(), dense_rank(), percent_rank(), cume_dist(), ntile(). See their help pages for more details.

## summarise()
> summarise() is not terribly useful unless we pair it with group_by(). 

**A good way to pronounce %>% when reading code is “then”.**

> You can use the pipe to rewrite multiple operations in a way that you can read left-to-right, top-to-bottom. 

> The only exception is ggplot2: it was written before the pipe was discovered. Unfortunately, the next iteration of ggplot2, ggvis, which does use the pipe, isn’t quite ready for prime time yet.

```r
flights %>% 
  group_by(year, month, day) %>% 
  summarise(mean = mean(dep_delay, na.rm = TRUE))
```

Remove the missing value:
```r
not_cancelled <- flights %>% 
  filter(!is.na(dep_delay), !is.na(arr_delay))
```

** A useful keyboard shortcut is Ctrl+Shift+P,you can resend the previously sent chunk from the editor to console.**

- Measures of location: we’ve used mean(x), but median(x) is also useful. 
- Measures of spread: sd(x), IQR(x), mad(x). 
- Measures of rank: min(x), quantile(x, 0.25), max(x). 
- Measures of position: first(x), nth(x, 2), last(x). 
- Counts: You’ve seen n(), which takes no arguments, and returns the size of the current group. To count the number of non-missing values, use sum(!is.na(x)). To count the number of distinct (unique) values, use n_distinct(x).

You can optionally provide a weight variable by using `wt=`

- Counts and proportions of logical values: sum(x > 10), mean(y == 0). This makes sum() and mean() very useful: sum(x) gives the number of TRUEs in x, and mean(x) gives the proportion.

- When you group by multiple variables, each summary peels off one level of the grouping. 

- If you need to remove grouping, and return to operations on ungrouped data, use ungroup().

# 6 Workflow: scripts

> Nevertheless, it’s a good idea to save your scripts regularly and to back them up.
You can press Ctrl+Shift+S to run execute the complete script.

>  It’s very antisocial to change settings on someone else’s computer!

# 7 Exploratory Data analysis
> EDA is a state of mind. 

You should ask two questions below:
- What type of variation occurs within my variables?
- What type of covariation occurs between my variables?

**You can use `count()` on discrete variables and count(cut_width(x,cutpoint))** on continous variables.**
The width measures the units of x variable.

> geom_freqpoly() performs the same calculation as geom_histogram(), but instead of displaying the counts with bars, uses lines instead. It’s much easier to understand overlapping lines than bars.

## Missing values
> You might want to compare the scheduled departure times for cancelled and non-cancelled times. You can do this by making a new variable with is.na().

```
nycflights13::flights %>% 
  mutate(
    cancelled = is.na(dep_time),
    sched_hour = sched_dep_time %/% 100,
    sched_min = sched_dep_time %% 100,
    sched_dep_time = sched_hour + sched_min / 60
  ) %>% 
  ggplot(mapping = aes(sched_dep_time)) + 
    geom_freqpoly(mapping = aes(colour = cancelled), binwidth = 1/4)
```

## Covariation
To make the trend easier to see, we can reorder class based on the median value of hwy
```
ggplot(data = mpg) +
  geom_boxplot(mapping = aes(x = reorder(class, hwy, FUN = median), y = hwy))
```

If you have long variable names, geom_boxplot() will work better if you flip it 90°. You can do that with coord_flip().
```
ggplot(data = mpg) +
  geom_boxplot(mapping = aes(x = reorder(class, hwy, FUN = median), y = hwy)) +
  coord_flip()
```

# 8 Workflow: projects

Never save workspace to .RData
There is a great pair of keyboard shortcuts that will work together to make sure you’ve captured the important parts of your code in the editor:
- Press Ctrl + Shift + F10 to restart RStudio.
- Press Ctrl + Shift + S to rerun the current script.

In summary, RStudio projects give you a solid workflow that will serve you well in the future:

- Create an RStudio project for each data analysis project.
- Keep data files there; we’ll talk about loading them into R in data import.
- Keep scripts there; edit them, run them in bits or as a whole.
- Save your outputs (plots and cleaned data) there.
- Only ever use relative paths, not absolute paths.

Everything you need is in one place, and cleanly separated from all the other projects that you are working on.


