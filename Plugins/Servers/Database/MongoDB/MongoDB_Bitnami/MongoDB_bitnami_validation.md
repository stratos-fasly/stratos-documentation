# MongoDB bitnami validation

Validation and Verification for MongoDB bitnami engine deployment.

## Functional Test

- MongoDB functional test can be done with MongoDB compass windows/MAC/Linux client and can be download from this website( https://www.mongodb.com/products/compass ).

- To Connect MongoDB cluster you need to create connection string URI string. If you have created single replica you will get single connection endpoint with port 27017. If you have created multiple replica cluster you have to get all replicas endpoint URL and create connection string.

    1. Get list of services from kubernetes cluster related to MongoDB deployment.
    ```
    kubectl get service -n default | grep {{plugin-name}}
    ```
    Ex:
    ```
    kubectl get service -n default | grep mongodb-demo-v090
    ```

    2. Then you will get external services endpoint URL’s with service type LoadBalancer.

    example out put in here I have created 2 replica cluster here you can see two LoadBalancer type endpoints has created.

    ![plot](./images/kubectl_get_svc_mongoDb.png)