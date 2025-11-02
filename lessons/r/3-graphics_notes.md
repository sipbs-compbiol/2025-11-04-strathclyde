# 2025-11-04-strathclyde 3. Creating Publication-Quality Graphics instructor notes

## Packages

- Packages are **THE FUNDAMENTAL UNIT OF REUSABLE CODE IN `R`**
- People write code, and **DISTRIBUTE IT IN PACKAGES**
- Packages exist for many **SPECIALIST AND USEFUL TOOLS**
- Over 10,000 packages can be found at CRAN - the Comprehensive R Archive Network
- When you write your own code, you can distribute it as a package

- **DEMO IN CONSOLE**
  - You can **SEE INSTALLED PACKAGES** with the function `installed.packages()`
  - To install a new package, use `install.packages("packagename")` as a string **EXPLAIN DEPENDENCIES**
  - **DEMO INSTALLATION IN `RStudio`: `Tools` $\rightarrow$ `Install packages...`**
  - **DEMO PACKAGE UPDATES IN `RStudio`**
  - You can update your installed packages to the newest version in the console with `update.packages()` **DON'T DO THIS - CAN TAKE TIME!**

```R
> installed.packages()
                  Package
BiocInstaller     "BiocInstaller"
bit               "bit"
bit64             "bit64"
data.table        "data.table"
[...]
> install.packages("dplyr")
Installing package into ‘/Users/lpritc/Library/R/3.4/library’
(as ‘lib’ is unspecified)
also installing the dependencies ‘bindrcpp’, ‘glue’, ‘rlang’
[...]
> update.packages(ask=FALSE)
> library(dplyr)
```

-----

## Challenge 07 (5min)

![images/red_green_sticky.png](images/red_green_sticky.png)

----

## 8. CREATING PUBLICATION-QUALITY GRAPHICS

-----

## Visualisation is Critical

- Visualisation **HELPS US UNDERSTAND OUR DATA**
- But **IT'S NOT FOOLPROOF** - people can interpret the same visualisation differently
- Good visualisation is **MORE THAN JUST USING A PLOTTING TOOL**

-----

## The Grammar of Graphics

- We'll be using the `ggplot2` package, which is part of the **TIDYVERSE**, created initially by Hadley Wickham.
  - The Tidyverse provides **OTHER USEFUL PACKAGES** but you can use `ggplot2` on its own

- `ggplot2` implements **A SET OF CONCEPTS CALLED THE GRAMMAR OF GRAPHICS**
  - This **SEPARATES DATA FROM THE WAY IT'S REPRESENTED** and we'll discuss it in detail
  - It's not the usual way you might have seen to create plots, but it's **highly effective for generating powerful visualisations**

-----

## A Basic Scatterplot

- You can use `ggplot2` in the **SAME WAY YOU'D USE BASE GRAPHICS**
  - This is not the best way to use all the power of the package

- **DEMO IN CONSOLE**
  - **IMPORT LIBRARY**
  - `ggplot2` has **`qplot()` - the equivalent to `plot()` in base graphics**
  - `plot()` takes `x` and `y` values, and will assign colours to `factor` columns
  - `qplot()` takes the name of `x` and `y` columns, plus the name of the source `data.frame`, and will assign colours to `factor` columns

```R
> library(ggplot2)
> plot(gapminder$lifeExp, gapminder$gdpPercap, col=gapminder$continent)
> qplot(lifeExp, gdpPercap, data=gapminder, colour=continent)
```

- **COMPARE THE GRAPHS**
  - Clearly, both graphs **show the same data**
  - The **FORMATTING IS QUITE DIFFERENT**
  - Your preference is your preference - **both methods can be heavily restyled**
  - My view is that **`ggplot2` has nicer default styles**
  - **`ggplot2` provides gridlines and legends by default, and the labelling is clearer** (no `gapminder$` prefix)

- **THIS ISN'T WHAT'S POWERFUL ABOUT `ggplot2`!**

----

## What is a Plot? *aesthetics*

- **TALK THROUGH THE POINTS**
- Each observation in the data is a *point*
- The *aesthetics* of a point determine how it is rendered in the plot
  - co-ordinates (x, y values) **ON THE IMAGE**
  - size
  - shape
  - colour
  - transparency
- *aesthetics* can be
  - *constant* (e.g. all points the same colour)
  - *mapped to variables* (e.g. colour mapped to continent)

-----

## What is a Plot? `geom`s

- So far **we've only defined the data and aesthetics**
  - **THIS ONLY TELLS US HOW DATA POINTS ARE REPRESENTED, NOT THE TYPE OF PLOT**
- `geom`s (short for *geometries*) **DEFINE THE KIND OF PLOT WE PRODUCE**
  - Showing the data **as points** is a *scatterplot*
  - Showing the data **as lines** is a *line plot*
  - Showing the data **as bars** is a *barchart*
- We can use **different `geom`s with the *same data and aesthetics***

-----

## What is a Plot? `geom`s

- **DEMO IN SCRIPT** (`gapminder.R`)
  - We **create a plot with the `ggplot()` function**.
  - We define the *data* as `data`, and *aesthetics* with `aes`
  - **WE PUT THE RESULT IN A VARIABLE FOR CONVENIENCE**
  - *Data* and *aesthetics* aren't enough to define a plot. **WE NEED A `geom`**
  - Use `geom_point()`

```R
# Generate plot of GDP per capita against life Expectancy
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap, color=continent))
p + geom_point()
```

- **WE'VE RECREATED THE SCATTERPLOT WE SAW EARLIER**

- **What happens if we change the `geom`?**
- **DEMO IN THE SCRIPT**

```R
p + geom_line()
```

- This looks terrible. **CHANGE IT BACK**
  - Preparing these plots in a script allows us to explore large and small differences in representation easily, and is very flexible

-----

## Challenge 08 (2min)

```R
# Plot life expectancy against time
p <- ggplot(data=gapminder, aes(x=year, y=lifeExp, colour=continent))
p + geom_point()
```

![images/red_green_sticky.png](images/red_green_sticky.png)

-----

## What is a Plot? *layers*

- Without drawing attention to it, **WE'VE JUST BEEN USING THE LAYERS CONCEPT**
  - **all `ggplot2` plots are built as layers**

- **ALL LAYERS HAVE TWO COMPONENTS**
  - *data* to be shown, and *aesthetics* for showing them
  - a `geom` defining the type of plot

- **The `ggplot` object describes a *base* layer, and can contain *data* and *aesthetics***
  - **THESE ARE INHERITED BY THE OTHER LAYERS IN THE PLOT**
  - **The values can also be overridden in specified layers**

----

## What is a Plot? *layers*

- In our first plot we defined a *base* with:
  - *data* from `gapminder`
  - *aesthetics* with *x* and *y* coordinates, and colouring by continent
- We defined a layer that:
  - had a `geom_point` `geom`
  - inherited *data* and *aesthetics* from the *base*

-*LAYERS ARE ADDED WITH THE `+` OPERATOR**

----

## What is a Plot? *layers*

- Now we will **override the base layer *aesthetics***
- **DEMO IN SCRIPT**
  - We'll **change the `geom`** to `geom_line`
  - We'll extend the *aesthetics* to **group datapoints by country**

```R
# Generate plot of GDP per capita against life expectancy
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap, color=continent))
p + geom_line(aes(group=country))
```

- **RENDER THE PLOT**

-----

**SLIDE: What is a Plot? *layers***

- We can **BUILD UP LAYERS OF `geom`S** to produce a more complex plot
- We **ADD A NEW `geom_point()` LAYER WITH `+`**
  - We use the layer's `alpha` argument to control transparency
- **DEMO IN SCRIPT**

```R
# Generate plot of GDP per capita against life expectancy
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap, color=continent))
p + geom_line(aes(group=country)) + geom_point(alpha=0.4)
```

- **RENDER THE PLOT**

-----

## Challenge 09 (5min)

```R
# Generate plot of life expectancy against time
p <- ggplot(data=gapminder, aes(x=year, y=lifeExp, color=continent))
p + geom_line(aes(group=country)) + geom_point(alpha=0.35)
```

![images/red_green_sticky.png](images/red_green_sticky.png)

----

## Transformations and `scale`s

- Another kind of layer is a *transformation* - handled with `scale` layers
- These map *data* to new aesthetics on the plot
  - new axis scales, e.g. log scale, reverse scale, time scale
  - colour scaling (changing palettes)
  - shape and size scaling

- **DEMO IN SCRIPT** (`gapminder.R`)
  - Rescale the plot first
  - Then change the colours

```R
# Generate plot of GDP per capita against life expectancy
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap, color=continent))
p <- p + geom_line(aes(group=country)) + geom_point(alpha=0.4)
p + scale_y_log10() + scale_color_grey()
```

-----

## Statistics layers

- Some `geom` layers transform the dataset
  - Usually this is a data summary (e.g. smoothing or binning)
  - The layer **may provide a new summary visual object**

- **DEMO IN SCRIPT**
  - This is working towards an informative figure
  - **Start with a new basic scatterplot**
  - **NOTE:** setting opacity helps see density in the data - **looks like two main points of density**
  - **NOTE:** looks like a general trend of GDP and life expectancy correlating

```R
# Generate summary plot of GDP per capita against life expectancy
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap))
p + geom_point(alpha=0.4) + scale_y_log10()
```

- **ADD A SMOOTHED FIT**
  - **NOTE:** The correlation is made quite clear

```R
# Generate summary plot of GDP per capita against life expectancy
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap))
p <- p + geom_point(alpha=0.4) + scale_y_log10()
p + geom_smooth()
```

- **ADD A CONTOUR PLOT OF DENSITY**
  - **NOTE:** Two populations are clear
  - **We might speculate that there is a difference in wealth/life expectancy across continents**

```R
# Generate summary plot of GDP per capita against life expectancy
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap))
p <- p + geom_point(alpha=0.4) + scale_y_log10()
p + geom_density_2d(color="purple")
```

- **ADD CONTINENT COLOURING**
  - **NOTE:** It's now clear that the two populations are centred on Europe (wealthy, long-lived) and Africa (poor, short-lived), respectively.

```R
p <- ggplot(data=gapminder, aes(x=lifeExp, y=gdpPercap))
p <- p + geom_point(alpha=0.4, aes(color=continent)) + scale_y_log10()
p + geom_density_2d(color="purple")
```

-----

## Multi-panel figures

- All our plots so far have been single figures, but **multi-panel plots can give clearer comparisons
  - These are also known as *small multiples plots*
- The **`facet_wrap()` layer allows us to make grids of plots, SPLIT BY A FACTOR**
- **DEMO IN THE SCRIPT**
  - We set a **default aesthetic grouping by country**
  - We generate a line plot, with log y axis
  - **The result is a bit messy.**

```R
# Compare life expectancy over time by country
p <- ggplot(data=gapminder, aes(x=year, y=lifeExp, colour=continent, group=country))
p + geom_line() + scale_y_log10()
```

- **using `facet_wrap()` to split by continent is clearer**
  - **NOTE:** the axes are consistent across facets

```R
p <- ggplot(data=gapminder, aes(x=year, y=lifeExp, colour=continent, group=country))
p <- p + geom_line() + scale_y_log10()
p + facet_wrap(~continent)
```

-----

## Challenge 10 (5min)

```R
# Contrast GDP per capita against population
p <- ggplot(data=gapminder, aes(x=pop, y=gdpPercap))
p <- p + geom_point(alpha=0.8, aes(color=continent))
p <- p + scale_y_log10() + scale_x_log10()
p + geom_density_2d(alpha=0.5) + facet_wrap(~year)
```

![images/red_green_sticky.png](images/red_green_sticky.png)

-----
