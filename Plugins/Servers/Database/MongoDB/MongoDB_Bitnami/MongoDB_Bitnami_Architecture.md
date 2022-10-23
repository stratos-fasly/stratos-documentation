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

