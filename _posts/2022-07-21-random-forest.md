Random Forest
================
Fang Wu

Random forest models are based on simple tree model. Trees are very
easily interpreted by looking at the tree plot. However, they are highly
variable based on the specific data used to build them. Random forest
models are one way to combat this.

Random forest models use bootstrap sampling to fit many trees, each one
using a random subset of predictors. Final prediction is the mean or
majority vote. The random subset procedure makes the predictions are not
dominated by some specific predictors.

The standard practice is to
use ![p/3](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;p%2F3 "p/3")
for numeric response and
![\\sqrt(n)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Csqrt%28n%29 "\sqrt(n)")
for categorical response, where p represents the total number of
predictors.

I am going to build a random forest model on a data set from
[Kaggle](https://www.kaggle.com/datasets/sameepvani/nasa-nearest-earth-objects).
This data set compiles the list of NASA certified asteroids that are
classified as the nearest earth object. I would like to predict
hazardous based on the available predictors.

Importing packages

Reading in data

``` r
data <- read_csv("../data/neo.csv", col_names=TRUE) %>% select(-c(id,name, orbiting_body))
data$hazardous <- as.factor(data$hazardous)
```

Spliting data into train and test sets

``` r
set.seed(100)
train_index <- createDataPartition(data$hazardous, p=0.7, list=FALSE)
train_data <- data[train_index, ]
test_data <- data[-train_index, ]
```

Building random forest model

``` r
fit <- train(hazardous~., train_data, method="rf",
             trControl=trainControl(method="cv", number=5),
             tuneGrid=data.frame(mtry=3:5),
             ntree=50)
```

CV error on train data

``` r
fit$results
```

    ##   mtry  Accuracy     Kappa
    ## 1    3 0.9160821 0.4156972
    ## 2    4 0.9124807 0.3995377
    ## 3    5 0.9106564 0.3817984
    ##    AccuracySD     KappaSD
    ## 1 0.001243621 0.007686478
    ## 2 0.001357060 0.011025773
    ## 3 0.001023076 0.012404580

-   Subsetting randomly 3 predictors for each tree leads to best
    accuracy for the train data.

performance on test data

``` r
fitValue <- predict(fit, data.frame(test_data))
confusionMatrix(fitValue, test_data$hazardous)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction FALSE  TRUE
    ##      FALSE 23969  1624
    ##      TRUE    629  1028
    ##                          
    ##                Accuracy :
    ##                  95% CI :
    ##     No Information Rate :
    ##     P-Value [Acc > NIR] :
    ##                          
    ##                   Kappa :
    ##                          
    ##  Mcnemar's Test P-Value :
    ##                          
    ##             Sensitivity :
    ##             Specificity :
    ##          Pos Pred Value :
    ##          Neg Pred Value :
    ##              Prevalence :
    ##          Detection Rate :
    ##    Detection Prevalence :
    ##       Balanced Accuracy :
    ##                          
    ##        'Positive' Class :
    ##                          
    ##                 
    ##  0.9173         
    ##  (0.914, 0.9206)
    ##  0.9027         
    ##  < 2.2e-16      
    ##                 
    ##  0.4348         
    ##                 
    ##  < 2.2e-16      
    ##                 
    ##  0.9744         
    ##  0.3876         
    ##  0.9365         
    ##  0.6204         
    ##  0.9027         
    ##  0.8796         
    ##  0.9392         
    ##  0.6810         
    ##                 
    ##  FALSE          
    ## 

-   Accuracy on test data is even better than on train data.
