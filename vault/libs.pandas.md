---
id: R2nk6un8cKgBVzDUFxuAv
title: Pandas
desc: ''
updated: 1644202769417
created: 1644119285641
---

A [[langs.python]] library for working with tabular data represented by Data Frames

## Correlation Calculation

```py
for col in df.columns:
    corelation = df.stat.corr("someColumn", col)    
```

`1` is maximum correlation (100% positive correlation)

`-1` is minimum correlation (100% inverse correlation)

`0` is no correlation