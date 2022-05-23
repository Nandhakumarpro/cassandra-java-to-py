```python
# common imports 
from cassandra.cluster import Cluster
from cassandra.policies import DCAwareRoundRobinPolicy , WhiteListRoundRobinPolicy
from cassandra.cluster import ExecutionProfile
from cassandra.cqlengine.management import query
from cassandra.query import BatchStatement, SimpleStatement
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
session = cluster.connect(keyspace)
```

```java
// java 
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1")
    .withLoadBalancingPolicy(newDCAwareRoundRobinPolicy()).build();
```
```python 
# python 
exec_profile = ExecutionProfile(load_balancing_policy= DCAwareRoundRobinPolicy())
cluster = Cluster(contact_points=contact_points, execution_profiles={"node1": exec_prof})
```

```java
SimpleStatementstatement = new SimpleStatement( "SELECT userid FROM user WHERE lastname= 'Smith'");
session.execute(statement);
```
```python
name = 'Smith'
simple_statement = SimpleStatement( "SELECT userid FROM user WHERE lastname= %s", (name,)) 
session.execute(simple_statement);
```
```java

```

```python
```







