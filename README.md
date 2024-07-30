# Boston Housing Prediction using KNN

## Overview

This project demonstrates two different approaches for predicting Boston Housing Prices using K-Nearest Neighbors (KNN) regression. KNN is a supervised machine learning algorithm used for both classification and regression tasks. This document outlines the methods used, including data preparation, model training, and evaluation.

## KNN Information

### What is KNN?

K-Nearest Neighbors (KNN) is a supervised machine learning algorithm used for classification and regression. It works by finding the 'K' closest data points to a given test point and making predictions based on those neighbors. 

- **K**: The number of neighbors to consider.

## Models Overview

### Model 1: KNN with Varying K Values

- **Objective**: Determine the best K value for predicting housing prices.
- **Data Split**: Training set (60%), Testing set (40%).
- **Normalization**: Data is normalized to ensure consistent scaling across features.
- **Evaluation**: Root Mean Squared Error (RMSE) is used to evaluate model performance.

#### Steps:
1. Load the data and remove NA values.
2. Drop columns with less correlation to the target variable.
3. Split the data into training and testing sets.
4. Normalize the data.
5. Compute KNN for K values from 1 to 5.
6. Determine the best K based on RMSE.

### Model 2: KNN with Caret Package

- **Objective**: Simplified KNN regression using the caret package.
- **Data Split**: Training set (85%), Testing set (15%).
- **Normalization**: X data is scaled.
- **Evaluation**: RMSE is used to evaluate model performance.

#### Steps:
1. Load the data and split into training and testing sets.
2. Scale the features.
3. Train the KNN model using the caret package.
4. Predict housing prices and evaluate RMSE.
5. Plot the predicted vs. actual values.

## Data Preparation

### Loading and Preparing Data

The dataset used is `data.csv`, which contains the following columns:

- `CRIM`, `ZN`, `INDUS`, `CHAS`, `NOX`, `RM`, `AGE`, `DIS`, `RAD`, `TAX`, `PTRATIO`, `B`, `LSTAT`, `MEDV`

#### Removing NA Values

```r
na_Values <- length(which(is.na(boston_df) == T))
if(na_Values > 0){
    boston_df <- boston_df[complete.cases(boston_df),]
}
```

#### Dropping Unnecessary Columns

```r
drop_Columns <- c('B', 'ZN', 'INDUS', 'NOX', 'AGE', 'RAD', 'LSTAT')
boston_df <- boston_df[, !colnames(boston_df) %in% drop_Columns]
```

## Model 1: KNN Implementation

### Code

```r
set.seed(123)
train_index <- sample(row.names(boston_df), 0.6 * dim(boston_df)[1])
testing_index <- setdiff(row.names(boston_df), train_index)

train_df <- boston_df[train_index, -7]
test_df <- boston_df[testing_index, -7]

# Normalization
norm_Values <- preProcess(train_df, method = c('center', 'scale'))
train_norm <- as.data.frame(predict(norm_Values, train_df))
test_norm <- as.data.frame(predict(norm_Values, test_df))

# Finding Best K
accuracy_df <- data.frame(k = seq(1, 5, 1), RMSE = rep(0, 5))
for(i in 1:5){
    knn.predict <- class::knn(train = train_norm[, -6], test = test_norm[, -6], cl = train_df[, 6], k = i)
    accuracy_df[i, 2] <- RMSE(as.numeric(as.character(knn.predict)), test_df[, 6])
}
print(accuracy_df)
```

## Model 2: KNN with Caret Package

### Code

```r
library(caret)

# Data Split
idx = createDataPartition(boston_df$MEDV, p = .85, list = F)
train_data = boston_df[idx, ]
test_data = boston_df[-idx, ]

train_x_data = train_data[, -7]
train_x_data = scale(train_x_data)[,]
train_y_data = train_data[,7]

test_x_data = test_data[, -7]
test_x_data = scale(test_x_data[,-7])[,]
test_y_data = test_data[,7]

# KNN Model
KNN_model = knnreg(train_x_data, train_y_data)
pred = predict(KNN_model, data.frame(test_x_data))

# Prediction Accuracy
rmse = caret::RMSE(test_y_data, pred)
cat("RMSE: ", rmse)

# Plot
x = 1:length(test_y_data)
plot(x, test_y_data, col = "yellow", type = "l", lwd=2, main = "MEDV Price Prediction")
lines(x, pred, col = "red", lwd=3)
legend("topright", legend = c("Org. MEDV", "Pred. MEDV"), fill = c("yellow", "red"), col = 2:3, adj = c(0, 0.6))
grid()
```

## Results and Discussion

- **Model 1**: The K value with the lowest RMSE is considered, though overfitting is a concern. Typically, a slightly higher K (e.g., 3) may be chosen to avoid overfitting.
- **Model 2**: Provides a simplified approach with the caret package, making it easier to apply KNN regression and evaluate performance.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

- [KNN Algorithm](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm)
- [Caret Package Documentation](https://topepo.github.io/caret/)

---
