```python
# common imports 
from cassandra.cluster import Cluster
from cassandra.policies import DCAwareRoundRobinPolicy , WhiteListRoundRobinPolicy
from cassandra.cluster import ExecutionProfile
from cassandra.cqlengine.management import query
from cassandra.query import BatchStatement, SimpleStatement, ValueSequence
import uuid
from cassandra.cqlengine import columns
from cassandra.cqlengine import connection
from datetime import datetime
from cassandra.cqlengine.management import sync_table
from cassandra.cqlengine.models import Model
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
exec_profile =ExecutionProfile(load_balancing_policy=..., retry_policy=..., )
cluster = Cluster(contact_points=contact_points, execution_profiles=\
    {"node1": exec_prof},reconnection_policy=... , )
session = cluster.connect(wait_for_all_pools=False) # True | False
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
DseClustercluster = DseCluster.builder() .addContactPoint("127.0.0.1") 
    .withLoadBalancingPolicy(new TokenAwarePolicy(
    new DCAwareRoundRobinPolicy("DC-West"));
```
```python
# python 

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
```java
// java
// 1.Initialize statement using ?Or :variable as variable placeholders in the CQL:
PreparedStatement prepared = session.prepare( "insert into product (sku, description) values (?, ?)")
//; Or:
PreparedStatement prepared = session.prepare( "insert into product (sku, description) values (:sku, :description)");
// 2.Bind the statement with bind() method of PrepareStatementobject:
BoundStatement bound = prepared.bind("234827", "Mouse");
// 3.Execute statement using execute() method of session object: 
session.execute(bound); 
// Nested example:
session.execute(prepared.bind("234827", "Mouse"));
```
```python
#python 
prepared = session.prepare( "insert into product (sku, description) values (?, ?)")
         # (or)
prepared = session.prepare( "insert into product (sku, description) values (:sku, :description)")

bound = prepared.bind(("234827", "Mouse")); 
       # (or)
bound = prepared.bind({"sku": "234827", "description": "Mouse"}); 
session.execute(bound); 
```
```java
// java
@UDT(name="address")
public class Address{
    private String street;
    private String city;
    private int zipCode;
    private List<Phone> phones;
    ...
}
```
[cassandra driver object mapper](https://docs.datastax.com/en/developer/python-dse-driver/2.11/object_mapper)

```python
# python

connection.register_connection('cluster1', ['127.0.0.1'])

class Videos(Model):
    __keyspace__ = "killrvideo"
    __connection__ = "cluster1"
    __table_name__ = "videos"  # optional
    uuid = columns.UUID( primary_key=True, default=uuid.uuid4) # partition
    txn_id = columns.UUID( )
    side_note = columns.Text(index=True) # clustering 
    tags = columns.Set(columns.Text()) 
    addresses = columns.List(columns.Text()) 
    phone_nos = columns.Map(columns.Text(), columns.BigInt()) 
    created_at = columns.DateTime(index=True, default=datetime.now)
    
sync_table(Vidoes)

# create empty object and set values one by one all the required attrs
v = Videos()    
v.phone_nos = {"personal": 1111111111, "office": 123456}
v.addresses = ["adderess 1", "address 2"]
v.side_note = "sample videos"
v.txn_id = uuid.uuid4()
v.tags = {"sample2", "test2",}
v.save() 
               # or
# create in one line 
Videos.objects.create( tags = {"sample2", "test2",}, txn_id = uuid.uuid4(), 
    side_note = "Sample videos", addresses = ["adderess 1"], phone_nos = \
    {"personal": 1111111111, "office": 123456}) 
    
# filtering 

```
useful links:
[connection register](https://docs.datastax.com/en/developer/python-dse-driver/2.4/cqlengine/connections/)








