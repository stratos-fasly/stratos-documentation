# Overview
The MongoDB bitnami solution comes with two architectures “standalone” and “replicaset”.

Standalone - In standalone architecture will deploy only one statefulset with one pod(replicaset=1) this will work as a primary database and there is no replication.

Replicaset- Replicaset architecture will come with 2 statefulset deployment that’s are called MongoDB cluster and MongoDB arbiter. The mongoDB cluster deploy with 3 replicaset nd arbiter will deploy with one replicaset.

## MongoDB Cluster

Mongo DB cluster will deploy below components.

Replicaset Pods - For stratos deployment we deploy 3 pod cluster in mongoDB deployments.

Service - MongoDB cluster will deploy clusterIP headless service the default port is 27017

LoadBalancers - If externl access enabled it will deploy kubernetes LoadBalancer type services for each replica.

Persistent volume - To keep data persistent, persistent volume will be create with persistent volume claim of 8GB (default value can be change with helm parameter).

## MongoDB Arbiter

Replicaset pod - Deploy with one pod 

Service - create kubernetes headless service.

## How MongoDB cluster works

A collection of datasets distributed across many shards (servers) in order to achieve horizontal scalability and better performance in read and write operations.

### How replicate Data

MongoDB achieves replication by the use of replica set. A replica set is a group of MongoDB instances that host the same data set. In a replica, one node is primary node that receives all write operations. All other instances, such as secondary's, apply operations from the primary so that they have the same data set.

### Sharding and Replication

Replication: The primary server node copies data onto secondary server nodes. This can help increase data availability and act as a backup, in case if the primary server fails.

Sharding: Handles horizontal scaling across servers using a shard key. This means that rather than copying data holistically, sharding copies pieces of the data (or “shards”) across multiple replica sets. These replica sets work together to utilize all of the data.

### MongoDB Arbiter

An arbiter participates in elections for primary but an arbiter does not have a copy of the data set and cannot become a primary.

### Read & Write Endpoints

In MongoDB we need to pass all endpoint to the MongoDB client when we provide connection string, Because MongoDB elect primary node arbitraly when primary goes down or fail the mongoDB select one of other node in replicaset as primary node. 

Therefore we need to pass all nodes access points the mongoDB clients know to select which is the primary the node.

The database connection endpoint.

```
mongodb://{{username}}:{{password}}@{{connection-endpoint-1}}:27017,{{connection-endpoint-2}}:27017,{{connection-endpoint-3}}:27017
```

The example connection string will be like below.

```
mongodb://root:*******@aac78e7993c9945f399a5089b8195c78-2108571102.us-west-2.elb.amazonaws.com:27017,a6455865c52dd42c89f1bc2f1d6e5298-1634874543.us-west-2.elb.amazonaws.com:27017,ad3ae00fadef54c54b1282d519f42caf-1996935322.us-west-2.elb.amazonaws.com:27017
```

### Implement MongoDB with externa endpoints

To create external endpoints we need to pass following set parameters 

```
--set externalAccess.enabled=true
--set externalAccess.autoDiscovery.enabled=true
--set rbac.create=true
```

Then MongoDB will create addition kubernetes LoadBalancer type service for each replicaset and that external endpoint can be used to connect to MongoDB cluster.