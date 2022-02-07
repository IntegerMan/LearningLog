---
id: qkCwY6GiYKmp8qsLoB7Jh
title: k-fold Validation
desc: ''
updated: 1644209583257
created: 1644209441417
---
> Source: [[courses.learn.perform-model-selection-with-hyperparameter-tuning]]


K-Fold Validation is used to avoid overfitting by validating it against smaller 
"folds" in the training data set.

Validation could be done exhaustively looking at all combinations of data, but typically you go with k-fold to reduce training time and training costs.

K-Fold Validation segments the data into validation folds and training folds and tunes hyper-parameters to find the best performing pipeline across all folds.

Once the ideal hyper-parameters are identified and validated, you train them with all available data.

