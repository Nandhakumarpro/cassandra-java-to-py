```python
# common imports 
from cassandra.cluster import Cluster
```

```java
// java
DseCluster cluster = DseCluster.builder().
	addContactPoints("127.0.0.1", "127.0.0.2")
	.withLoadBalancingPolicy( ... ).withRetryPolicy( ... )
	.withReconnectionPolicy( ... ).withPoolingOptions( ... )
	.build()
```

```python
# python

```

```java
// java
DseClustercluster = DseCluster.builder().addContactPoints(contactpoints).build();
DseSessionsession = cluster.connect(keyspace);
```

```python
# python
contact_points = ['127.0.0.1']  
keyspace = 'killrvideo'
cluster = Cluster(contact_points=contact_points)
session = cluster.connect('killrvideo')
```
