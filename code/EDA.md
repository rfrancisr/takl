# EDA | Takl
Francis Ratsimbazafy  



## Data {.tabset}

We are provided two different datasets. The first one contains a detail of Takl's operations, where information on the job, its status, scheduled date and time for the job are stored. The second data contains information for a specific zip code. In particular, it shows the region and the location (in terms of latitude and longitude) to which a zip code belongs. Let us look at few rows from each dataset.

### Appointment

    Id  chore                                   category                  subcategory                                                                                                                                                         skill                     responses   yes.responses   notif   disputes   zipcode  preceding.appt   counterbid    original.price   accepted.price  discounts   schedule.time             status      token                                  arrival.time              enroute   completed.time            created.at      updated.at    
------  --------------------------------------  ------------------------  ------------------------------------------------------------------------------------------------------------------------------------------------------------------  -----------------------  ----------  --------------  ------  ---------  --------  ---------------  -----------  ---------------  ---------------  ----------  ------------------------  ----------  -------------------------------------  ------------------------  --------  ------------------------  --------------  --------------
  7020  Remove 1 shed - disposal fee applies*   Haul away                 Outdoor                                                                                                                                                             Remove metal/wood shed            2               2      NA         NA     37076  No               FALSE                     NA              150  NA          2017-02-07 14:00:00 UTC   canceled    d3285bbf-06fd-4dbf-ad15-5a031a217ea9   NA                        NA        NA                        1/31/17 20:14   2/8/17 21:56  
 20466  Perform an extra small chore            Your Custom Job           Choose this option when you have a job that is not on our list. Pick the size of your job, take a few photos, include instructions as a Note, and you're all set!   Custom Job                       14               1      NA         NA     37204  No               FALSE                     NA               50  NA          NA                        finalized   fcb1c07b-5d19-4492-b6ed-bdfc8e2ea946   2017-04-30 17:19:36 UTC   NA        2017-04-30 18:50:39 UTC   4/30/17 11:25   4/30/17 14:58 
 25379  Clean 1 bedroom apartment               Cleaning & Housekeeping   Clean Apartment                                                                                                                                                     Clean apartment                  26               3      NA         NA     29707  No               FALSE                     NA               70  6F294       2017-05-21 18:00:00 UTC   canceled    b5474e3c-b6f5-4f4e-83c0-2b3b7e07f808   NA                        NA        NA                        5/20/17 19:26   5/21/17 10:32 

There are 10.000 jobs in this data. Most of them are either finalized or cancelled. 5212 jobs were cancelled, which represent 52.12% of the data, and 4649 jobs were finalized. 

![](EDA_files/figure-html/unnamed-chunk-1-1.png)<!-- -->





### Zip data

   Id   zipcode  created.at                updated.at                provider.signup   lnglat                                          region  
-----  --------  ------------------------  ------------------------  ----------------  ----------------------------------------------  --------
 5085     30683  2017-03-15 15:44:03 UTC   2017-03-15 15:44:04 UTC   TRUE              POINT (-9268606.119911239 4020158.329738545)    Atlanta 
 5084     30677  2017-03-15 15:43:55 UTC   2017-03-15 15:43:56 UTC   TRUE              POINT (-9284079.339888368 3998822.9380719666)   Atlanta 
 5083     30671  2017-03-15 15:43:52 UTC   2017-03-15 15:43:52 UTC   TRUE              POINT (-9258442.049276562 3995282.329624237)    Atlanta 
 5082     30669  2017-03-15 15:43:45 UTC   2017-03-15 15:43:46 UTC   TRUE              POINT (-9245931.475095455 3984395.1457336657)   Atlanta 
 5081     30667  2017-03-15 15:43:37 UTC   2017-03-15 15:43:38 UTC   TRUE              POINT (-9260260.542214263 4003222.667037119)    Atlanta 
 5080     30662  2017-03-15 15:43:30 UTC   2017-03-15 15:43:31 UTC   TRUE              POINT (-9250710.966300715 4062155.8771902123)   Atlanta 

There are duplicated rows in the zip data. I removed all of them (some of them were duplicate with respect to all the columns, others are created at a later date and time, but essentially have the same zipcode, longlat, region).

## Most popular jobs by Region
At a first glance, yard job, custom job, handyman, cleaning and housekeeping, and haul away are the most demanded jobs at Takl. The first graph shows the most popular jobs irrespective of the status. The second figure is limited to jobs that are finalized. As we can see, these categories remain top 5, although the order is a litte bit altered, with handyman being the top demanded job, followed by yard, custom jobs, cleaning and housekeeping, and haul away. Finally, the second row shows the most profitable finalized jobs. Again, the previous categories remain top five, and handyman being the most lucrative category.
![](EDA_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

The five most popular jobs identified earlier remain the most demanded jobs, even at a region level. As can be seen from the figure below, yard jobs, handyman, cleaning and housekeeping, haul away, and custom jobs are the top five most demanded jobs in all regions. The order varies by region, and in general, yard and handyman are the most popular. In the figure below, I restricted the data to finalized jobs only.
![](EDA_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

## Duration of jobs, by region
The heatmap belows shows the average duration of job categories for each region. The color gradient goes from white, indicating the lowest job duration, to blue. It can be seen that on average, most finalized jobs are completed within five to ten hours with few exceptions such as a home management done in South Florida that went over 20 hours. In general, there is not too much variation in terms of job duration across regions. In particular, if we look at three of the most popular jobs (yard, cleaning and housekeeping and handyman) we identified earlier, the duration is somehow uniform across regions.

![](EDA_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

## Cancellation rate over time
The figure below shows that at the beginning, the average cancellation rate had fluctuated a lot, with some day having all jobs being cancelled. However, this behavior can be explained by the limited total number of operations that Takl had at the beginning. Over time, the daily average cancellation rate levelled off around fifty percent, or one job out of two gets cancelled every day on average, and the number of jobs scheduled (regardless of status) have substantially increased.
![](EDA_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## Counter-bidding: positive or negative effect on cancellations?
To begin with, let us test first whether there is any dependency between counter-bidding and cancellations. In the table below, I run a Pearson chi-squared test with the null hypothesis being the two categorical variables are independent. The result shows that p-value is very small in magnitude. Thus, say at a significance level of 5 percent, we can find enough statistical evidence that the two variables have a relationship. Furthermore, the table, as well as the graph below it, states that jobs that have counterbid are twice as likely to be cancelled as jobs without counterbid. In this regard, there is a suggestion that counterbid have a positive effect on cancellations (positive effect meaning that the correlation between the two variables is positive, or in other words, jobs with counterbid are likely to be cancelled).


```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  10000 
## 
##  
##                       | bid.cancel$cancelled 
## bid.cancel$counterbid |     cancelled | not cancelled |     Row Total | 
## ----------------------|---------------|---------------|---------------|
##                 FALSE |          4825 |          4629 |          9454 | 
##                       |          0.48 |          0.46 |               | 
## ----------------------|---------------|---------------|---------------|
##                  TRUE |           387 |           159 |           546 | 
##                       |          0.04 |          0.02 |               | 
## ----------------------|---------------|---------------|---------------|
##          Column Total |          5212 |          4788 |         10000 | 
## ----------------------|---------------|---------------|---------------|
## 
##  
## Statistics for All Table Factors
## 
## 
## Pearson's Chi-squared test 
## ------------------------------------------------------------
## Chi^2 =  81.44107     d.f. =  1     p =  1.805665e-19 
## 
## Pearson's Chi-squared test with Yates' continuity correction 
## ------------------------------------------------------------
## Chi^2 =  80.64788     d.f. =  1     p =  2.69744e-19 
## 
## 
```

![](EDA_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## What else: a study of cancelled jobs? {.tabset}
Cancelled jobs mean a loss of revenue for Takl. To this respect, it is important to identify any reasons that may decrease job cancellation. Note that there were 5212 cancelled jobs in total over the period Takl has operated. In the following, we used the job's scheduled time to extract the information on month.

We can see that the total number of jobs cancellation increased month after month, with a peak in May. It is however very interesting to see that cancellation has substantially decreased for June, both in absolute and relative terms. The reduction in the absolute number of cancellation can be partly explained by the decrease of demand in June as shown by the third figure. But, that is not the entire story.

Upon further look, we see that the cancellation for law mowing and home cleaning have dropped a lot in June compared to the other months. Moreover, the decrease of cancelled jobs in Nahsville and South Florida that happened in June seems to influence this irregularity in June.

### Month

```
## Warning: Removed 1 rows containing missing values (position_stack).
```

![](EDA_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


### Skill
![](EDA_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

### Region
![](EDA_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

