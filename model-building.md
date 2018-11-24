
# Model building

## Introduction


```r
library(tidyverse)
library(modelr)
options(na.action = na.warn)
library("broom")

library(nycflights13)
library(lubridate)
```

## Why are low quality diamonds more expensive?


```r
diamonds2 <- diamonds %>%
  filter(carat <= 2.5) %>%
  mutate(lprice = log2(price), lcarat = log2(carat))

mod_diamond2 <- lm(lprice ~ lcarat + color + cut + clarity, data = diamonds2)
```

### Exercise <span class="exercise-number">24.2.3.1</span> {.unnumbered .exercise}

<div class="question">
In the plot of `lcarat` vs. `lprice`, there are some bright vertical strips. What do they represent?
</div>

<div class="answer">

The distribution of diamonds has more diamonds at round or otherwise human friendly numbers (fractions).

</div>

### Exercise <span class="exercise-number">24.2.3.2</span> {.unnumbered .exercise}

<div class="question">
If `log(price) = a_0 + a_1 * log(carat)`, what does that say about the relationship between `price` and `carat`?
</div>

<div class="answer">

A 1% increase in carat is associated with an $a_1$% increase in price.

</div>

### Exercise <span class="exercise-number">24.2.3.3</span> {.unnumbered .exercise}

<div class="question">
Extract the diamonds that have very high and very low residuals. Is there anything unusual about these diamonds? Are the particularly bad or good, or do you think these are pricing errors?
</div>

<div class="answer">

This was already discussed in the text. I don't see anything either.

</div>

### Exercise <span class="exercise-number">24.2.3.4</span> {.unnumbered .exercise}

<div class="question">
Does the final model, `mod_diamonds2`, do a good job of predicting diamond prices? Would you trust it to tell you how much to spend if you were buying a diamond?
</div>

<div class="answer">


```r
diamonds2 %>%
  add_predictions(mod_diamond2) %>%
  add_residuals(mod_diamond2) %>%
  summarise(sq_err = sqrt(mean(resid ^ 2)),
            abs_err = mean(abs(resid)),
            p975_err = quantile(resid, 0.975),
            p025_err = quantile(resid, 0.025))
#> # A tibble: 1 x 4
#>   sq_err abs_err p975_err p025_err
#>    <dbl>   <dbl>    <dbl>    <dbl>
#> 1  0.192   0.149    0.384   -0.369
```

</div>

## What affects the number of daily flights?

This code is copied from the book and needed for the exercises.


```r
library("nycflights13")
daily <- flights %>%
  mutate(date = make_date(year, month, day)) %>%
  group_by(date) %>%
  summarise(n = n())
daily
#> # A tibble: 365 x 2
#>   date           n
#>   <date>     <int>
#> 1 2013-01-01   842
#> 2 2013-01-02   943
#> 3 2013-01-03   914
#> 4 2013-01-04   915
#> 5 2013-01-05   720
#> 6 2013-01-06   832
#> # ... with 359 more rows

daily <- daily %>%
  mutate(wday = wday(date, label = TRUE))

term <- function(date) {
  cut(date,
    breaks = ymd(20130101, 20130605, 20130825, 20140101),
    labels = c("spring", "summer", "fall")
  )
}

daily <- daily %>%
  mutate(term = term(date))

mod <- lm(n ~ wday, data = daily)

daily <- daily %>%
  add_residuals(mod)

mod1 <- lm(n ~ wday, data = daily)
mod2 <- lm(n ~ wday * term, data = daily)
```

### Exercise <span class="exercise-number">24.3.5.1</span> {.unnumbered .exercise}

<div class="question">
Use your Google sleuthing skills to brainstorm why there were fewer than expected flights on Jan 20, May 26, and Sep 1. (Hint: they all have the same explanation.) How would these days generalize to another year?
</div>

<div class="answer">

These are the Sundays before Monday holidays Martin Luther King Jr. Day, Memorial Day, and Labor Day.
This would generalize to other years, by using the dates of the respective
holidays for those years; the third Monday of January for Martin Luther King Jr. Day,
the last Monday of May for Memorial Day, and the first Monday in September for
Labor Day.

</div>

### Exercise <span class="exercise-number">24.3.5.2</span> {.unnumbered .exercise}

<div class="question">
What do the three days with high positive residuals represent? How would these days generalize to another year?
</div>

<div class="answer">

The top three days correspond to the Saturday after Thanksgiving (November 30th),
the Sunday after Thanksgiving (December 1st), and the Saturday after Christmas (December 28th).

```r
top_n(daily, 3, resid)
#> # A tibble: 3 x 5
#>   date           n wday  term  resid
#>   <date>     <int> <ord> <fct> <dbl>
#> 1 2013-11-30   857 Sat   fall  112. 
#> 2 2013-12-01   987 Sun   fall   95.5
#> 3 2013-12-28   814 Sat   fall   69.4
```
We could generalize these to other years using the dates of those holidays on those
years.

</div>

### Exercise <span class="exercise-number">24.3.5.3</span> {.unnumbered .exercise}

<div class="question">
Create a new variable that splits the `wday` variable into terms, but only for Saturdays, i.e. it should have `Thurs`, `Fri`, but `Sat-summer`, `Sat-spring`, `Sat-fall` How does this model compare with the model with every combination of `wday` and `term`?
</div>

<div class="answer">

I'll use the function `case_when()` to do this, though there are other ways which it could be solved.

```r
daily <- daily %>%
  mutate(wday2 =
         case_when(wday == "Sat" & term == "summer" ~ "Sat-summer",
                   wday == "Sat" & term == "fall" ~ "Sat-fall",
                   wday == "Sat" & term == "spring" ~ "Sat-spring",
                   TRUE ~ as.character(wday)))
```


```r
mod3 <- lm(n ~ wday2, data = daily)

daily %>%
  gather_residuals(sat_term = mod3, all_interact = mod2) %>%
  ggplot(aes(date, resid, colour = model)) +
    geom_line(alpha = 0.75)
```



\begin{center}\includegraphics[width=0.7\linewidth]{model-building_files/figure-latex/unnamed-chunk-8-1} \end{center}

I think the overlapping plot is hard to understand.
If we are interested in the differences, it is better to plot the differences directly.
In this code, I use `spread_residuals()` to add one *column* per model, rather than `gather_residuals()` which creates a new row for each model.

```r
daily %>%
  spread_residuals(sat_term = mod3, all_interact = mod2) %>%
  mutate(resid_diff = sat_term - all_interact) %>%
  ggplot(aes(date, resid_diff)) +
    geom_line(alpha = 0.75)
```



\begin{center}\includegraphics[width=0.7\linewidth]{model-building_files/figure-latex/unnamed-chunk-9-1} \end{center}

The model with terms × Saturday has higher residuals in the fall, and lower residuals in the spring than the model with all interactions.

Using overall model comparison terms, `mod4` has a lower $R^2$ and regression standard error, $\hat{\sigma}$, despite using fewer variables.
More importantly for prediction purposes, this model has a higher AIC, which is an estimate of the out of sample error.

```r
glance(mod3) %>% select(r.squared, sigma, AIC, df)
#> # A tibble: 1 x 4
#>   r.squared sigma   AIC    df
#> *     <dbl> <dbl> <dbl> <int>
#> 1     0.736  47.4 3863.     9
```

```r
glance(mod2) %>% select(r.squared, sigma, AIC, df)
#> # A tibble: 1 x 4
#>   r.squared sigma   AIC    df
#> *     <dbl> <dbl> <dbl> <int>
#> 1     0.757  46.2 3856.    21
```

</div>

### Exercise <span class="exercise-number">24.3.5.4</span> {.unnumbered .exercise}

<div class="question">
Create a new `wday` variable that combines the day of week, term (for Saturdays), and public holidays. What do the residuals of that model look like?
</div>

<div class="answer">

The question is unclear how to handle public holidays. There are several questions to consider.

First, what are the public holidays? I will include all [federal holidays in the United States](https://en.wikipedia.org/wiki/Federal_holidays_in_the_United_States) in 2013.
Other holidays to consider would be Easter and Good Friday which is US stock market holiday and widely celebrated religious holiday, Mothers Day, Fathers Day,
and Patriots' Day, which is a holiday in several states, and other state holidays.

```r
holidays_2013 <-
  tribble(
    ~ holiday,                    ~ date,
    "New Year's Day",             20130101,
    "Martin Luther King Jr. Day", 20130121,
    "Washington's Birthday",      20130218,
    "Memorial Day",               20130527,
    "Independence Day",           20130704,
    "Labor Day",                  20130902,
    "Columbus Day",               20131028,
    "Veteran's Day",              20131111,
    "Thanksgiving",               20131128,
    "Christmas",                  20131225
  ) %>%
  mutate(date = lubridate::ymd(date))
```

The model could include a single dummy variable which indicates a day was a public holiday.
Alternatively, I could include a dummy variable for each public holiday.
I would expect that Veteran's Day and Washington's Birthday have a different effect on travel than Thanksgiving, Christmas, and New Year's Day.

Another question is whether and how I should handle the days before and after holidays.
Travel could be lighter on the holiday itself,
but heavier before and after.


```r
daily <- daily %>%
  mutate(wday3 =
         case_when(
           (date - 1L) %in% holidays_2013$date ~ "day before holiday",
           (date + 1L) %in% holidays_2013$date ~ "day after holiday",           
           date %in% holidays_2013$date ~ "holiday",           
           .$wday == "Sat" & .$term == "summer" ~ "Sat-summer",
           .$wday == "Sat" & .$term == "fall" ~ "Sat-fall",
           .$wday == "Sat" & .$term == "spring" ~ "Sat-spring",
           TRUE ~ as.character(.$wday)))

mod4 <- lm(n ~ wday3, data = daily)

daily %>%
  spread_residuals(resid_sat_terms = mod3, resid_holidays = mod4) %>%
  mutate(resid_diff = resid_holidays - resid_sat_terms) %>%
  ggplot(aes(date, resid_diff)) +
    geom_line(alpha = 0.75)
```



\begin{center}\includegraphics[width=0.7\linewidth]{model-building_files/figure-latex/unnamed-chunk-13-1} \end{center}

</div>

### Exercise <span class="exercise-number">24.3.5.5</span> {.unnumbered .exercise}

<div class="question">
What happens if you fit a day of week effect that varies by month (i.e. `n ~ wday * month`)? Why is this not very helpful?
</div>

<div class="answer">


```r
daily <- mutate(daily, month = lubridate::month(date))

mod6 <- lm(n ~ wday * month, data = daily)
```

There model has only 4--5 observations to estimate each (month, weekday)
combination parameter since only there only 4--5 weekdays in given month. Any
estimates will be very uncertain.

</div>

### Exercise <span class="exercise-number">24.3.5.6</span> {.unnumbered .exercise}

<div class="question">
What would you expect the model `n ~ wday + ns(date, 5)` to look like? Knowing what you know about the data, why would you expect it to be not particularly effective?
</div>

<div class="answer">
It will estimate a smooth seasonal trend (`ns(date, 5)`) with a day of the week cyclicality, (`wday`).
It probably will not be effective since
</div>

### Exercise <span class="exercise-number">24.3.5.7</span> {.unnumbered .exercise}

<div class="question">
We hypothesized that people leaving on Sundays are more likely to be business travelers who need to be somewhere on Monday. Explore that hypothesis by seeing how it breaks down based on distance and time: if it’s true, you’d expect to see more Sunday evening flights to places that are far away.
</div>

<div class="answer">

Looking at only day of the week, we see that Sunday flights are on average longer than the rest of the day of the week flights, but not as long as Saturday flights (perhaps vacation flights?).

```r
flights %>%
  mutate(date = make_date(year, month, day),
         wday = wday(date, label = TRUE)) %>%
  group_by(wday) %>%
  summarise(dist_mean =  mean(distance),
            dist_median = median(distance)) %>%
  ggplot(aes(y = dist_mean, x = wday)) +
  geom_point()
```



\begin{center}\includegraphics[width=0.7\linewidth]{model-building_files/figure-latex/unnamed-chunk-15-1} \end{center}

However, breaking it down by hour, I don't see much evidence at first.
Conditional on hour, the distance of Sunday flights seems similar to that of other days (excluding Saturday):


```r
flights %>%
  mutate(date = make_date(year, month, day),
         wday = wday(date, label = TRUE)) %>%
  group_by(wday, hour) %>%
  summarise(dist_mean =  mean(distance),
            dist_median = median(distance)) %>%
  ggplot(aes(y = dist_mean, x = hour, colour = wday)) +
  geom_point() +
  geom_line()
```



\begin{center}\includegraphics[width=0.7\linewidth]{model-building_files/figure-latex/unnamed-chunk-16-1} \end{center}

Can someone think of a better way to check this?

</div>

### Exercise <span class="exercise-number">24.3.5.8</span> {.unnumbered .exercise}

<div class="question">
It’s a little frustrating that Sunday and Saturday are on separate ends of the plot. Write a small function to set the levels of the factor so that the week starts on Monday.
</div>

<div class="answer">

See the chapter [Factors](http://r4ds.had.co.nz/factors.html) for the function `fct_relevel()`.
Use `fct_relevel()` to put all levels in-front of the first level ("Sunday").


```r
monday_first <- function(x) {
  forcats::fct_relevel(x, levels(x)[-1])  
}
```

Now Monday is the first day of the week,

```r
daily <- daily %>%
  mutate(wday = wday(date, label = TRUE))
ggplot(daily, aes(monday_first(wday), n)) +
  geom_boxplot() +
  labs(x = "Day of Week", y = "Number of flights")
```



\begin{center}\includegraphics[width=0.7\linewidth]{model-building_files/figure-latex/unnamed-chunk-18-1} \end{center}

</div>

## Learning more about models

No exercises
