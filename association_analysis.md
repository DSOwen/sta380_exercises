Association Rule Mining
================

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

First, let’s examine every rule generated with at least 100 instances of
the associated purchase pattern (support) and a 10% chance of the right
hand item(s) being purchased with the left-hand item. Since some items
are much more frequent than the others, we will also set an initial lift
threshold of 1, so we know there’s at least some significance to the
pattern.

``` r
# fix this
#kable(head(arules::inspect(sub_rules),15), format = "markdown", row.names = FALSE)
#arules::inspect(sub_rules)
```

Under these conditions, 500 rules are produced. The frequent single
items above are still overly present in the rules, even with the
restriction on lift.

![](association_analysis_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

The threshold will be raised to prune the rules.

![](association_analysis_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Now there are about 30. As displayed above, the most significant rules
have been picked. These include many larger frequent itemsets with a few
small frequent
    itemsets.

    ##      lhs                     rhs                      support confidence     lift count
    ## [1]  {herbs}              => {root vegetables}    0.007015760  0.4312500 3.956477    69
    ## [2]  {sliced cheese}      => {sausage}            0.007015760  0.2863071 3.047435    69
    ## [3]  {berries}            => {whipped/sour cream} 0.009049314  0.2721713 3.796886    89
    ## [4]  {beef}               => {root vegetables}    0.017386884  0.3313953 3.040367   171
    ## [5]  {beef,                                                                            
    ##       other vegetables}   => {root vegetables}    0.007930859  0.4020619 3.688692    78
    ## [6]  {beef,                                                                            
    ##       whole milk}         => {root vegetables}    0.008032537  0.3779904 3.467851    79
    ## [7]  {butter,                                                                          
    ##       whole milk}         => {whipped/sour cream} 0.006710727  0.2435424 3.397503    66
    ## [8]  {whipped/sour cream,                                                              
    ##       whole milk}         => {butter}             0.006710727  0.2082019 3.757185    66
    ## [9]  {domestic eggs,                                                                   
    ##       other vegetables}   => {root vegetables}    0.007320793  0.3287671 3.016254    72
    ## [10] {other vegetables,                                                                
    ##       tropical fruit}     => {whipped/sour cream} 0.007829181  0.2181303 3.042995    77
    ## [11] {other vegetables,                                                                
    ##       yogurt}             => {whipped/sour cream} 0.010167768  0.2341920 3.267062   100
    ## [12] {other vegetables,                                                                
    ##       pip fruit}          => {tropical fruit}     0.009456024  0.3618677 3.448613    93
    ## [13] {other vegetables,                                                                
    ##       tropical fruit}     => {pip fruit}          0.009456024  0.2634561 3.482649    93
    ## [14] {other vegetables,                                                                
    ##       tropical fruit}     => {citrus fruit}       0.009049314  0.2521246 3.046248    89
    ## [15] {citrus fruit,                                                                    
    ##       root vegetables}    => {other vegetables}   0.010371124  0.5862069 3.029608   102
    ## [16] {citrus fruit,                                                                    
    ##       other vegetables}   => {root vegetables}    0.010371124  0.3591549 3.295045   102
    ## [17] {root vegetables,                                                                 
    ##       yogurt}             => {tropical fruit}     0.008134215  0.3149606 3.001587    80
    ## [18] {root vegetables,                                                                 
    ##       tropical fruit}     => {other vegetables}   0.012302999  0.5845411 3.020999   121
    ## [19] {other vegetables,                                                                
    ##       tropical fruit}     => {root vegetables}    0.012302999  0.3427762 3.144780   121
    ## [20] {root vegetables,                                                                 
    ##       tropical fruit,                                                                  
    ##       whole milk}         => {other vegetables}   0.007015760  0.5847458 3.022057    69
    ## [21] {other vegetables,                                                                
    ##       tropical fruit,                                                                  
    ##       whole milk}         => {root vegetables}    0.007015760  0.4107143 3.768074    69
    ## [22] {other vegetables,                                                                
    ##       tropical fruit,                                                                  
    ##       whole milk}         => {yogurt}             0.007625826  0.4464286 3.200164    75
    ## [23] {other vegetables,                                                                
    ##       whole milk,                                                                      
    ##       yogurt}             => {tropical fruit}     0.007625826  0.3424658 3.263712    75
    ## [24] {other vegetables,                                                                
    ##       whole milk,                                                                      
    ##       yogurt}             => {root vegetables}    0.007829181  0.3515982 3.225716    77

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
