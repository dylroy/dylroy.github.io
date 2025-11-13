---
title: "Geospatial Data Science and Computing #881c1c"
excerpt: "This is a collection of work, knowledge, code, etc. that could potentially be turned into a course. Active work in progress.<br/><img src='/images/FourthCliffBluff.png'>"
collection: portfolio
published: true
---

# Work



# Knowledge

Notes regarding storm surge:
[notes1](../files/W5_Tues_Coastal.md)

Notes regarding wave breaking:
[notes2](../files/W10_Thurs_Coastal.md)

# Code

The following is R code for comparing ridge regression and lasso regression which are $$ \ell_2 $$ and $$ \ell_1 $$ penalized, respectively.

```
library(readr)
library(glmnet)

# Part a.)
College = read_csv("College.csv") # I put the csv in my wd, so path is relative
College = College[, -c(1)]
College$Private = as.factor(College$Private)

K = 5
n = nrow(College)
# Create folds
folds = sample(rep(1:K, length.out = n))
table(folds)
College$folds = folds

# Split data within each fold
Kfolds = lapply(1:K, function(k) {
  data_k <- College[College$folds == k, ]
  data_k = data_k[, -c(ncol(data_k))]
  list(
    #X = model.frame(Apps ~ ., data_k)[,-1],
    #y = data_k$Apps
  )
})

# Part b.)
x = model.matrix(Apps ~ ., College[,-c(ncol(College))])[,-1]
y = College$Apps

ols_mse = list()

for (k in 1:K){
  data_k_train = College[College$folds != k, ]
  data_k_train = data_k_train[, -c(ncol(data_k_train))] # remove folds column
  data_k_test = College[College$folds == k, ]
  data_k_test = data_k_test[, -c(ncol(data_k_test))] # remove folds column
  
  X_train.ole = model.frame(Apps ~ ., data_k_train)
  
  X_test.ole = model.frame(Apps ~ ., data_k_test)[,-1]
  y_test.ole = data_k_test$Apps
  
  ols = lm(Apps ~ ., data = X_train.ole)
  
  ols_pred = predict(ols, newdata = X_test.ole)
  mse = mean((y_test.ole - ols_pred)^2)
  ols_mse = c(ols_mse, mse)
}

# Part c.)
grid = 10^seq(10, -2, length = 100)

ridge_mse = list()
lasso_mse = list()
num_coef_lasso = list()

# data.nofolds = College[, -c(ncol(College))]
# X = model.frame(Apps ~ ., data.nofolds)[, -1]
# y = data.nofolds$Apps

for (k in 1:K){
  data_k_train = College[College$folds != k, ]
  data_k_train = data_k_train[, -c(ncol(data_k_train))] # remove folds column
  # data_k_train = data_k_train[, -c(1)] # remove private column
  data_k_test = College[College$folds == k, ]
  data_k_test = data_k_test[, -c(ncol(data_k_test))] # remove folds column
  # data_k_test = data_k_test[, -c(1)] # remove private column
  
  X_train = model.matrix(Apps ~ ., data_k_train)[,-1]
  y_train = data_k_train$Apps
  
  X_test = model.matrix(Apps ~ ., data_k_test)[,-1]
  y_test = data_k_test$Apps
  
  ridge.mod = glmnet(X_train, y_train, alpha = 0, lambda = grid)
  lasso.mod = glmnet(X_train, y_train, alpha = 1, lambda = grid)
  
  # five-fold CV ridge
  cv.outridge = cv.glmnet(X_train, y_train, alpha = 0, nfolds = 5) 
  bestlam.ridge = cv.outridge$lambda.min
  
  # five-fold CV lasso
  cv.outlasso = cv.glmnet(X_train, y_train, alpha = 1, nfolds = 5) 
  bestlam.lasso = cv.outlasso$lambda.min
  
  ridge.pred = predict(ridge.mod, s = bestlam.ridge , newx = X_test)
  testMSE.ridge = mean((y_test - ridge.pred)^2)
  ridge_mse = c(ridge_mse, testMSE.ridge)
  
  lasso.pred = predict(lasso.mod, s = bestlam.lasso , newx = X_test)
  testMSE.lasso = mean((y_test - lasso.pred)^2)
  lasso_mse = c(lasso_mse, testMSE.lasso)
  
  out <- glmnet(x, y, alpha = 1, lambda = grid)
  lasso.coef <- predict(out, type = "coefficients", s = bestlam.lasso)
  lasso.coef[lasso.coef != 0]
}

ols_mse.mean = mean(unlist(ols_mse))
lasso_mse.mean = mean(unlist(lasso_mse))
ridge_mse.mean = mean(unlist(ridge_mse))
```{:.language-r}

