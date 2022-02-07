---
id: lqg7NFFuZ7cwgBNm0izp3
title: PySpark
desc: ''
updated: 1644204301816
created: 1644119248133
---

A library for working with [[techs.spark]] from [[langs.python]] code in a way similar to [[libs.pandas]], but built for scale via Spark's distributed processing.

See [[libs.pyspark.mllib]] for Machine Learning functions

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
