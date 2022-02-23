---
id: QGMczrnMm6l5Iu3MdmStL
title: AppInsights
desc: ''
updated: 1644461898180
created: 1644461766657
---

> Source [[courses.coursera.build-and-operate-machine-learning-solutions-with-azure]]

## Writing to App Insights

```py
def init():
    global model
    model = joblib.load(Model.get_model_path('my_model'))

def run(raw_data):
    data = json.loads(raw_data)['data']
    predictions = model.predict(data)
    log_txt = 'Data:' + str(data) + ' - Predictions:' + str(predictions)
    print(log_txt)

    return predictions.tolist()
```

## Querying App Insights

```sql
traces
|where message == "STDOUT"
  and customDimensions.["Service Name"] = "my-svc"
| project  timestamp, customDimensions.Content
```

Additional resources: https://docs.microsoft.com/en-us/azure/machine-learning/how-to-enable-app-insights