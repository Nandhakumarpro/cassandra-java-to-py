```python
# common imports 
from cassandra.cluster import Cluster
from cassandra.policies import DCAwareRoundRobinPolicy , WhiteListRoundRobinPolicy
from cassandra.cluster import ExecutionProfile
from cassandra.cqlengine.management import query
from cassandra.query import BatchStatement, SimpleStatement, ValueSequence
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
    .withLoadBalancingPolicy(new DCAwareRoundRobinPolicy()).build();
```
```python 
# python 
exec_profile = ExecutionProfile(load_balancing_policy= DCAwareRoundRobinPolicy())
cluster = Cluster(contact_points=contact_points, execution_profiles={"node1": exec_prof})
```

```java
// java
// 1.Without parameter  placeholder:
Statement statement =new SimpleStatement("select * from t1 where c1 = 5");
//2.With parameter  placeholder:
Statement statement =new SimpleStatement("select * from t1 where c1 = ?", 5);
```
```python
# python
# 1.Without parameter  placeholder:
statement = SimpleStatement("select * from t1 where c1 = 5");

# 2.With parameter  placeholder:
statement = SimpleStatement("select * from t1 where c1 = %s");
values = (5,)
session.execute(statement, parameters=[ValueSequence(values)])
```
```java
// java
// Named Syntax (field names):
ResultSet rs= session.execute("SELECT * FROM names");
for(Row row : rs) {
    String fname= row.getString("fname");
    String lname= row.getString("lname");  
    System.out.println("Name: " + fname+ " " + lname);
}
//Positional Syntax (column index):
String fname= row.getString(0);
String lname= row.getString(1);
```
```python
# python
# Named Syntax (field names):
rs= session.execute("SELECT * FROM names");
for row in rs:
    fname = row.fname
    lname = row.lname
    print("Name: " + fname + " " + lname);
# Positional Syntax (column index):
fname = row[0]
lanme = rown[1]
```







