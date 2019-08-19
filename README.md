# STA380_exercises
This repository contains responses to the case study exercises for the STA 380 Predictive Models course in the UT McCombs MS Business Analytics program. The case studies can be found [here](https://github.com/jgscott/STA380/blob/master/exercises/README.md).

## Contributors
- Saurabh Bodas
- David Owen
- Pooja Shah
- Hannah Warren

# Visual story telling part 1: green buildings

## Exploratory Analysis

At first glance, it appears that the on-staff stats guru makes valid
points about the situaton.

As can be seen below, there is indeed a $2.6 difference in green and
non-green buildings. We control for occupancy rates \>10%.

    ## # A tibble: 2 x 3
    ##   green_rating med_rent count
    ##   <fct>           <dbl> <int>
    ## 1 0                25.0  6995
    ## 2 1                27.6   684

![](Green_buildings_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Non-green buildings also have a lot more outliers, and hence the guru's
rationale behind using the median instead of the mean holds true.

### Rent vs. Occupancy Rates

![](Green_buildings_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Although the minimum rent holds constant for all values of occupancy
rates, it shows a subtle increasing trend with increasing occupancy
rates. Let us explore if green buildings have higher occupancy rates.

    ## # A tibble: 2 x 3
    ##   green_rating med_occupancy count
    ##   <fct>                <dbl> <int>
    ## 1 0                     89.2  7209
    ## 2 1                     92.9   685

Green buildings do have slightly higher occupancy rates than non-green
buildings. As mentioned in the case study, this could be thanks to
better awareness and PR for non-green buildings, but we don't have
enough data to verify this claim.

The guru's assumption of 90% occupancy rates for the proposed green
building is in line with the occupancy rates suggested in our dataset.

### Age vs. Occupancy Rates

Are younger buildings more or less likely to be occupied?

![](Green_buildings_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

There doesn't appear to be any defined relationship between the two.

## Confounding Factors

Next, we explore the possibilites of any confounding factors that might
indirectly raise the rent for green buildings.

### 1\. Amenities

Most green buildings (72%) have amenities (only \~52% in case of
non-green), so could this be a factor in raising the rents for green
buildings?

    ## # A tibble: 4 x 4
    ## # Groups:   green_rating [2]
    ##   green_rating amenities med_rent count
    ##   <fct>        <fct>        <dbl> <int>
    ## 1 0            0             25    3550
    ## 2 0            1             25    3659
    ## 3 1            0             27     187
    ## 4 1            1             27.8   498

Controlling for amenities, i.e. comparing rents for green & non-green
buildings without amenities, there is still a premium for green
buildings ($2), and hence this theory does not hold up.

### 2\. Number of stories

![](Green_buildings_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

The minimum rent shows an increase with increasing number of stories.
Our client may thus be able to demand a higher rent with a taller
building.

    ## # A tibble: 2 x 2
    ##   green_rating med_stories
    ##   <fct>              <int>
    ## 1 0                     10
    ## 2 1                     11

Green buildings are in general 1 storey higher than non-green building,
but this difference is not strong enough to be considered a confounding
factor.

### 3\. Size

![](Green_buildings_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

From the graph above, we can see that the rent increases with increasing
available leasing space.

    ## # A tibble: 2 x 3
    ##   green_rating med_size mean_size
    ##   <fct>           <int>     <dbl>
    ## 1 0              118696   225977.
    ## 2 1              241150   325781.

Green buildings also appear to have more leasing space in general, and
thus, size could be a potential counfounding variable.

### 4\. Age

    ## # A tibble: 2 x 3
    ##   green_rating med_age count
    ##   <fct>          <int> <int>
    ## 1 0                 37  7209
    ## 2 1                 22   685

Green buildings are younger by around 15 years. Do newer buildings in
general demand higher rent?

![](Green_buildings_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

There doesn't appear to be a particular relationship between age and
rent, and hence, age doesn't appear to be a confounding factor.

``` r
summary(buildings$revenue)
```

    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ##         0    880975   2749485   6536261   7675431 468492211

## Further Recommendations

Can revenue for green buildings be maximised by choosing the right
locations to build them?

![](Green_buildings_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

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
    vs. traditional buildings, and this difference jumps up to $5 for
    clusters 430 - 600.

  - There is a subtle positive relationship bwtween rent and occupancy
    rates. Green buildings have a slightly better rate of occupancy,
    possibly due to their average young age and better PR & advertising
    opportunities.

  - There doesn't appear to be any correlation between age and occupancy
    rates.

  - On further analysis, we find that rent and size share a subtle
    positive relationship. Green buildings also, on average, are
    approximately 100,000 sq. ft. larger than non-green buildings, so
    this could be a confounding factor. But we need further data to
    corroborate this claim.

The on-site guru's hypothesis of being able to recuperate costs in
around 8-9 years holds true. The average age for green buildings is
currently 22 years, but as green buildings become more popular, we
expect them to age and thus, the guru's assumption that the building
would generate revenue for 30 years is fair.


# Visual story telling part 2: flights at ABIA

## Maps

For anyone unfortunate enough to be taking a flight, one of the biggest
concerns on their mind might be, "Will my plane be delayed?" Such a
possibility is frustratingly common and worse, unpredictable

On the *slightly* brighter side, we can at least tell which flights have
historically been delayed more often than most, be it due to weather,
traffic, or any other obstruction.

![](Airport_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Here is a map of flights to and from Austin-Bergstrom International
Airport. Several high-risk flights stand out in red.

![](Airport_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Compare that to how common flights to each destination are, and you can
see that the most-delayed flights are not the most common to occur.

## Cancellations

Worse than a flight being delayed is a complete cancellation. See the
plots below for the times were flights have historically been cancelled
most often, grouped by each airline.

![](Airport_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Now let's examine them by time of day:

![](Airport_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Northwest Airlines had no cancellations\!

Below are the types of cancellations that happen during the day.

![](Airport_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->


# Portfolio Modeling


Uploading the libraries

    ## Warning: package 'mosaic' was built under R version 3.6.1

    ## Loading required package: dplyr

    ## Warning: package 'dplyr' was built under R version 3.6.1

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    ## Loading required package: lattice

    ## Loading required package: ggformula

    ## Warning: package 'ggformula' was built under R version 3.6.1

    ## Loading required package: ggplot2

    ## Loading required package: ggstance

    ## Warning: package 'ggstance' was built under R version 3.6.1

    ## 
    ## Attaching package: 'ggstance'

    ## The following objects are masked from 'package:ggplot2':
    ## 
    ##     geom_errorbarh, GeomErrorbarh

    ## 
    ## New to ggformula?  Try the tutorials: 
    ##  learnr::run_tutorial("introduction", package = "ggformula")
    ##  learnr::run_tutorial("refining", package = "ggformula")

    ## Loading required package: mosaicData

    ## Warning: package 'mosaicData' was built under R version 3.6.1

    ## Loading required package: Matrix

    ## Registered S3 method overwritten by 'mosaic':
    ##   method                           from   
    ##   fortify.SpatialPolygonsDataFrame ggplot2

    ## 
    ## The 'mosaic' package masks several functions from core packages in order to add 
    ## additional features.  The original behavior of these functions should not be affected by this.
    ## 
    ## Note: If you use the Matrix package, be sure to load it BEFORE loading mosaic.

    ## 
    ## Attaching package: 'mosaic'

    ## The following object is masked from 'package:Matrix':
    ## 
    ##     mean

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     stat

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     count, do, tally

    ## The following objects are masked from 'package:stats':
    ## 
    ##     binom.test, cor, cor.test, cov, fivenum, IQR, median,
    ##     prop.test, quantile, sd, t.test, var

    ## The following objects are masked from 'package:base':
    ## 
    ##     max, mean, min, prod, range, sample, sum

    ## Warning: package 'quantmod' was built under R version 3.6.1

    ## Loading required package: xts

    ## Warning: package 'xts' was built under R version 3.6.1

    ## Loading required package: zoo

    ## Warning: package 'zoo' was built under R version 3.6.1

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ## Registered S3 method overwritten by 'xts':
    ##   method     from
    ##   as.zoo.xts zoo

    ## 
    ## Attaching package: 'xts'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     first, last

    ## Loading required package: TTR

    ## Warning: package 'TTR' was built under R version 3.6.1

    ## Registered S3 method overwritten by 'quantmod':
    ##   method            from
    ##   as.zoo.data.frame zoo

    ## Version 0.4-0 included new data defaults. See ?getSymbols.

    ## Warning: package 'foreach' was built under R version 3.6.1

Importing the stocks that we want to use

    ## 'getSymbols' currently uses auto.assign=TRUE by default, but will
    ## use auto.assign=FALSE in 0.5-0. You will still be able to use
    ## 'loadSymbols' to automatically load data. getOption("getSymbols.env")
    ## and getOption("getSymbols.auto.assign") will still be checked for
    ## alternate defaults.
    ## 
    ## This message is shown once per session and may be disabled by setting 
    ## options("getSymbols.warning4.0"=FALSE). See ?getSymbols for details.

    ## pausing 1 second between requests for more than 5 symbols
    ## pausing 1 second between requests for more than 5 symbols
    ## pausing 1 second between requests for more than 5 symbols

Adjusting for splits and dividends

Combining close to close changes in a single matrix

    ## [1] "Portfolio a: "

    ##               ClCl.EDENa     ClCl.GXCa    ClCl.CXSEa   ClCl.QEMMa
    ## 2014-06-06 -0.0022484355  0.0002689567 -0.0005862615  0.009933741
    ## 2014-06-09  0.0015023850  0.0084677014  0.0031286665 -0.002131164
    ## 2014-06-10  0.0007499906  0.0058643477  0.0077973101  0.000000000
    ## 2014-06-11 -0.0106801576 -0.0051676030 -0.0069632687  0.000000000
    ## 2014-06-12  0.0077651517 -0.0006659963 -0.0001947409  0.000000000
    ## 2014-06-13 -0.0052621501  0.0106624418  0.0126631205  0.018728422
    ##               ClCl.IEMGa
    ## 2014-06-06  0.0098951880
    ## 2014-06-09  0.0032661288
    ## 2014-06-10  0.0045959019
    ## 2014-06-11 -0.0032405262
    ## 2014-06-12 -0.0036336392
    ## 2014-06-13  0.0001919962

    ## [1] ""

    ## [1] "Portfolio b: "

    ##                ClCl.FSZb   ClCl.JPMVb     ClCl.FCAb    ClCl.TURb
    ## 2014-06-06 -0.0004515918  0.004694875 -0.0045641716  0.011635866
    ## 2014-06-09  0.0038400497 -0.007009365  0.0000000000  0.001860639
    ## 2014-06-10 -0.0045003826 -0.005098000  0.0050436041  0.013000169
    ## 2014-06-11 -0.0099458178  0.002759145 -0.0018248631 -0.051999983
    ## 2014-06-12  0.0002283562  0.007665075  0.0009141225  0.004043600
    ## 2014-06-13 -0.0034238986 -0.001755413  0.0000000000 -0.006303642

    ## [1] ""

    ## [1] "Portfolio c: "

    ##               ClCl.EWJc     ClCl.EWLc     ClCl.EWNc    ClCl.ASHRc
    ## 2014-01-03  0.005862710  0.0099348032 -0.0015673981 -0.0045454957
    ## 2014-01-06 -0.003330558 -0.0003073778 -0.0007849686 -0.0249066002
    ## 2014-01-07  0.004177130  0.0009224785  0.0062844464  0.0021286079
    ## 2014-01-08  0.001663852 -0.0012289094  0.0003902420  0.0004247239
    ## 2014-01-09 -0.003322259  0.0055367890  0.0011705424 -0.0148619115
    ## 2014-01-10  0.006666667  0.0104007345  0.0074045207  0.0073275859
    ##              ClCl.KFYPc   ClCl.GREKc   ClCl.ERUSc
    ## 2014-01-03 -0.010716442 -0.010118786 -0.005673712
    ## 2014-01-06 -0.001547601  0.004888933 -0.019495934
    ## 2014-01-07  0.002014972  0.026536841  0.004364597
    ## 2014-01-08  0.014385150  0.026712668 -0.002897127
    ## 2014-01-09  0.000000000  0.005874906 -0.005326804
    ## 2014-01-10  0.000000000  0.014184398  0.018500437

Computing the returns from the closing prices
![](finalproj_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->![](finalproj_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](finalproj_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->
Sampling a random return from the empirical joint distribution

Update the value of my holdings, starting with an equal distribution to
each asset

    ## [1] 100952.9

    ## [1] 98416.81

    ## [1] 100240.9

Loop over 4 trading
weeks

    ## [1] 100104.7

    ## [1] 102691

    ## [1] 94916.98

![](finalproj_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->![](finalproj_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->![](finalproj_files/figure-gfm/unnamed-chunk-8-3.png)<!-- -->

    ##               [,1]      [,2]      [,3]      [,4]      [,5]      [,6]
    ## result.1  99611.53  99215.10  96809.80  95829.40  94317.40  95026.12
    ## result.2 100677.99 100197.70 100759.50 100213.31 100721.31 100073.47
    ## result.3  99555.35 101895.77 101604.30 100125.35 100437.51 100878.09
    ## result.4 100096.03  99941.09 100049.19  99433.99  99170.13  99957.07
    ## result.5  98752.10 100013.43  99277.66  99183.40 100457.08  99956.65
    ## result.6  99086.47  98486.74  98394.70  99334.17  99552.39  98368.74
    ##               [,7]      [,8]      [,9]     [,10]     [,11]     [,12]
    ## result.1  95191.21  95389.06  95061.64  95083.53  95234.81  95015.11
    ## result.2 100783.77 102213.67 102463.05 102584.80 103492.89 103461.98
    ## result.3 101579.25 101375.36 100967.65 101848.41 101182.62 102946.12
    ## result.4 101917.95 102457.05 100247.66 101473.53 100236.21 101972.58
    ## result.5  99375.63  99003.65 100114.64 100020.44 101238.63 100565.79
    ## result.6  98133.63  98892.87  98359.16  97850.92  97742.41  98408.11
    ##              [,13]     [,14]     [,15]     [,16]     [,17]     [,18]
    ## result.1  96168.49  96600.89  95607.15  93961.68  95424.29  95627.37
    ## result.2 103992.29 106263.77 104676.26 103821.54 104423.08 105469.43
    ## result.3 104679.86 105244.25 105026.39 107053.76 107224.22 106647.97
    ## result.4 102244.93 102151.25 102736.69 104108.61 103968.79 103530.06
    ## result.5 100883.92 101571.38 100841.96 100863.64 102028.27 102665.45
    ## result.6  99721.42  99748.20  99860.20 101479.76 101469.15 101007.98
    ##              [,19]    [,20]
    ## result.1  94948.52  93645.5
    ## result.2 105560.39 106219.1
    ## result.3 107018.03 105052.5
    ## result.4 101108.95 100888.2
    ## result.5 101728.17 101362.0
    ## result.6 101339.96  99795.8

![](finalproj_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

    ##       5% 
    ## 93111.26

Second
    Portfolio

    ##               [,1]      [,2]      [,3]      [,4]      [,5]      [,6]
    ## result.1 100069.09  99504.49 100239.77 101742.54 102306.67 102292.39
    ## result.2 100212.53 100428.40 102200.01 104151.79 103875.69 103657.38
    ## result.3 100853.32 101869.97 102674.87 102639.20 103702.01 105245.96
    ## result.4 100871.72 101898.85 101842.91 102619.58 102834.31 102624.07
    ## result.5  99857.27  97376.31  98003.09  97887.97  97793.52  97413.71
    ## result.6 100527.75  99963.53  99971.77 101271.77 101545.28 101542.11
    ##               [,7]      [,8]      [,9]     [,10]     [,11]     [,12]
    ## result.1 102000.15 100125.32  99595.93 100079.51  99793.38  99690.28
    ## result.2 102396.45 102949.80 103368.60 103144.50 103589.20 103389.24
    ## result.3 105299.76 105808.16 106400.91 106658.02 107398.08 107251.09
    ## result.4 103254.17 102544.38 103836.22 104190.83 104714.52 103194.47
    ## result.5  97309.82  96628.53  97257.68  97443.53  96391.40  96322.86
    ## result.6 101347.86  99398.50  99657.77  99041.67  99408.89  99741.64
    ##              [,13]     [,14]     [,15]     [,16]    [,17]    [,18]
    ## result.1  99520.97 100455.89 100249.48 100049.60 101991.5 103088.5
    ## result.2 103184.53 104923.90 104668.27 104829.71 104458.0 104404.1
    ## result.3 106588.64 106738.72 107097.80 107051.86 107031.5 107344.6
    ## result.4 103500.55 103138.94 104110.34 104732.24 105984.9 106578.2
    ## result.5  96334.62  99057.13  99248.74  98873.59 100508.0 100180.5
    ## result.6 100194.32 100245.25 100638.97 102129.36 102503.9 102835.9
    ##             [,19]    [,20]
    ## result.1 103282.5 103049.8
    ## result.2 105836.4 105456.5
    ## result.3 105377.3 104223.6
    ## result.4 105441.5 107499.8
    ## result.5 102197.1 100493.1
    ## result.6 102194.3 101777.8

![](finalproj_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

    ##       5% 
    ## 93234.85

Portfolio
    C

    ##               [,1]      [,2]      [,3]      [,4]      [,5]      [,6]
    ## result.1  99842.84 100729.01 101138.92 100325.41 101124.20 102330.98
    ## result.2 101007.11 102304.86 104051.95 103549.75 102710.24 104394.14
    ## result.3 100201.08 100019.15 100282.81 100239.82  99771.65 100446.96
    ## result.4  99873.81 100154.12 100784.25 100679.43 101759.51 102122.56
    ## result.5  99026.10  98309.49  98402.94  98606.19  98723.05  98719.79
    ## result.6  99636.22  99993.29 100806.06 101094.28 100260.40 101122.38
    ##               [,7]      [,8]      [,9]     [,10]     [,11]     [,12]
    ## result.1 102570.79 102591.26 103943.39  99933.63 100376.19 102625.27
    ## result.2 105053.62 103970.13 104533.71 104930.00 105824.21 105838.19
    ## result.3 101159.16 102791.33 102825.78 103342.50 104319.07 103965.51
    ## result.4 101531.89  99416.44 100128.82  99554.44  99296.60  98838.27
    ## result.5  98190.96  95893.17  96722.86  98347.42  97685.76  96694.08
    ## result.6 100309.67  99290.68  98837.29  99065.03  99438.31  99733.73
    ##              [,13]     [,14]     [,15]     [,16]     [,17]     [,18]
    ## result.1  99717.27  98401.74  97314.15  96578.42  96056.96  95911.96
    ## result.2 105522.67 105864.06 105485.29 104840.33 105994.52 106543.87
    ## result.3 104245.80 106125.72 105430.60 104846.94 106302.47 107860.42
    ## result.4  97472.03  98428.83  97706.93  96901.01  95735.28  94260.28
    ## result.5  97078.66  96098.15  95387.28  95535.45  95745.94  95841.02
    ## result.6 100486.77 103565.86 103685.16 104106.71 105451.08 105620.23
    ##              [,19]     [,20]
    ## result.1  95723.14  93308.63
    ## result.2 109226.27 106359.30
    ## result.3 106852.67 105159.78
    ## result.4  91934.46  89580.77
    ## result.5  95689.65  96766.28
    ## result.6 105146.26 104784.34

![](finalproj_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

    ##       5% 
    ## 92038.78

I selected the portfolios with the intention of giving each one a
different aim, despite all of the ETF's being chosen from either the
China ETF category, the Japan ETF category, Euro ETF category, or the
Emerging Markets category. The first portfolio was chosen by being
comprised of ETF's which have the highest previous day's closing cost.
It contained 5 ETF's (EDEN, GXC, CXSE, QEMM, and IEMG), with closing
costs ranging from $47.99 to $87.82. The second portfolio contained 4
ETF's (FSZ, JPMV, FCA, and TUR), and those ETF's were chosen with the
aim of minimizing the percent change from the previous day (in order to
minimize short-term volatility). The third portfolio contained 7 ETF's
(EWJ, EWL, EWN, ASHR, KFYP, GREK, and ERUS), and were chosen with the
intention of maximizing YTD, in order to maximize long-term growth. For
the first portfolio the VaR was equal to 93091.06. That means that there
is if there were no trading over the course of a day, there is a 5%
chance that the stock would decrease in value by $93,091.06. For the 2nd
portfolio, the VaR was equal to 93291.91, and the third portfolio had a
VaR of 91864.87. These both translate to roughly the same thing as did
portfolio 1. Looking at the histograms, portofolio 1 had a maximum value
between 100000 and 102000, implying that at the very least, you would
make back what you'd invested a majority of the time. Portfolio 2 had an
absolute maximum value between 100000 and 101000, implying that you'd
also make back your money by investing in that stock a majority of the
time. Histogram 3 however had a majority of values between 98000 and
100000, implying that a majority of days, you would actually be losing
money. This narrows us down to Portfolio 1 and Portfolio 3. Looking back
at VaR, we see that portfolio 1 has a fatter tail than portfolio 3,
therefore we choose Portfolio 3 for the purpose of our experiment.


# Market Segmentation

## Data Description

We have a dataset of the number of tweets from customers of an
anonymised firm, in terms of categories of the tweets.

A brief summary of the data:

![](marketing-seg_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

‘Chatter' category (uncategorised posts) has the most number of posts,
which isn't informative. Photo-sharing, nutrition, cooking, and politics
are also popular categories.

Are there any categories that are positively correlated with each other?

![](marketing-seg_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Online gaming and universties are correlated categories. Let us plot a
correlation for all categories.

    ## Warning: package 'corrplot' was built under R version 3.6.1

    ## corrplot 0.84 loaded

![](marketing-seg_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

The following categories are highly correlated: (represented by dark
squares)

1)  Personal fitness & nutrition,
2)  fashion and cooking,
3)  politics and travel,
4)  religion and sports fandom
5)  Online gaming and universities

However, these show us correlations in two dimensions. To better
understand market segments, let us perform k-means clustering.

## K-means Clustering

Including only the measureable categories for clustering:

### Choosing the value of K

Choosing the number of clusters is completely subjective based either on
business goals or mathematical basis. Let us plot the elbow plot to get
a sense of the number of clusters to be used.

![](marketing-seg_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Let us choose k = 5 as the optimum number of clusters.

K-means clustering will provide us with information on what posts are
tweeted togeher, which will help us create buyer personas in order to
better tailor advertising
    campaigns.

### CLuster 1

    ##       chatter      politics photo_sharing   college_uni        travel 
    ##      6.533920      4.398869      3.885678      3.432161      3.168970 
    ##          news 
    ##      2.568467

### Cluster 2

    ##          cooking    photo_sharing          fashion          chatter 
    ##        11.241007         6.008993         5.719424         4.517986 
    ##           beauty health_nutrition 
    ##         3.994604         2.372302

### Cluster 3

    ## health_nutrition personal_fitness          chatter          cooking 
    ##        12.093574         6.506201         4.166855         3.267193 
    ##         outdoors    photo_sharing 
    ##         2.768884         2.506201

### Cluster 4

    ## sports_fandom      religion          food     parenting       chatter 
    ##      6.000000      5.401077      4.641992      4.161507      4.083445 
    ##        school 
    ##      2.769852

### Cluster 5

    ##          chatter    photo_sharing   current_events health_nutrition 
    ##        3.6615497        1.8652534        1.3421053        1.0913743 
    ##           travel         politics 
    ##        1.0609162        0.9973197

## Market Segments

Our clustering analysis suggests the following market segments:

1)  **Young parents:** This segment has shared posts about parenting and
    schooling - suggesting that they might be parents of a young child,
    discovering the ways of parenting. Posts about religion and sports
    may suggest that they're trying to generate an interest in these
    categories regarding these categories.

2)  **Knowledgeable college students**: This segment has shown activity
    in categories such as college, current events, and travelling.
    College students travel often for vacation, studies, or internships,
    and thus, this segment appears to represent smart college students.

3)  **Health freaks**: Cooking, health & nutrition, and personal fitness
    would mean that this segment is committed to maintain great health.

4)  **Tech-savvy business travellers**: With posts spanning politics,
    technology, news, and travel, this segment possibly represents
    well-educated business-people who travel often for work.

5)  **Millenial Influencers**: This segment indulges in shopping,
    fashion, and photo-sharing. They're possibly social media
    influencers.

As we can see, all segments generally point towards a young customer
base. These segments can help our client design better marketing
rhetoric.


# Author Attribution

### The problem is to predict the author of a given article. We have 2500 training articles of 50 different authors and the same number of test articles.

### Data Preprocessing

The first step is to read all the files.

    ## [1] "C50test/AaronPressman/421829newsML.txt"
    ## [2] "C50test/AaronPressman/424074newsML.txt"
    ## [3] "C50test/AaronPressman/42764newsML.txt" 
    ## [4] "C50test/AaronPressman/43033newsML.txt" 
    ## [5] "C50test/AaronPressman/433558newsML.txt"
    ## [6] "C50test/AaronPressman/436774newsML.txt"

The file names are messy so we can format them by taking the author name
and joining it with the names of each
    file.

    ## [1] "AaronPressman421829newsML.txt" "AaronPressman424074newsML.txt"
    ## [3] "AaronPressman42764newsML.txt"  "AaronPressman43033newsML.txt" 
    ## [5] "AaronPressman433558newsML.txt" "AaronPressman436774newsML.txt"

The author names are extracted by using the file paths and extracting
just the second word from the path, which is the author
    name

    ## [1] "AaronPressman" "AaronPressman" "AaronPressman" "AaronPressman"
    ## [5] "AaronPressman" "AaronPressman"

Now we need to perform different transformations on the data. 1. Convert
the all the alphabets to lowercase 2. Remove numbers and punctuations 3.
Strip whitespaces - spaces, tabs, etc 4. Stem words. This means that
words like sing, singing, sings, etc all become sing 5. Remove stop
words, that is words like the, a, to, etc, that occur frequently in
English.

Now we create the document term matrix(DTM), which is a matrix
containing the counts of word in each documents.

    ## <<DocumentTermMatrix (documents: 5000, terms: 32021)>>
    ## Non-/sparse entries: 1006748/159098252
    ## Sparsity           : 99%
    ## Maximal term length: 45
    ## Weighting          : term frequency (tf)

We can see that 99% of the natrix is sparse(0 values) and we have 32021
words

Now, we will remove values that are the most uncommon as they add noise
to the data. We remove the terms that have count 0 in \>95% of documents

    ## <<DocumentTermMatrix (documents: 5000, terms: 830)>>
    ## Non-/sparse entries: 645959/3504041
    ## Sparsity           : 84%
    ## Maximal term length: 13
    ## Weighting          : term frequency (tf)

We have 830 words and the matrix is 84% sparse.

Now we convert the DTM to a data frame and add the author labels to the
data frame

After this, we split the data into train and test sets. The first 2500
observations go into the training set and the rest go in the test set.

Now we create the Term Frequency'Inverse Document Frequency (tf-idf)
matrix. We add the author names and split it, similar to the way we did
for the DTM.

### Predictive Modelling

First we will run a random forest model on the DTM data.

    ## [1] 0.6212

The accuracy on the test data is 62.12%.

The plot below gives the list of important variables
![](AuthorAttribution_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

Using the variable importance values, we take the 250 most important and
rerun the model with just those variables

    ## [1] 0.5888

The accuracy is now 58.88% which is close to the accuracy with the model
with all the variables.

On running the Random Forest model on the tfidf data and we get 61.16%
accuracy.

    ## [1] 0.6116

Similar to the DTM, we find the important variables and re-run the
model.
![](AuthorAttribution_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

    ## [1] 0.6012

The accuracy is 60.12% which is almost the same as the accuracy with all
variables on the tfidf data.

### Principal Component Analysis

First we run the PCA on the DTM data
![](AuthorAttribution_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

On plotting the proportion of variance explained for each Principal
Components, we can see that around 400 Principal Components explain 80%
of the variance in the data.

We run the Random Forest Model again on this data. A point to be noted
here is that we cannot run PCA on the train and test data together. Nor
can we run PCA on train data and then use it to predict the test data
values.

So we take the transformations of the training data and use them to
change the test data. This can be done simply by using the predict
function

Now we test our model on this new transformed data.

    ## [1] 0.4764

We get an accuracy of 47.64% Which is not as good as the accuracy of the
model run on the actual variables instead of the Principal Components.

Now we will perform the same steps for the tfidf data.

![](AuthorAttribution_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

Here, around 350 Principle Components explain 80% of the variance so run
the model with that many PCs and get an accuracy of 52.84% which is much
better than the accuracy on the DTM-PC model.

    ## [1] 0.5284

We also tried a naive bayes model on the tfidf principle components data
and got an accuracy of 47.08%.

    ## [1] 0.4708

Based on the models we have run, the Random Forest Model on the DTM data
with the 250 variables is the best one. Even though the model with all
variables gives a slightly better accuracy, the selected model makes up
for it by being less complex.


# Association Rule Mining

## The Data in Question

The data to be examined contains fifteen thousand grocery store
transactions. Each transaction contains between 1 and 4 items,
inclusive. The head of the dataset is previewed
below.

| V1               | V2                  | V3             | V4                       |
| :--------------- | :------------------ | :------------- | :----------------------- |
| citrus fruit     | semi-finished bread | margarine      | ready soups              |
| tropical fruit   | yogurt              | coffee         | NA                       |
| whole milk       | NA                  | NA             | NA                       |
| pip fruit        | yogurt              | cream cheese   | meat spreads             |
| other vegetables | whole milk          | condensed milk | long life bakery product |
| whole milk       | butter              | yogurt         | rice                     |

Before looking for association rules apriori, it is important to examine
the frequency with which each item is purchased in case there is a
heavily skewed support distribution. Such analysis gives an indication
of the importance of lift in the rules to be produced.

![](association_analysis_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

A closer inspection of the far right spike finds that the following
items are very common in the dataset. Here they are:

| Item             | Occurrences |
| :--------------- | ----------: |
| yogurt           |        1372 |
| soda             |        1715 |
| rolls/buns       |        1809 |
| other vegetables |        1903 |
| whole milk       |        2513 |

None of the most common items are surprising to see in the table. Given
that information, a threshold for minimum lift will be considered
carefully, and rules produced which include the above items will be
heavily scrutinized.

## Generate Rules

First, let's examine every rule generated with at least 100 instances of
the associated purchase pattern (support) and a 10% chance of the right
hand item(s) being purchased with the left-hand item. Since some items
are much more frequent than the others, we will also set an initial lift
threshold of 1, so we know there's at least some significance to the
pattern.

![](association_analysis_files/sub_rules.png)

Under these conditions, 500 rules are produced. The frequent single
items above are still overly present in the rules, even with the
restriction on lift.

![](association_analysis_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

The threshold will be raised to prune the rules.

![](association_analysis_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Now there are about 30. As displayed above, the most significant rules
have been picked. These include many larger frequent itemsets with a few
small frequent itemsets.

![](association_analysis_files/better_rules.png)

## Analysis

Under the above conditions, an even balance of complex and simple rules
are produced. The common items still apear in many of the rules, but
with the restrictions on lift, we can confidently say that the rules
indicate a significant tendency for them to appear in those itemsets.
Some of the rules also exist in both directions with high confidence,
showing that the correlation is mutual.

Objectively, we can say that these rules are superior by the high
threshold for lift. Subjectively, many of them are sensible, like the
correlations between herbs, fruits, and vegetables or butter and milk.

![](association_analysis_files/rules.png)
