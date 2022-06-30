Interesting Programming Experience Using R
================
Fang Wu

## Coolest thing about programming in R.

I think package `caret` is really cool, as it put everything we need to
fit model in one package and make the processing really simply and
straight forward. Especially, we can put pre processing of data in
function `train` to ensure that the test set got the exactly same
transformation as the train set. We also can specify the model method,
cross validation argument etc. in function `train`. It is also very easy
for compare metric for diffetent models.

One example:

-   Read in data

``` r
data <- read_csv("../../../558 data science for statistician/HW/module 8/SeoulBikeData.csv", col_names=TRUE, locale=locale(encoding="latin1")) 
data_convention <- data %>% rename(Rented_count=`Rented Bike Count`, Wind_speed=`Wind speed (m/s)`,
                                   Dew_temperature=`Dew point temperature(°C)`,
                                   Temperature=`Temperature(°C)`,
                                   Humidity=`Humidity(%)`, Radiation=`Solar Radiation (MJ/m2)`,
                                   Functioning_day=`Functioning Day`,
                                   Visibility=`Visibility (10m)`, Rainfall=`Rainfall(mm)`,
                                   Snowfall=`Snowfall (cm)`)
data$Date <- as.Date(data_convention$Date, "%d/%m/%Y")
data_convention
```

    ## # A tibble: 8,760 x 14
    ##    Date     Rented_count  Hour
    ##    <chr>           <dbl> <dbl>
    ##  1 01/12/2~          254     0
    ##  2 01/12/2~          204     1
    ##  3 01/12/2~          173     2
    ##  4 01/12/2~          107     3
    ##  5 01/12/2~           78     4
    ##  6 01/12/2~          100     5
    ##  7 01/12/2~          181     6
    ##  8 01/12/2~          460     7
    ##  9 01/12/2~          930     8
    ## 10 01/12/2~          490     9
    ## # ... with 8,750 more rows,
    ## #   and 11 more variables:
    ## #   Temperature <dbl>,
    ## #   Humidity <dbl>,
    ## #   Wind_speed <dbl>,
    ## #   Visibility <dbl>,
    ## #   Dew_temperature <dbl>, ...

-   Split data using function `createDataPartition` from package `caret`

``` r
set.seed(100)
train_index <- createDataPartition(data_convention$Functioning_day, p=0.75, list=FALSE)
train_data <- data_convention[train_index, ]
test_data <- data_convention[-train_index, ]
```

-   MLR Modeling with cross validation

I am going to involve Hour, Temperature, Radiation, Visibility, Seasons
and Holiday based on correlation between continuous variables.

``` r
lrfit1 <- train(Rented_count~Hour+Temperature+Seasons+Holiday+Functioning_day, data=train_data,
               method="lm",
               trControl=trainControl(method="cv", number=5))
lrfit2 <- train(Rented_count~Hour*Temperature+Seasons+Holiday+Functioning_day, data=train_data,
               method="lm",
               preProcess=c("center", "scale"),
               trControl=trainControl(method="cv", number=5))
lrfit3 <- train(Rented_count~Hour+Temperature++Radiation+Seasons+Holiday+Functioning_day, data=train_data,
               method="lm",
               preProcess=c("center", "scale"),
               trControl=trainControl(method="cv", number=5))
lrfit4 <- train(Rented_count~Hour+Temperature+Visibility+Seasons+Holiday+Functioning_day, data=train_data,
               method="lm",
               preProcess=c("center", "scale"),
               trControl=trainControl(method="cv", number=5))
lrfit5 <- train(Rented_count~Hour*Temperature+Seasons+Visibility+Seasons+Holiday+Functioning_day, data=train_data,
               method="lm",
               preProcess=c("center", "scale"),
               trControl=trainControl(method="cv", number=5))
```

-   Compare metric

``` r
results <- data.frame(t(lrfit1$results), t(lrfit2$results), t(lrfit3$results), t(lrfit4$results), t(lrfit5$results))
names(results)=c("fit1", "fit2", "fit3", "fit4", "fit5" )
results
```

    ##                   fit1
    ## intercept    1.0000000
    ## RMSE       465.3976582
    ## Rsquared     0.4879232
    ## MAE        344.3707431
    ## RMSESD      10.2802246
    ## RsquaredSD   0.0172969
    ## MAESD        8.5497026
    ##                    fit2
    ## intercept  1.000000e+00
    ## RMSE       4.428638e+02
    ## Rsquared   5.363242e-01
    ## MAE        3.128336e+02
    ## RMSESD     1.042180e+01
    ## RsquaredSD 7.643049e-03
    ## MAESD      6.226162e+00
    ##                    fit3
    ## intercept    1.00000000
    ## RMSE       464.16442820
    ## Rsquared     0.49037299
    ## MAE        343.73072109
    ## RMSESD      14.00310166
    ## RsquaredSD   0.01525954
    ## MAESD        8.95334501
    ##                    fit4
    ## intercept    1.00000000
    ## RMSE       454.88097701
    ## Rsquared     0.51063623
    ## MAE        335.64281452
    ## RMSESD       5.42038817
    ## RsquaredSD   0.01473154
    ## MAESD        8.65088791
    ##                    fit5
    ## intercept    1.00000000
    ## RMSE       432.96605030
    ## Rsquared     0.55661053
    ## MAE        305.51838105
    ## RMSESD      10.26004545
    ## RsquaredSD   0.01537876
    ## MAESD        7.23463962
