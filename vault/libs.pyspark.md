---
id: lqg7NFFuZ7cwgBNm0izp3
title: PySpark
desc: ''
updated: 1644203745512
created: 1644119248133
---

A library for working with [[techs.spark]] from [[langs.python]] code in a way similar to [[libs.pandas]], but built for scale via Spark's distributed processing.

## Vector Columns

> From [[courses.learn.databricks.perform-machine-learning-with-azure-databricks]] "What is Machine Learning" lab

PySpark requires a single column that is a `Vector` for training. This uses `VectorAssembler` to create this column:

```py
from pyspark.ml.feature import VectorAssembler

features = ["year", "month", "bodies"]

assembler = VectorAssembler(inputCols=features, outputCol="new_vector_col_name")
```

## Model Fitting

```py
from pyspark.ml.regression import LinearRegression

lr = LinearRegression(labelCol="temperature", featuresCol="features")

model = lr.fit(df)
```

### Regression Evaluation

```py
from pyspark.ml.evaluation import RegressionEvaluator

# MSE = Mean Squared Error
evaluator = RegressionEvaluator(predictionCol="prediction", labelCol="label_col_name", metricName="mse")
```

## Correlation

```py
from pyspark.ml.stat import Correlation

pearsonCorr = Correlation.corr(sparkDF, 'features').collect()[0][0]
pandasDF = pd.DataFrame(pearsonCorr.toArray())
```

Rendered as heatmap:

```py
import matplotlib.pyplot as plt
import seaborn as sns

fig, ax = plt.subplots()
sns.heatmap(pandasDF)
display(fig.figure)
```
