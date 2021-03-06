```python
# python 
# common imports and variables create 
import pandas as pd 
import numpy as np 
from cassandra.cluster import Cluster
from pyspark.sql.types import StructType,StructField, StringType, IntegerType,\
    ArrayType, DoubleType, TimestampType
 from pyspark.sql.functions import array_contains, lower, col

cluster = Cluster()
session = cluster.connect()
session.set_keyspace("killrvideo") 

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
# create cassandra table 
df = people_df.to_pandas_on_spark()
cols = list(df.columns) 
types = list(df.dtypes) 
type_mapper = {np.dtype("int32"):"int", np.dtype("int64") :"int", 
    np.dtype("float64") : "int", np.dtype("O"): "TEXT"}
primary_key_column = cols[0] 
table_name = "people" 
flds_typ_str = ",".join(tuple([ i + " " + type_mapper.get(j) for i, j in 
    zip(cols, types)]) + (f"primary key({primary_key_column})",) )
qry_str = f"create table {table_name} ( {flds_typ_str} )" 
session.execute(qry_str)

# insert dataframe rows to cassandra table
people_df.write.format("org.apache.spark.sql.cassandra").mode('append')
    .options(table="people", keyspace="killrvideo").save() 
```
```scala
// scala
import org.apache.spark.sql.types._val 
meSchema = StructType(Array(StructField("video_id", StringType),
    StructField("avg_rating",FloatType),
    StructField("description", StringType),
    StructField("genres", ArrayType(StringType)),
    StructField("mpaa_rating",StringType),
    StructField("release_date", TimestampType),
    StructField("title", StringType),
    StructField("type", StringType),
    StructField("user_id", StringType)))
    
val videos = spark.read.option("header", 
    "true").schema(meSchema).csv("file:///home/ubuntu/data/videos.csv")
videos.printSchema
```
```python
# python
schema = StructType([StructField("video_id", StringType(), True), 
    StructField("avg_rating", DoubleType(), True),
    StructField("description", StringType(), True),
    StructField("genres", ArrayType(StringType()), True),
    StructField("mpaa_rating", StringType(), True),
    StructField("release_date", TimestampType(), True),
    StructField("release_year", IntegerType(), True),
    StructField("title", StringType(), True),
    StructField("user_id", StringType(), True),]) 
videos = spark.read.option("header", "true").option("inferSchema", 
    "true").schema(schema).csv("file:///videos-v2.csv")
videos.printSchema() 
```

```scala
// scala
spark.sparkContext.textFile("file:///video-years.csv").flatMap(_.split(",")
    .drop(2)).map((_,1)).reduceByKey(_ + _).collect.foreach(println)
```
```python   
# python
spark.sparkContext.textFile("file:///video-years.csv")
    .map(lambda x: (int(x.split(",")[2]), 1)).reduceByKey(
    lambda x,y: x + y ).collect() 
```
```scala
// scala
spark.sparkContext.cassandraTable("killrvideo", "videos").
     | where("release_year = 2015").select("title").limit(5).
     | collect.foreach(println) 
```
```python
# python
spark.read.format("org.apache.spark.sql.cassandra").options(table="videos",     
    keyspace="killrvideo").load().filter(col("release_year")==2015)
    .select("title").limit(5).collect() 
```
```scala
// scala
spark.sparkContext.cassandraTable("killrvideo","videos_by_actor").where("actor = 'Johnny 
    Depp'").filter(row => row.getString("title").toLowerCase.contains("pirate"))
    .map(row => (row.getString("title"), row.getInt("release_year"), 
    row.getFloat("avg_rating"))).collect.foreach(println)
```

```python
# python
videos_df = spark.read.format("org.apache.spark.sql.cassandra")
    .options(table="videos",keyspace="killrvideo").load() 
videos_df.filter(videos_df.title.contains("Alice")).limit(5).rdd.map( lambda row: (str(row.title), int(row.release_year))).collect()
```
```scala
// scala
spark.sparkContext.cassandraTable("killrvideo","videos_by_actor").where("actor = 'Johnny 
    Depp'").filter(row =>row.getSet[String]("genres").contains("Adventure") &&
    row.getFloat("avg_rating") >= 6.2).map(row => (row.getString("title"),
    row.getInt("release_year"), row.getFloat("avg_rating"))).collect.foreach(println)
```
```python
# python
videos_df = spark.read.format("org.apache.spark.sql.cassandra")
    .options(table="videos",keyspace="killrvideo").load() 
videos_df.filter(array_contains(videos_df.genres,"Drama") & 
    (videos_df.avg_rating>=6.2)).limit(5).rdd.map( 
    lambda row: (str(row.title), int(row.release_year), 
    float(row.avg_rating))).collect() 
```
```scala
// scala
spark.sparkContext.cassandraTable("killrvideo","videos_by_actor")
    .where("actor = 'Johnny Depp'").select("release_year")
    .as((year:Int) => (year,1) ).reduceByKey(_ + _).collect
    .foreach(println)
```
```python
# python
videos_df = spark.read.format("org.apache.spark.sql.cassandra")
    .options(table="videos",keyspace="killrvideo").load() 
videos_df.filter(array_contains(videos_df.genres,"Drama") & 
    (videos_df.avg_rating>=6.5)).limit(1000).select("release_year").rdd.map( 
    lambda row: (int(row.release_year),1) ).reduceByKey( lambda x, y: x+y) .collect() 
```




