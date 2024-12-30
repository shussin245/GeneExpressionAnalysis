# README: Gene Expression Analysis and Immunotherapy Response Prediction

## Introduction

Welcome to this walk-through tutorial on analyzing gene expressions to develop predictive models for patient responses to immunotherapy. This guide covers key terms, code explanations, and practical steps to create elastic net models. Simplified datasets and parameters are used for clarity, but the concepts can be scaled to more complex real-world applications.

Through this tutorial, you’ll learn:
- The importance of statistics and computational analysis in biology.
- How to implement elastic net models for gene expression data.
- Techniques to evaluate model performance.

## Prerequisites

This project requires the following libraries to be installed. Installation may take 3-5 minutes. 

### Required Libraries
```
install.packages("dplyr")
install.packages("glmnet")
install.packages("ROCR")
install.packages("tidyr")
install.packages("vip")

library(dplyr)
library(glmnet)
library(ROCR)
library(tidyr)
library(vip)
```

## The Data

### Training and Testing Data

- **Training Dataset**: Used to train the model.
- **Testing Dataset**: Used to evaluate the model's performance.

### Variables

- `x` and `xt`: Gene expressions for training and testing datasets.
- `y` and `yt`: Patient responses to immunotherapy (0 = no response, 1 = response).

Load datasets using the following code:

```
# Training Dataset
train <- read.csv("https://raw.githubusercontent.com/shussin245/NGS/main/Sample_Train.csv")
x <- as.matrix(train[1:249])
y <- train$Response                

# Testing Dataset
test <- read.csv("https://raw.githubusercontent.com/shussin245/NGS/main/Sample_Test.csv")
xt <- as.matrix(test[1:249])
yt <- test$Response
```

You can inspect the datasets by typing:

```
train
test
```

## Key Concepts

### Regularization
- Reduces model complexity and eliminates features with low predictive value.
- **L1 Regularization (Lasso)**: Penalizes absolute values, reducing some coefficients to zero.
- **L2 Regularization (Ridge)**: Penalizes squared coefficients, shrinking them closer to zero without elimination.
  
### Elastic Net
- Combines L1 and L2 penalties.
- Controlled by the alpha (𝛼) parameter:
  1. 𝛼 = 0: Ridge regression.
  2. 𝛼 = 1: Lasso regression.
  3. Intermediate values mix the two approaches.

### Cross-Validation
- Identifies the best lambda (𝜆) value to minimize cross-validated error.
- Prevents overfitting.

## Code Walkthrough

### Setting Up
- Set a seed for reproducibility: `set.seed(123)`

### Model Creation
- Build models for multiple alpha values (e.g., 0.3 and 1).
- Use cross-validation to find the best lambda value:
```
model <- cv.glmnet(x, y, alpha=0.3, nlambda=1000, family="binomial", type.measure="auc", nfolds=3)
```

### Model Evaluation
- Validate models with AUC scores and ROC curves:
  - **AUC**: Measures model performance (higher is better).
  - **ROC Curve**: Visualizes true positive rate vs. false positive rate.

### Feature Importance
- Visualize the most important genes using the vip package:
```
gene_imp <- vip(best_lam_model, num_features=20L, geom="col", aesthetics=list(col="pink"))
plot(gene_imp)
```

## Running the Code

To execute the full analysis:
1. Load the datasets.
2. Iterate through alpha values and evaluate models.
3. Visualize results:
   - AUC scores
   - ROC curves
   - Gene importance plots
     
Example:

```
for (al in c(0.3, 1)) {
  for (i in 1:2) {
    # Model building, validation, and visualization
    ...
  }
}
```

## Outputs

- Model Performance:
  - Cross-validated AUC score.
  - Validation AUC score.

- Visualizations:
  - ROC curves.
  - Feature importance plots.

- Selected Genes:
  - List of genes contributing to the model.

## Notes and Suggestions

- Modify alpha values to observe their impact on feature selection.
- Try using different datasets for a more comprehensive analysis.
- Feel free to extend the script for more iterations or larger datasets.
