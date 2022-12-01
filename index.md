# Spearman's Correlation Coefficient

*Matt Prill*

## Tutorial Contents:

1.  What is Spearman's Correlation Coefficient
2.  What Separates Spearman's from Linear Regressions and Other Coefficients?
3.  Spearman's Correlation Coefficient Equation
4.  Spearman's Assumptions and Requirements
5.  The Data
6.  Completing a Spearman's Test
7.  Interpreting and Reporting the Results
8.  The Shortcut
9.  Learning Outcomes
10. Extra Resources

## 1. What Is Spearman's Correlation Coefficient?

Spearman's Correlation Coefficient, also known as 'Spearman's Rank-Order Correlation', or 'Spearman's rho' (we'll just call it Spearman's), is a measure of the strength and direction of association between two ranked variables.

## 2. What Separates Spearman's from Linear Regressions and Other Coefficients?

At this point you may be thinking, 'OK, a measure of the strength of a relationship between two variables... sounds a lot like the a regression', and you'd be absolutely right! Regression theory has been covered previously in the [Linear Model Tutorial](https://ourcodingclub.github.io/tutorials/modelling/). On top of that, you may have heard of another correlation coefficient called 'Pearson's Correlation Coefficient' which shares many similarities with Spearman's.

This all begs the question: What's so special about Spearman's? Well, lets start with how our correlation coefficients differ from regression coefficients.

#### Correlation vs Regression

The primary distinction between the two is that regressions imply a cause and effect whereas coefficients do not. We know that regressions have an 'explanatory variables' and a 'response variables' which are manifested respectively in the x and y axis of the corresponding figures. In contrast, correlation coefficients simply measure the strength and direction of the relationship which means, it doesn't really matter which axis they go on because neither reflect cause or effect.

#### Spearman's vs Pearson's

The correlation coefficient most similar to Spearman's is the 'Pearson's Correlation Coefficient'. They both test the strength of relationships between variables. But how so? Well, the two key distinctions are as follows:

1.  Spearman's can measure monotonic relationships between two variables whereas Pearson's is restricted to linear distributions only. A monotonic distribution is where the two variables are correlated in some way, but do not necessarily always change proportionally to one another. We can visualise this in the stormy graph below: ![](/data_visualisation/lightning/monotonic_lightning.png) This striking figure (pun intended) illustrates a hypothetical relationship between the number of lightning strikes a place experiences and the average temperature. We can interpret that the relationship is certainly positive, but not necessarily linear. We can see that at some points, the change of one variable is not proportional to the change of the other. The benefit of Spearman's is that it can work with monotonic relationships like this one, something that other correlation coefficients, such as Pearson's, cannot.

2.  Spearman's assigns ranks to the raw data which makes it non-parametric. This means that ordinal data can be utilised when studying relationships, a trait that its non-parametric equivalent Pearson's is lacking. This means that, as well as numerical data, non-continuous data with a clear hierarchy can be analysed by converting it into a numerical a rank. We call this type of data **ordinal data**. An example might be grades from a test (A, B, C etc.). We could assign the rank 1 to A, 2 to B and 3 to C.

    Another common form of ordinal data can be found in survey question responses (Very Poor, Poor, Neutral, Good, Very Good). We could assign ranks to these values too because they have a clear hierarchy. This separates ordinal data from nominal data which is categorical and without hierarchy e.g. colour, occupation, ethnicity, country etc.

So, lets quickly summarise the key similarities and differences between what Spearman's and Pearson's can and can't work with:

| Coefficient | Ordinal Data | Nominal Data | Continuous data | Monotonic Distribution |
|:--------------|:-------------:|:-------------:|:-------------:|:-------------:|
| Spearman's  |     Yes      |      No      |       Yes       |          Yes           |
| Pearson's   |      No      |      No      |       Yes       |           No           |

## Spearman's Equation

Here's the equation for Spearman's:

![The Spearman's equation. Source: <https://www.dataanalytics.org.uk/spearman-rank-correlation-in-excel/>](/external_content/spearmans_equation/spearmans_equation.png)

#### Spearheading an attempt to understand the formula:

$r_s$ = Spearman's rank correlation coefficient which is between -1 (Perfect negative correlation) and +1 (perfect positive correlation).\
\
$\sum_{}D^2$ = The sum of the squared differences between paired ranks.\
\
$n$ = The number of samples.

#### Pair them all together et voilà:

$$\ r_s = 1 -\left(\frac{6\sum_{}D^2}{n(n^2-1)}\right)$$

## Spearman's Assumptions and Requirements

Its important that we understand the assumptions and requirements of Spearman's so that we use it correctly:

-   Samples must represent paired observations i.e. 2 observations per sample
-   Each vector is either ordinal, interval or ratio data
-   Ideally, there should be at least 15 pairs of observations
-   The variables are monotonically related

## The Data

It's almost time to have a go at our own Spearman's rank. But first lets take a look at the data we're going to use:

Clone and download this repository! Then, in your R script, set the working directory to the location where you save the repository.

``` r
setwd("PATH/TO/FOLDER")
```

Now, lets import the data that we want to test:

``` r
forest <- read.csv("data/forest.csv")
```

We've touched on lightning already, but now we'll move onto a natural hazard that it can create:

![](/external_content/wildfire/wild_fire.png) Source: [Matthew Abbott  ](https://www.nytimes.com/2020/01/10/world/australia/australia-wildfires-photos.html)

The data, sourced from [Worldometer](https://www.worldometers.info/population/countries-in-europe-by-population/) and [Global Forest Watch](https://www.globalforestwatch.org/dashboards/global/?category=fires&dashboardPrompts=eyJzaG93UHJvbXB0cyI6dHJ1ZSwicHJvbXB0c1ZpZXdlZCI6WyJzaGFyZVdpZGdldCJdLCJzZXR0aW5ncyI6eyJzaG93UHJvbXB0cyI6dHJ1ZSwicHJvbXB0c1ZpZXdlZCI6W10sInNldHRpbmdzIjp7Im9wZW4iOmZhbHNlLCJzdGVwSW5kZXgiOjAsInN0ZXBzS2V5IjoiIn0sIm9wZW4iOnRydWUsInN0ZXBJbmRleCI6MCwic3RlcHNLZXkiOiJzaGFyZVdpZGdldCJ9LCJvcGVuIjp0cnVlLCJzdGVwc0tleSI6ImRvd25sb2FkRGFzaGJvYXJkU3RhdHMifQ%3D%3D&gfwfires=true&location=WyJnbG9iYWwiXQ%3D%3D&map=eyJjZW50ZXIiOnsibGF0IjoxNi45MDkyNDU0OTkzMjczNzMsImxuZyI6LTUyLjU3NzE4NDY5NDk1OTIzNn0sImRhdGFzZXRzIjpbeyJkYXRhc2V0IjoicG9saXRpY2FsLWJvdW5kYXJpZXMiLCJsYXllcnMiOlsiZGlzcHV0ZWQtcG9saXRpY2FsLWJvdW5kYXJpZXMiLCJwb2xpdGljYWwtYm91bmRhcmllcyJdLCJib3VuZGFyeSI6dHJ1ZSwib3BhY2l0eSI6MSwidmlzaWJpbGl0eSI6dHJ1ZX0seyJkYXRhc2V0IjoidHJlZS1jb3Zlci1sb3NzLWZpcmVzIiwibGF5ZXJzIjpbInRyZWUtY292ZXItbG9zcy1maXJlcyJdLCJvcGFjaXR5IjoxLCJ2aXNpYmlsaXR5Ijp0cnVlLCJ0aW1lbGluZVBhcmFtcyI6eyJzdGFydERhdGUiOiIyMDAxLTAxLTAxIiwiZW5kRGF0ZSI6IjIwMjEtMTItMzEiLCJ0cmltRW5kRGF0ZSI6IjIwMjEtMTItMzEifSwicGFyYW1zIjp7InRocmVzaG9sZCI6MzAsInZpc2liaWxpdHkiOnRydWUsImFkbV9sZXZlbCI6ImFkbTAifX1dfQ%3D%3D&showMap=true&treeLossFires=eyJwYWdlIjowfQ%3D%3D) contains the 20 countries with the highest forest fire losses between 2001 and 2021. The countries' total forest loss (kha) and total size (km $^2$ ) are included. Let's check out the distribution between forest fire losses and country size.

``` r
library(tidyverse)

forest %>% 
  ggplot(aes(x = area, y = forest_loss)) +  # Plotting area and forest loss
  geom_point() +  # Plotting the raw data
  geom_smooth(se = F)  # Plotting the line of best fit
```

![](/data_visualisation/initial_distribution/first_data_distribution.png)

It's a little tricky to interpret our data here. It seems that overall, the relationship between forest fire loss and country area is positive, but the distribution of the data certainly isn't linear. Sometimes we can log-transform our data to make it fit a linear distribution but this still doesn't help us, check out [this tutorial](https://ourcodingclub.github.io/tutorials/data-scaling/) for more information on this topic. You can see for yourself by changing variables to `log(area)` and/or `log(forest_loss)`. This rules out the use of Pearson's to test the variables correlation; our relationship appears to be monotonic like the distribution we saw earlier! So, let's dig deeper into the strength of the relationship using Spearman's.

## Completing a Spearman's Test

First things first, Spearman's measures the strength of ranked variables. So.... let's rank our variables!

R has ranking function aptly named 'rank()' which we can apply to each variable:

``` r
# Creating a Dataframe of Ranked Variables
ranked_data <- data.frame(cbind(forest$country,  # Adding the list of countries
  rank(forest$area, ties.method = "average"),  # Ranking the countries' areas
  rank(forest$forest_loss, ties.method = "average")))  # Ranking the countries' forest fire losses 
```

This ranks each set of variables independently with `rank()` (1 = minimum rank). The `ties.method = "average"` specifies how we rank variables of the same value. In Spearman's, we take the 'average' rank of the equal variables. We can see this manifested in our new data frame:

``` r
# Observe the equal ranks
view(ranked_data)
```

We can see that Mongolia and Portugal are both ranked 8.5 because they shared the same value for forest loss. Rather than ranking them 8 and 9, we take the mean (8.5) and rank the next highest rank '10'.

Next, we collated the two vectors of ranked variables and their corresponding countries with `cbind()` and assigned it all to a new data frame with the `data.frame()` function.

Our new data frame is missing column names which we can swiftly add with:

``` r
# Assign column names
colnames(ranked_data) <- c('Country','Area_Rank','Forest_Loss_Rank') 
```

Now if we, look back at our formula... $$\ r_s = 1 -\left(\frac{6\sum_{}D^2}{n(n^2-1)}\right)$$ ... we can see that we need the sum of the squared differences between our ranks - $\sum_{}D^2$.

To do this, we first need to set our ranked columns as numeric values because the 'rank()' function returns the class 'character'.

``` r
# Set ranks as numeric
ranked_data$Area_Rank <- as.numeric(ranked_data$Area_Rank)
ranked_data$Forest_Loss_Rank <- as.numeric(ranked_data$Forest_Loss_Rank)
```

Next we create a column for the differences between the ranks called 'Differences':

``` r
# Differences = Area Ranks - Forest Loss Ranks
ranked_data['Differences'] = ranked_data['Area_Rank'] - ranked_data['Forest_Loss_Rank']
```

We want to find the sum of these values which we can calculate with using similar syntax:

``` r
# Add square of differences column
ranked_data['Squared_Differences'] = ranked_data['Differences']^2 
```

OK so, lets look at the magnificent data frame we've created:

``` r
# Check out the new data frame
view(ranked_data)
```

Great, we have the squares of the differences between the ranks. Now we just need to total them:

``` r
# Calculate sum of the squares
sum_of_sqaured_diffs <- sum(ranked_data$Squared_Differences)
```

Next, we need to find the bottom half of the equation - $n(n^2-1)$. Remember, 'n' is our sample size which we can easily find with:

``` r
# Assign sample size to 'n'
n <- length(ranked_data$Country)
# 'ranked_data' or our original data frame 'forest' will have the same output.
```

So we've got objects for the sum of the squared differences between ranks as well as the sample size - nice! All we need to do now is plug these into the Spearman's equation:

``` r
# Calculate the rs value
1-((6 * sum_of_sqaured_diffs) / (n*(n^2 - 1)))
```

The output is our $r_s$ value: **0.735**. Time to make sense of it!

## Interpreting and Reporting the Results

Now remember, the output should fall between -1 and +1:

![Correlation scale bar](/external_content/correlation_scale/correlation_scale.png)

Our output falls on the positive end of the spectrum, suggesting a positive relationship between our two variables. But to test whether the value is statistically significant i.e. not down to chance, we must refer to a table to critical values:

![](/external_content/critical_values/critical_values_table.png)

The confidence we have in our $r_s$ value increases with sample size. We have a sample size of 20 (countries). When we look to our table, we can see that, Where n = 20, the critical value for the 0.01 confidence interval is met (0.57). Similar to other frequentist analyses, we can use this threshold to conclude that the probability of our results being down to chance are \< 1%.

#### Reporting the Results

So, our results are significant; there is a positive relationship between country area and forest fire losses between 2001 and 2021. "But how do we report this though?". Well, similarly to how we reported our our ANOVA analysis in the ["ANOVA FROM A TO (XY)Z"](https://ourcodingclub.github.io/tutorials/anova/). We need to report:

-   The statistical test we used
-   The $r_s$ (rho) value
-   The p value threshold
-   The sample size

So an example of how our finding would be reported is:

"We found that there was a positive relationship between the country total area and forest loss between 2001 and 2021 (Spearman's Rank Correlation Coefficient, n = 20, $r_s$ = 0.735, p \< 0.01)."

## The Shortcut

Congratulations! You've successfully managed to manually calculate a Spearman's Rank by applying our understanding of it's assumptions and formula. I hope this method has allowed you to understand the logic behind each step.

You may have noticed my choice of words; could '*shortcut*' and '*manually*' suggest that there's an easy breezy method out there? Well, indeed there is! Once you've understood each step we've gone through to calculate the Spearman's $r_s$ value, treat yourself to this code:

``` r
cor(forest$forest_loss, forest$area, method = c("spearman"))
```

This code completes each step we conducted. It ranks the variables, finds the differences between ranks, squares the differences, finds the sample size and calculates the $r_s$ value using the Spearman's equation.

The `cor()` function is used to test the correlation between our specified variables (`forest$forest_loss, forest$area`) using our named correlation coefficient test: `method = c("spearman")`.

## Learning Outcomes
Wow, well done for making it to the end! We've covered a lot of ground - here's a recap of what we've learnt:

- What Spearman's is
- What makes it unique to other stats and coefficients
- The Spearman's equation and its assumptions
- How to complete a Spearman's both manually and automatically
- How to report the findings

I hope you enjoyed and feel free to take a look at some extra resources to further embed the concepts into your understanding!

## Extra Resources

Useful links:

-   Spearman's overview: <https://www.nagwa.com/en/explainers/412129796494/>

-   Another example of Spearmans using ecological data: <https://www.moorsforthefuture.org.uk/__data/assets/pdf_file/0034/87649/Statistical-Skills-example-sheet-Spearmans-Rank-education.pdf>

-   Ranking ordinal data: <https://towardsdatascience.com/r-rank-vs-order-753cc7665951>

-   The cor() function for other tests: <https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/cor>

