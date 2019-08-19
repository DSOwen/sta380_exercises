Market Segmentation
================

``` r
df = read.csv('social_marketing.csv')
```

## Data Description

We have a dataset of the number of tweets from customers of an
anonymised firm, in terms of categories of the tweets.

A brief summary of the data:

``` r
xxx = df[,-1]
#barplot(xxx)
x_vector<-colSums(xxx)
x_vector<-sort(x_vector, decreasing = TRUE)
barplot(x_vector, las=2)
```

![](marketing-seg_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

‘Chatter’ category (uncategorised posts) has the most number of posts,
which isn’t informative. Photo-sharing, nutrition, cooking, and politics
are also popular categories.

Are there any categories that are positively correlated with each other?

``` r
ggplot(df, aes(college_uni, online_gaming)) + geom_point()
```

![](marketing-seg_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Online gaming and universties are correlated categories. Let us plot a
correlation for all categories.

``` r
library(corrplot)
```

    ## corrplot 0.84 loaded

``` r
X = df[,-1]
rownames(df) <- df$User
correlation <- cor(X)
corrplot(correlation, method = "square")
```

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

``` r
X = scale(X, center=TRUE, scale=TRUE) #standardising the variables
```

``` r
mu = attr(X,"scaled:center")
sigma = attr(X,"scaled:scale") 
```

### Choosing the value of K

Choosing the number of clusters is completely subjective based either on
business goals or mathematical basis. Let us plot the elbow plot to get
a sense of the number of clusters to be used.

``` r
# Iterating over the number of clusters to optimize k
num_cent <- c(2:18)
rmse_all <- NULL

for(num_center in num_cent){
  cluster <- kmeans(X, centers = num_center, nstart = 50, iter.max = 20)
  rmse <- sqrt(mean(cluster$withinss))
  rmse_all <- c(rmse_all, rmse)
}

#plotting the RMSE by choosing various K's

plot(num_cent, rmse_all, type = 'b',
     xlab = "Number of Centers", ylab = "RMSE", main = "RMSE vs Number of Clusters")
```

![](marketing-seg_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Let us choose k = 5 as the optimum number of clusters.

K-means clustering will provide us with information on what posts are
tweeted togeher, which will help us create buyer personas in order to
better tailor advertising
    campaigns.

``` r
clust1 = kmeans(X, 5, nstart=25)
```

### CLuster 1

``` r
head(sort((clust1$center[1,]*sigma + mu), decreasing=TRUE))
```

    ##          chatter    photo_sharing   current_events      college_uni 
    ##         3.650288         1.839885         1.362176         1.093720 
    ## health_nutrition           travel 
    ##         1.085810         1.049616

### Cluster 2

``` r
head(sort((clust1$center[2,]*sigma + mu), decreasing=TRUE))
```

    ##      politics        travel          news       chatter     computers 
    ##      9.164179      5.808955      5.264179      4.376119      2.583582 
    ## photo_sharing 
    ##      2.398507

### Cluster 3

``` r
head(sort((clust1$center[3,]*sigma + mu), decreasing=TRUE))
```

    ## sports_fandom      religion          food     parenting       chatter 
    ##      6.022973      5.414865      4.628378      4.137838      4.095946 
    ##        school 
    ##      2.737838

### Cluster 4

``` r
head(sort((clust1$center[4,]*sigma + mu), decreasing=TRUE))
```

    ##       chatter photo_sharing       cooking   college_uni       fashion 
    ##      6.936364      5.513986      4.964336      3.244755      2.763636 
    ##      shopping 
    ##      2.717483

### Cluster 5

``` r
head(sort((clust1$center[5,]*sigma + mu), decreasing=TRUE))
```

    ## health_nutrition personal_fitness          chatter          cooking 
    ##        12.265517         6.559770         4.091954         3.485057 
    ##         outdoors    photo_sharing 
    ##         2.790805         2.555172

## Market Segments

Our clustering analysis suggests the following market segments:

1)  **Young parents:** This segment has shared posts about parenting and
    schooling - suggesting that they might be parents of a young child,
    discovering the ways of parenting. Posts about religion and sports
    may suggest that they’re trying to generate an interest in these
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
    fashion, and photo-sharing. They’re possibly social media
    influencers.

As we can see, all segments generally point towards a young customer
base. These segments can help our client design better marketing
rhetoric.
