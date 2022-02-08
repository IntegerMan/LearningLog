---
id: RPBB77brYlknGXxyUWCFo
title: Petastorm
desc: ''
updated: 1644291100370
created: 1644290770035
---

A library that allows for distributed or single machine processing of deep learning models in [[techs.parquet]] form.

[GitHub Repo](https://github.com/uber/petastorm)

> Source: [[courses.learn.deep-learning-with-horovod-distributed-training]]

```py
from petastorm import make_batch_reader
from petastorm.tf_utils import make_petastorm_dataset

abs_file_path = file_path.replace("dbfs:/", "/dbfs/")

with make_batch_reader("file://" + abs_file_path, num_epochs=None) as reader: 
  dataset = make_petastorm_dataset(reader).map(lambda x: (tf.reshape(x.features, [-1,8]), tf.reshape(x.label, [-1,1])))
  model = build_model()
  optimizer = keras.optimizers.Adam(lr=0.001)
  model.compile(optimizer=optimizer,
                loss='mse',
                metrics=['mse'])
  model.fit(dataset, steps_per_epoch=10, epochs=10)
```