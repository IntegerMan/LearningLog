---
id: tAm7XPwvVMhFSja0UcbxU
title: Mlflow
desc: ''
updated: 1644207861672
created: 1644206973138
---

[Website](https://mlflow.org/)

> Source: [[courses.learn.work-with-mlflow-azure-databricks]]

Helps:
- Track Machine Learning Experiments
- Make Code Reproducable
- Standardize Model Packaging and Deployment

Serves as a Model Registry to track models

---

Runs store:

- Parameters
- Metrics
- Artifacts
- Source Code

Sample Experiment Code from an [[azure.databricks]] notebook:

```py
import mlflow
import mlflow.spark
from pyspark.ml.regression import LinearRegression
from pyspark.ml.feature import VectorAssembler
from pyspark.ml import Pipeline
from pyspark.ml.evaluation import RegressionEvaluator

# Load the Data for the experiment
filePath = "dbfs:/mnt/training/airbnb/sf-listings/sf-listings-2019-03-06-clean.parquet/"
airbnbDF = spark.read.parquet(filePath)
(trainDF, testDF) = airbnbDF.randomSplit([.8, .2], seed=42)

# Start an experiment
mlflow.set_experiment(f"/Users/{username}/tr-mlflow")

# Start a run within the experiment
with mlflow.start_run(run_name="LR-Single-Feature") as run:
  # Define pipeline
  vecAssembler = VectorAssembler(inputCols=["bedrooms"], outputCol="features")
  lr = LinearRegression(featuresCol="features", labelCol="price")
  pipeline = Pipeline(stages=[vecAssembler, lr])
  pipelineModel = pipeline.fit(trainDF)
  
  # Log parameters
  mlflow.log_param("label", "price-bedrooms")
  
  # Log model
  mlflow.spark.log_model(pipelineModel, "model")
  
  # Evaluate predictions
  predDF = pipelineModel.transform(testDF)
  regressionEvaluator = RegressionEvaluator(predictionCol="prediction", labelCol="price", metricName="rmse")
  rmse = regressionEvaluator.evaluate(predDF)
  
  # Log metrics
  mlflow.log_metric("rmse", rmse)
```

Seems to be extremely similar to the [[azure.ml.sdk]]

## Querying Past Runs

```py
from mlflow.tracking import MlflowClient

client = MlflowClient()
client.list_experiments()

experiment_id = run.info.experiment_id
runs = client.search_runs(experiment_id, order_by=["attributes.start_time desc"], max_results=1)
runs[0].data.metrics
```

