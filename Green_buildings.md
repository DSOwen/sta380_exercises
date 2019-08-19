Green\_Buildings
================

## Initial Data Processing

``` r
#converting categorical variables to factors
buildings$green_rating = as.factor(buildings$green_rating)
buildings$LEED = as.factor(buildings$LEED)
buildings$Energystar = as.factor(buildings$Energystar)
buildings$amenities = as.factor(buildings$amenities)
buildings$renovated = as.factor(buildings$renovated)
buildings$class_a = as.factor(buildings$class_a)
buildings$class_b = as.factor(buildings$class_b)
buildings$net = as.factor(buildings$net)
```

``` r
#creating a separate class variable
buildings$class = ifelse(buildings$class_a == 1, 'A', ifelse(buildings$class_b == 1, 'B', 'C'))
```

## Exploratory Analysis

At first glance, it appears that the on-staff stats guru makes valid
points about the situaton.

As can be seen below, there is indeed a $2.6 difference in green and
non-green buildings. We control for occupancy rates
\>10%.

``` r
buildings %>% filter(leasing_rate>=10)%>% group_by(green_rating) %>% summarise(med_rent = median(Rent), count = n())
```

    ## # A tibble: 2 x 3
    ##   green_rating med_rent count
    ##   <fct>           <dbl> <int>
    ## 1 0                25.0  6995
    ## 2 1                27.6   684

``` r
ggplot(buildings, aes(green_rating, Rent)) + geom_boxplot()
```

![](R2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Non-green buildings also have a lot more outliers, and hence the guru’s
rationale behind using the median instead of the mean holds
true.

### Rent vs. Occupancy Rates

``` r
ggplot(buildings, aes(leasing_rate, Rent, color = green_rating)) + geom_point()
```

![](R2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Although the minimum rent holds constant for all values of occupancy
rates, it shows a subtle increasing trend with increasing occupancy
rates. Let us explore if green buildings have higher occupancy
rates.

``` r
buildings %>% group_by(green_rating) %>% summarise(med_occupancy = median(leasing_rate), count = n())
```

    ## # A tibble: 2 x 3
    ##   green_rating med_occupancy count
    ##   <fct>                <dbl> <int>
    ## 1 0                     89.2  7209
    ## 2 1                     92.9   685

Green buildings do have slightly higher occupancy rates than non-green
buildings. As mentioned in the case study, this could be thanks to
better awareness and PR for non-green buildings, but we don’t have
enough data to verify this claim.

The guru’s assumption of 90% occupancy rates for the proposed green
building is in line with the occupancy rates suggested in our dataset.

### Age vs. Occupancy Rates

Are younger buildings more or less likely to be
occupied?

``` r
ggplot(buildings, aes(age, leasing_rate, color = green_rating)) + geom_point()
```

![](R2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

There doesn’t appear to be any defined relationship between the two.

## Confounding Factors

Next, we explore the possibilites of any confounding factors that might
indirectly raise the rent for green buildings.

### 1\. Amenities

Most green buildings (72%) have amenities (only \~52% in case of
non-green), so could this be a factor in raising the rents for green
buildings?

``` r
buildings %>% group_by(green_rating, amenities) %>% summarise(med_rent = median(Rent), count = n())
```

    ## # A tibble: 4 x 4
    ## # Groups:   green_rating [2]
    ##   green_rating amenities med_rent count
    ##   <fct>        <fct>        <dbl> <int>
    ## 1 0            0             25    3550
    ## 2 0            1             25    3659
    ## 3 1            0             27     187
    ## 4 1            1             27.8   498

Controlling for amenities, i.e. comparing rents for green & non-green
buildings without amenities, there is still a premium for green
buildings ($2), and hence this theory does not hold
up.

### 2\. Number of stories

``` r
ggplot(buildings, aes(stories, Rent, color = green_rating)) + geom_point()
```

![](R2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

The minimum rent shows an increase with increasing number of stories.
Our client may thus be able to demand a higher rent with a taller
building.

``` r
buildings %>% group_by(green_rating) %>% summarise(med_stories = median(stories))
```

    ## # A tibble: 2 x 2
    ##   green_rating med_stories
    ##   <fct>              <int>
    ## 1 0                     10
    ## 2 1                     11

Green buildings are in general 1 storey higher than non-green building,
but this difference is not strong enough to be considered a confounding
factor.

### 3\. Size

``` r
ggplot(buildings, aes(size, Rent, color = green_rating)) + geom_point()
```

![](R2_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

From the graph above, we can see that the rent increases with increasing
available leasing space.

``` r
buildings %>% group_by(green_rating)%>%
summarize(med_size = median(size), mean_size = mean(size))
```

    ## # A tibble: 2 x 3
    ##   green_rating med_size mean_size
    ##   <fct>           <int>     <dbl>
    ## 1 0              118696   225977.
    ## 2 1              241150   325781.

Green buildings also appear to have more leasing space in general, and
thus, size could be a potential counfounding
variable.

### 4\. Age

``` r
buildings %>% group_by(green_rating) %>% summarise(med_age = median(age), count = n())
```

    ## # A tibble: 2 x 3
    ##   green_rating med_age count
    ##   <fct>          <int> <int>
    ## 1 0                 37  7209
    ## 2 1                 22   685

Green buildings are younger by around 15 years. Do newer buildings in
general demand higher rent?

``` r
ggplot(buildings, aes(age, Rent, color = green_rating)) + geom_point()
```

![](R2_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

There doesn’t appear to be a particular relationship between age and
rent, and hence, age doesn’t appear to be a confounding
factor.

``` r
buildings$revenue = 1.000*(buildings$size * buildings$Rent * buildings$leasing_rate)/100
```

``` r
summary(buildings$revenue)
```

    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ##         0    880975   2749485   6536261   7675431 468492211

## Further Recommendations

Can revenue for green buildings be maximised by choosing the right
locations to build
them?

``` r
ggplot(buildings, aes(cluster, Rent, color = green_rating)) + geom_point()
```

![](R2_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

``` r
buildings %>% filter(cluster>=430 & cluster<=600) %>% group_by(green_rating) %>%summarise(med = median(Rent), count = n())
```

    ## # A tibble: 2 x 3
    ##   green_rating   med count
    ##   <fct>        <dbl> <int>
    ## 1 0             34.2  1492
    ## 2 1             39.4   131

There is a difference of $5 between green and non-green buildings for
clusters 430 - 600, possibly due to increased environmental awareness.
We could thus generate an additional revenue of $5 \* 250,000 = $1.25
million per year.

## Insights

To summarise, we have the following points:

  - There is an additional $2.6 in revenue for green buildings
    vs. traditional buildings, and this difference jumps up to $5 for
    clusters 430 - 600.

  - There is a subtle positive relationship bwtween rent and occupancy
    rates. Green buildings have a slightly better rate of occupancy,
    possibly due to their average young age and better PR & advertising
    opportunities.

  - There doesn’t appear to be any correlation between age and occupancy
    rates.

  - On further analysis, we find that rent and size share a subtle
    positive relationship. Green buildings also, on average, are
    approximately 100,000 sq. ft. larger than non-green buildings, so
    this could be a confounding factor. But we need further data to
    corroborate this claim.

The on-site guru’s hypothesis of being able to recuperate costs in
around 8-9 years holds true. The average age for green buildings is
currently 22 years, but as green buildings become more popular, we
expect them to age and thus, the guru’s assumption that the building
would generate revenue for 30 years is fair.
