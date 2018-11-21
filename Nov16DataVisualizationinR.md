Notes for Data Visualization with ggplot2 (Part1) (DataCamp)
============================================================

1. Introduction
---------------

-   Exploratory plots aren't meant to be pretty.
-   Have to specify when a variable is categorical

<!-- -->

    # Load the ggplot2 package
    library(ggplot2)

    # Change the command below so that cyl is treated as factor
    ggplot(mtcars, aes(x = factor(cyl), y = mpg)) +
      geom_point()

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-1-1.png)

    #X axis doesnot contain values outside of the dataset

    # Can change the COLOUR of the points based on the data in a certain column: ex. the displacement of the car engine
    ggplot(mtcars, aes(x = wt, y = mpg, color = disp)) +
      geom_point()

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-2-1.png)

    # Can change the SIZE of the points based on the data in a certain column: ex. the displacement of the car engine
    ggplot(mtcars, aes(x = wt, y = mpg, size = disp)) +
      geom_point()

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-3-1.png)

Can't change the shape of the points based on displacement of the car
engine because 'A continuous variable can not be mapped to shape', means
that shape doesn't exist on a continuous scale here.

    #Can fit a smoothened line over the data points with geom_smooth()
    ggplot(diamonds, aes(x = carat, y = price)) +
      geom_point() +
      geom_smooth()

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    #can call se = FALSE within the geom_smooth function to get rid of error shading

    #Can plot just the smoothen curve and colour based on column of clarity
    ggplot(diamonds, aes(x = carat, y = price, color = clarity)) +
      geom_smooth()

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    #Can plot the points and colour based on clarity + change opacity
    ggplot(diamonds, aes(x = carat, y = price, color = clarity)) + geom_point(alpha = 0.4)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    # Create the object containing the data and aes layers: dia_plot
    dia_plot <- ggplot(diamonds, aes(x = carat, y = price))

    # Add a geom layer with + and geom_point()
    dia_plot + geom_point()

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    # Add the same geom layer, but with aes() inside
    dia_plot + geom_point(aes(color = clarity))

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-7-2.png)

2. Data
-------

    # Plot the correct variables of mtcars
    plot(mtcars$wt, mtcars$mpg, col = mtcars$cyl)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-8-1.png)

    # Change cyl inside mtcars to a factor
    mtcars$fcyl <- as.factor(mtcars$cyl)

    # Make the same plot as in the first instruction
    plot(mtcars$wt, mtcars$mpg, col = mtcars$fcyl)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-8-2.png)

    # lm for each cyl value and lm for whole group (with group = 1)
    ggplot(mtcars, aes(x = wt, y = mpg, col = cyl)) +
     geom_point() +
      geom_smooth(se = FALSE, method = lm) +
      geom_smooth(aes(group = 1), method = lm, se = FALSE, linetype = 2)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-9-1.png)

    library(tidyr)
    # Consider the structure of iris, iris.wide and iris.tidy (in that order)
    str(iris)

    ## 'data.frame':    150 obs. of  5 variables:
    ##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
    ##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
    ##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
    ##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
    ##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

    # Tidy up the data 
    iris.tidy <- iris %>%
      gather(key, Value, -Species) %>%
      separate(key, c("Part", "Measure"), "\\.")
      
    str(iris.tidy)

    ## 'data.frame':    600 obs. of  4 variables:
    ##  $ Species: Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Part   : chr  "Sepal" "Sepal" "Sepal" "Sepal" ...
    ##  $ Measure: chr  "Length" "Length" "Length" "Length" ...
    ##  $ Value  : num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...

    # Plot the tidy data
    ggplot(iris.tidy, aes(x = Species, y = Value, col = Part)) +
      geom_jitter() +
      facet_grid(. ~ Measure)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-10-1.png)

3. Aesthetics
-------------

    #can colour outline and fill in the points with diff colours
    ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl, col = am)) +
      geom_point(shape = 21, size = 4, alpha = 0.6)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-11-1.png)

    my_color <- "#4ABEFF"
    #colour inside geom_point will override what's said above
    ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl)) +
      geom_point(colour = my_color, size = 10, shape = 23)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-12-1.png)

In this chapter we saw aesthetics and attributes. Variables in a data
frame are mapped to aesthetics in aes(). (e.g. aes(col = cyl)) within
ggplot(). Visual elements are set by attributes in specific geom layers
(geom\_point(col = "red")). Don't confuse these two things.

    #can show 5 dimensions here just within aesthetics
    ggplot(mtcars, aes(x = mpg, y = qsec, colour = factor(cyl), shape = factor(am), size = (hp/wt))) + geom_point()

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-13-1.png)

Label and shape are only applicable to categorical data.

    #base layer
    cyl.am <- ggplot(mtcars, aes(x = factor(cyl), fill = factor(am)))

    cyl.am + geom_bar() #position is stack by default

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    # can change the position, scales, and legend
    val = c("#E41A1C", "#377EB8")
    lab = c("Manual", "Automatic")
    cyl.am +
      geom_bar(position = "dodge") +
      scale_x_discrete('Cylinders') + 
      scale_y_continuous('Number') +
      scale_fill_manual('Transmission', 
                        values = val,
                        labels = lab) 

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-14-2.png)

    #if no y variable, put 0. also, geom_jitter so that points are not in a straight line, set limits on axis
    ggplot(mtcars, aes(x = mpg, y = 0)) +
      geom_jitter() +
      scale_y_continuous(limits = c(-2, 2))

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-15-1.png)

You'll have to deal with overplotting when you have: - Large datasets -
Imprecise data and so points are not clearly separated on your plot (you
saw this in the video with the iris dataset) - Interval data (i.e. data
appears at fixed values) - Aligned data values on a single axis

Solutions:

    #size 1 is hollow points, lower transparency
    ggplot(mtcars, aes(x = wt, y = mpg, colour = cyl)) +
      geom_point(size = 4, shape = 1, alpha = 0.6)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    # Scatter plot: carat (x), price (y), clarity (color)
    ggplot(diamonds, aes(x = carat, y = price, colour = clarity)) +
      geom_point()

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-16-2.png)

    # Adjust for overplotting
    ggplot(diamonds, aes(x = carat, y = price, colour = clarity)) +
      geom_point(alpha = 0.5)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-16-3.png)

    # Scatter plot: clarity (x), carat (y), price (color)
    ggplot(diamonds, aes(x = clarity, y = carat, colour = price)) +
      geom_point(alpha = 0.5)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-16-4.png)

    # Dot plot with jittering
    ggplot(diamonds, aes(x = clarity, y = carat, colour = price)) +
      geom_point(alpha = 0.5, position = "jitter")

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-16-5.png)

4. Geometries
-------------

    #jittering
    ggplot(mtcars, aes(x = cyl, y = wt)) +
      geom_point(position = position_jitter(0.1))

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-17-1.png)

    #for histograms, ..xxx.. stores how many counts are in each bin 
    ggplot(mtcars, aes(x = mpg)) +
      geom_histogram(binwidth = 1, aes(y = ..density..), fill = '#377EB8')

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-18-1.png)

    #different bar graphs

    # Change the position argument to stack (this is the default)
    ggplot(mtcars, aes(x = cyl, fill = am)) +
      geom_bar(position = "stack")

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-19-1.png)

    # Change the position argument to fill
    ggplot(mtcars, aes(x = cyl, fill = am)) +
      geom_bar(position = "fill")

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-19-2.png)

    # Change the position argument to dodge
    ggplot(mtcars, aes(x = cyl, fill = am)) +
      geom_bar(position = "dodge")

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-19-3.png)

stack (the default), dodge (preferred), and fill (to show proportions)

    #show overlapping dodge bar graphs
    posn_d <- position_dodge(width = 0.2)
    ggplot(mtcars, aes(x = cyl, fill = am)) +
      geom_bar(position = posn_d, alpha = 0.6)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-20-1.png)

    ggplot(mtcars, aes(mpg, colour = cyl)) +
      geom_freqpoly(binwidth = 1, position = "identity")

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-21-1.png)

    # Example of how to use a brewed color palette
    ggplot(mtcars, aes(x = cyl, fill = am)) +
      geom_bar() +
      scale_fill_brewer(palette = "Set1")

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-22-1.png)

    #OVERLAPPING 
    ggplot(mtcars, aes(mpg, fill = am)) +
      geom_histogram(binwidth = 1, position = "identity", alpha = 0.4)

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-23-1.png)

    #Making line plots with geom_line()
    ggplot(economics, aes(x = date, y = unemploy/pop)) + geom_line()

![](Nov16DataVisualizationinR_files/figure-markdown_strict/unnamed-chunk-24-1.png)
