```python
# python 
# common imports and variables
import pandas as pd 
import numpy as np 
from cassandra.cluster import Cluster

cluster = Clusrter()
session = cluster.connect()

```

```scala
//scala
import org.apache.spark.sql.cassandra._val people = spark.read.option("header", 
    "true").option("inferSchema", "true").csv("file:///home/ubuntu/data/people.csv")
people.createCassandraTable("killrvideo","people",
    partitionKeyColumns = Some(Seq("name")))
people.write.format("org.apache.spark.sql.cassandra")
    .options(Map("keyspace" -> "killrvideo", "table" -> "people")).save()
```

```scala
```
```python 
# python 
people_df = spark.read.format("org.apache.spark.sql.cassandra").option("header", 
    "true").option("inferSchema", "true").csv("/root/Documents/people.csv");
type_mapper = {np.int32:"int", np.int64:"int", np.float64: "int", np.object0: "TEXT"}
df = people_df.to_pandas_on_spark()


```