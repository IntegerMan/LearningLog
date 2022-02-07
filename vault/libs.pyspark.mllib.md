---
id: B6DOC7zvZktowZ2vTUECl
title: Spark MLlib
desc: ''
updated: 1644206153900
created: 1644204121310
---

## Definitions

> Transformers takes a DataFrame as an input and returns a new DataFrame with one or more columns appended to it.
> 
> -- [[courses.learn.databricks.train-machine-learning-model]]

Note: Trained models _are_ transformers

> Estimators take a DataFrame as an input and returns a model, which is also a transformer
>
> -- [[courses.learn.databricks.train-machine-learning-model]]

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

# Linear equation:
intercept = model.intercept
coefficients = model.coefficients
```

### Regression Evaluation

```py
from pyspark.ml.evaluation import RegressionEvaluator

# MSE = Mean Squared Error
evaluator = RegressionEvaluator(predictionCol="prediction", labelCol="label_col_name", metricName="mse")
```

---
> Source: [[courses.learn.databricks.train-machine-learning-model]]

Available on `model.summary`:

- coefficientStandardErrors
- degreesOfFreedom
- devianceResiduals
- explainedVariance
- featuresCol
- labelCol
- meanAbsoluteError
- meanSquaredError
- numInstances
- objectiveHistory
- pValues (low means not random)
- predictionCol
- predictions
- r2 (higher is better)
- r2adj
- residuals
- rootMeanSquaredError
- tValues
- totalIterations

## Featurization

### One-Hot Encoding

> Source: [[courses.learn.databricks.train-machine-learning-model]]

```py
from pyspark.ml.feature import StringIndexer
from pyspark.ml.feature import OneHotEncoder

# Extract indexes from a column
uniqueTypesDF = df.select("some_string_column").distinct()
indexer = StringIndexer(inputCol="some_string_column", outputCol="type_index")
indexerModel = indexer.fit(uniqueTypesDF)                                  
indexedDF = indexerModel.transform(uniqueTypesDF)                           

# Encode things into a vector
encoder = OneHotEncoder(inputCols=["type_index"], outputCols=["encoded_column"])
encoderModel = encoder.fit(indexedDF)
encodedDF = encoderModel.transform(indexedDF)
```

## Missing Values

### Dropping Missing
> Source: [[courses.learn.databricks.train-machine-learning-model]]

```py
df = df.na.drop(subset=["some_column", "another_column"])
```

### Imputing Values

> Source: [[courses.learn.databricks.train-machine-learning-model]]
 
```py
from pyspark.ml.feature import Imputer

imputeCols = [
  "some_column",
  "another_column",
]

imputer = Imputer(strategy="median", inputCols=imputeCols, outputCols=imputeCols)
imputerModel = imputer.fit(df)
imputedDF = imputerModel.transform(df)
```


## Pipelines

> Pipelines combines together transformers and estimators to make it easier to combine multiple algorithms.
>
> -- [[courses.learn.databricks.train-machine-learning-model]]

```py
from pyspark.ml import Pipeline

pipeline = Pipeline(stages=[
  indexer, 
  encoder, 
  imputer
])
```
