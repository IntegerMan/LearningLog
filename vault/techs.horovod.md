---
id: OBjZ9QfF1a76sMRkQzltj
title: Horovod
desc: ''
updated: 1644290762247
created: 1644289459982
---

A distributed training framework for machine learning.

[GitHub Repo](https://github.com/horovod/horovod)


> Source: [[courses.learn.deep-learning-with-horovod-distributed-training]]
```py
import horovod.tensorflow.keras as hvd
from tensorflow.keras import optimizers

def run_training():
    hvd.init()

    hvd.rank() # gets the unique process ID (0 to size)
    hvd.size() # gets the number of processors available to Horovod

    # Horovod: adjust learning rate based on number of GPUs/CPUs.
    optimizer = optimizers.Adam(lr=0.001*hvd.size())
    hvd.DistributedOptimizer(optimizer)

from sparkdl import HorovodRunner

hr = HorovodRunner(np=-1)
hr.run(run_training_horovod)
```

Horovod also includes callbacks to report metrics from various engines